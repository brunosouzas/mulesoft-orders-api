# mulesoft-orders-api: project context

## Purpose

A small Mule 4 application used as the reference for a corporate-style delivery flow: `main` to TEST, a release branch to UAT, the tag to PROD with an approval, versioning with the **maven-release-plugin**, and **GitHub Actions** reusable workflows deploying to **CloudHub 2.0**.

## Technology

Stack: Mule 4, MUnit, Maven, GitHub Actions, CloudHub 2.0
Delivery: C environments
Integration branch: `main`
Data: none
Article: `50-writing/mulesoft-delivery-azure-devops/`

- Build: `mvn -B clean package -DskipMunitTests`
- Test: `mvn clean test` (MUnit; needs the MuleSoft Enterprise repository credentials in ~/.m2/settings.xml, so it runs locally, not in the public pipeline)
- Run locally: `mvn clean package`, then deploy the jar to a local Mule 4.9 runtime and call GET /api/health
- Deliver and verify: `.github/workflows/delivery.yml` (the reusable `mulesoft-app.yml` of github-reusable-workflows, pinned by SHA): a merge into `main` deploys TEST; Criar release opens `release/X.Y.Z`, which deploys UAT; Release makes the tag, whose run deploys UAT and then PROD after Bruno's approval; each deploy job checks the application RUNNING the version and `GET /api/health` 200; then the back-merge pull request into `main`

## Technology declarations

- `app.runtime = 4.9.17` — [pom.xml](pom.xml).
- `mule.maven.plugin.version = 4.9.1` — [pom.xml](pom.xml).
- `munit.version = 3.7.1` — [pom.xml](pom.xml).
- `mule.http.connector.version = 1.11.3` — [pom.xml](pom.xml).
- `Declared minimum Mule runtime 4.9.0` — [mule-artifact.json](mule-artifact.json).

These are source declarations, not evidence of installed runtimes. Maven properties may describe build/test dependencies rather than supported runtime minima; unresolved expressions remain inherited until verified.

## Layout and operation sources

Top-level source/documentation directories: `deployment`, `src`.

- [README.md](README.md).
- [mule-artifact.json](mule-artifact.json).
- [pom.xml](pom.xml).
- [.github/workflows/delivery.yml](.github/workflows/delivery.yml).

GitHub default branch inspected on 2026-10-06: `main`. Since 2.0.0 the integration line is `main`; `develop` belongs to the 1.x GitFlow delivery. Build/publish commands mentioned by those sources are context, not authorization.

## Project rules

Before planning, reviewing or changing this project, read [the applicable project rules](rules/README.md).

- Article repository: this repository was made for a published article and keeps what the article published (its code, branches and pipeline). A change here changes the article, so every merge is Bruno's (level 2), also under a delegation.
- Tracker: Linear brunosouzas, team SW, label mulesoft.
- OpenSpec: no: every change takes the small path.
- Branches since 2.0.0 (see CONTRIBUTING.md): a change is a pull request from `feature/sw-<n>-<slug>` into `main`; a UAT fix is a pull request into the open `release/X.Y.Z`; squash merges with a message written by hand and no skip-CI marker. Criar release, Release and PROD are Bruno's runs. The tag `v1.0.0` keeps the Azure DevOps GitFlow pipeline the first article series describes.
