---
title: Public release channel and LAS v2 boundary
slug: local-ai-services-releases-system-boundary
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Responsibility and trust-boundary model for the binary-only release repository.
tags: [local-ai-services, architecture, security, releases]
aliases: [LAS v2 release boundary, binary-only channel]
type: reference
status: active
---

# Public release channel and LAS v2 boundary

```text
private LAS v2 source (topolar/local-ai-services-v2)
  workflow_dispatch build -> GitHub Actions artifact (temporary)
  verified publisher    -> GitHub Release assets in this public repository
                               -> end-user download and local hash check
  private runtime/control plane remains outside this repository
```

## Responsibilities

| Component | Owner/source of truth | What is known from inspected files |
|---|---|---|
| Public release repository | `topolar/local-ai-services-v2-releases` | Git tags, user-facing README/security policy, GitHub Release attachment destination |
| Build matrix and artifact assembly | related private `local-ai-services` source | `.github/workflows/build-edge-installers.yml`, manually dispatched and tied to a reviewed source SHA |
| Manifest construction | related private source | `scripts/build_installer_manifest.py` writes version, source commit, names, SHA-256, and byte sizes |
| Publication transaction | related private source | `scripts/publish_edge_installers.py` validates an Action run then creates/verifies a draft before publication |
| Runtime, enrollment, updates, API, secrets | private LAS v2 runtime/control plane | deliberately absent from this repository |

The project registry calls this repository a **companion release repository**
of `local-ai-services`; that relation was verified with `projectctl project
show` on 2026-09-13. It does not grant access to the private source or prove a
running service.

## Trust and data boundaries

- Public: installer assets, their SHA-256 sidecars, an immutable manifest, and
  release metadata. These may be downloaded but still require integrity and
  version verification.
- Private: runtime bundles, source, enrollment values, Cloudflare service
  credentials, node/worker tokens, and logs/configuration containing them.
  `SECURITY.md` prohibits putting them in public discussions or attachments.
- Local staging: `.artifacts/` is ignored by Git. It is a transport/staging
  convenience only; it is neither a signed provenance record nor a substitute
  for a GitHub Release read-back.

## Consequences for agents

An agent working here can improve public documentation and inspect release
metadata, but must switch to the registered `local-ai-services` project before
touching the builder or publisher. Do not copy private source/bundles to make
this repository appear self-contained. If the requested task crosses this
boundary, stop and obtain explicit scope for the other repository and any
external release action.
