---
title: Distribuční kanály, tagy a lokální .artifacts
slug: local-ai-services-releases-publication-surface
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: GitHub Releases jako volitelný host vedle primárního marketplace endpointu, význam Git tagů, stav .artifacts/ a absence GitHub Actions.
tags: [local-ai-services, releases, github, infrastructure]
aliases: [GitHub Releases, artifact staging, marketplace installers, distribuční kanály]
type: reference
status: active
---

# Distribuční kanály, tagy a lokální `.artifacts`

Instalátory edge klienta se distribuují dvěma kanály. Oba plní operátor ručně
ze stejného buildu v `local-ai-services`; tento repozitář je jen druhý z nich.

## Kanály

| Kanál | Adresa | Zdroj souborů | Stav 2026-09-30 |
|---|---|---|---|
| **Primární:** marketplace origin `local-ai-services` | `https://workers-v2.mistergroup.org/v3/marketplace/installers` (katalog) a `/v3/marketplace/installers/{version}/{platform}` (binárka, hlavička `X-Content-SHA256`, `Cache-Control: immutable`) | `EDGE_RELEASE_DIR/installers/<version>/` na serveru; nabízená verze = `EDGE_PUBLIC_RELEASE_VERSION` | Katalog vrací `0.4.68` (lokálně `127.0.0.1:9285`, veřejně HTTP 200) |
| **Volitelný:** GitHub Releases tohoto repa | `https://github.com/topolar/local-ai-services-v2-releases/releases` | `local-ai-services/artifacts/installers-<version>/dist` přes `publish_edge_installers.py` a `gh` | Latest `v0.4.62` (2026-09-17) |

`local-ai-services/docs/edge-client.md`: „GitHub Releases remains an optional
download host, independent of building.“ `docs/marketplace.md` uvádí marketplace
jako „Primary distribution origin“. Kód serveru (`PLATFORMS` v
`package_installers.py`) i publisher mají shodnou sadu tří platforem.

> **Otevřená otázka:** Publikace na GitHub se od 0.4.62 zastavila, zatímco
> server nabízí 0.4.68. Je to záměr (GitHub jen příležitostný mirror), nebo
> nedokončený krok release procesu? `releases/latest` odkazovaný z README dnes
> ukazuje na starší verzi než server.

## Build a publikace: bez GitHub Actions

Repozitář nemá `.github/` ani žádný workflow. Podle `docs/edge-client.md` se
instalátory i worker image staví na vlastních strojích, Windows smoke běží na
vlastním Windows hostu a publikace jde lokálně přes `gh` s přihlášením operátora.
Postup: [[local-ai-services-releases/local-ai-services-releases-verified-publication]].

## Git tagy vs. čísla releasů

- 31 lightweight tagů `v0.3.1`–`v0.4.62` (lokálně i na remote, `git ls-remote`
  2026-09-30). Mezi čísly jsou mezery (např. chybí `v0.4.33`, `v0.4.38`).
- Tag vzniká při `gh release create` a ukazuje na aktuální HEAD `main` **tohoto**
  repa, tedy na dokumentační commit (`8168920`, `7c48fba`, `f6572ee`) — ne na kód.
- Zdrojový commit runtime je jen v `installer-manifest.json` → `source_commit`.
- Čísla verzí LAS, které nikdy nešly na GitHub (např. 0.4.66, 0.4.68), tu tag
  nemají. Absence tagu neznamená, že verze neexistuje.
- Lokální klon může být nefetchnutý; o stavu remote rozhoduje
  `gh release list` / `git ls-remote --tags origin`.

## `.artifacts/` a `ARTIFACTS.md`

`.artifacts/` je v `.gitignore` a **není součástí publikačního toku**: publisher
čte z `dist/` v repu `local-ai-services` a vyžaduje kompletní sadu devíti souborů.
K 2026-09-30 obsahuje jen osamocený `las-edge-installer-0.4.53-windows-x86_64.exe`
se sidecarem (hash sedí). `ARTIFACTS.md` popisuje `.artifacts/` jako staging před
publikací — to neodpovídá skutečnému postupu (zastaralé, stejně jako README).
Soubory v `.artifacts/` neber jako důkaz release ani jako totožné s remote assetem.

> **Otevřená otázka:** Má `.artifacts/` (a `ARTIFACTS.md`) zůstat, nebo je
> soubor 0.4.53 zbytek k odstranění? Rozhoduje vlastník; nemazat bez pokynu.

## Nezjištěno

Politika retence releasů, podepisování instalátorů (`local-ai-services/docs/edge-client.md`
mluví o „signed native Manager installer“, klíč ani postup podpisu tu popsané
nejsou) a monitoring dostupnosti GitHub kanálu. Nevymýšlet; ověřit u vlastníka `local-ai-services`.

Rodič: [[local-ai-services-releases/local-ai-services-releases-infrastructure]].
