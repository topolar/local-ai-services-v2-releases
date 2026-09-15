---
title: GitHub Release publication surface
slug: local-ai-services-releases-publication-surface
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: What is and is not known about the public release destination, local staging, and hosting.
tags: [local-ai-services, releases, github, infrastructure]
aliases: [GitHub Releases, artifact staging]
type: reference
status: active
---

# GitHub Release publication surface

## Known from repository configuration

- Git remote `origin` is `https://github.com/topolar/local-ai-services-v2-releases.git`.
- The README directs end users to that repository's `releases/latest` page.
- Release payloads are GitHub Release assets, not Git-tracked binaries.
- `.artifacts/` is ignored; `ARTIFACTS.md` says locally staged binaries and
  `.sha256` sidecars are only held there before an explicit verified release
  operation.

## Unknown or intentionally out of scope

No tracked configuration identifies a self-hosted service, DNS record,
Cloudflare route, build runner ownership, release retention policy, signing
key, backup policy, monitoring, cost, SLA, or current GitHub Release state.
Do not invent any of these. Verify a particular release through authorized
GitHub API/UI read-back; verify private build/runtime infrastructure in the
related LAS v2 project under separately authorized scope.

## Local staging safety

Treat `.artifacts/` as sensitive candidate material. Do not add it to Git,
upload it to an issue, rename a binary to make a sidecar pass, or assume it is
the same byte sequence as a remote asset. A checksum can only be interpreted
relative to its exact named file in the same directory. The staging location
does not establish release provenance or authorisation.
