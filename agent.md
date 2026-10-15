# Agent Instructions

## Mission

Implementar os tickets aprovados respeitando o contrato do projeto, mantendo o sistema seguro, verificável e sem ampliar silenciosamente o escopo.

## Required reading order

1. `project-contract.md`
2. `status.md`
3. `tickets.md`
4. código e documentação atual do repositório

## Source-of-truth priority

Em caso de conflito:

1. decisão explícita mais recente do maintainer;
2. `project-contract.md`;
3. Agent Brief do ticket;
4. `status.md`;
5. implementação existente;
6. inferência do agent.

Se contrato e implementação divergirem, registrar a divergência em vez de escolher silenciosamente um lado.

## How to choose work

Escolher somente tickets:

- `ready-for-agent`; e
- sem blockers; ou
- cujos blockers estejam `done`.

No estado atual, a fronteira inicial inclui:

- T-001
- T-002
- T-003
- T-009
- T-011

## Before implementation

Para cada ticket:

- explorar o código atual;
- confirmar se o comportamento já existe;
- localizar contratos e testes relevantes;
- identificar riscos não previstos;
- confirmar blockers;
- não assumir que APIs públicas citadas como exemplo estão autorizadas para integração.

## Implementation principles

- implementar comportamento observável, não roteiro mecânico;
- preferir a menor mudança que satisfaça integralmente o contrato;
- não adicionar histórico “por segurança”;
- não introduzir funcionalidades fora do MVP;
- não expor segredos em browser/log;
- nunca interpretar falha externa como contador zero;
- preservar a única URL do overlay ao trocar configuração;
- manter um único personagem ativo;
- respeitar rate limits antes de latência;
- manter o overlay leve;
- servir sprites pela infraestrutura do sistema;
- permitir múltiplos sprites por Pokémon quando disponíveis;
- tratar MySQL como persistência e Redis como estado efêmero/coordenador;
- manter adapters isolados das regras centrais.

## Security rules

- senhas usando password hashing apropriado;
- segredos de integração criptografados em repouso;
- segredos de infraestrutura fora do repositório;
- token de overlay longo e aleatório;
- token de overlay concede somente leitura/renderização + heartbeat;
- logs sanitizados;
- rate limiting em autenticação, refresh, heartbeat e endpoints públicos relevantes;
- admin normal nunca revela segredo salvo.

## Integration rules

Uma integração real exige autorização/interesse do parceiro.

Cada adapter deve:

- declarar campos;
- classificar sensibilidade;
- mapear personagem/Pokémon/balls;
- normalizar respostas;
- distinguir erro de zero;
- declarar rate limit/backoff/timeout;
- ter fixtures de teste.

Não implementar Psoul, ou qualquer outro jogo, apenas por haver exemplo público de endpoint.

## Completion rules

Antes de marcar captura:

1. projeto precisa estar ativo;
2. ao menos um contador positivo precisa ter sido observado durante monitoramento;
3. reset completo válido deve ser observado;
4. segunda leitura válida deve confirmar;
5. falhas/timeout/formato inválido nunca contam como reset.

Casos negativos obrigatórios:

- `0 -> 0`;
- erro -> zero;
- queda parcial;
- uma leitura zerada;
- retorno após intervalo offline.

## Testing

Preferir teste do comportamento externo.

Obrigatório:

- testes de adapter com fixtures;
- testes da máquina de conclusão;
- testes de auth/sessão;
- testes de permissions do token;
- testes de rate limiting/coordenação;
- teste E2E do fluxo principal;
- validação em OBS Studio para o MVP.

Não declarar sucesso quando teste necessário não puder ser executado.

## Completion contract

Ticket só pode virar `done` quando:

- todos os critérios aplicáveis estão satisfeitos;
- testes necessários foram executados;
- limitações foram registradas;
- evidência de verificação existe.

## End-of-run update

Ao terminar:

- atualizar status dos tickets trabalhados;
- registrar evidências;
- registrar blockers;
- registrar novas decisões que exigem maintainer;
- atualizar `status.md`;
- recalcular execution frontier;
- não alterar decisão durável de `project-contract.md` sem justificativa explícita.
