---
title: Maintain release repository documentation
slug: local-ai-services-releases-maintenance
created: 2026-09-13
updated: 2026-09-17
authors: [Codex]
description: Evidence-driven maintenance procedure for the binary-only release repository documentation.
tags: [local-ai-services, releases, documentation, development]
aliases: [update release docs]
type: runbook
status: active
---

# Maintain release repository documentation

## Scope

Permitted local scope is documentation and links describing this repository.
Do not introduce a build system, copy private runtime code, commit ignored
artifacts, change registry or mount configuration, or claim runtime or remote
release state without direct evidence.

## Evidence-first update path

1. Read `AGENTS.md`, `README.md`, `SECURITY.md`, `ARTIFACTS.md`,
   `services.json`, and current Git status. Preserve unrelated changes.
2. Ground changes in the tracked source of this repository. For a fact about a
   specific remote release, use separately authorized read-only remote
   evidence; do not represent it as a source-repository fact.
3. When evidence conflicts or is incomplete, state the boundary and impact
   rather than selecting a version by plausibility.
4. Update only the relevant current page and its `updated` metadata. Do not add
   changelogs, session narratives, sensitive material, or deployment claims.

## Available validation

The tracked repository has no project-local formatter, package manifest, test
suite, CI workflow, or release publisher. Useful checks for documentation work
are:

```bash
cd /home/mistercz/projects/local-ai-services-releases
git diff --check
git status --short
git diff -- docs/wiki
```

For changed Wiki pages, reindex only this project's mount and verify the
indexed page metadata and outgoing links. Reindexing proves index ingestion,
not a repository push, GitHub Release publication, runtime deployment, or
installer execution.

## Handoff

Report changed paths, release-repository source commit, cursor state, checks
actually run, mount/link verification, local commit, and unresolved evidence
limits. State separately whether anything was pushed or externally changed.
