---
title: Local AI Services Releases — rozhraní
slug: local-ai-services-releases-interfaces
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Jediné rozhraní repozitáře je sada souborů v GitHub Release; HTTP API ani CLI tu nejsou.
tags: [local-ai-services, releases, interface, security]
aliases: [rozhraní release repozitáře, release interfaces]
type: overview
status: active
---

# Rozhraní

Repozitář nemá HTTP API, socket, package manifest ani CLI. Jeho rozhraním je
sada devíti souborů v každém GitHub Release — viz
[[local-ai-services-releases/local-ai-services-releases-public-release-contract]]
(soubory, schéma `installer-manifest.json`, sidecary, úskalí `sha256sum -c`).

Příbuzné HTTP rozhraní `GET /v3/marketplace/installers` a
`GET /v3/marketplace/installers/{version}/{platform}` patří
`local-ai-services` (`src/las_v2/package_api.py`); jeho vztah k tomuto repu je v
[[local-ai-services-releases/local-ai-services-releases-publication-surface]].

Rodič: [[local-ai-services-releases/local-ai-services-releases]].
