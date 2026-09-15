---
title: Official Local AI Edge Client installers
slug: local-ai-services-releases-official-installers
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Public download and integrity-verification contract, including unresolved release documentation differences.
tags: [local-ai-services, edge-client, installer, security]
aliases: [download installers, verify installer]
type: reference
status: active
---

# Official Local AI Edge Client installers

## Purpose and supported scenario

The intended public flow is: obtain an installer **from a GitHub Release**,
obtain its adjacent SHA-256 material, verify the bytes, and only then execute
it. The public release repository is not a source-code distribution and an
asset copied from a local `.artifacts/` directory is not official merely
because its filename looks correct.

`README.md` currently names assets in the form
`las-edge-installer-<version>-<platform>`. It links to the latest GitHub
Release and requires verification against `installer-manifest.json` or an
adjacent `.sha256` file.

## Integrity check (READ)

Run only against a deliberately downloaded release asset in a separate
directory. The sidecar records the expected filename, so run the check from
the asset's directory:

```bash
cd /path/to/downloaded/release
sha256sum -c las-edge-installer-<version>-<platform>.sha256
```

Expected result is `<asset>: OK` and exit code `0`. A missing file, filename
mismatch, or non-zero exit is a STOP condition: do not execute, rename to fit
the sidecar, or republish anything. Keep the original download available only
if incident handling authorizes it; do not attach it to a public issue.

On Windows, calculate SHA-256 with `Get-FileHash` and compare its lowercase
hex value to the first whitespace-delimited field of the downloaded sidecar.
This is a local read of the bytes, not proof that the release is authorized.

## Current platform and onboarding ambiguity

There are two conflicting source claims:

- this repository's `README.md` lists Linux `x86_64`, macOS `aarch64`, macOS
  `x86_64`, and Windows `x86_64`, and describes credential prompts;
- the current private LAS v2 workflow at commit `38af586…0341` packages Linux
  `x86_64`, Linux `aarch64`, and Windows `x86_64`; its
  `docs/edge-client.md` says the normal public flow has no enrollment or
  Cloudflare credential prompt, while legacy technical-preview enrollment is
  separate.

Neither statement proves the assets at a particular published tag. Before
changing download instructions, creating a release, or supporting an end user,
inspect that exact released manifest/assets with authorized GitHub access and
get an owner decision on the intended public contract.

## What this feature does not do

It does not enroll a node, issue credentials, expose a control-plane endpoint,
build binaries, upload an asset, decide compatibility, or repair an installed
client. Those responsibilities remain in LAS v2 or with an approved operator;
see [System boundary](../architecture/system-boundary.md).

## Sources

`README.md` and `SECURITY.md` in this repository; related source
`.github/workflows/build-edge-installers.yml`,
`scripts/build_installer_manifest.py`, and `docs/edge-client.md`, inspected
2026-09-13. No installer was run for this documentation.
