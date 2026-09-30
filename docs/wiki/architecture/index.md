---
title: Local AI Services Releases — architektura
slug: local-ai-services-releases-architecture
created: 2026-09-13
updated: 2026-09-30
authors: [Codex, Claude]
description: Mapa odpovědností a hranic důvěry mezi veřejným release repem a privátním local-ai-services.
tags: [local-ai-services, architecture, security]
aliases: [architektura release repa, release architecture]
type: overview
status: active
---

# Architektura

Repozitář nemá vlastní nasaditelnou komponentu. Jediný architektonický pohled je
tok build → publikace → download a hranice mezi veřejným obsahem a privátním
`local-ai-services`: [[local-ai-services-releases/local-ai-services-releases-system-boundary]].

Rodič: [[local-ai-services-releases/local-ai-services-releases]].
