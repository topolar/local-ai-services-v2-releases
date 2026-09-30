---
title: Local AI Services Releases — funkce
slug: local-ai-services-releases-features
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Katalog toho, co release repozitář uživatelům poskytuje — veřejný download instalátorů a integritní materiál.
tags: [local-ai-services, edge-client, releases]
aliases: [funkce release repozitáře, release repository features]
type: overview
status: active
---

# Funkce

Repozitář neposkytuje runtime, API, build ani publisher. Jeho jedinou funkcí je
veřejně dostupný download host (GitHub Releases) pro instalátory, které vznikají
v `local-ai-services`.

| Funkce | Výsledek pro uživatele | Stav a důkaz | Detail |
|---|---|---|---|
| Stažení instalátoru | Uživatel získá instalátor pro `linux-x86_64`, `linux-aarch64` nebo `windows-x86_64` | GitHub: nejnovější `v0.4.62` (`gh release list`, 2026-09-30); primární je marketplace endpoint `local-ai-services` | [[local-ai-services-releases/local-ai-services-releases-official-installers]] |
| Integritní materiál | Uživatel ověří SHA-256 sidecarem `<asset>.sha256` nebo manifestem | Formát vynucuje `publish_edge_installers.py` | [[local-ai-services-releases/local-ai-services-releases-public-release-contract]] |

`.artifacts/` není funkce ani součást publikačního toku — viz
[[local-ai-services-releases/local-ai-services-releases-publication-surface]].

Kam dál: [[local-ai-services-releases/local-ai-services-releases]],
[[local-ai-services-releases/local-ai-services-releases-system-boundary]].
