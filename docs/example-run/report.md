# Sprint retro — Kanban sprint 1 (mid-sprint)

**Sprint window:** 2026-04-28 → 2026-05-11 (14d nominaal, dag 6)
**Gegenereerd:** 2026-05-05T06:27Z
**Bron:** [ConductionNL Project 4 · Conduction KanBan](https://github.com/orgs/ConductionNL/projects/4)
**Benchmarks:** [DORA 2023 State of DevOps Report](https://dora.dev/research/2023/dora-report/)

> Mid-sprint stand. Cijfers veranderen nog tot 2026-05-11.
> Demo run vanaf personal repo, niet de uiteindelijke kit-installatie.

## Flow metrics (met industry tier)

| Metric | Onze waarde | DORA-tier | Range | Caveat |
|---|---|---|---|---|
| Lead time | 0.2h (median) | **Elite** | (< 1h) | proxy: PR open→merge i.p.v. commit→live |
| PR onder 24u | 86.6% | n/a (geen DORA-metric) | high-performers >70% | LinearB/GitClear benchmark |
| Deploy frequency | 6.5/day (39 in 6d) | **Elite** | (>= 1/day) | proxy: releases ≈ deploys |
| Change failure rate | n/a | — | — | vereist revert/hotfix-correlatie |
| MTTR | n/a | — | — | vereist incident-data |

### Caveats bij de tiers
- DORA's strikte lead time meet *first commit → live*. Wij meten *PR opened → merged* — een proxy die de tijd in de branch vóór PR negeert, en de deploy-tijd na merge. Onze proxy is **systematisch lager** dan de echte lead time, dus 'Elite' is hier eerder een *plafond* dan een claim.
- Deploy frequency telt GitHub releases. Als jullie pipeline auto-deployt op release is dat ≈ accurate; anders een overschatting.

## Cycle time issues (created → closed)

- Mediaan: **10.3d**
- P90: **15.8d**
- 21 issues gesloten, 0 nieuw geopend

```
     <1d   0
    1-2d   0
    2-4d   0
    4-7d   0
   7-14d  ████████████████████ 17
    >14d  █████ 4
```

**Industry context:** geen formele DORA-tier voor cycle time. Atlassian's *DevOps Maturity* en LeanKit's *Flow Metrics* literatuur citeren typisch 3-7d als gezond voor agile teams; >10d wordt vaak gelezen als signaal voor grote scope, multi-PR werk of intake-friction. Bron: o.a. [Atlassian Open DevOps](https://www.atlassian.com/devops) — geen één canonieke benchmark zoals DORA.

## Sprint stickers
- Smooth issues (gesloten ≤3d, <2 comments): **0**
- Nieuwe issues deze sprint: **0** — alle gesloten werk is carry-over

## Retro signals

### Lange threads (>10 comments)
- <https://github.com/ConductionNL/woo-website-template-apiv2/issues/126|#126> (woo-website-template-apiv2) — 15 comments

### Friction-taal scan
n/a deze run.

### Sprint-spillover
n/a deze run.

## Deploys per repo
- openregister: 21
- opencatalogi: 16
- docudesk: 2

---

## Headline insight

Op DORA-proxies scoort de PR/release-pipeline **Elite** (lead time mediaan 12 min, 6.5 deploys/dag). Maar issue cycle time is **10.3d mediaan** en *geen enkele* nieuwe issue is deze sprint geopend en gesloten — alles is carry-over.

Dat patroon is zelden een PR/CI-probleem. Hypotheses voor het team:
- Issues zijn te grof opgeknipt (één issue → meerdere sprints' werk)
- Intake/grooming is vertraagd
- Issues blijven open hangen na de feitelijke 'klaar' door admin/review-step

De data alleen zegt dit niet — de retro is om dit te checken.

## Methodologie & disclaimers

1. **Geen per-persoon data.** Alle cijfers op team-niveau. Geen author-velden, login-handles of @-mentions in de output.
2. **Proxies, geen strikte DORA.** Lead time = PR open→merge. Deploy freq = GitHub releases. CFR/MTTR niet berekend.
3. **Mid-sprint.** Cijfers veranderen nog. Een afgesloten retro zou dezelfde workflow op de afgeronde sprint draaien.
4. **Demo-run vanaf personal repo.** Echte installatie hoort op `ConductionNL/team-ops` of `.github`.