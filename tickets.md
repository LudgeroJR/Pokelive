# Tickets

## Visão geral

| ID | Título | Categoria | Estado | Bloqueado por |
|---|---|---|---|---|
| T-001 | Conta, autenticação e aceite legal | enhancement | ready-for-agent | — |
| T-002 | Estrutura de parceiros e adapters | enhancement | ready-for-agent | — |
| T-003 | Biblioteca de assets e seleção de sprite | enhancement | ready-for-agent | — |
| T-004 | Personagens salvos e personagem ativo | enhancement | blocked | T-002 |
| T-005 | Configuração de projetos e preview | enhancement | blocked | T-003, T-004 |
| T-006 | Overlay privado no OBS | enhancement | blocked | T-005 |
| T-007 | Polling coordenado, presença e rate limits | enhancement | blocked | T-002, T-004, T-006 |
| T-008 | Conclusão automática por reset confirmado | enhancement | blocked | T-007 |
| T-009 | Landing page, parceiros e doações PIX | enhancement | ready-for-agent | — |
| T-010 | Painel administrativo mínimo | enhancement | blocked | T-001, T-002 |
| T-011 | Segurança operacional, logs, métricas e backup | enhancement | ready-for-agent | — |
| T-012 | Exclusão de conta e retenção | enhancement | blocked | T-001, T-004, T-005 |
| T-013 | Fluxo E2E do MVP com parceiro autorizado | enhancement | blocked | T-001..T-012 |

---

## T-001 — Conta, autenticação e aceite legal

**Category:** enhancement  
**Status:** ready-for-agent  
**Blocked by:** None

### What this delivers

Um streamer consegue criar conta com e-mail, senha e nome/apelido, verificar o e-mail, autenticar-se, recuperar senha, gerenciar sessões e aceitar as versões atuais dos Termos e da Política de Privacidade.

### Context

Sem conta segura não é possível associar personagens, overlay, token e configurações.

### Agent Brief

**Current behavior**

Projeto ainda não possui fluxo verificado de autenticação.

**Desired behavior**

- cadastro por e-mail + senha + nome/apelido;
- verificação de e-mail;
- recuperação de senha;
- sessão persistente segura;
- logout;
- logout de todas as sessões;
- alteração de senha invalida sessões anteriores;
- checkbox obrigatório de aceite;
- armazenar versão/data do aceite;
- sem login social;
- sem 2FA.

**Key contracts / interfaces**

- senha é hash, nunca reversível;
- e-mail é identificador principal;
- não coletar telefone, CPF, endereço, nascimento ou pagamento;
- rate limiting em login/recuperação.

**Acceptance criteria**

- [ ] Conta não pode acessar dashboard antes da verificação de e-mail.
- [ ] Senha não é armazenada de forma reversível.
- [ ] Recuperação de senha usa token seguro e expira conforme política adequada.
- [ ] Troca de senha invalida sessões anteriores.
- [ ] Usuário consegue encerrar todas as sessões.
- [ ] Cadastro exige aceite de Termos e Privacidade.
- [ ] Versão/data do aceite ficam persistidas.
- [ ] Login e recuperação estão protegidos contra abuso básico.

**Out of scope**

- login social;
- 2FA;
- perfil público.

**Verification notes**

Validar comportamento externo via testes de autenticação e sessão.

---

## T-002 — Estrutura de parceiros e adapters

**Category:** enhancement  
**Status:** ready-for-agent  
**Blocked by:** None

### What this delivers

O sistema possui um contrato interno para integrações específicas de jogos, permitindo adicionar parceiros sem acoplar o domínio principal ao formato de uma API particular.

### Agent Brief

**Current behavior**

Nenhum adapter real está verificado.

**Desired behavior**

Criar modelo de parceiro e interface/contrato de adapter capaz de:

- declarar campos de configuração;
- classificar campos sensíveis;
- identificar personagens;
- mapear Pokémon/variantes;
- mapear balls;
- consultar contadores;
- normalizar erros;
- declarar rate limits e timeout;
- suportar estado INTERNAL/ACTIVE/MAINTENANCE/DISABLED;
- ser versionável.

**Key contracts / interfaces**

Resposta interna deve distinguir claramente:

- sucesso;
- autenticação inválida;
- not found;
- rate limited;
- timeout;
- formato inválido;
- falha temporária.

Nunca transformar falha em contadores zero.

**Acceptance criteria**

- [ ] Core do produto não depende de campos específicos de um jogo.
- [ ] Adapter de teste/fixture demonstra mapeamento de campos e balls.
- [ ] Campos sensíveis podem ser marcados e armazenados criptografados.
- [ ] Rate limits e política de polling são declarados pelo adapter.
- [ ] Partner pode ser INTERNAL sem aparecer para usuários.
- [ ] MAINTENANCE/DISABLED bloqueiam polling.
- [ ] Resposta inválida produz erro normalizado, não contadores zero.
- [ ] Existem testes de fixture para casos positivos e negativos definidos no contrato.

**Out of scope**

- implementar jogo sem autorização;
- construtor no-code de APIs.

**Verification notes**

O adapter do primeiro parceiro autorizado será outro trabalho ou uma extensão deste ticket após obter documentação real.

---

## T-003 — Biblioteca de assets e seleção de sprite

**Category:** enhancement  
**Status:** ready-for-agent  
**Blocked by:** None

### What this delivers

O sistema hospeda e serve uma biblioteca própria de sprites por Pokémon/alvo, e o streamer pode escolher entre as opções disponíveis.

### Agent Brief

**Desired behavior**

- cadastro/importação administrativa de assets;
- associação de múltiplos sprites a um Pokémon/variante/contexto;
- registro de origem/licença/autorização;
- preview;
- seleção pelo streamer;
- fallback para placeholder;
- suporte a imagens estáticas e animadas;
- runtime serve asset pela nossa infraestrutura.

**Key contracts / interfaces**

PokeAPI é fonte auxiliar possível, não dependência de runtime.

Parceiro não é obrigado a fornecer sprite.

**Acceptance criteria**

- [ ] Um Pokémon pode ter múltiplos sprites disponíveis.
- [ ] Streamer consegue escolher um deles ao configurar projeto.
- [ ] Overlay carrega asset da infraestrutura do sistema.
- [ ] Asset ausente/quebrado exibe placeholder sem interromper dados do card.
- [ ] Origem/autorização do asset pode ser registrada.
- [ ] Sistema suporta assets estáticos e animados sem exigir animação.

**Out of scope**

- upload livre pelo streamer;
- editor de imagens.

---

## T-004 — Personagens salvos e personagem ativo

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-002

### What this delivers

Streamer pode manter até 10 personagens salvos em diferentes parceiros, testar suas configurações e selecionar exatamente um como ativo.

### Agent Brief

**Desired behavior**

- adicionar personagem conforme formulário definido pelo adapter;
- reutilizar credenciais comuns quando o adapter suportar modelo conta -> personagens;
- testar conexão;
- mascarar segredos salvos;
- listar personagens por parceiro;
- ativar um personagem;
- manter exatamente um ativo;
- mesma string de nome em parceiros diferentes representa personagens distintos.

**Acceptance criteria**

- [ ] Conta suporta até 10 personagens salvos.
- [ ] Personagem pertence exatamente a um parceiro.
- [ ] Formulário muda conforme adapter.
- [ ] “Testar conexão” valida credencial, personagem e acesso aos dados necessários.
- [ ] Segredos não voltam integralmente ao navegador.
- [ ] Apenas um personagem pode estar ativo.
- [ ] Dois personagens homônimos em parceiros diferentes coexistem.
- [ ] Falha permanente de credencial exige ação do usuário e suspende polling.

**Out of scope**

- múltiplos personagens ativos.

---

## T-005 — Configuração de projetos e preview

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-003, T-004

### What this delivers

Streamer configura de 1 a 4 projetos para o personagem ativo, escolhe balls, sprite, ordem e layout, visualiza preview e aplica alterações em lote.

### Agent Brief

**Desired behavior**

- quatro slots máximos;
- projeto usa personagem ativo;
- selecionar alvo/variante conforme adapter;
- selecionar no mínimo 1 ball;
- selecionar sprite disponível;
- impedir duplicidade equivalente;
- reordenar;
- escolher Grade/Horizontal/Vertical;
- preview usa os mesmos componentes visuais do overlay;
- botão Aplicar altera o overlay de uma vez.

**Acceptance criteria**

- [ ] Não é possível salvar mais de 4 projetos.
- [ ] Não é possível salvar projeto sem ball.
- [ ] Catálogo respeita o parceiro ativo.
- [ ] Duplicidade equivalente é bloqueada.
- [ ] Ordem é persistida.
- [ ] Três famílias de layout estão disponíveis.
- [ ] Preview reflete configuração antes de aplicar.
- [ ] Projeto com 1 ball não exibe Total.
- [ ] Projeto com 2+ balls exibe Total das balls selecionadas.
- [ ] Remover projeto elimina seus dados atuais do produto.

**Out of scope**

- nome customizado;
- pause;
- histórico;
- layout livre.

---

## T-006 — Overlay privado no OBS

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-005

### What this delivers

Streamer recebe uma única URL privada de Browser Source que renderiza o overlay e recebe mudanças de configuração sem precisar trocar a URL.

### Agent Brief

**Desired behavior**

- token longo e aleatório;
- não expira automaticamente;
- somente leitura/renderização + heartbeat;
- regeneração invalida token anterior;
- fundo transparente;
- layout responsivo;
- atualização por push ou equivalente;
- reconexão automática;
- estado offline visualmente limpo;
- baixo uso de recursos.

**Acceptance criteria**

- [ ] URL funciona no OBS Studio suportado.
- [ ] Token não concede acesso ao dashboard.
- [ ] Token não permite mutações.
- [ ] Regenerar invalida a URL anterior imediatamente.
- [ ] Alterar projeto/layout no dashboard atualiza overlay sem mudar URL.
- [ ] Queda temporária reconecta automaticamente com backoff.
- [ ] Mudanças de contadores não recarregam a página nem causam flicker perceptível.
- [ ] Após receber o estado do backend, meta de render inicial é ~2s em condição normal.
- [ ] Asset ausente usa placeholder.
- [ ] Erro técnico não exibe stack trace.

**Out of scope**

- suporte oficial a Streamlabs/XSplit;
- áudio.

---

## T-007 — Polling coordenado, presença e rate limits

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-002, T-004, T-006

### What this delivers

O backend consulta parceiros somente quando há consumidor ativo, compartilha resultados entre dashboard/overlay e respeita limites globais e específicos sem duplicar trabalho.

### Agent Brief

**Desired behavior**

- heartbeat de dashboard/overlay;
- referência inicial: heartbeat ~30s, offline ~90s;
- polling somente com presença;
- scheduler centralizado;
- Redis para locks/presença/cache/rate limit;
- estratégia de consulta delegada ao adapter;
- limite global e por credencial quando necessário;
- backoff;
- manual refresh sem furar limites;
- distribuição justa quando capacidade do parceiro for insuficiente.

**Acceptance criteria**

- [ ] Sem dashboard e sem overlay presentes, não há polling regular.
- [ ] Abrir múltiplas abas/Browser Sources não multiplica chamadas equivalentes.
- [ ] Dois workers não executam simultaneamente a mesma unidade de consulta.
- [ ] Limite global do parceiro é respeitado.
- [ ] `Retry-After` é respeitado quando aplicável.
- [ ] Manual refresh não viola backoff/rate limit.
- [ ] Se capacidade ficar insuficiente, intervalos aumentam sem criar prioridade paga.
- [ ] Dados ativos permanecem efêmeros, não viram histórico no MySQL.

**Out of scope**

- polling configurável pelo streamer.

---

## T-008 — Conclusão automática por reset confirmado

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-007

### What this delivers

Projeto detecta captura automaticamente durante monitoramento confiável e exibe o resultado final com carimbo CONCLUÍDO, sem falsos positivos decorrentes de falhas externas.

### Agent Brief

**Desired behavior**

Máquina de estado:

`ACTIVE -> COMPLETION_PENDING -> COMPLETED`

Somente após:

- valor positivo previamente observado;
- reset completo válido;
- segunda leitura válida confirmando reset.

Ao concluir:

- persistir snapshot final atual;
- parar polling do projeto;
- manter posição;
- aplicar carimbo.

“Iniciar novamente” remove snapshot e volta a consultar estado atual.

**Acceptance criteria**

- [ ] `0 -> 0` não conclui.
- [ ] Erro -> zero não conclui.
- [ ] Redução parcial não conclui.
- [ ] Uma única leitura zerada não conclui.
- [ ] Reset confirmado conclui.
- [ ] Snapshot exibido é o resultado anterior ao reset.
- [ ] Concluído não continua polling.
- [ ] Apenas card afetado recebe CONCLUÍDO.
- [ ] Reiniciar remove estado concluído.
- [ ] Retomada após longo período offline não infere captura retroativa.

**Out of scope**

- captura detectada por Twitch/eventos externos;
- histórico de conclusões;
- som.

---

## T-009 — Landing page, parceiros e doações PIX

**Category:** enhancement  
**Status:** ready-for-agent  
**Blocked by:** None

### What this delivers

Visitante entende a proposta, vê jogos parceiros, pode cadastrar-se e encontra opção de apoio voluntário via PIX.

### Agent Brief

**Desired behavior**

Landing com:

- apresentação;
- demonstração;
- como funciona;
- parceiros;
- CTA;
- apoio;
- links legais.

Parceiros:

- nome;
- logo;
- texto;
- link;
- ordem neutra/alfabética.

Doações:

- QR Code;
- copia e cola;
- sem gateway;
- sem associação à conta;
- sem benefícios;
- sem valor arrecadado.

**Acceptance criteria**

- [ ] Parceiros ACTIVE aparecem publicamente.
- [ ] INTERNAL/MAINTENANCE/DISABLED não são apresentados como disponíveis para cadastro.
- [ ] Doação não altera estado/limites do usuário.
- [ ] Admin pode editar dados do PIX e ocultar a seção.
- [ ] Landing aponta para Termos e Privacidade.

---

## T-010 — Painel administrativo mínimo

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-001, T-002

### What this delivers

Operador consegue controlar usuários e integrações sem acesso a segredos em texto claro.

### Agent Brief

**Desired behavior**

Admin consegue:

- visualizar contas;
- bloquear/desbloquear;
- listar integrações;
- alternar estados;
- visualizar métricas/erros;
- gerenciar metadados públicos;
- gerenciar PIX;
- colocar parceiro em INTERNAL antes de publicar.

**Acceptance criteria**

- [ ] Admin comum não consegue revelar credenciais salvas.
- [ ] Bloquear conta impede autenticação/uso conforme política.
- [ ] Partner MAINTENANCE/DISABLED interrompe polling.
- [ ] Partner INTERNAL não aparece na seleção pública.
- [ ] Métricas não expõem histórico de balls.
- [ ] Operações administrativas relevantes são auditáveis sem registrar segredos.

---

## T-011 — Segurança operacional, logs, métricas e backup

**Category:** enhancement  
**Status:** ready-for-agent  
**Blocked by:** None

### What this delivers

Base operacional segura para produção: logging sanitizado, métricas, segredos protegidos, backup diário e restauração verificável.

### Agent Brief

**Desired behavior**

- segredos fora do código;
- logs sem PII/segredos desnecessários;
- retenção de logs operacionais 30 dias;
- métricas agregadas;
- backup diário do MySQL;
- teste periódico de restauração;
- Redis tratado como reconstruível.

**Acceptance criteria**

- [ ] Nenhum segredo aparece em logs de aplicação.
- [ ] URLs com query sensível são sanitizadas.
- [ ] Logs operacionais expiram em 30 dias conforme política.
- [ ] Métricas incluem volume/sucesso/erro/latência/rate limit/presença.
- [ ] Backup diário inclui dados persistentes definidos no contrato.
- [ ] Procedimento de restauração é testável.
- [ ] Perda do Redis não exige restauração de histórico de contadores.

---

## T-012 — Exclusão de conta e retenção

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-001, T-004, T-005

### What this delivers

Usuário consegue excluir a própria conta e remover dados aplicáveis sem deixar overlay/token utilizável.

### Agent Brief

**Desired behavior**

Após confirmação:

- invalidar sessões;
- invalidar token;
- remover personagens;
- remover credenciais;
- remover projetos;
- remover snapshot concluído;
- remover configuração;
- remover dados pessoais aplicáveis;
- manter somente retenções legalmente obrigatórias.

**Acceptance criteria**

- [ ] Token antigo deixa de renderizar overlay após exclusão.
- [ ] Usuário não consegue autenticar após exclusão.
- [ ] Dados de personagens e credenciais não permanecem acessíveis.
- [ ] Projetos/snapshots deixam de existir.
- [ ] Fluxo possui confirmação explícita.
- [ ] Política de backup considera expurgo conforme janela operacional/legal definida na implementação.

---

## T-013 — Fluxo E2E do MVP com parceiro autorizado

**Category:** enhancement  
**Status:** blocked  
**Blocked by:** T-001..T-012

### What this delivers

Demonstração verificável de que o produto completo atende à definição de MVP com pelo menos um jogo oficialmente autorizado.

### Agent Brief

**Desired behavior**

Executar fluxo ponta a ponta:

1. cadastrar/verificar conta;
2. configurar parceiro autorizado;
3. adicionar/testar personagem;
4. ativar personagem;
5. criar 1–4 projetos;
6. escolher balls/sprite/layout;
7. abrir URL no OBS;
8. observar polling;
9. atualizar contadores;
10. confirmar que dashboard e OBS compartilham dados;
11. simular/observar reset confirmado;
12. ver CONCLUÍDO;
13. trocar hunt sem trocar URL;
14. confirmar suspensão por ausência;
15. testar indisponibilidade/credencial inválida;
16. trocar personagem e verificar limpeza de projetos;
17. excluir conta;
18. verificar controles admin.

**Acceptance criteria**

- [ ] Os 21 pontos da definição de MVP no `project-contract.md` são demonstráveis.
- [ ] Não há falsos positivos de conclusão nos cenários negativos obrigatórios.
- [ ] Rate limits do parceiro são respeitados.
- [ ] Nenhum segredo é exposto no navegador ou logs.
- [ ] OBS Studio renderiza corretamente os três layouts.
- [ ] Resultado de testes, limitações e evidências são registrados antes de marcar `done`.

**Out of scope**

- alegar compatibilidade com parceiro não autorizado;
- adicionar novas features durante validação.

**Verification notes**

Este ticket não pode ser `done` sem evidência real de execução.
