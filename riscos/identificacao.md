---
tipo: Identificação de Riscos
projeto: DevNotes — Expansão para Plataforma Web
etapa: 1 — Identificação de Riscos
fonte: Prompt_gerenciamento_do_projeto.md
projeto_analisado: C:\Projetos_Pos\DevNotes
---

# Identificação de Riscos — Expansão do DevNotes para Web

## Contexto da análise

O DevNotes foi concebido e documentado como uma aplicação **local, individual e monolítica**: FastAPI + Jinja2 renderizando HTML no servidor, banco SQLite em arquivo único, sem autenticação, sem API JSON e sem infraestrutura de nuvem. Essas escolhas estão registradas como decisões arquiteturais deliberadas em `docs/arquitetura/visao-geral.md`, junto com uma lista explícita do que **não** deveria ser implementado (login, múltiplos usuários, SPA, nuvem, microsserviços).

As quatro demandas recebidas — modernização de interface, interface arrojada, suporte a 2.000 conexões simultâneas e armazenamento de documentos dos usuários em banco de dados — representam, em conjunto, uma ruptura com essas premissas originais. Os riscos abaixo decorrem principalmente dessa distância entre o MVP entregue e o novo objetivo.

---

## Riscos de Arquitetura e Tecnologia

| ID    | Risco                                                  | Descrição breve                                                                                                                | Contexto em que pode ocorrer                                                                                                                             |
| ----- | ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RI-01 | Arquitetura atual incompatível com o novo objetivo     | O sistema foi desenhado como app local monolítica de usuário único; não há camada de API, autenticação ou modelo multiusuário. | Ao iniciar o trabalho de suportar múltiplos usuários e acesso remoto simultâneo, presumindo que a base atual poderia ser apenas "estendida".             |
| RI-02 | SQLite incompatível com alta concorrência              | SQLite usa um modelo de bloqueio que serializa escritas; não é indicado para centenas ou milhares de conexões simultâneas.     | Ao tentar atingir a meta de 2.000 conexões simultâneas mantendo o banco atual sem migrar para um SGBD cliente-servidor (ex.: PostgreSQL).                |
| RI-03 | Ausência de API JSON para o novo frontend              | O README do projeto declara explicitamente que não há API REST pública; as rotas atuais servem HTML via Jinja2.                | Quando o desenvolvedor Angular tentar consumir dados do backend e constatar que é necessário criar uma API inteira do zero.                              |
| RI-04 | Servidor de aplicação em modo de desenvolvimento       | A execução documentada usa Uvicorn com `--reload`, sem múltiplos workers, load balancer ou configuração de produção.           | Ao expor a aplicação publicamente sem antes redesenhar o modo de execução para suportar carga real.                                                      |
| RI-05 | Armazenamento de arquivos em disco local não escalável | Uploads são salvos na pasta `uploads/` do próprio servidor; não há storage compartilhado ou externo.                           | Caso a aplicação precise rodar em múltiplas instâncias para atender 2.000 conexões simultâneas, cada instância teria uma visão diferente dos arquivos.   |
| RI-06 | Ausência de estratégia de migração de schema           | O projeto reconhece não ter estratégia de migração (ex.: Alembic); mudanças estruturais hoje exigem recriação manual do banco. | Ao alterar o modelo de dados para suportar múltiplos usuários e documentos, arriscando perda ou inconsistência dos dados já existentes em `devnotes.db`. |
| RI-07 | Dependência de CDN externo para recursos de interface  | O Highlight.js é carregado via CDN; a aplicação depende de conectividade externa mesmo depois de virar um serviço web.         | Em cenário de indisponibilidade do CDN ou de política de segurança que bloqueie recursos externos, afetando todos os usuários simultaneamente.           |

---

## Riscos de Segurança e Dados

| ID | Risco | Descrição breve | Contexto em que pode ocorrer |
|---|---|---|---|
| RI-08 | Ausência de autenticação e autorização | Não existe hoje conceito de usuário, login, sessão ou permissão de acesso. | Quando a demanda "usuários guardarem seus documentos em banco de dados" exigir isolar dados por usuário, e essa camada inteira ainda não existir. |
| RI-09 | Exposição de conteúdo sensível em ambiente compartilhado | A aplicação lida com regras de negócio e trechos de sistemas legados, hoje protegidos apenas por estarem em uma máquina local. | Ao publicar a aplicação na web sem controle de acesso, esse conteúdo passa a ficar potencialmente acessível a qualquer usuário autenticado ou não. |
| RI-10 | Upload de arquivos sem validação de segurança aprofundada | A validação atual cobre extensão e tamanho, mas não há verificação de conteúdo malicioso. | Em um ambiente multiusuário exposto na web, o risco de upload malicioso aumenta em relação ao uso local individual original. |

---

## Riscos de Escopo e Requisitos

| ID | Risco | Descrição breve | Contexto em que pode ocorrer |
|---|---|---|---|
| RI-11 | Demandas recebidas ainda não traduzidas em requisitos | "Interface arrojada", "fluidez e modernização" e "2.000 conexões simultâneas" são objetivos de alto nível, sem requisitos funcionais/não funcionais formalizados. | Ao iniciar o desenvolvimento sem antes detalhar critérios de aceitação, gerando retrabalho por interpretação divergente entre gerente, equipe e solicitante. |
| RI-12 | Ambiguidade sobre "armazenar documentos em banco de dados" | Não está claro se documentos devem ser guardados como BLOB no banco, se cada usuário terá espaço isolado, nem se há limites de quota. | Durante o desenho da modelagem de dados, caso a equipe assuma uma interpretação que depois seja rejeitada pelo solicitante. |
| RI-13 | Salto de escopo em relação ao propósito didático original | O projeto foi concebido com escopo de MVP de poucas horas para fins acadêmicos; o novo pedido aproxima-se de uma aplicação corporativa de produção. | Ao planejar prazos e esforço assumindo o ritmo de entrega do MVP anterior, subestimando a complexidade do novo objetivo. |
| RI-14 | Ausência de meta clara de não funcional para 2.000 conexões | Não há definição se são 2.000 usuários simultâneos ativos, conexões HTTP concorrentes, ou picos momentâneos — a métrica não está especificada. | Ao dimensionar infraestrutura e arquitetura sem uma definição precisa e testável do requisito de carga. |

---

## Riscos de Equipe e Recursos

| ID | Risco | Descrição breve | Contexto em que pode ocorrer |
|---|---|---|---|
| RI-15 | Descompasso entre stack atual e composição da equipe | O backend existente é em Python (FastAPI); a equipe tem um desenvolvedor .NET dedicado sem tecnologia correspondente no projeto atual, e um desenvolvedor Angular apenas part-time para construir um frontend inteiramente novo. | Ao definir quem assume a reescrita da camada de API e a construção do novo frontend, sem clareza de papéis e sem stack unificada. |
| RI-16 | Capacidade reduzida da equipe frente ao aumento de escopo | Apenas dois membros são dedicados em tempo integral (.NET e Python); Angular e testes são part-time, enquanto o escopo cresce de MVP local para plataforma web multiusuário de alta concorrência. | Ao planejar cronograma assumindo capacidade full-time da equipe inteira, gerando atrasos ou queda de qualidade. |
| RI-17 | Cobertura de testes insuficiente para os novos requisitos | Os 66 testes automatizados atuais validam apenas regras funcionais do MVP local; não há testes de carga, concorrência ou segurança multiusuário. | Ao validar a entrega da meta de 2.000 conexões simultâneas ou do isolamento de dados entre usuários sem testes específicos para esses cenários. |

---

## Riscos de Infraestrutura e Operação

| ID | Risco | Descrição breve | Contexto em que pode ocorrer |
|---|---|---|---|
| RI-18 | Ausência de infraestrutura de hospedagem, CI/CD e monitoramento | O projeto foi desenhado deliberadamente sem nuvem, Docker ou pipeline de deploy. | Ao migrar de execução local (`localhost:8000`) para uma plataforma web acessível externamente, toda essa infraestrutura precisa ser criada do zero. |
| RI-19 | Ausência de rotina de backup e recuperação | O projeto reconhece não ter backup automático nem exportação formal dos dados. | Ao passar a armazenar documentos de múltiplos usuários, a perda de dados por falha ou incidente afetaria muito mais pessoas do que no uso individual local. |

---

## Observação

O projeto já possui um registro de riscos técnicos do MVP original em `docs/riscos/R-registro-de-riscos.md` (DevNotes), focado nos riscos internos da versão local (encoding, upload, FTS5, etc.). Os riscos listados aqui são complementares e específicos ao novo objetivo de expansão para a plataforma web, não substituindo aquele registro.
