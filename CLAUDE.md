# Lectern

Lectern is a fork of [Grimmory](https://github.com/grimmory-tools/grimmory) (a self-hosted ebook, comic and audiobook library, itself descended from BookLore). The goal is to keep all of Grimmory's functionality while rebuilding the UI, theming and speed. This is a long-running project.

Read these before doing anything substantial:

- `docs/lectern/README.md` — goals and how the project is organised
- `docs/lectern/baseline.md` — known problems in the code we started from
- `docs/lectern/setup.md` — how to build and run it for development. **Read the port 6060 warning before starting any dev stack.**
- `docs/lectern/upstream-sync.md` — branch layout and how to pull Grimmory changes in
- `docs/lectern/backlog.md` — known wants, not yet designed
- `DEVELOPMENT.md`, `frontend/DEVELOPMENT.md`, `backend/DEVELOPMENT.md` — upstream's own dev guides, still accurate
- `docs/lectern/private/` — if it exists in your checkout, read every file in it. It holds the maintainer's local notes and is git-ignored.

## Rules for working here

- **This repository is public. Nothing personal goes in it.** No names of people, no usernames or email addresses, no machine paths, hostnames, IP addresses, ports of real services, network or storage details, library contents or sizes, or facts about who uses a real install. This covers file contents, commit messages, branch names, PR and issue text. Write "the maintainer", not a name. Anything of that kind belongs in `docs/lectern/private/`, which is git-ignored. Before every commit, read the staged diff for such details; before every push, remember that a pushed commit cannot be reliably taken back.
- **Branches.** `lectern` is our main branch. `develop` mirrors upstream and never gets our commits. Feature work goes on short branches off `lectern`.
- **Never push to upstream** (`grimmory-tools/grimmory`). The `upstream` remote has its push URL disabled on purpose. No PRs or issues to upstream unless the maintainer asks; their `AI_POLICY.md` requires disclosure and human understanding of every change.
- **Licence.** The code is AGPL-3.0. Keep `LICENSE` and `NOTICE` intact, keep the fork's source public, and don't remove upstream copyright notices.
- **Never point a dev build at a real library.** Not its database, not its data folder. Development uses its own port and its own database; real book files are mounted read-only if at all.
- **Our files live in new paths** (`docs/lectern/`, `CLAUDE.md`) so upstream merges don't conflict with them. When changing upstream files, prefer changes that are easy to carry across merges, and record anything that will conflict in `docs/lectern/upstream-sync.md`.
- **Measure before and after.** Speed work needs a number from a realistically large library before and after. Record the method in the spec; keep numbers from any real install in the private folder.
- **Design decisions get written down.** Specs go in `docs/lectern/specs/YYYY-MM-DD-<topic>-design.md`; shorter one-off decisions go in `docs/lectern/decisions/`. Both are public: describe the software, not the person or their setup.

## Stack (inherited from Grimmory)

- `frontend/` — Angular 22, TypeScript, pnpm, TanStack Query, Transloco i18n, Vitest
- `backend/` — Spring Boot, Java 25, Gradle, MariaDB, Flyway migrations
- Root `Justfile` is the command surface (`just check`, `just test`, `just dev-up`, `just image-build`)

The Docker dev stack needs no local Node, pnpm or Java. See `docs/lectern/setup.md`.
