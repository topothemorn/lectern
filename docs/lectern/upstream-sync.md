# Staying in step with Grimmory

## Remotes

| Remote | Repository | Use |
|---|---|---|
| `origin` | the Lectern fork | Push here. |
| `upstream` | `github.com/grimmory-tools/grimmory` | Fetch only. Push URL is set to `DISABLED`. |

## Branches

| Branch | Rule |
|---|---|
| `develop` | Exact mirror of upstream `develop`. Never commit here. |
| `lectern` | Our main branch and the fork's default branch on GitHub. |
| anything else from upstream | Came along with the fork. Ignore. |

Forked from upstream `develop` at `c80ce813b`.

## Pulling upstream changes

```bash
git fetch upstream --tags
git checkout develop && git merge --ff-only upstream/develop && git push origin develop
git checkout lectern && git merge develop
```

Merge, don't rebase, so `lectern` history stays stable. Run the checks (`just check`) after every merge.

How often to sync, and whether to keep syncing at all once the rewrite goes deep, is an open decision (see the charter). Until it is made, sync before starting each new piece of work.

## GitHub Actions

Actions are switched off on the fork. Upstream's workflows include release and image-publishing jobs that are not set up for this repository. Turn Actions back on only after deciding which workflows Lectern keeps.

## Files where we have diverged from upstream

Keep this list current. It is the list of likely merge conflicts.

| Path | What we changed | Since |
|---|---|---|
| (none yet) | | |

New files of our own (`CLAUDE.md`, `docs/lectern/**`) don't exist upstream and won't conflict.
