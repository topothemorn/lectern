# Setup

What it takes to build and run Lectern for development. **Nothing in the "Dev environment" section has been run yet**; it is drawn from reading upstream's files. Update this file as steps are proven or corrected.

## Requirements

| Tool | Version | Needed for |
|---|---|---|
| Git | any recent | On Windows set `core.longpaths=true`; some paths are long. |
| Docker | any recent | Option A, and the dev database in Option B |
| Node | 26+ (`.nvmrc`, `package.json`) | Option B |
| pnpm | 12.11.2 (pinned upstream) | Option B |
| JDK | 25 | Option B |
| `just` | any recent | Optional. The root `Justfile` is upstream's command surface. |

## Dev environment

### Read this first: port 6060

Upstream's dev setup assumes it owns port **6060**, which is also the port a normal Grimmory install uses:

- `dev.docker-compose.yml` publishes the dev backend on `${BACKEND_PORT:-6060}`.
- `frontend/src/environments/environment.ts` hardcodes the dev frontend's API address as `http://<host>:6060`.

If a real Grimmory already runs on the same machine, the dev backend fails to bind, and the dev frontend on port 4200 sends its requests to the **real** install. Setting `BACKEND_PORT` alone does not fix the second part.

**First setup task:** give Lectern's dev stack its own backend port (suggestion: 6160) in both places, and record the change in `upstream-sync.md` because `environment.ts` is an upstream file. Do not start the dev stack next to a real install before this is done.

### Option A: Docker dev stack

Needs no local Node, pnpm or Java. From the repo root:

```bash
BACKEND_PORT=6160 docker compose -f dev.docker-compose.yml up
```

(`just dev-up` does the same thing if `just` is installed.)

| Service | Default address | Notes |
|---|---|---|
| Frontend | http://localhost:4200 | Angular dev server with hot reload |
| Backend | port from `BACKEND_PORT` | Gradle `bootRun` inside a JDK 25 container |
| MariaDB | localhost:3366 | Dev database, user/password/database all `grimmory` |
| Java debug | 127.0.0.1:5005 | |

The stack mounts `./shared/data`, `./shared/books` and `./shared/bookdrop` from the repo folder. `/shared/` is git-ignored. First start downloads Gradle and npm dependencies and will be slow. Give Docker enough memory for a Gradle build plus MariaDB.

### Option B: local tools

```bash
just db-up        # dev database only, in Docker
just api run      # backend
just ui install && just ui dev   # frontend on :4200
```

The same port-6060 warning applies.

### Checks and tests

```bash
just check        # full verification pass (backend + frontend)
just test         # both test suites
just ui test      # frontend only (Vitest)
just api test     # backend only (JUnit)
just image-build  # build the production image as grimmory:local
```

## Working with real data

- Never connect a dev build to a real install's database or data folder. Use a copy.
- Database dumps contain real accounts and password hashes. They never go in the repo.
- Mount real book files read-only.
- Notes about any particular machine, network or install go in `docs/lectern/private/` (git-ignored), never in tracked files.
