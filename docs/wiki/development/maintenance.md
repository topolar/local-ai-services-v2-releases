---
title: Maintain release repository documentation
slug: local-ai-services-releases-maintenance
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Evidence-driven maintenance procedure for docs and release guidance in the binary-only repository.
tags: [local-ai-services, releases, documentation, development]
aliases: [update release docs]
type: runbook
status: active
---

# Maintain release repository documentation

## Scope

Permitted local scope is documentation and links describing this repository.
Do not introduce a build system, copy private runtime code, commit ignored
artifacts, change registry/mount configuration, or claim runtime/release state
without its source evidence.

## Evidence-first update path (READ then WRITE)

1. Read `AGENTS.md`, `README.md`, `SECURITY.md`, `ARTIFACTS.md`, and current
   Git status. Preserve unrelated changes.
2. For public-release behavior, inspect a particular GitHub Release with
   authorized read-only access. For build/publisher behavior, inspect the
   registered LAS v2 source at a recorded commit. State which one supports
   each changed assertion.
3. When sources conflict, document the discrepancy and its impact rather than
   selecting a version by plausibility. The current README/platform/onboarding
   conflict is an example.
4. Update only the relevant current page and its `updated` metadata. Do not
   add changelogs or task diaries to the Wiki documentation.

## Available validation

The release repository has no tracked formatter, package manifest, test suite,
or CI workflow to run locally (verified 2026-09-13). Minimum useful checks are:

```bash
cd /home/mistercz/projects/local-ai-services-releases
git diff --check
git status --short
git diff -- docs/wiki
```

For a changed release-contract claim, validate the referenced private source
test or workflow only in the related project and only within the requested
scope; it does not validate a remote release. For a changed download claim,
use an authorized remote release read-back and a separate checksum check of
the exact asset. Neither test was run as part of initial documentation creation.

## Handoff

Report changed paths, release-repository commit and dirty state, cited LAS v2
commit if used, checks actually run, and unresolved disagreements. State
explicitly whether content has been mounted/reindexed in Wiki (it has not as
of 2026-09-13), committed/pushed, or externally published.
