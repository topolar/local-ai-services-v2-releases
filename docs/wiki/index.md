---
title: Local AI Services Releases
slug: local-ai-services-releases
created: 2026-09-13
updated: 2026-09-17
authors: [Codex]
description: Entry point for the public, binary-only Local AI Edge Client release repository.
tags: [local-ai-services, edge-client, releases, distribution]
aliases: [LAS releases, Local AI Edge Client releases]
type: overview
status: active
---

# Local AI Services Releases

This repository is the public distribution channel for Local AI Edge Client
installer assets. It deliberately contains documentation and Git tags rather
than the private Local AI Services runtime or control plane. Official installer
binaries are GitHub Release assets; a repository checkout or local
`.artifacts/` directory is not an installed client or evidence of a release.

## Repository contract

- **Project:** `local-ai-services-releases`; canonical repository:
  `topolar/local-ai-services-v2-releases`.
- **Public material:** the README describes the latest-release download link,
  supported asset names, and verification against `installer-manifest.json` or
  an adjacent `.sha256` sidecar.
- **Local staging:** `.artifacts/` is Git-ignored candidate storage. It is not
  a published-release record, provenance proof, or substitute for a remote
  release read-back.
- **Security boundary:** do not commit or disclose sensitive enrollment,
  service-access, node/worker, runtime, configuration, or log material. See
  `SECURITY.md`.
- **Authority boundary:** building installers, operating the private runtime,
  and creating or changing GitHub Releases are outside this repository's
  tracked source and require separately approved scope.

## Choose the smallest relevant document

| Task | Read next |
|---|---|
| Find the repository's public download and integrity guidance | [Official installers](features/official-installers.md) |
| Understand this repository's boundary | [System boundary](architecture/system-boundary.md) |
| Check the public asset and checksum contract | [Public release contract](interfaces/public-release-contract.md) |
| Assess a release without publishing it | [Verified publication](operations/verified-publication.md) |
| Handle a bad download or suspected disclosure | [Failure handling](operations/failure-handling.md) |
| Maintain the repository documentation | [Development notes](development/maintenance.md) |

## Documentation scope

The current tracked source consists of repository instructions, public README
and security guidance, ignored-artifact guidance, the service declaration, and
this documentation. It contains no runtime source, build workflow, local
publisher, deploy configuration, or project-local test suite. Absence of those
components here is a repository boundary, not evidence about another project.

These pages state only contracts grounded in the tracked files of this
repository. They do not establish the state of a GitHub Release, downloaded
asset, installer execution, private runtime, or deployment.
