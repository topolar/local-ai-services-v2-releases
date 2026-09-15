---
title: Handle installer release verification failures
slug: local-ai-services-releases-failure-handling
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Stop conditions and evidence-preserving response for bad artifacts, mismatched releases, and secret exposure.
tags: [local-ai-services, releases, incident, security]
aliases: [bad installer, checksum mismatch, release incident]
type: runbook
status: active
---

# Handle installer release verification failures

## Immediate safe response

- **Checksum, size, manifest, or filename mismatch:** do not run the asset,
  upload it, rename it, regenerate a sidecar, or retry publication. Record the
  exact tag/URL, asset name, expected and observed digest/size, command exit
  code, and time without copying secret-bearing output.
- **Unexpected platform or README/workflow disagreement:** stop publication
  and get a release-owner decision. Do not fabricate a missing platform asset
  or edit public instructions to match a guess.
- **Possible secret exposure:** do not open a public issue or attach logs.
  Follow `SECURITY.md`: contact the repository owner privately. Treat
  enrollment, Cloudflare Access, node/worker tokens, private runtime archives,
  and credential-bearing configuration/logs as sensitive.

## Triage (READ)

1. Preserve the artifact filename and source/tag identity. If available,
   inspect the associated manifest and sidecar alongside the exact bytes.
2. Read back the remote release under authorized access and compare its asset
   names, sizes, and digests to the local verified manifest. A tag alone is
   insufficient evidence.
3. Determine whether the failure is before publication, in an owned draft, or
   in an already published release. The response authority differs.
4. For workflow/publisher behavior, inspect the related LAS v2 source and its
   tests; this release repository cannot repair the build or runtime.

## Escalation and recovery

Only the source publisher's narrowly checked owned-draft cleanup is documented
in code. It is not a general deletion authority. Do not manually delete a
release, overwrite an asset, revoke a credential, or publish a replacement
without an owner-approved incident/release plan. Keep public claims limited to
facts independently read back from the release service.
