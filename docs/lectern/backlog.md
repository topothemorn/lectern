# Backlog

Wants and ideas, in the order they came up. Nothing here is designed or committed to. Items move into `specs/` once designed.

## Goals

- Main page shows books quickly after sign-in. See `baseline.md` for the cause.
- Page turns by tapping the side of the screen feel right on a phone. Upstream has the feature as of v3.5.0; how it feels is not yet judged.
- Big changes to the UI.
- Theming.
- Speed in general.

## Observations from reading the code

- Tap-to-turn delay and possibly dropped taps in the ebook reader (`baseline.md`).
- `GET /api/v1/authors` is slow on the main page.
- Static files are sent with `no-store`, so anything not covered by the service worker is re-downloaded every time.
- 34 frontend files still depend on the full in-memory book list at our fork point.
- Other tools can depend on Grimmory's API. Treat API changes as breaking unless shown otherwise.
