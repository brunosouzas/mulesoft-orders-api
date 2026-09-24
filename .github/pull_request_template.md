## What changes

<!-- One or two sentences. Link the work item. -->

## Type

- [ ] feature → develop
- [ ] release → main
- [ ] hotfix → main
- [ ] back-merge main → develop

## Checks

- [ ] MUnit passes locally (`mvn clean test`) — CI does not run MUnit, see the README
- [ ] Version untouched (the pipeline manages versions)
- [ ] `deployment/*.yaml` reviewed if runtime, replicas or vCores changed
