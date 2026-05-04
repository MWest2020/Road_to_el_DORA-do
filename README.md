# Sprint retro & review automatisering

Wekelijkse geautomatiseerde retrospective-data voor het Conduction dev team,
gebaseerd op GitHub Project 4 (`ConductionNL/projects/4`).

## Wat het doet

Elke **vrijdag 09:00 (NL-tijd)** draait een GitHub Actions workflow die:

1. De huidige sprint detecteert uit het `kanban-sprint` field
2. DORA-metrics berekent (lead time, deploy freq, change fail rate, recovery,
   rework)
3. Cycle time histogram maakt (issue created → done)
4. Team-stickers berekent (geen individuele leaderboards)
5. Retro signals detecteert (sprint-spillover, lange threads, frictie-taal)
6. Een Slack-bericht post naar het afgesproken kanaal

Output ook beschikbaar als workflow artifact (`report.md` + `raw-data.json`)
voor 90 dagen, voor audit en reproduceerbaarheid.

## Ontwerpprincipe: speel op de bal, niet op de mens

- Alle metrics op team-niveau, geen per-persoon uitsplitsing
- Geen auteursnamen of @-mentions in de output
- Geen late-night detectie of "celebrities" lijsten
- Sentiment alleen op issue-niveau, deterministisch via keyword-lijst (geen
  LLM black-box)
- Stickers alleen haalbaar als heel het team meedoet — niet door 1 persoon
  hard te werken

## Setup

### 1. Repo

Plaats de bestanden:

```
.github/
├── workflows/
│   └── sprint-retro.yml
└── prompts/
    └── sprint-retro.md
```

Suggestie: in een centrale ops-repo zoals `ConductionNL/team-ops` of
`ConductionNL/.github`. Niet in een product-repo.

### 2. Secrets

In de repo (of org-level voor herbruikbaarheid):

| Secret | Inhoud |
|---|---|
| `SPRINT_ANALYSIS_GH_TOKEN` | Fine-grained PAT of GitHub App token. Scopes: `read:project`, `contents:read` op de relevante repos, `issues:read`, `pull_requests:read` |
| `ANTHROPIC_API_KEY` | API-key voor Claude Code SDK |
| `SLACK_WEBHOOK_RETRO` | Incoming webhook URL voor het retro-kanaal |

**Voor de PAT**: gebruik bij voorkeur een GitHub App met fine-grained
permissies in plaats van een persoonlijke PAT — auditable, niet gekoppeld
aan iemand die later vertrekt.

### 3. Slack incoming webhook

In Slack: app aanmaken in de Conduction workspace → "Incoming Webhooks" →
nieuwe webhook voor het retro-kanaal. URL als secret opslaan.

Saaie keuze: één kanaal-specifieke webhook per kanaal, geen Slack app met
brede scopes.

### 4. Eerste run

Trigger handmatig via "Run workflow" knop in de Actions tab. Check:

- [ ] `slack-payload.json` artifact aanwezig
- [ ] Bericht in Slack zoals verwacht
- [ ] `report.md` artifact bevat geen auteursnamen
- [ ] `raw-data.json` is herleidbaar naar de cijfers in het bericht

## Aanpassen

**Andere tijd**: pas de cron in `sprint-retro.yml` aan. UTC. Vergeet niet
dat NL zomertijd kent.

**Andere project**: pas in `sprint-retro.md` de org/project nummer aan.

**Extra metrics**: voeg toe aan stap 4/5 in `sprint-retro.md`. Houd
team-niveau aan.

**Andere drempels** (bv. "lange thread" niet >10 maar >15 comments): in
`sprint-retro.md` aanpassen.

## Iteratiepad

V1 (deze versie): GitHub Actions, deterministische analyse, statisch
Slack-bericht.

V2 mogelijk: 
- LLM-samenvatting van retro signals (boven op de ruwe data, niet ipv)
- Web-dashboard met historie over sprints
- Trends grafiek (lead time over tijd)

V2 alleen bouwen als V1 daadwerkelijk gebruikt wordt en het team om meer
vraagt. Niet andersom.

## Audit trail

- Workflow runs zichtbaar in Actions tab, 90 dagen retentie
- Artifacts (incl. raw data) 90 dagen beschikbaar
- Geen state buiten de workflow run (geen DB, geen externe storage)
- Slack webhook URL is het enige externe egress-punt — beperkt en logbaar
- ISO 27001: deze workflow is een geautomatiseerde rapportage, geen
  productie-systeem. Past onder "monitoring & measurement" controls.
