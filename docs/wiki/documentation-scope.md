---
title: Rozsah a zdroje dokumentace Local AI Services Releases
slug: local-ai-services-releases-documentation-scope
created: 2026-09-30
updated: 2026-09-30
authors: [Claude]
description: Z jakých zdrojů wiki release repozitáře vychází, proč README není kontrakt a jak zdroje znovu ověřit.
tags: [local-ai-services, releases, documentation]
aliases: [rozsah dokumentace LAS releases, zdroje pravdy release repa]
type: reference
status: active
---

# Rozsah a zdroje dokumentace Local AI Services Releases

## Účel této dokumentace

Agent má z této wiki poznat, co tento veřejný repozitář skutečně distribuuje,
kde leží kód, který releasy vytváří, a co smí udělat bez schválení (jen
dokumentace a read-only kontroly). Wiki nepopisuje runtime edge klienta — to je
dokumentace `local-ai-services`.

## Základ a zdroje pravdy

| Zdroj | Co z něj přebíráme | Jak ověřit aktuálnost |
|---|---|---|
| Toto repo: `AGENTS.md`, `SECURITY.md`, `services.json`, `.gitignore`, `ARTIFACTS.md` | Identita projektu, bezpečnostní politika, deklarace „bez služeb“ | `git -C /home/mistercz/projects/local-ai-services-releases status --short` a čtení souborů |
| Toto repo: `README.md` | **Jen** veřejný odkaz na `releases/latest` a zásadu „ověř hash před spuštěním“. Platformy a instalační dialog v README jsou zastaralé (odpovídají legacy v0.3.1) | Porovnat s `publish_edge_installers.py` |
| `local-ai-services/scripts/publish_edge_installers.py` | Povinná sada assetů, schéma `installer-manifest.json`, formát sidecarů, publikační postup do GitHub Releases | Přečíst funkce `verify_artifact_directory`, `publish_verified_files`, `main` |
| `local-ai-services/scripts/build_edge_installers.py` | Vznik `dist/`, `installer-manifest.json.sha256`, `checksums.txt` | Konec skriptu (zápis sidecarů) |
| `local-ai-services/src/las_v2/package_installers.py`, `package_api.py`, `settings.py` | Primární distribuce `GET /v3/marketplace/installers[/{version}/{platform}]`, `EDGE_RELEASE_DIR`, `EDGE_PUBLIC_RELEASE_VERSION` | Přečíst `PLATFORMS`, `installer_catalog`, `installer_response` |
| `local-ai-services/docs/edge-client.md`, `docs/marketplace.md` | Build na vlastních strojích bez GitHub Actions, Windows smoke, párování kódem z Control Center, GitHub jako volitelný host | Sekce „Public installer build“, „Control Center pairing“, „Operator release workflow“ |
| Runtime (read-only) | Poslední GitHub release a jeho assety; verze v marketplace katalogu | `gh release list/view --repo topolar/local-ai-services-v2-releases`; `curl -s http://127.0.0.1:9285/v3/marketplace/installers` |

Kód `local-ai-services` byl čten na commitu `4120c31` (pracovní strom má cizí
necommitnuté změny mimo dotčené soubory). Runtime readback: 2026-09-30.

## Co tato projektová wiki přidává

- Přesný obsah GitHub Release a úskalí ověřování checksumů (sidecar manifestu a
  `checksums.txt` obsahují cestu `dist/…`) —
  [[local-ai-services-releases/local-ai-services-releases-public-release-contract]].
- Vztah dvou distribučních kanálů, role `.artifacts/` a význam tagů —
  [[local-ai-services-releases/local-ai-services-releases-publication-surface]].
- Uživatelské stažení, ověření a aktuální instalační flow —
  [[local-ai-services-releases/local-ai-services-releases-official-installers]].
- Publikační a incidentní runbooky s hranicí oprávnění —
  [[local-ai-services-releases/local-ai-services-releases-verified-publication]],
  [[local-ai-services-releases/local-ai-services-releases-failure-handling]].

## Co sem nepatří

- Chování runtime, update/drain/force/rollback klienta, párování na serveru,
  edge admin CLI — patří do [[local-ai-services/edge-fleet]],
  [[local-ai-services/edge-operations]] a [[local-ai-services/edge-admin-cli]].
  README tohoto repa sice update chování stručně zmiňuje, kanonický popis je v
  `local-ai-services/docs/edge-client.md`.
- Seznam vydaných verzí a jejich poznámky (changelog) — zdrojem je GitHub Releases.
- Jakékoli tokeny, Cloudflare Access údaje, pairing kódy, binárky a logy.

## Údržba rozsahu

Revidovat při změně `publish_edge_installers.py`, `package_installers.py`,
`build_edge_installers.py` nebo sekcí „Public installer build“ / „Control Center
pairing“ v `docs/edge-client.md`, a při změně README. Postup je v
[[local-ai-services-releases/local-ai-services-releases-maintenance]].
