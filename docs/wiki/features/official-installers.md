---
title: Official Local AI Edge Client installers
slug: local-ai-services-releases-official-installers
created: 2026-09-13
updated: 2026-09-17
authors: [Codex]
description: Public download and integrity-verification guidance from the release repository README.
tags: [local-ai-services, edge-client, installer, security]
aliases: [download installers, verify installer]
type: reference
status: active
---

# Official Local AI Edge Client installers

## Public download contract

The repository README directs users to the latest GitHub Release for an
installer asset. The checked-in naming guidance is:

```text
las-edge-installer-<version>-linux-x86_64.tar.gz
las-edge-installer-<version>-macos-aarch64.tar.gz
las-edge-installer-<version>-macos-x86_64.tar.gz
las-edge-installer-<version>-windows-x86_64.zip
```

A filename pattern is guidance from the README, not proof that a particular
asset, version, platform, or release currently exists. Obtain the exact remote
release metadata and assets through authorized read-only access before making
such a claim.

## Integrity check

The README requires verifying a downloaded asset against
`installer-manifest.json` or its adjacent `.sha256` sidecar before execution.
For a deliberately downloaded asset and matching sidecar in a separate local
directory, a standard SHA-256 check is:

```bash
sha256sum -c las-edge-installer-<version>-<platform>.sha256
```

A non-zero result, missing file, filename mismatch, or digest mismatch is a
stop condition. Do not execute, rename, upload, or republish the candidate.
A successful local checksum only verifies the selected bytes against the
selected integrity material; it does not prove release authorization or
installer success.

## Scope limit

This repository does not build installers, expose a control-plane API, enroll
nodes, issue sensitive material, or repair installed clients. It contains no
tracked implementation for those responsibilities. See [System
boundary](../architecture/system-boundary.md).
