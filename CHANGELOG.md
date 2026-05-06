# Changelog

All notable changes to this repo are tracked here.

## 2026-05-06

### Added
- `docs/example-run/` — frozen demo artefacts from a manual run on
  2026-05-04 against the live `ConductionNL/projects/4` board
  (mid-sprint, day 6 of 14):
  - `report.md` — Markdown report posted nowhere automatically; for
    team to read alongside the Slack message.
  - `slack-payload.json` — exact Block Kit payload as posted.
  - `README.md` — frames the artefact as mid-sprint, demo, and lists
    what was deliberately skipped vs the kit spec.

### Done in this session (no code change, kept for audit)
- Smoke-tested `test-slack.yml` via `workflow_dispatch` against the
  retro Slack channel — webhook + Block Kit rendering verified.
- Authorised `gh` CLI with `read:project,project` scopes and pulled
  real Project 4 data (39 items in active Kanban sprint 1, 11 repos).
- Computed and posted V2 sprint-retro output to Slack including DORA
  tier labels (Lead time → Elite proxy, Deploy freq → Elite proxy)
  with explicit "proxy not strict DORA" caveats and a link to the
  DORA 2023 State of DevOps report.
- Headline insight surfaced: PR/release pipeline scores Elite on
  proxies, but issue cycle time is 10.3d median with 0 issues opened
  *and* closed in the same sprint — bottleneck likely upstream of
  PR (intake / scope / grooming).

### Notes for the team conversation
- The demo run was generated from this *personal* repo using the
  user's own `gh` token. Any further regular runs (cron) should
  move to `ConductionNL/team-ops` (or `.github`) with a GitHub App
  token at org level.
- The Slack incoming-webhook URL was visible during the session and
  should be rotated before the channel is used in production.

## 2026-05-04

### Added
- Sprint retro & review automation kit (delivered as `~/Downloads/road2.zip`,
  placed unmodified at the layout the kit's own README specifies):
  - `.github/workflows/sprint-retro.yml` — weekly Friday workflow that runs
    the analysis and posts to Slack.
  - `.github/prompts/sprint-retro.md` — Claude Code prompt that drives the
    analysis (renamed from the zip's `sprint-retro-prompt.md` to match the
    path documented in the kit README).
  - `README.md` — setup, secrets, audit-trail documentation. Replaces the
    original placeholder README.
- `.gitignore` covering secrets (`.env*`, `*.pem`, `*.key`), local
  Python/Node artefacts, and local workflow outputs (`tmp/`,
  `slack-payload.json`, `report.md`).
- `CHANGELOG.md` (this file).
- `.github/workflows/test-slack.yml` — `workflow_dispatch`-only smoke
  test that posts a hardcoded Block Kit message to
  `SLACK_WEBHOOK_RETRO`. Lets you validate the Slack side of the kit
  without needing Anthropic API access or GitHub project access. Not
  part of the original kit; added for staged testing on this personal
  repo.
- `.env.example` documenting the env vars needed for local prompt runs
  (`GH_TOKEN`, `ANTHROPIC_API_KEY`, `SLACK_WEBHOOK_URL`). Note: in CI
  these come from repo secrets, not from a committed file.

### Open review notes — discuss with team before promoting to ConductionNL org

These were spotted during a quick read of the kit. Nothing was changed in
the kit content itself; this list is for the team conversation.

1. **`fix:` overcounts in CFR and tech-debt heuristics.**
   The prompt's change-failure-rate (§4) and tech-debt-signals (§5) regex
   on `fix:`, but `fix:` is a Conventional Commits prefix that matches
   almost every PR/commit. CFR especially gets inflated.
   Options: limit to `revert` + `hotfix` only, or restrict `fix:` matches
   to commits/PRs landing ≤48h after a merge that touches the same files
   (the prompt mentions this constraint loosely for CFR but not for
   tech-debt).

2. **DST comment in `sprint-retro.yml` cron is reversed.**
   Comment claims `07:00 UTC = 09:00 Europe/Amsterdam (zomertijd 08:00 UTC)`.
   Actually: in zomertijd (CEST, UTC+2), `07:00 UTC = 09:00 Amsterdam`; in
   wintertijd (CET, UTC+1), `07:00 UTC = 08:00`. Comment also has a
   doubled phrase ("het in beide het in beide").

3. **No anti-username guard before the Slack POST.**
   The kit's design forbids author names / @-mentions in output, but no
   step grep-checks `slack-payload.json` for `@` before posting. A
   defensive grep step would make the policy enforceable rather than
   aspirational.

4. **Failure notification uses the same Slack webhook** as the success
   post. If the webhook itself breaks, both paths fail silently.
   Acceptable for V1; worth knowing.

### Not changed
The kit's content is intentionally left as-delivered so the team can
evaluate the original artefact. If they want any of the above fixed
before adoption, that becomes a separate, reviewable change.
