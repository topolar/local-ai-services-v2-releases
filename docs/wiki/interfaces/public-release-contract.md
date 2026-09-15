---
title: Public installer release contract
slug: local-ai-services-releases-public-release-contract
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Machine-checkable artifact manifest contract derived from the LAS v2 publisher and builder.
tags: [local-ai-services, releases, manifest, checksum]
aliases: [installer manifest, release asset contract]
type: reference
status: active
---

# Public installer release contract

This reference describes the **current private publisher implementation**
inspected at LAS v2 commit `38af586548166ef2df5ba3538759c831c8870341`, not a
claim that every historical public release conforms to it.

## Expected artifact set

The publisher accepts exactly these current build outputs for one version:

```text
las-edge-installer-<version>-linux-x86_64.tar.gz
las-edge-installer-<version>-linux-aarch64.tar.gz
las-edge-installer-<version>-windows-x86_64.exe
<each asset>.sha256
installer-manifest.json
installer-manifest.json.sha256
checksums.txt
```

`installer-manifest.json` must be a JSON object with exactly
`schema_version` (`1`), `version`, a 40-character lowercase
`source_commit`, and `assets`. Every asset object has exactly `name`,
`sha256`, and positive integer `size_bytes`. The `.sha256` sidecar must be
`<64-hex-digest><two spaces><asset filename>`; the manifest sidecar uses
`dist/installer-manifest.json`. `checksums.txt` must be sorted and include all
asset lines plus the manifest line.

The publisher rejects extra files, missing platforms, symbolic links, bad
sizes, unexpected names, and mismatched byte digests before remote upload.
This makes the manifest an integrity/provenance assertion for the verified
workflow output; it does not prove end-user installation succeeded.

## Publication preconditions and remote read-back

The source publisher requires numeric `--run-id`, a valid semantic version,
and full expected source SHA. It checks the referenced GitHub Actions run was
successful, was `workflow_dispatch`, used
`.github/workflows/build-edge-installers.yml`, built `main`, and has that exact
head SHA. It rejects an existing `v<version>` tag/release before creating a
draft. After upload it compares remote asset names, byte sizes, and GitHub's
reported SHA-256 digest to the locally verified files before attempting to make
the draft public.

These are implementation facts, not permission to invoke the publisher.
Publishing calls GitHub release create/upload/edit and is DEPLOY/WRITE. See
[Verified publication](../operations/verified-publication.md).

## Compatibility warning

The public README's current macOS names do not match this exact current
publisher allowlist. A release manager must reconcile the reviewed source,
requested version, and actual remote asset set; an agent must not add a file
to satisfy either document by guesswork.

## Sources

Related LAS v2 files `scripts/publish_edge_installers.py`,
`scripts/build_installer_manifest.py`, and
`.github/workflows/build-edge-installers.yml`, inspected 2026-09-13.
