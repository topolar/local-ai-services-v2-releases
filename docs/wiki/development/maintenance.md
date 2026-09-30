---
title: Údržba dokumentace release repozitáře
slug: local-ai-services-releases-maintenance
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Jak bezpečně měnit README, SECURITY.md a wiki tohoto repa a jak ověřit, že odpovídají publisheru v local-ai-services.
tags: [local-ai-services, releases, documentation, development]
aliases: [update release docs, údržba release dokumentace]
type: runbook
status: active
---

# Údržba dokumentace release repozitáře

## Rozsah

V repu se mění jen dokumentace: `README.md`, `SECURITY.md`, `ARTIFACTS.md`,
`docs/wiki/`. Nezavádět build systém, nekopírovat kód LAS, necommitovat
`.artifacts/` ani `.temp/` (obojí v `.gitignore`), neměnit registr ani mounty.

Repo nemá package manifest, testy, formatter ani CI. Má jen `services.json`
(schema v2, `services: []`) — deklaraci pro Control Center, že projekt nemá
žádnou službu.

## Postup (WRITE jen do dokumentace)

1. READ: `AGENTS.md`, `README.md`, `SECURITY.md`, `services.json`,
   `git status --short`. Zachovat cizí změny.
2. Fakta o assetech, manifestu a publikaci ověřit v
   `local-ai-services/scripts/publish_edge_installers.py`,
   `src/las_v2/package_installers.py` a `docs/edge-client.md`; stav remote přes
   `gh release list/view` (read-only). Mapa zdrojů:
   [[local-ai-services-releases/local-ai-services-releases-documentation-scope]].
3. Při rozporu README ↔ kód platí kód; rozpor popsat, ne volit verzi podle odhadu.
4. Upravit věcnou stránku a její `updated` (prosté datum). Žádné changelogy.
5. Kontroly:
   ```bash
   cd /home/mistercz/projects/local-ai-services-releases
   git diff --check
   git status --short
   ```
   a ověřit, že všechny wikilinky tvaru `mount/slug` vedou na existující slugy.
6. Aktualizovat `docs/wiki/.wiki-update-state.json`
   (`last_documented_source_commit` = commit repa, ze kterého wiki vychází).

Commit ve wiki není push, publikace release ani nasazení; hlásit zvlášť.

## Známý dluh

README a `ARTIFACTS.md` jsou zastaralé (platformy macOS/`.zip`, enrollment token
a Cloudflare credentials, `.artifacts/` jako staging). Oprava čeká na rozhodnutí
vlastníka — viz otevřené otázky na
[[local-ai-services-releases/local-ai-services-releases-publication-surface]].

Rodič: [[local-ai-services-releases/local-ai-services-releases-development]].
