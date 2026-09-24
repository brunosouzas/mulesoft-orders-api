# mulesoft-orders-api

A small Mule 4 application used as the reference for a corporate-style delivery flow: **GitFlow**, versioning with the **maven-release-plugin**, and **Azure Pipelines** templates deploying to **CloudHub 2.0** with approvals.

The API itself is deliberately simple. The interesting parts are the branch rules, the release process and the pipeline.

## Endpoints

| Method | Path | Result |
|---|---|---|
| GET | `/api/health` | `{ status, application, environment }` — confirms which environment answered |
| GET | `/api/orders/{orderId}` | An order, or 404 |
| POST | `/api/orders` | 201 with the total, or 400 for invalid input |

## Delivery flow

```
feature/* ──PR──▶ develop ──▶ SNAPSHOT to Exchange ──▶ test
                     │
                release/x.y.z ──PR──▶ main ──▶ Maven release (tag vx.y.z) ──▶ uat ──▶ prod (approval)
                                        ▲
hotfix/x.y.z ──────────────PR───────────┘
```

Branch rules are in [CONTRIBUTING.md](CONTRIBUTING.md). The pipeline is a thin [`azure-pipelines.yml`](azure-pipelines.yml) that extends the shared templates in [azure-devops-mulesoft-pipelines](https://github.com/brunosouzas/azure-devops-mulesoft-pipelines), pinned to a tag. The reasoning behind each choice is recorded as ADRs in that repository.

## Configuration

| File | Purpose |
|---|---|
| `src/main/resources/config/config-<env>.yaml` | Application properties per environment (selected by `mule.env`) |
| `deployment/<env>.yaml` | CloudHub 2.0 sizing per environment: application name, target, runtime, replicas, vCores |

Secrets never live in the repository: the pipeline receives the Anypoint Connected App credentials from an Azure DevOps variable group and writes `settings.xml` at runtime.

## Tests

```bash
mvn clean test
```

MUnit needs the MuleSoft Enterprise repository (customer credentials in `~/.m2/settings.xml`). The public pipeline does not have them, so it builds with `-DskipMunitTests` and MUnit runs locally before each pull request — see [ADR 4](https://github.com/brunosouzas/azure-devops-mulesoft-pipelines/blob/main/docs/adr/0004-munit-not-in-public-ci.md).

## Using it with your own Anypoint organization

Change `groupId` in `pom.xml` to your organization ID, adjust `deployment/*.yaml` to your environments and target, and create the Azure DevOps items listed in the templates README.

## Licence

[MIT](LICENSE) © Bruno Pinto de Souza
