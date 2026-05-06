# Example run — 2026-05-04 (mid-sprint)

Bevroren output van een handmatige run van het sprint-retro kit tegen
`ConductionNL/projects/4` ("Conduction KanBan"), dag 6 van Kanban sprint 1
(2026-04-28 → 2026-05-11).

**Doel:** het team de vorm van de output laten zien zonder dat ze de
workflow zelf hoeven te draaien.

## Hoe gegenereerd

- Lokale `claude` / `gh` calls vanaf een personal repo, niet via
  GitHub Actions. Reden: nog geen org-token op de personal repo gezet
  voor een demo (zie kit-README, sectie "Setup").
- `gh auth refresh -s read:project,project` voor toegang tot ProjectV2.
- Deterministische analyse in Python (geen LLM-call voor sentiment).
- Output gepost naar het retro-kanaal via incoming webhook.

## Wat er in zit

| Bestand | Inhoud |
|---|---|
| `report.md` | Markdown rapport met tabellen, histogram, methodologie |
| `slack-payload.json` | Block Kit JSON zoals naar Slack gepost |

## Wat ontbreekt vs. de volledige kit-spec

Bewust niet uitgevoerd voor deze demo (markeren als `n/a` in de output):

- Friction-taal scan op issue comments (~39 extra API-calls)
- Change failure rate (vereist revert/hotfix file-overlap analyse)
- Recovery time (vereist incident-data)
- Sprint-spillover (vereist iteration field-history)
- Strikte DORA lead time (eerste commit → live; nu PR open → merge als proxy)

De kit kan dit allemaal — V1 demo heeft alleen de goedkoopste 80% gepakt.

## Disclaimer

- **Mid-sprint stand**, geen afgesloten retro. Cijfers veranderen tot
  2026-05-11.
- **Geen per-persoon data.** Op team-niveau geaggregeerd, conform het
  "speel op de bal"-principe in de kit-README.
- **Demo, geen productie.** Echte installatie hoort op
  `ConductionNL/team-ops` of `.github` met org-secrets en GitHub App.
