---
title: Local AI Services Releases — features
slug: local-ai-services-releases-features
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Catalog of the narrow public distribution capabilities owned by the release repository.
tags: [local-ai-services, edge-client, releases]
aliases: [release repository features]
type: overview
status: active
---

# Features

This repository does not provide the AI runtime, a web API, a package manager,
or an installer build. Its user-facing capability is a trustworthy **public
release surface** for installer assets produced elsewhere.

| Capability | Result | Current evidence | Detail |
|---|---|---|---|
| Official installer discovery | A user can choose a released platform asset | `README.md` points to GitHub Releases | [Official installers](official-installers.md) |
| Integrity material | A user can compare an asset to a SHA-256 manifest/sidecar | `README.md`, `SECURITY.md`; exact shape comes from LAS v2 source | [Public release contract](../interfaces/public-release-contract.md) |
| Local staging boundary | Operators can hold ignored candidate files without committing them | `.gitignore`, `ARTIFACTS.md` | [Publication surface](../infrastructure/publication-surface.md) |

Read [System boundary](../architecture/system-boundary.md) before treating an
installer description as a statement about private runtime behavior.
