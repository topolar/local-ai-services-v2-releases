---
title: Prepare a verified installer publication
slug: local-ai-services-releases-verified-publication
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Approved-operator runbook for assessing the LAS v2 publisher path without silently publishing.
tags: [local-ai-services, releases, runbook, security]
aliases: [publish installers, release verification]
type: runbook
status: active
---

# Prepare a verified installer publication

## Purpose, authority, and boundary

Use this only when an owner has explicitly approved a **specific version and
publication scope**. The actual publisher resides in the related
`local-ai-services` checkout, not here. Running it creates a draft, uploads
assets, and may make a release public: that is DEPLOY/WRITE, not a harmless
test.

Before any mutation, reconcile the public README against the current private
workflow's platform/onboarding contract. The known conflict is documented in
[Official installers](../features/official-installers.md). STOP if the owner
has not resolved it for the requested release.

## READ preflight

1. In this repository, inspect `git status --short`, `git tag --list`, and the
   exact existing remote release/tag with authorized read-only GitHub access.
   Expected: no ambiguous pre-existing target tag and a recorded baseline.
2. In registered `/home/mistercz/projects/local-ai-services`, inspect the
   exact reviewed commit, `.github/workflows/build-edge-installers.yml`, and
   `scripts/publish_edge_installers.py`. Confirm the desired platform set
   matches the owner-approved contract.
3. Confirm the Action run is successful, manually dispatched, from `main`, and
   has the exact reviewed 40-character source SHA. The publisher independently
   checks these conditions; preflight does not replace that check.
4. Downloaded Action artifact verification must pass the strict manifest,
   name, size, sidecar, and digest validation described in [Public release
   contract](../interfaces/public-release-contract.md). Do not use an arbitrary
   local `.artifacts/` file as a substitute.

## Approved DEPLOY operation

The source implementation's command shape is:

```bash
cd /home/mistercz/projects/local-ai-services
python scripts/publish_edge_installers.py \
  --run-id <numeric-successful-run-id> \
  --version <approved-version-without-v> \
  --expected-head-sha <40-character-reviewed-main-SHA>
```

It downloads the workflow artifact to a temporary directory, validates it,
creates an owned draft release, uploads the verified files, compares remote
names/sizes/digests, then publishes. It must be run only with explicit
approval and authenticated GitHub authority. Never put tokens, credentials,
or private bundle locations in its arguments or captured output.

## Verify, retry, and STOP

Expected success is a returned release URL *and* an independent authorized
read-back of the public tag, exact asset names, byte sizes, SHA-256 digests,
and intended draft/public state. Verify a downloaded asset separately before
claiming the end-user path works.

Do not retry after a timeout or ambiguous error by issuing a second publish:
read back the target tag first. The implementation only deletes a draft if it
can prove its own ownership marker; it does not authorize manual cleanup of a
pre-existing or another publisher's release. STOP and escalate for an existing
tag, mismatched remote assets, contract conflict, failed digest, unclear
ownership, or any request to alter a published asset.

## Rollback

No safe generic rollback is established here. The code cleans up only a
confirmed owned **draft** after its own failure. Published-release correction,
asset removal, replacement version, and user communication require a
release-owner decision and separate approved procedure. Do not delete or
overwrite release assets on assumption that they are recoverable.
