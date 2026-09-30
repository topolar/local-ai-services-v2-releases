---
title: Kontrakt veřejného release instalátorů
slug: local-ai-services-releases-public-release-contract
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Přesná sada souborů v GitHub Release, schéma installer-manifest.json, formát sidecarů a známé úskalí sha256sum -c.
tags: [local-ai-services, releases, manifest, checksum]
aliases: [installer manifest, release asset contract, installer-manifest.json]
type: reference
status: active
---

# Kontrakt veřejného release instalátorů

Každý release `v<version>` v `topolar/local-ai-services-v2-releases` (od řady
0.4.x) obsahuje přesně devět souborů. Kontrakt definuje a vynucuje
`local-ai-services/scripts/publish_edge_installers.py`
(`verify_artifact_directory`); toto repo ho nedefinuje, jen hostuje výsledek.
Používá ho uživatel při ověření stažení a operátor při publikaci.

## Sada souborů

| Soubor | Obsah |
|---|---|
| `las-edge-installer-<v>-linux-x86_64.tar.gz` | instalátor |
| `las-edge-installer-<v>-linux-aarch64.tar.gz` | instalátor |
| `las-edge-installer-<v>-windows-x86_64.exe` | instalátor |
| `<každý z výše>.sha256` (3×) | jeden řádek `<sha256>  <název assetu>` |
| `installer-manifest.json` | manifest (níže) |
| `installer-manifest.json.sha256` | `<sha256>  dist/installer-manifest.json` |
| `checksums.txt` | seřazené řádky všech tří assetů + řádek manifestu s `dist/` |

Publisher odmítne neúplnou sadu platforem („installer platform set is
incomplete“) i jakýkoli soubor navíc („aggregate artifact contains unexpected
files“). Ověřeno readbackem `v0.4.62` dne 2026-09-30. Výjimka: `v0.3.1` (legacy
technical preview) má macOS `.tar.gz` a Windows `.zip` — z ní pochází seznam v README.

## Schéma `installer-manifest.json`

```json
{
  "schema_version": 1,
  "version": "0.4.62",
  "source_commit": "<plný 40znakový SHA commitu local-ai-services>",
  "assets": [
    {"name": "las-edge-installer-0.4.62-linux-x86_64.tar.gz", "sha256": "<64 hex>", "size_bytes": 123}
  ]
}
```

Publisher vyžaduje přesně tyto čtyři klíče a u assetu přesně `name`, `sha256`,
`size_bytes`. `source_commit` je jediný spolehlivý odkaz na zdrojový kód — Git
tag v tomto repu ukazuje na dokumentační commit tohoto repa, ne na kód.

Serverový katalog (`package_installers.installer_catalog`) čte ze stejného
manifestu navíc volitelné `published_at`; manifest nasazený na serveru ho
obsahuje (katalog 0.4.68 vrací `published_at`). Takový manifest by GitHub
publisher kvůli klíči navíc odmítl — oba kanály tedy nemusí mít bajtově stejný
manifest.

## Ověření stažených souborů

```bash
# funguje: sidecar platformního assetu
sha256sum -c las-edge-installer-<v>-windows-x86_64.exe.sha256

# NEfunguje v plochém adresáři: odkazuje na dist/installer-manifest.json
sha256sum -c installer-manifest.json.sha256   # "FAILED open or read", exit 1
sha256sum -c checksums.txt                     # instalátory OK, manifest FAILED, exit 1
```

Manifest ověř ručně (`sha256sum installer-manifest.json` a porovnat s hodnotou v
sidecaru), nebo `sha256sum -c --ignore-missing checksums.txt`, které ověří jen
tři instalátory. Prefix `dist/` zapisuje build (`build_edge_installers.py`) a
publisher ho vynucuje. Chování ověřeno na souborech `v0.4.62` dne 2026-09-30.

> **Otevřená otázka:** Je `dist/` prefix v `installer-manifest.json.sha256` a
> `checksums.txt` záměr, nebo chyba buildu? Rozhoduje vlastník `local-ai-services`;
> tato wiki jen popisuje současné chování.

Digest shoda nedokazuje autorizaci, aktuálnost ani úspěšnou instalaci.

Rodič: [[local-ai-services-releases/local-ai-services-releases-interfaces]].
Uživatelský postup: [[local-ai-services-releases/local-ai-services-releases-official-installers]].
