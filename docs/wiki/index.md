---
title: Local AI Services Releases
slug: local-ai-services-releases
created: 2026-09-13
updated: 2026-09-13
authors: [Codex]
description: Agent entrypoint for the public, binary-only Local AI Edge Client release repository.
tags: [local-ai-services, edge-client, releases, distribution]
aliases: [LAS releases, Local AI Edge Client releases]
type: overview
status: active
---

# Local AI Services Releases

This repository is the public distribution channel for **Local AI Edge Client**
installer assets. It deliberately contains documentation and Git tags, not the
private Local AI Services v2 runtime or its control plane. Official installers
are attached to GitHub Releases; a repository checkout or `.artifacts/` is not
itself an installed client or evidence that a release is healthy.

## Agent entrypoint

- **Project:** `local-ai-services-releases`; canonical cwd:
  `/home/mistercz/projects/local-ai-services-releases`; repository:
  `topolar/local-ai-services-v2-releases`.
- **First READ checks:** run `git status --short`, `git tag --list`, then read
  `README.md`, `SECURITY.md`, and `ARTIFACTS.md`. Expected result: this is a
  small binary-release repository; `.artifacts/` is ignored and must not be
  treated as publishable evidence.
- **Source of truth by question:** public asset names and user-facing download
  guidance are this repository's README and a released tag; release build and
  publishing implementation are in the related private `local-ai-services`
  project, notably
  `.github/workflows/build-edge-installers.yml` and
  `scripts/publish_edge_installers.py` (inspected at source commit
  `38af586548166ef2df5ba3538759c831c8870341` on 2026-09-13).
- **READ / WRITE boundary:** inspecting tags, files, and checksums is READ.
  Creating, editing, uploading, publishing, deleting, or making a GitHub
  Release public is a consequential external WRITE/DEPLOY action and requires
  explicit approval. This repository has no local publisher command.
- **STOP:** do not place enrollment material, Cloudflare credentials, node or
  worker tokens, private runtime bundles, or credential-bearing logs in Git,
  wiki, shell argv, or chat. Do not infer service health from registry status
  (`unconfirmed` as of 2026-09-13), a Git tag, or an HTTP response.
- **Done means:** the exact asset/version/tag, source commit and manifest are
  reconciled, the requested checksum verification succeeds, and any intended
  remote mutation has an approved, independently verifiable read-back. A
  commit, local artifact, or draft alone is not completion.

## Choose the smallest relevant document

| Task | Read next |
|---|---|
| Find what users can download and what is intentionally absent | [Official installers](features/official-installers.md) |
| Understand repository versus LAS v2 responsibility | [System boundary](architecture/system-boundary.md) |
| Check public asset/manifest contract | [Public release contract](interfaces/public-release-contract.md) |
| Prepare a release without performing it | [Verified publication](operations/verified-publication.md) |
| Investigate a bad download, wrong asset, or suspected secret exposure | [Failure handling](operations/failure-handling.md) |
| Change documentation or assess checks | [Development notes](development/maintenance.md) |

## Current scope and evidence

The tracked tree at release-repository commit
`918ee182ec12e285531769c626c09c8f40ecec20` contains only the public README,
security policy, artifact-staging note, and instructions. It has no build
workflow, package manifest, runtime source, deployment configuration, test
suite, or release publisher. The private LAS v2 source owns those components;
this is a boundary, not a missing checkout.

The release repository is not currently a mounted Wiki source (verified with
`wiki mount list` on 2026-09-13). These files are canonical project
documentation ready for mounting, but have not been registered, reindexed, or
published to the Wiki by this documentation task.

## Important documentation conflict to resolve before a public change

The checked-in `README.md` lists macOS assets and says the installer asks for a
one-time enrollment token plus Cloudflare Access credentials. The current
private build workflow inspected above builds Linux `x86_64`, Linux `aarch64`,
and Windows `x86_64`; its current edge-client documentation says normal public
installation has no credential prompt and labels enrollment credentials as a
legacy technical-preview path. No release/tag/remote asset review was done
here to decide which contract is currently public. Treat this as a release
blocker, not a documentation typo to silently choose between.

## Documentation map

- [Features](features/index.md) — public user outcome and download/verification.
- [Architecture](architecture/index.md) — ownership, trust boundaries, and the
  private LAS v2 relationship.
- [Interfaces](interfaces/index.md) — expected file names, manifest and
  checksum contract; not a control-plane API reference.
- [Infrastructure](infrastructure/index.md) — GitHub Release and ignored local
  staging surface, with explicit unknowns.
- [Operations](operations/index.md) — approved release preparation, verification
  and incident stop conditions.
- [Development](development/index.md) — safe documentation maintenance and
  available validation.

## Sources and limits

Facts in this documentation are derived from the repository files above and,
where explicitly marked, the related LAS v2 checkout at the cited commit. No
authenticated GitHub Release inspection, runtime health check, installer
execution, build, deployment, publishing, restart, or secret access occurred.
