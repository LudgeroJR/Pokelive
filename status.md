# Project Status

**Updated:** 2026-10-14  
**Phase:** specification

## Current objective

Transformar as decisões do discovery/grill em uma implementação MVP verificável com pelo menos um jogo parceiro autorizado.

## Completed

- Visão e problema definidos.
- Escopo e não escopo definidos.
- Modelo de Project definido.
- Modelo de personagens definido.
- Regra de um personagem ativo definida.
- Modelo de integração por adapter definido.
- Regra de polling e heartbeat definida.
- Rate limit por parceiro definido.
- Regra de detecção de conclusão definida.
- Comportamento offline definido.
- URL/token do overlay definido.
- Três famílias de layout definidas.
- Modelo de sprite/asset definido.
- Autenticação e retenção de dados definidas.
- Administração mínima definida.
- Landing page, parceiros e doações definidos.
- Arquitetura inicial definida: monólito modular + MySQL + Redis.
- Contrato de testes definido.
- Definição de MVP pronto aceita.

## In progress

Nenhuma implementação foi verificada no repositório neste Web Chat.

## Execution frontier

Após criação do repositório/base do projeto, os tickets sem bloqueadores iniciais são:

- T-001 — Conta, autenticação e aceite legal.
- T-002 — Estrutura de parceiros, adapters e modo interno.
- T-003 — Biblioteca de assets/sprites.
- T-009 — Landing page, parceiros e doações.
- T-011 — Base operacional, observabilidade e backups.

T-004 pode iniciar assim que T-002 fornecer o contrato de integração necessário.

## Blocked

### Primeira integração real
**Motivo:** ainda não existe parceiro explicitamente autorizado nesta conversa.  
**Desbloqueio:** obter autorização/interesse de pelo menos um jogo e documentação suficiente da API.

### Validação com API real
**Motivo:** não foi executada nenhuma chamada real neste Web Chat.  
**Desbloqueio:** coding agent/maintainer configurar conta de teste e validar com parceiro autorizado.

### Licenciamento/origem dos sprites
**Motivo:** a biblioteca final de sprites ainda não foi inventariada.  
**Desbloqueio:** registrar origem/autorização de cada asset incorporado.

## Open decisions

Não há decisões de produto críticas silenciosamente abertas para iniciar o MVP.

Decisões de implementação que permanecem delegadas ao coding agent:

- framework frontend/backend;
- ORM;
- biblioteca de jobs/scheduler;
- SSE versus WebSocket;
- provedor de e-mail;
- provedor de infraestrutura;
- secret manager específico;
- resolução exata recomendada do canvas OBS;
- algoritmo/biblioteca concreta de password hashing.

Essas escolhas devem respeitar o `project-contract.md`.

## Pending verification

- Compatibilidade prática com versões atuais do OBS Studio.
- Capacidade do primeiro parceiro de sustentar o intervalo de polling desejado.
- Formato real da primeira API autorizada.
- Regras reais de normal/shiny do primeiro parceiro.
- Qualidade e formato dos sprites disponíveis.
- Restauração real de backup.
- Comportamento de concorrência com múltiplos workers.
- Meta de renderização do overlay em aproximadamente até 2 segundos.

## Recent decisions

- D-008 — MySQL é o banco persistente.
- D-011 — assets são hospedados pelo próprio sistema.
- D-012 — streamer escolhe entre sprites disponíveis.
- D-013 — áudio fora do MVP.
- D-014 — apenas um personagem ativo alimenta o overlay.
- D-016 — limite de 10 personagens salvos por conta, global entre parceiros.
- D-017 — Account Credential compartilhável entre Saved Characters do mesmo parceiro, quando o adapter suportar o modelo conta -> personagens.

## Next recommended step

1. Bootstrap do repositório (git init + skeleton dos módulos conceituais do contrato) — precondição para qualquer ticket `ready-for-agent`, não é um ticket em si.
2. Depois do bootstrap: executar T-001, T-002, T-003, T-009 e T-011 em paralelo quando possível, mantendo T-004 dependente do contrato de parceiros/adapters (agora incluindo o conceito de Account Credential).

## Handoff notes

- Não implementar Psoul ou outro jogo apenas porque uma API pública foi citada como exemplo.
- Uma integração real só entra após autorização/interesse de parceria.
- Nunca tratar resposta desconhecida como zero.
- Não adicionar histórico de projetos “por precaução”.
- Não misturar personagens diferentes no overlay ativo.
- Não introduzir som, nome customizado ou múltiplos overlays no MVP.
