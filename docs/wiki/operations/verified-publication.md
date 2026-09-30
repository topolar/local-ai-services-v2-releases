---
title: Ověření a publikace release instalátorů
slug: local-ai-services-releases-verified-publication
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Read-only kontrola existujícího release a schválený postup build → Windows smoke → verify-only → publikace přes publish_edge_installers.py.
tags: [local-ai-services, releases, runbook, security]
aliases: [release verification, publikace instalátorů, publish edge installers]
type: runbook
status: active
---

# Ověření a publikace release instalátorů

## K čemu postup slouží

(a) Bezpečně zjistit, co je v GitHub Release dané verze, a (b) se schválením
vlastníka publikovat novou verzi. Publikace je nevratný veřejný zápis; tag se
nikdy nepřepisuje. Nic z toho se nespouští z tohoto repa — publisher žije v
`/home/mistercz/projects/local-ai-services`.

## A. Read-only kontrola release (READ)

```bash
gh release list --repo topolar/local-ai-services-v2-releases --limit 10
gh release view v<version> --repo topolar/local-ai-services-v2-releases \
  --json isDraft,publishedAt,assets --jq '{isDraft,publishedAt,assets:[.assets[]|{name,size,digest}]}'
curl -s https://workers-v2.mistergroup.org/v3/marketplace/installers
```

Očekávání: `isDraft: false`, přesně 9 assetů dle
[[local-ai-services-releases/local-ai-services-releases-public-release-contract]];
GitHub `digest` (`sha256:…`) se shoduje s `sha256` v `installer-manifest.json`.
Katalog serveru může nabízet jinou (novější) verzi než GitHub — to není chyba
čtení. Stažení malých souborů (`-p 'installer-manifest.json*' -p '*.sha256'
-p checksums.txt`) do scratch adresáře je read-only vůči remote.

STOP: jiná sada souborů, draft s cizím markerem, nesoulad digestu, verze bez
`source_commit` → eskalace, nic nepřepisovat.

## B. Publikace nové verze (DEPLOY, jen se schválením)

Předpoklady: Linux builder s Python 3.12+, Go 1.22+, Node 22+, uv 0.12.1; `gh`
přihlášené s právem zápisu do `topolar/local-ai-services-v2-releases`; vlastní
Windows stroj pro smoke; plný 40znakový commit LAS, jehož `pyproject.toml`
verze odpovídá `--version`. cwd: `/home/mistercz/projects/local-ai-services`.

1. **Build** (nový výstupní adresář):
   `python3 scripts/build_edge_installers.py --version <v> --source-commit <SHA> --output artifacts/installers-<v> --uv-cache artifacts/uv-cache`
   — na konci sám spustí publisher s `--verify-only`.
2. **Windows smoke** na vlastním Windows hostu: `scripts/smoke_edge_installer.ps1`
   s `-Installer`, `-Version`, `-ExpectedSha256`, `-OutputDirectory`; výsledný
   `windows-smoke.json` zkopírovat do `artifacts/installers-<v>/`. Report musí mít
   `status: passed`, správnou verzi, platformu `windows-x86_64`, hash EXE a
   kontroly `embedded-ui`, `setup-version`, `manager-takeover`, `powershell-syntax`.
3. **Verify-only:** `python3 scripts/publish_edge_installers.py --directory artifacts/installers-<v>/dist --version <v> --expected-head-sha <SHA> --verify-only`
   → „Verified 9 local release files; nothing published“.
4. **Publikace:** tentýž příkaz bez `--verify-only` a s
   `--windows-smoke-report artifacts/installers-<v>/windows-smoke.json`.
   Chování: odmítne, pokud tag `v<v>` už existuje → vytvoří draft
   „Local AI Edge Client <v>“ s markerem `edge-publisher-id` → nahraje 9 souborů
   → porovná remote názvy, velikosti a digesty → `--draft=false --latest` →
   vypíše URL release. Při jakékoli chybě smaže **jen vlastní draft** (podle
   markeru); publikovaný release nemaže nikdy.
5. **Serverový kanál** je samostatný ruční krok (staging do
   `EDGE_RELEASE_DIR/installers/<v>`, registrace runtime, změna
   `EDGE_PUBLIC_RELEASE_VERSION`, restart API) — viz
   `local-ai-services/docs/marketplace.md`, „Operator release workflow“, a
   [[local-ai-services/edge-operations]].

## Ověření a STOP

Po publikaci proveď část A. Neopakuj publikaci po timeoutu naslepo: nejdřív
readback; když tag existuje, publisher další pokus odmítne („public release tag
already exists“). Zbytek draftu s vlastním markerem, který cleanup nestihl
smazat, řeší vlastník ručně.

## Rollback

Není implementován. Publikovaný release ani tag se nepřepisuje; oprava = nová
verze. Smazání publikovaného release je rozhodnutí vlastníka (neotestovaný postup).

Rodič: [[local-ai-services-releases/local-ai-services-releases-operations]].
