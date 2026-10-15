# Project Contract — Ferramenta de Projetos para PokéTibia

## Vision

Construir uma ferramenta gratuita para streamers de PokéTibia exibirem no OBS, por meio de uma única Browser Source, o andamento de seus projetos de captura sem editar textos manualmente durante a live.

O streamer configura sua conta, escolhe um jogo parceiro, seleciona um personagem ativo, monta de 1 a 4 projetos e define quais Pokébolas quer exibir. O backend consulta a API oficial/parceira do jogo, normaliza os dados e atualiza o overlay automaticamente.

O serviço deve priorizar segurança, respeito aos limites das APIs parceiras, confiabilidade dos dados, disponibilidade e só então velocidade de atualização e efeitos visuais.

## Problem

Hoje streamers costumam exibir seus projetos de captura no OBS usando fontes editadas manualmente. Isso exige alterar nome, imagem e contadores manualmente durante a live.

A ferramenta elimina esse trabalho manual ao:

- receber os dados diretamente de APIs fornecidas por jogos parceiros;
- oferecer uma única URL privada de Browser Source;
- renderizar até quatro projetos simultaneamente;
- detectar automaticamente a conclusão de um projeto quando os contadores do jogo são resetados;
- adaptar a experiência às particularidades de cada parceiro.

## Users / Actors

### Streamer
Usuário principal. Cria conta, configura personagens, escolhe o personagem ativo, monta o overlay e usa a URL no OBS.

### Viewer
Pessoa que assiste à live. Não interage com o sistema; apenas vê o overlay renderizado pelo OBS.

### Administrator
Opera o serviço, acompanha integrações, usuários, saúde do sistema e configurações públicas.

### Partner Game
Jogo de PokéTibia que autoriza a integração e fornece uma API capaz de retornar contadores de Pokébolas por personagem/Pokémon.

### Integration Adapter
Componente específico de cada parceiro que traduz o contrato particular da API do jogo para o modelo interno do sistema.

## Scope

O MVP inclui:

- cadastro com e-mail, senha e nome/apelido;
- verificação de e-mail;
- recuperação de senha;
- uma conta gratuita por usuário;
- múltiplos personagens salvos;
- exatamente um personagem ativo por vez;
- um único overlay por conta;
- uma única URL de Browser Source por conta;
- token longo, não expira automaticamente e pode ser regenerado;
- até 4 projetos simultâneos;
- projetos vinculados ao personagem ativo;
- seleção de Pokémon/alvo conforme regras do parceiro;
- seleção de variante conforme regras do parceiro;
- seleção de 1 ou mais Pokébolas válidas para o parceiro;
- escolha de sprite entre as opções disponíveis para o Pokémon;
- três famílias de layout: grade, horizontal e vertical;
- preview no dashboard;
- reordenação de projetos;
- aplicação das alterações de configuração em lote;
- atualização automática do overlay sem trocar a URL;
- integração backend-to-backend com APIs dos parceiros;
- teste de conexão da integração;
- polling condicionado à presença do dashboard ou da Browser Source;
- rate limiting e backoff por parceiro;
- cache/estado efêmero;
- detecção automática de conclusão;
- carimbo CONCLUÍDO apenas no card concluído;
- administração de usuários e parceiros;
- landing page com parceiros;
- doações voluntárias via PIX;
- Termos de Uso e Política de Privacidade;
- exclusão de conta;
- backups;
- observabilidade;
- testes automatizados e fluxo ponta a ponta.

## Non-goals

Fora do MVP:

- alertas sonoros;
- nomes personalizados de projeto;
- histórico de projetos;
- histórico de capturas;
- histórico permanente de contadores;
- projetos pausados;
- múltiplos overlays por conta;
- múltiplos personagens ativos simultaneamente;
- login social;
- 2FA;
- upload livre de assets por streamer;
- editor livre de layout;
- analytics para o streamer;
- recompensas por doação;
- perfis públicos;
- integração com Twitch;
- integração com YouTube;
- jogos sem parceria/autorização;
- aplicativo mobile nativo;
- suporte oficial a software de streaming diferente de OBS Studio;
- construtor no-code de integrações;
- plano premium ou qualquer vantagem paga;
- mensagens administrativas customizadas por parceiro.

## Product invariants

1. O serviço é gratuito para streamers.
2. Doações não desbloqueiam recursos, prioridade, limites maiores ou suporte diferenciado.
3. Existe somente um overlay por conta.
4. Cada conta possui uma única URL de overlay ativa por vez.
5. A URL do overlay é de leitura/renderização e heartbeat; nunca permite alterar configuração.
6. Até quatro projetos podem estar configurados simultaneamente.
7. Projeto presente no overlay significa projeto em andamento ou concluído; não existe estado pausado.
8. Remover um projeto remove sua configuração e seus dados atuais do sistema.
9. Não existe histórico de projeto/captura no MVP.
10. Apenas um personagem pode estar ativo por vez.
11. Trocar o personagem ativo remove todos os projetos atuais.
12. Trocar de parceiro também remove todos os projetos atuais e exige nova configuração compatível.
13. Personagens salvos permanecem associados ao parceiro a que pertencem.
14. Dois personagens com o mesmo nome em parceiros diferentes são entidades distintas.
15. O backend é o único componente autorizado a chamar a API do jogo parceiro.
16. Dashboard e overlay nunca consultam diretamente a API do parceiro.
17. Abrir múltiplas abas/fontes não multiplica proporcionalmente as consultas ao parceiro.
18. Nunca interpretar erro, timeout, resposta inválida ou ausência de dados como contador zero.
19. Frequência de polling é controlada pelo sistema e pela política do parceiro, nunca pelo streamer.
20. O sistema nunca viola deliberadamente um rate limit de parceiro.
21. Se a capacidade do parceiro for insuficiente, o intervalo de atualização aumenta de forma justa entre usuários.
22. O projeto concluído permanece no mesmo espaço do overlay até ser reiniciado ou removido.
23. O carimbo CONCLUÍDO aparece apenas no projeto concluído.
24. Não existe alerta sonoro.
25. O overlay deve continuar usando a mesma URL após troca de projetos, layout, parceiro ou personagem.
26. Credenciais de jogos e outros segredos nunca aparecem no overlay nem são retornados integralmente ao frontend após salvos.
27. Nenhum segredo pode ser escrito em logs.
28. Imagens/sprites usados no runtime são servidos pela infraestrutura do próprio sistema.
29. A API/PokeAPI pode ser usada como fonte auxiliar de dados/assets, mas não deve ser dependência obrigatória do runtime do overlay.
30. Falha de sprite nunca interrompe o monitoramento; usa-se placeholder.
31. Somente parceiros autorizados são integrados.
32. Cada parceiro pode ter regras próprias de identidade, parâmetros, Pokémon, variantes, balls e rate limit.
33. Uma nova integração exige implementação e release do sistema.
34. Uma conta pode ter no máximo 10 personagens salvos no total, somando todos os parceiros.
35. Quando um adapter define identidade a nível de conta (Account Credential), remover um Saved Character não remove a credencial compartilhada enquanto outro Saved Character da mesma conta ainda depender dela.

## Domain language

### Project
Objetivo atual de captura exibido no overlay, por exemplo “Shiny Charizard”. Representa um alvo comunitariamente entendido como “projeto”.

Um Project contém:

- personagem ativo;
- parceiro;
- alvo Pokémon/variante;
- 1 ou mais Pokébolas selecionadas para exibição;
- sprite escolhido;
- posição/ordem no overlay;
- estado atual.

_Avoid:_ histórico, campanha persistente, tentativa individual.

### Active Character
Único personagem atualmente selecionado para alimentar os projetos do overlay.

### Saved Character
Personagem cadastrado na conta, pertencente a um jogo parceiro. Pode existir sem estar ativo.

### Account Credential
Credencial de nível conta (ex. e-mail/login do jogo) declarada por um adapter que suporta o modelo conta -> personagens. Pode ser reutilizada por múltiplos Saved Characters do mesmo parceiro dentro da mesma conta, evitando recadastro da mesma credencial por personagem.

_Avoid:_ confundir com a identidade do personagem em si; a credencial autentica a conta do jogo, o Saved Character identifica o alvo dentro dela.

### Partner
Jogo autorizado a integrar com o sistema.

### Integration Adapter
Implementação específica de um parceiro. Conhece parâmetros, autenticação, formato de resposta, catálogo/mapeamento, rate limits e comportamento de erro daquele jogo.

### Ball Counter
Quantidade acumulada retornada pelo parceiro para uma Pokébola específica e um alvo Pokémon/personagem.

### Displayed Ball
Pokébola selecionada pelo streamer para aparecer no card do projeto.

### Detection Ball Set
Conjunto completo de contadores de Pokébolas retornados/considerados pela integração para detectar reset do alvo. Não se limita às balls exibidas.

### Total
Soma apenas das Pokébolas selecionadas para exibição no projeto.

### Overlay
Página web transparente destinada a uma Browser Source do OBS. Existe uma por conta.

### Overlay Token
Token longo e privado que identifica o overlay. Não expira automaticamente. Pode ser revogado/regenerado.

### Heartbeat
Sinal periódico de presença enviado pelo dashboard ou overlay para indicar que há um consumidor ativo e que o polling pode ser necessário.

### Completion
Transição automática de um Project de ativo para concluído após reset confirmado dos contadores segundo as regras desta especificação.

### Partner Asset
Sprite/imagem disponível para um Pokémon em determinada integração. Pode ter origem em PokeAPI, parceiro ou outra fonte autorizada, mas é servido pelo nosso sistema em runtime.

## Domain rules

### Project lifecycle

Estados funcionais:

- `EMPTY` — slot sem projeto;
- `ACTIVE` — projeto configurado e monitorado;
- `COMPLETION_PENDING` — reset candidato observado, aguardando confirmação;
- `COMPLETED` — reset confirmado e snapshot final congelado.

Não existe `PAUSED`.

Transições:

- `EMPTY -> ACTIVE`: streamer configura e aplica um projeto;
- `ACTIVE -> COMPLETION_PENDING`: leitura válida indica reset completo após já ter havido contador positivo observado;
- `COMPLETION_PENDING -> COMPLETED`: segunda leitura válida confirma o reset;
- `COMPLETION_PENDING -> ACTIVE`: leitura seguinte não confirma o reset;
- `COMPLETED -> ACTIVE`: streamer usa “Iniciar novamente” e a integração é consultada;
- `ACTIVE|COMPLETION_PENDING|COMPLETED -> EMPTY`: streamer remove/substitui o projeto.

### Initial counter

Ao criar um projeto, os valores atuais retornados pela API são usados diretamente.

Não existe baseline próprio do sistema. O projeto não começa artificialmente em zero.

Exemplo:

- API retorna Ultra Ball = 1250;
- projeto recém-criado exibe 1250.

### Ball selection

- mínimo de 1 Pokébola selecionada;
- balls válidas são definidas pelo parceiro;
- uma ball não selecionada pode participar da detecção de reset;
- apenas balls selecionadas aparecem no overlay;
- quando existe apenas uma ball selecionada, não mostrar linha redundante de total;
- quando existem 2 ou mais balls selecionadas, mostrar cada quantidade e o `Total`.

### Completion detection

Conclusão automática só pode ocorrer se:

1. o projeto estiver `ACTIVE`;
2. o sistema já tiver observado pelo menos um contador positivo no conjunto de detecção durante o período monitorado;
3. uma leitura válida indicar reset completo segundo a integração;
4. a leitura não for erro, timeout, resposta parcial ou formato inválido;
5. uma segunda leitura válida, respeitando rate limit/backoff, confirmar o reset.

Por padrão, quando a integração expõe todos os contadores de balls para o Pokémon, o reset completo significa que todas as balls relevantes estão zeradas.

Uma diminuição parcial, por exemplo `1500 -> 1300`, não conclui o projeto.

`0 -> 0` nunca conclui um projeto.

Após conclusão:

- congelar o snapshot anterior ao reset necessário para exibir o resultado final;
- parar polling daquele projeto;
- manter o card no mesmo lugar;
- aplicar carimbo `CONCLUÍDO`.

### Offline gap

Se o polling esteve suspenso e, ao retornar, a contagem atual for menor que a última observada, não inferir retroativamente uma captura.

Exemplo:

- último valor observado: 800;
- período sem monitoramento;
- novo valor: 20.

O sistema adota o estado atual retornado pelo parceiro e continua o monitoramento. A garantia de detecção automática vale durante períodos efetivamente monitorados.

### API offline

Quando a API parceira estiver indisponível:

- não exibir valor antigo como se estivesse atualizado;
- não substituir por zero;
- overlay informa indisponibilidade de forma discreta;
- dashboard informa indisponibilidade;
- dashboard oferece “Tentar atualizar”;
- atualização manual continua sujeita a rate limit e backoff.

### Invalid partner credentials

Quando o parceiro responder inequivocamente que a credencial/configuração é inválida:

- suspender polling daquela integração/personagem;
- overlay indica indisponibilidade;
- dashboard solicita correção;
- “Testar conexão” valida novamente;
- polling retorna apenas após validação bem-sucedida.

### Character switching

Ao trocar o personagem ativo:

- exigir confirmação;
- remover os 1–4 projetos atuais;
- manter layout;
- manter URL do overlay;
- manter personagens salvos;
- novo personagem passa a ser o único elegível para novos projetos.

### Partner switching

Ao trocar de parceiro:

- exigir confirmação;
- remover projetos atuais;
- limpar personagem ativo;
- manter personagens salvos do parceiro anterior;
- manter URL do overlay;
- exigir seleção/adicionamento de personagem do novo parceiro antes de novos projetos.

## Partner integration contract

Cada integração é específica.

O parceiro não é obrigado a usar um formato universal, mas o adapter deve conseguir normalizar o suficiente para o produto.

### Minimum functional information

A integração deve permitir obter, de forma inequívoca:

- identidade do personagem consultado;
- alvo Pokémon ou identificador equivalente;
- nome/identificador de cada Pokébola relevante;
- quantidade acumulada de cada Pokébola;
- sucesso/erro da consulta.

### Partner-provided integration information

Antes da implementação devem ser conhecidos:

- autorização de uso;
- endpoints;
- parâmetros necessários;
- autenticação;
- exemplos de resposta;
- identificação do personagem;
- identificação do Pokémon;
- regras de normal/shiny/variantes;
- mapeamento de balls;
- rate limits;
- comportamento de erros;
- contato técnico quando disponível.

### Character identity

Cada parceiro define os campos exigidos.

Exemplo de uma API possível pode exigir e-mail da conta do jogo, account name, nome do character e ID do Pokémon. Esse exemplo não cria um contrato universal.

### Account-level credential (shared across characters)

Um adapter pode declarar que parte da identidade é de nível conta (Account Credential) em vez de nível personagem — por exemplo, e-mail/login do jogo compartilhado por vários characters da mesma conta de jogo.

Quando o adapter suporta esse modelo:

- streamer cadastra a Account Credential uma vez;
- cada Saved Character adicionado depois referencia a credencial existente em vez de recadastrá-la;
- “Testar conexão” sempre valida o par credencial + personagem + acesso aos dados, mesmo quando a credencial é compartilhada;
- remover um Saved Character não remove a Account Credential enquanto outro Saved Character da mesma conta ainda a referenciar;
- a Account Credential segue as mesmas regras de campo sensível (criptografada em repouso, mascarada, nunca retornada integralmente, nunca em log).

Adapters que não suportam esse modelo tratam toda a identidade como pertencente ao personagem individualmente, sem conceito de credencial compartilhada.

### Pokémon identity

O identificador pode ser:

- número de Pokédex;
- ID interno do jogo;
- slug;
- combinação específica;
- outro identificador definido pelo parceiro.

O adapter resolve o mapeamento.

### Normal / shiny / variants

Não existe regra global.

Um parceiro pode:

- usar o mesmo ID/contador para normal e shiny;
- usar IDs/contadores distintos;
- suportar outras variantes.

A UI deve explicar a convenção do parceiro quando necessário.

### Partner balls

Cada parceiro define:

- identificador;
- nome exibido;
- mapeamento de resposta;
- disponibilidade para seleção;
- ícone, quando disponível.

## Sprite / asset model

### Source

O runtime usa assets hospedados pelo próprio sistema.

Um asset pode ser obtido de:

- PokeAPI;
- parceiro;
- outra fonte com uso autorizado;
- biblioteca mantida pelo projeto.

Fornecer assets não é obrigação do parceiro.

### Selection

Quando o streamer escolhe um Pokémon, o sistema apresenta as opções de sprite disponíveis para aquele alvo.

Exemplo:

- Charizard Sprite A;
- Charizard Sprite B;
- Charizard Shiny 3D;
- Charizard estático.

O streamer escolhe entre as opções disponíveis.

### Fallback

Se um asset falhar:

- usar placeholder;
- manter nome, balls e contadores;
- não interromper o projeto.

### Animation

Animação não é requisito.

A biblioteca pode conter imagens estáticas ou animadas.

### Rights

Assets incorporados ao serviço devem ter origem/uso autorizado. Caso um parceiro forneça asset, a autorização para hospedagem/exibição deve ser confirmada.

## Overlay behavior

### URL

- uma URL por conta;
- token longo, aleatório e privado;
- sem expiração automática;
- regenerável pelo streamer;
- regeneração invalida imediatamente o token anterior.

### Permissions

O token permite apenas:

- carregar/renderizar overlay;
- receber atualizações;
- enviar heartbeat técnico.

Não permite:

- alterar projeto;
- trocar personagem;
- iniciar novamente;
- testar integração;
- ler credenciais;
- alterar layout;
- acessar dashboard.

### Layouts

Famílias obrigatórias:

1. Grade
2. Horizontal
3. Vertical

O layout se adapta a 1–4 projetos e reorganiza espaços vazios de forma apropriada.

Um projeto concluído não causa redistribuição dos demais cards.

### Card contents

Card ativo contém:

- sprite;
- nome/variante;
- cada ball selecionada com quantidade;
- total quando houver 2+ balls.

Card concluído mantém as informações finais e recebe carimbo `CONCLUÍDO`.

### Project order

Streamer pode reordenar projetos no dashboard. Overlay respeita a ordem configurada.

### Apply changes

O dashboard permite preparar alterações e aplicá-las em conjunto.

Somente após salvar/aplicar a nova configuração o overlay recebe a mudança.

### Live update

Mudanças aplicadas devem chegar automaticamente ao overlay aberto sem alterar a URL e sem exigir reload manual no OBS.

Tecnologia de push pode ser SSE, WebSocket ou equivalente.

### Browser Source target

OBS Studio é a plataforma oficialmente suportada no MVP.

Outros softwares podem funcionar por compatibilidade web, mas não têm suporte oficial.

### Canvas

Usar uma resolução padrão recomendada para Browser Source e tornar os layouts responsivos dentro dela. A resolução exata pode ser definida na fase de UI sem alterar este contrato.

## Dashboard

Áreas:

- Overlay
- Personagens
- OBS
- Conta
- Apoie

### Overlay
- slots de 1–4 projetos;
- reordenação;
- configuração de Pokémon/variante;
- seleção de balls;
- seleção de sprite;
- layout;
- preview;
- aplicar alterações;
- estado de API/conexão.

### Personagens
- listar personagens salvos;
- adicionar;
- remover;
- testar conexão;
- ativar;
- indicar parceiro.

### OBS
- URL privada;
- botão copiar;
- aviso de segurança;
- regenerar URL;
- instruções de Browser Source.

### Conta
- nome/apelido;
- e-mail;
- alteração de senha;
- logout;
- logout de todas as sessões;
- exclusão de conta.

### Apoie
- QR Code PIX;
- código copia e cola;
- texto de contribuição voluntária.

## Public website

A landing page contém:

1. apresentação;
2. demonstração visual;
3. como funciona;
4. jogos parceiros;
5. CTA de cadastro;
6. apoio/doações;
7. Termos;
8. Privacidade.

### Partner showcase

Cada parceiro pode ter:

- nome;
- logo;
- pequeno texto;
- link oficial;
- indicação de parceiro.

Ordenação neutra, preferencialmente alfabética.

Sem ranking pago.

## Donations

MVP:

- PIX;
- QR Code;
- código copia e cola;
- sem gateway;
- sem vínculo entre doação e usuário;
- sem recompensa;
- sem saldo;
- sem exibir total arrecadado.

Mensagem recomendada:

“As contribuições são voluntárias e ajudam a custear infraestrutura, domínio, serviços e manutenção da ferramenta.”

Admin pode alterar QR Code, PIX, texto e visibilidade da seção.

## Authentication and account security

### Account fields

Obrigatórios:

- e-mail;
- senha;
- nome/apelido.

Não pedir no MVP:

- nome civil;
- telefone;
- endereço;
- CPF;
- data de nascimento;
- dados de pagamento.

### Authentication

- e-mail + senha;
- e-mail verificado;
- recuperação por e-mail;
- sessão persistente segura;
- logout;
- logout de todas as sessões;
- troca de senha invalida sessões anteriores.

Sem login social e sem 2FA no MVP.

### Password storage

Senha nunca é armazenada de forma reversível. Usar função de hashing apropriada para senhas.

### Sensitive integration fields

Cada adapter classifica seus campos.

Campos sensíveis:

- criptografados em repouso;
- mascarados após salvos;
- não retornados integralmente ao frontend;
- nunca inseridos no overlay;
- nunca registrados em logs.

## Data retention

### Persisted

Persistir apenas o necessário para operação atual:

- conta;
- e-mail/nome;
- autenticação;
- parceiros;
- personagens salvos;
- configuração sensível da integração;
- personagem ativo;
- configuração atual dos 1–4 projetos;
- layout;
- sprite escolhido;
- token do overlay;
- estado atual do projeto;
- snapshot final de projeto `COMPLETED`;
- configurações administrativas;
- versão/data de aceite de termos.

### Ephemeral

Preferencialmente em Redis/cache:

- heartbeat;
- presença;
- resultado atual de polling;
- locks;
- rate limits;
- estado temporário de scheduler;
- eventos de atualização.

### Not stored as product history

- sequência histórica de contadores;
- projetos removidos;
- capturas anteriores;
- snapshots de hunts anteriores.

### Completion snapshot

É permitido persistir o snapshot final enquanto o projeto permanece `COMPLETED`, pois ele faz parte do estado atual do overlay.

Ao reiniciar/remover, eliminar esse snapshot.

## Privacy

A Política de Privacidade deve explicar:

- dados coletados;
- finalidade;
- dados das integrações;
- armazenamento de segredos;
- logs;
- backups;
- retenção;
- exclusão;
- compartilhamento necessário com parceiro;
- consultas feitas às APIs dos jogos.

Dados internos que o parceiro não precisa receber não devem ser enviados ao parceiro.

## Terms of Use

Cobrir no mínimo:

- gratuidade;
- indisponibilidade possível;
- limites técnicos;
- proibição de abuso;
- responsabilidade por credenciais fornecidas;
- suspensão de conta abusiva;
- dependência de APIs externas;
- ausência de garantia de continuidade permanente.

Aceite obrigatório no cadastro.

Guardar versão/data do aceite.

## Account deletion

Ao excluir conta:

- invalidar autenticação;
- invalidar token do overlay;
- remover personagens;
- remover credenciais;
- remover projetos;
- remover configuração;
- remover dados pessoais aplicáveis;
- preservar somente dados cuja retenção seja legalmente obrigatória.

## Administration

Painel administrativo mínimo:

- visualizar contas;
- bloquear/desbloquear conta;
- visualizar status de integrações;
- habilitar/desabilitar parceiros;
- colocar integração em manutenção;
- visualizar métricas e erros;
- regenerar/revogar acessos excepcionais quando apropriado;
- gerenciar dados de doação;
- preparar integração em modo interno/não publicado.

Administrador comum não pode revelar segredos armazenados.

## Partner states

- `INTERNAL` — disponível apenas para validação administrativa;
- `ACTIVE` — selecionável pelos usuários;
- `MAINTENANCE` — polling suspenso;
- `DISABLED` — não utilizável.

Não é necessário texto administrativo customizado por parceiro no MVP.

## Polling and presence

### Presence

Manter polling apenas quando:

`overlay presente OR dashboard presente`

Heartbeat recomendado:

- aproximadamente a cada 30 segundos;
- considerar offline após aproximadamente 90 segundos sem heartbeat.

Valores são configuração interna, não opção do streamer.

### Scheduler

Não criar timer in-memory independente por streamer.

Usar modelo centralizado de trabalhos vencidos.

A unidade concreta de consulta é definida pelo adapter e pode ser:

- por Pokémon;
- por personagem;
- batch;
- outro formato eficiente.

### Distributed locking

Evitar consultas equivalentes simultâneas.

Locks/idempotência devem considerar a unidade de consulta real do adapter.

### Rate limiting

Adapter declara:

- intervalo mínimo;
- limite global;
- limite por credencial/personagem quando existir;
- backoff;
- timeout;
- tratamento de `Retry-After` quando aplicável.

### Manual refresh

- permitido no dashboard;
- nunca fura backoff;
- nunca viola limite global;
- pode receber prioridade apenas quando houver capacidade.

## Technical architecture

### Form

Monólito modular no MVP.

Módulos conceituais:

- auth/accounts;
- partners/integrations;
- characters;
- projects/overlay;
- assets;
- polling/scheduler;
- admin;
- public site;
- donations configuration.

Workers podem rodar como processos separados usando a mesma base.

### Database

MySQL.

Usar para dados persistentes.

### Ephemeral store

Redis.

Usar para:

- cache efêmero;
- locks;
- heartbeats;
- presença;
- rate limiting;
- coordenação de workers;
- eventos/estado temporário.

### Push updates

SSE, WebSocket ou tecnologia equivalente.

A escolha fica para implementação.

### Environments

Obrigatórios:

- desenvolvimento/local;
- produção.

Staging não é requisito de lançamento do MVP, mas arquitetura/configuração deve permitir adicioná-lo.

### Secrets

Segredos de infraestrutura ficam fora do código/repositório, em secret manager ou configuração protegida.

## Performance

### Overlay

Requisitos:

- fundo transparente;
- baixo uso de CPU;
- baixo uso de memória;
- assets otimizados;
- sem dependência contínua de PokeAPI;
- sem vídeos;
- sem efeitos pesados desnecessários;
- atualizações pontuais sem reload da página;
- sem flicker perceptível.

### Initial render target

Meta de qualidade:

- após receber o estado do backend, renderizar estado inicial em aproximadamente até 2 segundos em conexão normal.

Não constitui SLA externo.

### Reconnection

Overlay reconecta automaticamente com backoff em caso de queda temporária.

Pequenas falhas devem ter tolerância antes de exibir estado offline para evitar flicker.

## Browser support

Dashboard:

- Chrome moderno;
- Edge moderno;
- Firefox moderno;
- Safari moderno.

Overlay:

- OBS Studio / Browser Source das versões contemporâneas suportadas pelo projeto.

Sem Internet Explorer.

## Accessibility

Dashboard deve:

- usar labels;
- permitir navegação por teclado;
- ter contraste adequado;
- mostrar erros em texto;
- não depender só de cor;
- ter estados de loading claros.

Overlay não depende apenas de cor para conclusão; usa carimbo textual.

## Language

MVP em Português do Brasil.

Estruturar textos para futura internacionalização, sem implementar múltiplos idiomas agora.

## Logging

Pode registrar:

- ID interno;
- parceiro;
- operação lógica;
- status;
- duração;
- código de erro normalizado;
- alvo consultado quando não sensível.

Não registrar:

- senha;
- token;
- credencial;
- URL completa com query sensível;
- segredo em texto claro.

Retenção padrão de logs operacionais: 30 dias.

## Observability

Métricas agregadas:

- volume de consultas;
- sucesso;
- erro;
- latência;
- rate limit;
- usuários/overlays ativos;
- última consulta bem-sucedida por integração.

Não armazenar histórico de balls para produzir métricas.

Alertas automáticos sofisticados ficam fora do MVP, mas observabilidade deve permitir adicioná-los.

## Backups

Backup diário automático dos dados persistentes relevantes.

Incluir:

- usuários;
- personagens;
- integrações/configurações;
- projetos atuais;
- layout;
- snapshots concluídos atuais;
- configuração administrativa.

Não é necessário restaurar cache Redis.

Executar testes periódicos de restauração.

## Testing contract

### Partner adapter tests

Cada adapter deve cobrir fixtures para:

- resposta válida;
- zeros;
- várias balls;
- resposta inválida;
- credencial inválida;
- timeout;
- rate limit;
- campo ausente;
- mudança inesperada de formato.

Testes automatizados comuns não devem depender da API real.

### Completion tests

Cobertura obrigatória:

- `0 -> valor positivo -> reset -> confirmação -> COMPLETED`;
- `0 -> 0` não conclui;
- erro seguido de zero não conclui;
- diminuição parcial não conclui;
- uma leitura zerada sem confirmação não conclui;
- retomada após período offline não infere captura retroativa.

### End-to-end seam

Fluxo de maior valor:

cadastro -> personagem -> teste de integração simulada -> projeto -> overlay -> atualização -> conclusão.

## Quality contract

Prioridade:

1. segurança;
2. respeito às APIs;
3. confiabilidade;
4. disponibilidade;
5. velocidade;
6. efeitos visuais.

O sistema deve falhar fechado em casos ambíguos de captura.

Erros externos nunca podem produzir falsa conclusão.

## Fair-use contract

O serviço é gratuito, mas pode aplicar limites técnicos necessários para:

- segurança;
- estabilidade;
- custo;
- disponibilidade;
- respeito às APIs parceiras.

Limites não criam categoria premium.

## Decision log

### D-001 — Um único overlay por conta
**Status:** accepted

Cada conta possui uma única URL de Browser Source. Layout e projetos mudam sem mudar essa URL.

### D-002 — Adapter específico por parceiro
**Status:** accepted

Não existe contrato externo universal. Cada parceiro recebe implementação dedicada para garantir compatibilidade com suas particularidades.

### D-003 — Backend exclusivo para chamadas aos parceiros
**Status:** accepted

Browser Source e dashboard nunca chamam diretamente APIs dos jogos.

### D-004 — Polling dependente de presença
**Status:** accepted

Heartbeat de dashboard/overlay determina se há necessidade de consultar parceiros.

### D-005 — Sem histórico de projetos
**Status:** accepted

Projetos removidos não são preservados. Estado atual é suficiente para o MVP.

### D-006 — Detecção por reset confirmado
**Status:** accepted

Conclusão depende de reset observado e confirmado em duas leituras válidas após valor positivo previamente observado.

### D-007 — Personagem único ativo
**Status:** accepted

Vários personagens podem estar salvos, mas apenas um alimenta o overlay por vez.

### D-008 — MySQL como banco persistente
**Status:** accepted

Dados persistentes do MVP usarão MySQL.

### D-009 — Redis para estado efêmero e coordenação
**Status:** accepted

Heartbeats, locks, rate limits, cache e coordenação vivem fora do histórico persistente.

### D-010 — Monólito modular
**Status:** accepted

MVP evita microserviços. Workers podem ser processos separados.

### D-011 — Assets servidos pela própria infraestrutura
**Status:** accepted

PokeAPI ou parceiros podem ser origem de sprites, mas o overlay consome assets hospedados pelo sistema.

### D-012 — Streamer escolhe sprite disponível
**Status:** accepted

Ao configurar um Pokémon, streamer pode selecionar entre os sprites existentes para aquele alvo.

### D-013 — Sem áudio
**Status:** accepted

Alertas sonoros foram removidos do escopo para simplificar o produto.

### D-014 — Um único personagem por overlay
**Status:** accepted

Projetos não podem misturar personagens diferentes na mesma configuração ativa.

### D-015 — Gratuito sem vantagem por doação
**Status:** accepted

Doações voluntárias não influenciam recursos nem limites individuais.

### D-016 — Limite de 10 personagens salvos por conta
**Status:** accepted

Decisão deliberada do maintainer, não derivada de restrição técnica de parceiro. Limite é global por conta, somando todos os parceiros. Ao atingir o limite, adicionar novo personagem exige remover um existente; não há arquivamento.

### D-017 — Account Credential compartilhável entre personagens
**Status:** accepted

Adapters que seguem o modelo conta -> personagens podem declarar uma credencial de nível conta reutilizável por múltiplos Saved Characters do mesmo parceiro, evitando recadastro redundante. Ver "Account-level credential" em Partner integration contract.

## Out of scope and durable rejections

### OOS-001 — Alertas sonoros
**Decision:** não suportar no MVP.
**Reason:** simplificação deliberada do produto.
**Reconsider when:** houver demanda clara da comunidade.

### OOS-002 — Histórico
**Decision:** não armazenar histórico de projetos/capturas.
**Reason:** minimizar dados e complexidade.
**Reconsider when:** histórico se tornar uma necessidade comprovada.

### OOS-003 — Múltiplos overlays
**Decision:** não suportar.
**Reason:** uma única URL cobre o caso principal.
**Reconsider when:** streamers demonstrarem necessidade consistente de cenas independentes.

### OOS-004 — Jogos sem parceria
**Decision:** não integrar.
**Reason:** respeito a autorização, estabilidade e contratos de API.
**Reconsider when:** existir mecanismo oficial autorizado.

### OOS-005 — Editor livre de overlay
**Decision:** não suportar no MVP.
**Reason:** layouts pré-definidos reduzem complexidade e problemas de UX.
**Reconsider when:** presets forem insuficientes.

### OOS-006 — Login social e 2FA
**Decision:** adiado.
**Reason:** não necessários para validar o MVP.
**Reconsider when:** base de usuários ou risco justificar.

### OOS-007 — Integração Twitch/YouTube
**Decision:** não suportar no MVP.
**Reason:** presença do próprio overlay é sinal suficiente para polling.
**Reconsider when:** houver benefício de produto independente.

## MVP definition of done

O MVP está funcionalmente pronto quando, com pelo menos um parceiro autorizado, for possível demonstrar:

1. cadastro e verificação;
2. adição de personagem;
3. teste de conexão;
4. ativação de personagem;
5. configuração de 1–4 projetos;
6. seleção de alvo, balls e sprite;
7. seleção de layout;
8. uso da única URL no OBS;
9. sprites servidos pela própria infraestrutura;
10. polling respeitando rate limit;
11. atualização automática de contadores;
12. compartilhamento de cache entre dashboard/overlay;
13. detecção correta de conclusão;
14. carimbo somente no projeto concluído;
15. troca de hunt sem trocar URL;
16. suspensão de polling sem presença;
17. falha de API sem falso zero;
18. troca de personagem limpando projetos;
19. exclusão de conta;
20. controle administrativo mínimo;
21. testes de segurança, adapter, conclusão e E2E atendidos.
