---
title: Řešení chyb ověření a publikace instalátorů
slug: local-ai-services-releases-failure-handling
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Stop podmínky a postup při nesouhlasném hashi, chybějícím assetu, selhané publikaci nebo úniku secretu.
tags: [local-ai-services, releases, incident, security]
aliases: [bad installer, checksum mismatch, release incident, chyba instalátoru]
type: runbook
status: active
---

# Řešení chyb ověření a publikace instalátorů

## Okamžitá bezpečná reakce

| Situace | Co udělat | Co nedělat |
|---|---|---|
| `sha256sum -c` na sidecaru platformy selže, nesedí velikost | Zapsat tag/URL, název assetu, očekávaný a zjištěný digest, exit code; eskalovat vlastníkovi LAS | Spouštět, přejmenovat, generovat nový sidecar, znovu publikovat |
| `sha256sum -c` selže jen na `installer-manifest.json.sha256` nebo řádku `dist/installer-manifest.json` v `checksums.txt` | Známé chování (cesta `dist/`), ne poškození — ověřit manifest ručně, viz [[local-ai-services-releases/local-ai-services-releases-public-release-contract]] | Hlásit jako kompromitovaný release |
| Release má jinou sadu platforem (macOS, `.zip`) | U `v0.3.1` je to legacy stav; u 0.4.x eskalovat | Doplňovat chybějící asset ručně |
| Uživatel se ptá na enrollment token / Cloudflare credentials podle README | Vysvětlit, že aktuální instalátor používá párovací kód z Control Center; README je zastaralé | Radit zadávat Cloudflare credentials |
| Možný únik secretu | Soukromě kontaktovat vlastníka repa (`SECURITY.md`) | Zakládat veřejné issue, přikládat logy |

## Selhaná publikace (publisher `local-ai-services`)

1. READ: `gh release view v<v> --repo topolar/local-ai-services-v2-releases --json isDraft,body,assets`.
2. Neexistuje → publisher svůj draft smazal; lze opakovat až po odstranění příčiny.
3. Draft s markerem `edge-publisher-id` z tohoto běhu → cleanup nestihl; smazat
   jen se schválením vlastníka.
4. Publikovaný (`isDraft: false`) → hotovo i přes chybu klienta; publisher tento
   případ po chybě `gh release edit` sám bere jako úspěch. Ověřit assety (runbook A).
5. Chyby ověření lokálního `dist/` (hlášky `installer manifest shape is invalid`,
   `installer platform set is incomplete`, `aggregate checksums differ`,
   `Windows smoke report does not match the installer`) → opravit build v LAS,
   nic neobcházet.

Postup publikace: [[local-ai-services-releases/local-ai-services-releases-verified-publication]].

## Eskalace

Mazání či přepis publikovaného release, revokace credentials a náhradní release
vyžadují plán schválený vlastníkem. Veřejná tvrzení jen na základě readbacku.

Rodič: [[local-ai-services-releases/local-ai-services-releases-operations]].
