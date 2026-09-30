---
title: Oficiální instalátory Local AI Edge Client
slug: local-ai-services-releases-official-installers
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Co si uživatel stahuje, odkud, jak ověří SHA-256 a jak aktuální instalátor probíhá (párovací kód, žádné credential prompty).
tags: [local-ai-services, edge-client, installer, security]
aliases: [stažení instalátoru, ověření instalátoru, download installers, verify installer]
type: reference
status: active
---

# Oficiální instalátory Local AI Edge Client

## K čemu funkce slouží

Majitel počítače s NVIDIA GPU si stáhne instalátor Local AI Edge Client pro svou
platformu, ověří jeho SHA-256 a spustí ho. Instalátor připraví Docker, stáhne
hash-ověřený runtime a zaregistruje počítač jako edge uzel pro vybrané služby
(OCR, STT, TTS). Tento repozitář poskytuje jen jeden ze dvou download hostů.

## Rychlý přehled

| Otázka | Odpověď |
|---|---|
| Stav | Poslední GitHub release `v0.4.62` (Latest, 2026-09-17); marketplace endpoint serveru nabízí `0.4.68` (readback 2026-09-30) |
| Pro koho | Majitelé Windows/Linux počítačů s NVIDIA GPU, kteří mají párovací kód z Control Center |
| Platformy | `linux-x86_64` (`.tar.gz`), `linux-aarch64` (`.tar.gz`), `windows-x86_64` (`.exe`). macOS není podporován |
| Odkud | Primárně `https://workers-v2.mistergroup.org/v3/marketplace/installers/{version}/{platform}`; volitelně GitHub Releases tohoto repa |
| Automatizace | Žádná v tomto repu; publikaci spouští operátor ručně z `local-ai-services` |
| Implementace | `local-ai-services`: `scripts/build_edge_installers.py`, `scripts/publish_edge_installers.py`, `src/las_v2/package_installers.py` |

## Jak se používá

1. Zjisti aktuální verzi a hash: `GET https://workers-v2.mistergroup.org/v3/marketplace/installers`
   vrací `version`, `source_commit` a pro každou platformu `path`, `size_bytes`,
   `sha256`.
2. Stáhni instalátor z `path` (odpověď nese hlavičku `X-Content-SHA256`), nebo
   z GitHub Releases, pokud tam daná verze existuje.
3. Ověř hash. Pro GitHub download ve stejném adresáři jako sidecar:
   ```bash
   sha256sum -c las-edge-installer-<version>-<platform>.<ext>.sha256
   # např. las-edge-installer-0.4.62-windows-x86_64.exe.sha256
   ```
   Sidecar se jmenuje **podle celého názvu assetu včetně přípony**
   (`.tar.gz.sha256`, `.exe.sha256`). Obsah je `<sha256>  <název assetu>`.
   Neověřuj přes `installer-manifest.json.sha256` ani `checksums.txt` pomocí
   `sha256sum -c` — řádek pro manifest v nich obsahuje cestu
   `dist/installer-manifest.json`, takže v plochém download adresáři hlásí
   chybějící soubor (detail v
   [[local-ai-services-releases/local-ai-services-releases-public-release-contract]]).
4. V Control Center (stránka AI fronty) si nech vydat jednorázový párovací kód
   a spusť instalátor.

## Podrobný návod a chování

Aktuální instalační flow (podle `local-ai-services/docs/edge-client.md`,
sekce „One-client public installation“ a „Control Center pairing (0.4.55)“):

- Uživatel vybere libovolnou neprázdnou kombinaci OCR, STT, TTS (výchozí jsou
  všechny tři).
- Na Windows instalátor připraví WSL2/Docker Desktop, na Linuxu vyžaduje Docker
  Engine/Compose s NVIDIA Container Toolkit; čeká na Docker a přístup ke GPU.
- Zadá se **párovací kód z Control Center**. Kód je jednorázový, platí 30 minut,
  váže se na povolené služby; Manager ho uloží do privátního `pairing.code`, nikdy
  ne do argumentů příkazu. Při vypršení během přípravy Dockeru se vydá nový kód.
- Klient stáhne a ověří runtime, vytvoří jednu identitu uzlu, jednou se
  zaregistruje a spustí jeden worker na vybranou službu.
- **Instalátor se neptá na enrollment token ani Cloudflare Access client
  ID/secret.** To je legacy flow technical preview v0.3.1 (jen pro již
  zapsané testovací uzly). README tohoto repa ho stále popisuje — je zastaralé.

Zastavovací podmínky pro uživatele i agenta: nesouhlasí hash, velikost nebo
název souboru, sidecar chybí, nebo platforma není jedna ze tří výše. Soubor
nespouštěj, nepřejmenovávej a nenahrávej jinam.

## Omezení a zdroje

- macOS: Docker nemá NVIDIA/Metal passthrough; GPU vertikála není na macOS
  deklarována (`docs/edge-client.md`, „Platform service behavior“). macOS assety
  existují jen v release `v0.3.1`.
- Úspěšný hash ověřuje jen bajty proti zvolenému zdroji hashe; nedokazuje, že
  instalace proběhne ani že je verze aktuální pro server.
- Update, drain/force a rollback nainstalovaného klienta řídí server —
  viz [[local-ai-services/edge-fleet]] a [[local-ai-services/edge-operations]].

Rodič: [[local-ai-services-releases/local-ai-services-releases-features]].
