# Lectern — project charter

## Why this exists

Grimmory does a lot, but it is slow to load on a large library and its interface leaves room for a different take. Lectern forks it to keep everything it can do and rebuild how it looks, feels and performs.

## What is decided

- Fork of `grimmory-tools/grimmory`.
- Working name: **Lectern**. Nothing in the code is renamed yet.
- Our work happens on the `lectern` branch; `develop` mirrors upstream.
- Forked from upstream commit `c80ce813b`, which is ahead of release v3.5.0.

## What is not decided yet

Everything about the redesign itself, including:

- What the new UI should be like, and which screens matter most (reading on a phone in the installed web app is the known priority)
- Theming: what should be changeable
- How far the rebuild goes: restyle the existing Angular app, rebuild screens one at a time, or replace the frontend
- How closely to keep following upstream. The more we rewrite, the harder upstream merges get. This is the biggest long-term trade-off and needs an explicit decision.
- Whether the backend changes at all beyond speed fixes

## How the project is organised

| Path | Purpose |
|---|---|
| `CLAUDE.md` | Standing rules for anyone (human or agent) working in the repo |
| `docs/lectern/README.md` | This charter |
| `docs/lectern/baseline.md` | Known problems in the code we started from |
| `docs/lectern/setup.md` | Building and running for development |
| `docs/lectern/backlog.md` | Wants and ideas, unsorted until designed |
| `docs/lectern/upstream-sync.md` | Branch layout, sync procedure, list of files we've diverged on |
| `docs/lectern/specs/` | Approved designs, one per piece of work |
| `docs/lectern/decisions/` | Short records of decisions that shape later work |
| `docs/lectern/private/` | Local notes. Git-ignored, never published |

Everything else is upstream's layout, unchanged so far.

## Ways of working

- Design first, written up as a spec before building.
- Work in pieces small enough to run and judge on a realistically large library.
- Speed claims come with before-and-after numbers.
- The repository is public: no personal, machine or network details in it (see `CLAUDE.md`).
