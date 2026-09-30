---
title: Local AI Services Releases — infrastruktura
slug: local-ai-services-releases-infrastructure
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Repozitář nemá host, proces ani port; distribuci zajišťuje GitHub Releases (volitelně) a marketplace endpoint local-ai-services (primárně).
tags: [local-ai-services, releases, infrastructure]
aliases: [infrastruktura release repozitáře, release infrastructure]
type: overview
status: active
---

# Infrastruktura

Repozitář nemá žádný host, proces, port, kontejner, databázi ani zálohovaná
data (`services.json`: `services: []`). Jediná „infrastruktura“ je GitHub
Releases jako volitelný download host; primární distribuce běží na serveru
`local-ai-services`. Není tu žádný CI workflow.

| Téma | Stránka |
|---|---|
| Dva distribuční kanály, Git tagy, `.artifacts/`, absence GitHub Actions | [[local-ai-services-releases/local-ai-services-releases-publication-surface]] |
| Hosting marketplace endpointu (porty, PM2, tunel) | [[local-ai-services/local-ai-services]] |

Zálohy: nic k zálohování — obsah releasů je reprodukovatelný jen buildem
z `source_commit` v `local-ai-services`; zdroj dokumentace je v Gitu.

Rodič: [[local-ai-services-releases/local-ai-services-releases]].
