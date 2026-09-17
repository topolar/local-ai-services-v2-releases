---
title: Public release channel boundary
slug: local-ai-services-releases-system-boundary
created: 2026-09-13
updated: 2026-09-17
authors: [Codex]
description: Responsibility and data-boundary model for the binary-only release repository.
tags: [local-ai-services, architecture, security, releases]
aliases: [LAS release boundary, binary-only channel]
type: reference
status: active
---

# Public release channel boundary

```text
public release repository
  README, SECURITY.md, Git tags, GitHub Release asset destination
      -> end-user obtains a selected released asset and verifies its bytes

private runtime and control-plane implementation
  remains outside this repository
```

## Responsibilities

| Boundary | What this repository establishes |
|---|---|
| Public repository | User-facing README and security guidance, Git tags, and the destination for GitHub Release assets |
| GitHub Release | The README identifies Releases as the official installer download location; remote release state requires a separate read-back |
| Local staging | `.artifacts/` is ignored candidate storage, not Git-tracked release evidence |
| Private implementation | Runtime, control plane, build process, and release publication implementation are intentionally absent |

The project registry identifies this repository as a companion release
repository for `local-ai-services`. That relationship does not authorize work
in the related project or establish a running-service state.

## Data boundary

Public documentation may describe installer names and integrity verification.
Sensitive enrollment, service-access, node/worker, runtime, configuration, and
log material must not be committed, attached to public discussions, or copied
into this Wiki. `SECURITY.md` is the repository policy for that boundary.

An agent can update this repository's documentation and inspect its tracked
metadata. Requests to build, publish, modify releases, or alter the private
runtime require explicitly approved scope and must be handled outside this
repository.
