# mulesoft-orders-api

[![delivery](https://github.com/brunosouzas/mulesoft-orders-api/actions/workflows/delivery.yml/badge.svg)](https://github.com/brunosouzas/mulesoft-orders-api/actions/workflows/delivery.yml)

A small Mule 4 application used as the reference for a corporate-style delivery flow: `main` to TEST, a release branch to UAT, the tag to PROD with an approval, versioning with the **maven-release-plugin**, and **GitHub Actions** reusable workflows deploying to **CloudHub 2.0**. Every run and its logs are public.

The API itself is deliberately simple. The interesting parts are the branch rules, the release process and the pipeline.

## Endpoints

| Method | Path | Result |
|---|---|---|
| GET | `/api/health` | `{ status, application, environment }` — confirms which environment answered |
| GET | `/api/orders/{orderId}` | An order, or 404 |
| POST | `/api/orders` | 201 with the total, or 400 for invalid input |

## Delivery flow

```
feature/* ──PR──▶ main ──▶ TEST
                   │  Criar release (the chosen pull requests, onto the latest tag)
                   ▼
             release/X.Y.Z ──▶ UAT  ◀──PR── fix/* (a defect found in UAT)
                   │  Release (Maven release, the tag)
                   ▼
              tag vX.Y.Z ──▶ UAT ──approval──▶ PROD ──▶ back-merge into main
```

Branch rules are in [CONTRIBUTING.md](CONTRIBUTING.md). The workflow is a thin [`.github/workflows/delivery.yml`](.github/workflows/delivery.yml) that calls the reusable `mulesoft-app.yml` of [github-reusable-workflows](https://github.com/brunosouzas/github-reusable-workflows), pinned by commit SHA; its `mulesoft/README.md` explains each step, the security of a public repository and the setup.

Version 1.x was delivered by the Azure DevOps GitFlow templates of [azure-devops-mulesoft-pipelines](https://github.com/brunosouzas/azure-devops-mulesoft-pipelines), as the first article series describes; the tag `v1.0.0` keeps that pipeline.

## Environments

| Environment | Application |
|---|---|
| TEST | `gh-mulesoft-orders-api-test` |
| UAT | `gh-mulesoft-orders-api-uat` |
| PROD | `gh-mulesoft-orders-api-prod` |

Each deploy's run prints the application's public URL and its `/api/health` answer. They run on an Anypoint Platform trial and stop when the trial ends.

## Configuration

| File | Purpose |
|---|---|
| `src/main/resources/config/config-<env>.yaml` | Application properties per environment (selected by `mule.env`) |
| `deployment/<env>.yaml` | CloudHub 2.0 sizing per environment (application name, target, runtime, replicas, vCores) and the `healthPath` checked after each deploy |

Secrets never live in the repository: each GitHub environment (test, uat, prod, release) holds the Anypoint Connected App's secret, and the workflow writes a `settings.xml` that reads it from the environment at runtime. A pull request, from a fork or not, gets no secret.

## Tests

```bash
mvn clean test
```

MUnit needs the MuleSoft Enterprise repository (customer credentials in `~/.m2/settings.xml`). The public workflow does not have them, and never will (a customer credential is not for a public repository), so it builds with `-DskipMunitTests` and MUnit runs locally before each pull request; see "Why no MUnit" in the reusable workflows' `mulesoft/README.md`.

## Using it with your own Anypoint organization

Fork it, adjust `deployment/*.yaml` to your environments and target, and do the setup in the reusable workflows' `mulesoft/README.md` (a release GitHub App, the environments and their secrets, the rulesets). `pom.xml` keeps its canonical groupId: the workflow publishes under your organisation's ID.

## Licence

[MIT](LICENSE) © Bruno Pinto de Souza
