---
title: Hranice veřejného release kanálu
slug: local-ai-services-releases-system-boundary
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Co vlastní tento veřejný repozitář a co local-ai-services; tok od buildu k uživateli a datová hranice.
tags: [local-ai-services, architecture, security, releases]
aliases: [LAS release boundary, binary-only channel, hranice release repa]
type: reference
status: active
---

# Hranice veřejného release kanálu

Tento repozitář je pasivní veřejný cíl publikace. Veškerá logika — build,
ověření, publikace, serverová distribuce, registrace a update klientů — je
v privátním `local-ai-services`.

## Tok

```text
local-ai-services (privátní, vlastní stroje, bez GitHub Actions)
  scripts/build_edge_installers.py  -> artifacts/installers-<v>/dist/ (9 souborů)
  scripts/smoke_edge_installer.ps1  -> windows-smoke.json (vlastní Windows host)
  scripts/publish_edge_installers.py --windows-smoke-report ...
      |                                   \
      | gh release create/upload/edit      \ ruční staging operátorem
      v                                      v
  GitHub Releases tohoto repa          EDGE_RELEASE_DIR/installers/<v>/ na serveru
  (volitelný host, tag -> docs commit)  -> GET /v3/marketplace/installers/... (primární)
      \___________________ uživatel stáhne, ověří SHA-256, spustí ________/
                              instalátor se páruje kódem z Control Center
```

## Odpovědnosti

| Část | Vlastník | Co tu platí |
|---|---|---|
| README, `SECURITY.md`, wiki, `services.json` | toto repo | README je zastaralé (platformy, instalační dialog) — viz [[local-ai-services-releases/local-ai-services-releases-official-installers]] |
| Git tagy `v<version>` | vznikají publisherem LAS | ukazují na docs commit tohoto repa, ne na kód |
| GitHub Release assety | `publish_edge_installers.py` v LAS | přesná sada 9 souborů, neměnné po publikaci |
| `.artifacts/` | nikdo (ignorované, mimo tok) | viz [[local-ai-services-releases/local-ai-services-releases-publication-surface]] |
| Build, runtime, server, párování, update | `local-ai-services` | [[local-ai-services/edge-fleet]], [[local-ai-services/edge-security]] |

## Datová hranice

Do repa, issues, release poznámek ani wiki nepatří: enrollment tokeny,
Cloudflare Access client ID/secret, node/worker bearer tokeny, pairing kódy,
privátní runtime archivy, konfigurace a logy s credentials (`SECURITY.md`).
Release poznámky publisheru obsahují jen text „no credentials are included“ a
marker `edge-publisher-id: <uuid>`. Klienti neobsahují GitHub PAT; runtime
stahují přes autentizovaný broker serveru.

Agent smí v tomto repu měnit dokumentaci a číst metadata. Build, publikace,
změny releasů a runtime jsou mimo toto repo a vyžadují výslovné schválení.

Rodič: [[local-ai-services-releases/local-ai-services-releases-architecture]].
