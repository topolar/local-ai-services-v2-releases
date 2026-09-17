---
title: Assess an installer release without publication
slug: local-ai-services-releases-verified-publication
created: 2026-09-13
updated: 2026-09-17
authors: [Codex]
description: Read-only assessment boundary for release metadata and installer integrity material.
tags: [local-ai-services, releases, runbook, security]
aliases: [release verification]
type: runbook
status: active
---

# Assess an installer release without publication

## Authority boundary

This repository contains no tracked publisher command or build workflow.
Creating, uploading, changing, deleting, or making a GitHub Release public is
a consequential external write and requires explicit approval outside this
read-only assessment procedure.

## Read-only assessment

1. Record the repository commit and inspect `git status --short` so unrelated
   local work is preserved.
2. Identify the exact requested version or tag. A tag alone is not evidence of
   the remote asset set.
3. With authorized read-only GitHub access, read back the exact release and
   record its asset names, sizes, and integrity material. Do not download,
   upload, or modify assets as part of metadata inspection.
4. Compare the observed asset naming and integrity material to the repository
   README. A conflict, absence, or ambiguity is a stop condition, not a reason
   to edit instructions or create a replacement by guesswork.
5. If local asset verification is separately authorized, verify only the exact
   selected bytes and matching sidecar or manifest as described in [Public
   release contract](../interfaces/public-release-contract.md).

## Stop and escalate

Do not retry a release mutation after a timeout or uncertain result by issuing
another mutation. Read back the exact target first. Escalate an existing tag
with uncertain ownership, mismatched assets, failed integrity check, or any
request to alter a published asset. The tracked source establishes no generic
rollback or deletion authority.
