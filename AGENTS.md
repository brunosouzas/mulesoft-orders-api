# mulesoft-orders-api: project context

## Purpose

A small Mule 4 application used as the reference for a corporate-style delivery flow: **GitFlow**, versioning with the **maven-release-plugin**, and **Azure Pipelines** templates deploying to **CloudHub 2.0** with approvals.

## Technology

Stack: Mule 4, MUnit, Maven, Azure Pipelines, CloudHub 2.0
Delivery: C environments
Integration branch: `develop`
Data: none
Article: `50-writing/mulesoft-delivery-azure-devops/`

- Build: `mvn -B clean package -DskipMunitTests`
- Test: `mvn clean test` (MUnit; needs the MuleSoft Enterprise repository credentials in ~/.m2/settings.xml, so it runs locally, not in the public pipeline)
- Run locally: `mvn clean package`, then deploy the jar to a local Mule 4.9 runtime and call GET /api/health
- Deliver and verify: `a merge into develop runs the Azure Pipelines GitFlow template (azure-devops-mulesoft-pipelines, pinned tag): build, publish x.y.z-SNAPSHOT to Exchange, deploy to test; a release/x.y.z pull request into main runs the Maven release, deploys to uat and, after Bruno approves the environment, to prod; verify GET /api/health in each environment reached answers with that environment's name (URLs in README.md) (today the Anypoint trial has ended and the CloudHub environments return 404, and the pipeline fails on AB#72, so a merge delivers nothing; verify that the links and paths the article cites still work)`

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
- [azure-pipelines.yml](azure-pipelines.yml).

GitHub default branch inspected on 2026-10-06: `main`. This context is prepared against develop, the integration line selected from the project pipeline/release sources. The GitHub default is separately main. Build/publish commands mentioned by those sources are context, not authorization.

## Project rules

Before planning, reviewing or changing this project, read [the applicable project rules](rules/README.md).

- Article repository: this repository was made for a published article and keeps what the article published (its code, branches and pipeline). A change here changes the article, so every merge is Bruno's (level 2), also under a delegation.
- Board area: `mulesoft` (Azure Boards, project `brunosouzas`).
- OpenSpec: no: every change takes the small path.
- Branches follow GitFlow as published (see CONTRIBUTING.md): changes go to `develop`; `main` only through a `release/` or `hotfix/` pull request, because a merge into `main` runs the Maven release.
