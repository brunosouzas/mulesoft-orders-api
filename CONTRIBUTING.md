# Contributing

This repository follows GitFlow. The pipeline decides what happens from the branch name, so the branch you choose *is* the instruction.

| Branch | Created from | Merges into | What the pipeline does |
|---|---|---|---|
| `feature/<ticket>-<desc>` | `develop` | `develop` (PR) | Build |
| `develop` | — | — | Build, publish `x.y.z-SNAPSHOT` to Exchange, deploy to **test** |
| `release/x.y.z` | `develop` | `main` (PR) | Build |
| `main` | — | — | Build, **Maven release** (tag `vx.y.z`), deploy to **uat**, deploy to **prod** after approval |
| `hotfix/x.y.z` | `main` | `main` (PR) | Build |

After a release or hotfix reaches `main`, open a PR `main → develop` so `develop` receives the fix and the version bump.

## Versions

Never edit the version in `pom.xml` by hand. `develop` always carries a `-SNAPSHOT`; the release pipeline removes it, tags the commit, publishes the release to Exchange and moves `main` to the next `-SNAPSHOT`.

When you cut `release/x.y.z`, bump `develop` to the next minor `-SNAPSHOT` in the same change (`mvn versions:set -DnewVersion=x.(y+1).0-SNAPSHOT`), so snapshots published from `develop` never collide with the release.

## Tests

Run `mvn clean test` before opening a PR. MUnit needs the MuleSoft Enterprise repository, which the public pipeline does not have access to (see the README).
