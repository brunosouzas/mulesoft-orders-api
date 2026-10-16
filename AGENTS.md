# mulesoft-orders-api: project context

## Purpose

A small Mule 4 application used as the reference for a corporate-style delivery flow: **GitFlow**, versioning with the **maven-release-plugin**, and **Azure Pipelines** templates deploying to **CloudHub 2.0** with approvals.

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
