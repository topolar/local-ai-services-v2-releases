---
title: Public installer release contract
slug: local-ai-services-releases-public-release-contract
created: 2026-09-13
updated: 2026-09-17
authors: [Codex]
description: Public asset and integrity-material contract grounded in this repository's README and security policy.
tags: [local-ai-services, releases, manifest, checksum]
aliases: [installer manifest, release asset contract]
type: reference
status: active
---

# Public installer release contract

The public interface owned by this repository is a GitHub Release asset set,
not an HTTP API, daemon socket, package manifest, or project-local CLI.

## Documented integrity material

The README requires an installer to be checked against either
`installer-manifest.json` or an adjacent `.sha256` file before execution.
`SECURITY.md` further states that installer assets are immutable per release
and should be verified with SHA-256 material.

For a selected release, retain the exact release identity, asset filename, and
corresponding integrity material while checking the bytes. A digest match does
not by itself prove that the release is authorized, current, compatible, or
successfully installed. Those claims require the relevant remote read-back or
local execution evidence.

## What this contract does not establish

Tracked source in this repository does not define a machine-readable manifest
schema, an exhaustive remote asset allowlist, a publisher command, or remote
release state. Do not infer those details from a filename pattern or add files
to make an assumed contract pass. Treat missing, ambiguous, or conflicting
release information as a stop condition pending an authorized owner decision.

See [Official installers](../features/official-installers.md) for README-level
asset naming and checksum guidance.
