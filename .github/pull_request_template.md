## What changes

<!-- One or two sentences. Link the work item. -->

## Type

- [ ] feature → main
- [ ] fix → release/X.Y.Z (a defect found in UAT)
- [ ] back-merge → main (opened by the Release run)

## Checks

- [ ] MUnit passes locally (`mvn clean test`) — CI does not run MUnit, see the README
- [ ] Version untouched (Criar release and the Maven release manage versions)
- [ ] Squash message written by hand, with no skip-CI marker
- [ ] `deployment/*.yaml` reviewed if runtime, replicas or vCores changed
