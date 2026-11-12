# Baseline — the code we started from

Findings about Grimmory v3.5.0 and the commit we forked from (`c80ce813b`). Each item says how it was established. Measurements taken on any real install are kept out of this repository.

## Speed: main page

- In v3.5.0 the dashboard waits on a request for the whole library: `GET /api/v1/books?stripForListView=false`, issued in `frontend/src/app/features/book/service/book.service.ts` and consumed by `dashboard-book.service.ts`. The response carries full metadata, including descriptions, for every book. On a library of several thousand books that is a multi-megabyte response taking seconds, and no covers appear until it finishes. Observed in the browser's resource timings.
- The paged endpoint `GET /api/v1/books/page` returns a page of 50 in a small fraction of the size and time.
- Upstream is migrating away from the full list: at our fork point `dashboard-row.component.ts` uses the paged `BookQueryService`, and files depending on the full list dropped from 45 (v3.5.0) to 34. Not yet verified by running it: whether the main page at our fork point still triggers the full-library request from somewhere else.
- `GET /api/v1/authors` is also slow on the main page.

## Speed: app shell

- v3.5.0 serves JavaScript uncompressed: the server's compression list has `application/javascript`, but the files are sent as `text/javascript`. Workaround on a v3.5.0 install: set `SERVER_COMPRESSION_MIME_TYPES` to a list that includes `text/javascript`.
- Upstream fixed the list in `backend/src/main/resources/application.yaml` after v3.5.0, so our fork point already has it.
- Static files are served with `Cache-Control: no-store`; repeat loads rely on the Angular service worker instead. Not investigated further.

## Reader: tap to turn the page (read from code, not tested on a device)

- v3.2.4 disabled tap-to-turn on touch devices in the ebook reader. v3.5.0 added a "Tap to Turn Page" setting, default on: left 30% back, right 30% forward, middle toggles toolbars. Code is in `frontend/src/app/features/readers/ebook-reader/core/event.service.ts`.
- Two suspected flaws in that code, from reading only:
  - Every tap waits 300 ms (`DOUBLE_CLICK_INTERVAL_MS`) before the page turns.
  - `processIframeClick` returns early if `longHoldTimeout` has already expired. That timer runs 500 ms from touch start, and processing happens 300 ms after touch end, so a tap held longer than about 200 ms appears to be dropped.
- Comic reader has a "click to paginate" switch that is off by default. PDF reader has its own tap zones.

## How to measure the main page

Open the app logged in, wait for it to settle, then in the page console read `performance.getEntriesByType('resource')` and look at entries whose URL starts with `/api`. Compare `responseStart - requestStart` (server time) and `decodedBodySize` for the books request, and the `startTime` of the first thumbnail.
