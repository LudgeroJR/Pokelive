# Domain Docs

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

## Before exploring, read these

- **`project-contract.md`** at the repo root — product spec, invariants, domain language and decision log. Acts as this repo's glossary until a standalone `CONTEXT.md` is created.
- **`CONTEXT.md`** at the repo root, if it exists, or
- **`CONTEXT-MAP.md`** at the repo root if it exists: it points at one `CONTEXT.md` per context. Read each one relevant to the topic.
- **`docs/adr/`**: read ADRs that touch the area you're about to work in. In multi-context repos, also check `src/<context>/docs/adr/` for context-scoped decisions.

If `CONTEXT.md`/`CONTEXT-MAP.md` don't exist yet, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

## File structure

Single-context repo (this repo):

```
/
├── project-contract.md   ← product spec, invariants, domain language, decision log
├── status.md              ← current phase, recent decisions, next steps
├── tickets.md              ← tickets and acceptance criteria
├── agent.md                ← coding agent instructions
├── docs/adr/                ← architectural decisions (created lazily)
└── src/
    ├── auth/
    ├── partners/
    ├── characters/
    ├── projects-overlay/
    ├── assets/
    ├── polling/
    ├── admin/
    ├── public-site/
    └── donations/
```

A standalone `CONTEXT.md` doesn't exist yet; `project-contract.md`'s "Domain language" and "Domain rules" sections serve that role until `/domain-modeling` creates one.

## Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `project-contract.md` → Domain language (e.g. Project, Active Character, Saved Character, Account Credential, Partner, Integration Adapter, Ball Counter, Displayed Ball, Detection Ball Set, Overlay, Overlay Token, Heartbeat, Completion, Partner Asset). Don't drift to synonyms the glossary explicitly avoids (see each term's `_Avoid_` note).

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

## Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-000X (...), but worth reopening because…_

If your output contradicts a decision in `project-contract.md`'s Decision log, surface it the same way instead of silently overriding.
