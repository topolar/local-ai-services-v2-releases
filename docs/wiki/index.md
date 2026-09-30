---
title: Local AI Services Releases
slug: local-ai-services-releases
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Vstupní bod veřejného binárního repozitáře instalátorů Local AI Edge Client — co obsahuje, co ne, odkud se publikuje a kde je skutečný kontrakt.
tags: [local-ai-services, edge-client, releases, distribution]
aliases: [LAS releases, Local AI Edge Client releases, instalátory edge klienta]
type: overview
status: active
---

# Local AI Services Releases

Veřejný GitHub repozitář `topolar/local-ai-services-v2-releases`, do jehož
GitHub Releases se nahrávají hotové instalátory Local AI Edge Client. Samotný
repozitář obsahuje jen dokumentaci (README, `SECURITY.md`, tato wiki) a Git tagy;
neobsahuje zdrojový kód runtime, build, publisher ani server. Instalátory se
**sestavují a publikují z repozitáře `local-ai-services`** a GitHub Releases je
jen **volitelný** download host vedle primární distribuce přes marketplace
endpoint `local-ai-services`.

## Agent entrypoint

- **Identita:** project ID `local-ai-services-releases`, canonical cwd
  `/home/mistercz/projects/local-ai-services-releases`, remote
  `https://github.com/topolar/local-ai-services-v2-releases.git`, větev `main`.
  Registr projektů ho vede jako `unconfirmed` (viz `AGENTS.md`).
- **Zdroj pravdy pro kontrakt instalátorů není README tohoto repa**, ale kód v
  `/home/mistercz/projects/local-ai-services`:
  `scripts/publish_edge_installers.py` (GitHub publisher),
  `scripts/build_edge_installers.py` (build), `src/las_v2/package_installers.py`
  (serverová distribuce) a `docs/edge-client.md`. Mapa zdrojů je v
  [[local-ai-services-releases/local-ai-services-releases-documentation-scope]].
- **README je zastaralé** (ověřeno 2026-09-30): uvádí macOS a Windows `.zip`
  (sada assetů z legacy v0.3.1) a interaktivní zadávání enrollment tokenu a
  Cloudflare Access credentials. Aktuální instalátory mají platformy
  `linux-x86_64.tar.gz`, `linux-aarch64.tar.gz`, `windows-x86_64.exe` a párují se
  jednorázovým kódem z Control Center. README neber jako kontrakt a nikomu neraď
  zadávat Cloudflare credentials. Oprava README je rozhodnutí vlastníka.
- **Bezpečné první kontroly (READ):**
  ```bash
  cd /home/mistercz/projects/local-ai-services-releases
  git status --short && git log --oneline -3
  gh release list --repo topolar/local-ai-services-v2-releases --limit 5
  curl -s http://127.0.0.1:9285/v3/marketplace/installers   # katalog LAS API
  ```
  Očekávání k 2026-09-30: čistý strom na `f6572ee`; nejnovější GitHub release
  `v0.4.62` (Latest, 2026-09-17); lokální marketplace katalog vrací verzi
  `0.4.68`. Rozdíl verzí je aktuální stav, ne chyba čtení.
- **Oprávnění a STOP:** z tohoto repa smíš měnit jen dokumentaci. Build,
  publikace, úprava či mazání GitHub Release, změna tagů a staging na server
  patří do `local-ai-services` a vyžadují výslovné schválení vlastníka.
  Nesahej na `.artifacts/`, necommituj binárky, nikam nevkládej tokeny.
- **Hotovo znamená:** tvrzení ve wiki odpovídají kódu v `local-ai-services` a
  read-only readbacku (GitHub, katalog), `wikilinky` vedou na existující slugy,
  commit obsahuje jen soubory ve `docs/wiki/`.

## Mapa dokumentace

| Úkol | Stránka |
|---|---|
| Z čeho wiki vychází a co je zdroj pravdy | [[local-ai-services-releases/local-ai-services-releases-documentation-scope]] |
| Co uživatel stahuje, jak ověří hash, jak probíhá instalace | [[local-ai-services-releases/local-ai-services-releases-official-installers]] |
| Přesná sada souborů v release, schéma manifestu, sidecary | [[local-ai-services-releases/local-ai-services-releases-public-release-contract]] |
| GitHub Releases vs. marketplace endpoint, `.artifacts/`, tagy | [[local-ai-services-releases/local-ai-services-releases-publication-surface]] |
| Hranice odpovědnosti mezi tímto repem a `local-ai-services` | [[local-ai-services-releases/local-ai-services-releases-system-boundary]] |
| Jak se release sestaví, ověří a publikuje (jen se schválením) | [[local-ai-services-releases/local-ai-services-releases-verified-publication]] |
| Nesedí hash, chybí asset, únik secretu, neúspěšná publikace | [[local-ai-services-releases/local-ai-services-releases-failure-handling]] |
| Údržba této dokumentace | [[local-ai-services-releases/local-ai-services-releases-maintenance]] |

Oblastní indexy: [[local-ai-services-releases/local-ai-services-releases-features]],
[[local-ai-services-releases/local-ai-services-releases-interfaces]],
[[local-ai-services-releases/local-ai-services-releases-infrastructure]],
[[local-ai-services-releases/local-ai-services-releases-architecture]],
[[local-ai-services-releases/local-ai-services-releases-operations]],
[[local-ai-services-releases/local-ai-services-releases-development]].

## Stav a odpovědnost

- **Provoz:** repozitář nemá žádnou službu, proces ani port (`services.json` má
  `services: []`). Nic se tu nespouští.
- **GitHub Releases (readback 2026-09-30):** 31 tagů `v0.3.1`–`v0.4.62`; Latest
  je `v0.4.62` publikovaný 2026-09-17 se sadou 3 platforem. Novější verze
  (např. 0.4.66 a 0.4.68) na GitHubu publikované nejsou; primární kanál je marketplace
  endpoint `local-ai-services` (lokálně vrací 0.4.68).
- **Vlastník:** repozitář patří účtu `topolar`; odpovědná osoba/role pro
  rozhodování o releasech není v repu zapsána.

> **Otevřená otázka:** Má se na GitHub Releases dál publikovat každá verze, nebo
> je publikace pozastavená / nahrazená marketplace endpointem? A má se README
> přepsat na aktuální platformy a párovací flow? Viz
> [[local-ai-services-releases/local-ai-services-releases-publication-surface]].

## Souvislosti

Přehled celého systému: [[wiki/system-prehled]], lokální AI a edge:
[[wiki/system-lokalni-ai-a-edge]]. Dokumentace zdrojového projektu:
[[local-ai-services/local-ai-services]], edge flotila
[[local-ai-services/edge-fleet]], provoz edge [[local-ai-services/edge-operations]].
