# AGENTS.md

Instructions shared by any coding agent working in this repo (Claude, Codex, etc.).

## Required reading order

See `agent.md` for the full mission, source-of-truth priority, and implementation/security/testing rules. Read in this order:

1. `project-contract.md`
2. `status.md`
3. `tickets.md`
4. `agent.md`
5. code and current docs in the repo

## Agent skills

### Issue tracker

Issues and specs live as GitHub issues in `LudgeroJR/Pokelive`, via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context repo. `project-contract.md` is the glossary/spec of record until a standalone `CONTEXT.md` exists. See `docs/agents/domain.md`.
