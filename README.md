# PokéTibia Live Projects

Ferramenta gratuita para streamers de PokéTibia exibirem no OBS, via Browser Source, o andamento
dos seus projetos de captura.

## Leitura obrigatória, nesta ordem

1. [`project-contract.md`](./project-contract.md) — visão, escopo, invariantes, regras de domínio.
2. [`status.md`](./status.md) — fase atual, decisões recentes, próximos passos.
3. [`tickets.md`](./tickets.md) — tickets e critérios de aceite.
4. [`agent.md`](./agent.md) — instruções pro coding agent.
5. [`AGENTS.md`](./AGENTS.md) — issue tracker, triage labels, domain docs (compartilhado entre Claude e Codex).

## Estrutura

```
src/
├── auth/              # T-001
├── partners/           # T-002
├── characters/          # T-004
├── projects-overlay/    # T-005, T-006
├── assets/              # T-003
├── polling/             # T-007, T-008
├── admin/               # T-010
├── public-site/         # T-009
└── donations/           # T-009 (seção Apoie)
docs/adr/                # decisões arquiteturais (ver critérios no README da pasta)
```

Stack técnica (framework, ORM, scheduler, SSE/WS, secret manager, hashing) ainda não travada —
delegada ao coding agent em cada ticket, respeitando `project-contract.md`.

## Estado

Fase: especificação concluída, bootstrap de repositório feito, implementação não iniciada.
