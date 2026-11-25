# Changelog

## 2.0.0

- Delivery on GitHub Actions (the reusable `mulesoft-app.yml` of github-reusable-workflows) instead of the Azure DevOps GitFlow pipeline: `main` to TEST, `release/X.Y.Z` to UAT, the tag to UAT and then PROD after approval, and a back-merge into `main`. Every run and its logs are public.
- No `develop`: a change is a pull request into `main`; a defect found in UAT is a pull request into the open release.
- The groupId in Git is `com.brunosouzas.mulesoft`; the workflow publishes under the Anypoint organisation's ID, so a fork needs no change to `pom.xml`.
- The CloudHub 2.0 applications are named `gh-mulesoft-orders-api-<environment>`.

## 1.0.0

- Health check with the environment name (`GET /api/health`).
- Read an order (`GET /api/orders/{orderId}`) and create an order (`POST /api/orders`) with validation.
- Delivery flow: GitFlow, Maven release and CloudHub 2.0 deployments to test, uat and prod.
