---
tipo: Análise Qualitativa de Riscos
projeto: DevNotes — Expansão para Plataforma Web
etapa: 2 — Análise Qualitativa de Riscos
fonte: Prompt_gerenciamento_do_projeto.md
insumo: riscos/identificacao.md
---

# Análise Qualitativa de Riscos — Expansão do DevNotes para Web

## 1. Metodologia

A análise qualitativa combina duas dimensões para cada risco identificado em `riscos/identificacao.md`:

- **Probabilidade** — chance de o risco se concretizar ao longo do projeto, dado o contexto atual do DevNotes e as demandas recebidas.
- **Impacto** — efeito sobre prazo, custo, escopo ou qualidade, caso o risco se concretize.

### Escalas utilizadas

| Nível | Probabilidade                                                          | Impacto                                                                                  |
| ----- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Alta  | Tende a ocorrer com o caminho atual do projeto, salvo ação preventiva  | Compromete entrega, gera retrabalho relevante ou afeta múltiplos usuários/áreas          |
| Média | Pode ocorrer dependendo de decisões ainda não tomadas                  | Gera atraso, custo adicional ou degradação perceptível, mas contornável                    |
| Baixa | Depende de fatores externos ou pouco prováveis no contexto atual        | Efeito pontual, absorvível sem grande esforço de correção                                 |

### Matriz de classificação (Nível de Exposição)

| Probabilidade \ Impacto | Baixo      | Médio      | Alto       |
| ------------------------ | ---------- | ---------- | ---------- |
| **Alta**                  | Médio      | Alto       | **Crítico** |
| **Média**                 | Baixo      | Médio      | Alto       |
| **Baixa**                 | Muito Baixo| Baixo      | Médio      |

O **Nível de Exposição** resultante orienta a priorização das respostas a cada risco (tratada na próxima etapa do processo, fora do escopo deste documento).

---

## 2. Matriz consolidada de Probabilidade x Impacto

| ID    | Risco                                                    | Probabilidade | Impacto | Nível de Exposição |
| ----- | --------------------------------------------------------- | -------------- | ------- | ------------------- |
| RI-01 | Arquitetura atual incompatível com o novo objetivo        | Alta           | Alto    | **Crítico**          |
| RI-02 | SQLite incompatível com alta concorrência                 | Alta           | Alto    | **Crítico**          |
| RI-03 | Ausência de API JSON para o novo frontend                 | Alta           | Alto    | **Crítico**          |
| RI-04 | Servidor de aplicação em modo de desenvolvimento           | Média          | Alto    | Alto                 |
| RI-05 | Armazenamento de arquivos em disco local não escalável    | Média          | Médio   | Médio                |
| RI-06 | Ausência de estratégia de migração de schema               | Alta           | Médio   | Alto                 |
| RI-07 | Dependência de CDN externo para recursos de interface      | Baixa          | Baixo   | Muito Baixo          |
| RI-08 | Ausência de autenticação e autorização                     | Alta           | Alto    | **Crítico**          |
| RI-09 | Exposição de conteúdo sensível em ambiente compartilhado   | Média          | Alto    | Alto                 |
| RI-10 | Upload de arquivos sem validação de segurança aprofundada  | Média          | Alto    | Alto                 |
| RI-11 | Demandas recebidas ainda não traduzidas em requisitos      | Alta           | Alto    | **Crítico**          |
| RI-12 | Ambiguidade sobre "armazenar documentos em banco de dados" | Alta           | Médio   | Alto                 |
| RI-13 | Salto de escopo em relação ao propósito didático original  | Alta           | Alto    | **Crítico**          |
| RI-14 | Ausência de meta clara de não funcional para 2.000 conexões| Alta           | Alto    | **Crítico**          |
| RI-15 | Descompasso entre stack atual e composição da equipe       | Alta           | Alto    | **Crítico**          |
| RI-16 | Capacidade reduzida da equipe frente ao aumento de escopo   | Alta           | Alto    | **Crítico**          |
| RI-17 | Cobertura de testes insuficiente para os novos requisitos   | Alta           | Médio   | Alto                 |
| RI-18 | Ausência de infraestrutura de hospedagem, CI/CD e monitoramento | Alta       | Alto    | **Crítico**          |
| RI-19 | Ausência de rotina de backup e recuperação                  | Média          | Alto    | Alto                 |

**Resumo por nível:** 9 riscos críticos, 8 riscos altos, 1 risco médio, 1 risco muito baixo — perfil de risco elevado, coerente com a distância entre o MVP entregue e o novo objetivo de plataforma web multiusuário.

---

## 3. Análise detalhada por categoria

### 3.1 Riscos de Arquitetura e Tecnologia

**RI-01 — Arquitetura atual incompatível com o novo objetivo** (Crítico)
- *Impacto:* Afeta escopo, prazo e custo simultaneamente — se o time presumir que a base atual pode ser "estendida", parte do trabalho já iniciado pode precisar ser descartado ou redesenhado quando a incompatibilidade for percebida.
- *Fatores condicionantes:* Ausência de uma etapa formal de reavaliação arquitetural antes do início do desenvolvimento; pressão para "aproveitar" o que já existe por já ter sido entregue como MVP.

**RI-02 — SQLite incompatível com alta concorrência** (Crítico)
- *Impacto:* Compromete diretamente o requisito de 2.000 conexões simultâneas; pode gerar indisponibilidade ou lentidão severa em produção, além de retrabalho de migração de dados sob pressão.
- *Fatores condicionantes:* Decisão de migrar (ou não) para um SGBD cliente-servidor tomada cedo no projeto; volume real de escritas concorrentes, que agrava o gargalo de bloqueio do SQLite.

**RI-03 — Ausência de API JSON para o novo frontend** (Crítico)
- *Impacto:* Bloqueia o desenvolvedor Angular até que uma API completa seja construída; risco alto de subestimação de esforço, já que o projeto documenta explicitamente a ausência dessa camada.
- *Fatores condicionantes:* Momento em que a necessidade é constatada (quanto mais tarde, maior o impacto no cronograma); dependência do único desenvolvedor Python para desenhar e implementar essa camada.

**RI-04 — Servidor de aplicação em modo de desenvolvimento** (Alto)
- *Impacto:* Afeta estabilidade e segurança em produção — reload automático, ausência de múltiplos workers e de load balancer tendem a gerar quedas ou degradação sob carga real.
- *Fatores condicionantes:* Antecipação (ou não) da configuração de produção antes da exposição pública; falta de expertise em deploy de aplicações Python na equipe atual.

**RI-05 — Armazenamento de arquivos em disco local não escalável** (Médio)
- *Impacto:* Caso a aplicação precise rodar em múltiplas instâncias, cada uma teria uma visão diferente dos arquivos, gerando inconsistência para os usuários.
- *Fatores condicionantes:* Estratégia de escalabilidade escolhida (horizontal vs. vertical); se a meta de 2.000 conexões for atendida por uma única instância mais robusta, o risco se reduz.

**RI-06 — Ausência de estratégia de migração de schema** (Alto)
- *Impacto:* Alterações no modelo de dados para suportar múltiplos usuários e documentos podem causar perda ou inconsistência dos dados já existentes em `devnotes.db`.
- *Fatores condicionantes:* Volume de dados já existentes a preservar; adoção (ou não) de uma ferramenta de migração como Alembic antes de qualquer alteração estrutural.

**RI-07 — Dependência de CDN externo para recursos de interface** (Muito Baixo)
- *Impacto:* Indisponibilidade do CDN ou bloqueio por política de segurança afetaria a formatação/destaque de sintaxe para todos os usuários simultaneamente, mas sem comprometer funcionalidades essenciais.
- *Fatores condicionantes:* Ambiente de rede do usuário final (corporativo com políticas restritivas vs. acesso livre); estabilidade do provedor de CDN utilizado.

### 3.2 Riscos de Segurança e Dados

**RI-08 — Ausência de autenticação e autorização** (Crítico)
- *Impacto:* Sem essa camada, é impossível isolar dados por usuário — requisito central da nova demanda; atraso aqui bloqueia todas as funcionalidades multiusuário subsequentes.
- *Fatores condicionantes:* Definição (ainda pendente) do modelo de autenticação a adotar; prioridade dada a essa camada no início do desenvolvimento.

**RI-09 — Exposição de conteúdo sensível em ambiente compartilhado** (Alto)
- *Impacto:* Regras de negócio e trechos de sistemas legados hoje protegidos apenas por estarem em máquina local passam a ficar potencialmente acessíveis a terceiros, com implicações de confidencialidade e conformidade.
- *Fatores condicionantes:* Momento em que autenticação/autorização (RI-08) é implementada em relação à publicação da aplicação; natureza e sensibilidade real do conteúdo migrado por cada usuário.

**RI-10 — Upload de arquivos sem validação de segurança aprofundada** (Alto)
- *Impacto:* Em ambiente multiusuário exposto na web, um upload malicioso pode comprometer outros usuários ou a própria infraestrutura, diferente do risco controlado do uso local individual.
- *Fatores condicionantes:* Ausência atual de verificação de conteúdo (apenas extensão/tamanho são validados); superfície de ataque cresce proporcionalmente ao número de usuários simultâneos.

### 3.3 Riscos de Escopo e Requisitos

**RI-11 — Demandas recebidas ainda não traduzidas em requisitos** (Crítico)
- *Impacto:* Sem critérios de aceitação formalizados, há alto risco de retrabalho por interpretação divergente entre gerente, equipe e solicitante, afetando prazo e custo de forma transversal a todo o projeto.
- *Fatores condicionantes:* Disponibilidade do solicitante para validar requisitos antes do início do desenvolvimento; maturidade do processo de levantamento de requisitos da equipe.

**RI-12 — Ambiguidade sobre "armazenar documentos em banco de dados"** (Alto)
- *Impacto:* Uma modelagem de dados construída sobre premissa equivocada (ex.: BLOB vs. storage externo, quotas por usuário) pode exigir redesenho do banco já em uso multiusuário.
- *Fatores condicionantes:* Grau de detalhamento obtido junto ao solicitante antes da modelagem; decisão já tomada (ou não) sobre RI-05 (armazenamento de arquivos), que está diretamente relacionada.

**RI-13 — Salto de escopo em relação ao propósito didático original** (Crítico)
- *Impacto:* Planejar prazos e esforço no ritmo de um MVP acadêmico de poucas horas, para um objetivo próximo de aplicação corporativa de produção, tende a gerar subestimação sistemática de custo e prazo em todas as frentes.
- *Fatores condicionantes:* Percepção da liderança/solicitante sobre a real complexidade da mudança; comparação implícita com o esforço de entrega do MVP anterior.

**RI-14 — Ausência de meta clara de não funcional para 2.000 conexões** (Crítico)
- *Impacto:* Sem definição se são usuários simultâneos ativos, conexões HTTP concorrentes ou picos momentâneos, o dimensionamento de infraestrutura e arquitetura pode ser sub ou superdimensionado, com impacto direto em custo de infraestrutura e capacidade real de atendimento.
- *Fatores condicionantes:* Disposição do solicitante em detalhar a métrica; existência (ou não) de dados históricos de uso que ajudem a estimar a carga real esperada.

### 3.4 Riscos de Equipe e Recursos

**RI-15 — Descompasso entre stack atual e composição da equipe** (Crítico)
- *Impacto:* Sem clareza de papéis, a reescrita da API e a construção do frontend podem ficar sem dono efetivo, gerando atrasos e retrabalho de coordenação.
- *Fatores condicionantes:* Disposição do desenvolvedor .NET em atuar fora de sua stack principal (Python) ou definição de uma frente de trabalho compatível com seu perfil; grau de dedicação real do desenvolvedor Angular part-time frente ao volume de frontend a construir.

**RI-16 — Capacidade reduzida da equipe frente ao aumento de escopo** (Crítico)
- *Impacto:* Cronograma planejado com premissa de capacidade full-time da equipe inteira tende a atrasar ou sacrificar qualidade, já que apenas dois membros são dedicados em tempo integral.
- *Fatores condicionantes:* Necessidade (ou não) de contratação/realocação adicional; priorização das frentes de trabalho part-time (Angular e testes) frente às demais entregas.

**RI-17 — Cobertura de testes insuficiente para os novos requisitos** (Alto)
- *Impacto:* A validação da meta de 2.000 conexões simultâneas e do isolamento de dados entre usuários fica comprometida sem testes de carga, concorrência e segurança multiusuário, elevando o risco de defeitos em produção.
- *Fatores condicionantes:* Regime part-time do tester único da equipe; necessidade de novas ferramentas/competências (testes de carga) ainda não presentes no processo atual.

### 3.5 Riscos de Infraestrutura e Operação

**RI-18 — Ausência de infraestrutura de hospedagem, CI/CD e monitoramento** (Crítico)
- *Impacto:* Toda a infraestrutura de deploy, integração contínua e observabilidade precisa ser criada do zero antes de qualquer exposição pública, afetando diretamente o prazo de entrega.
- *Fatores condicionantes:* Ausência de decisão sobre provedor de nuvem e ferramentas de CI/CD; falta de experiência prévia da equipe nesse tipo de infraestrutura, dado o histórico local do projeto.

**RI-19 — Ausência de rotina de backup e recuperação** (Alto)
- *Impacto:* Ao passar a armazenar documentos de múltiplos usuários, uma falha ou incidente sem rotina de backup afetaria muito mais pessoas do que no uso individual local, com potencial perda de dados irreversível.
- *Fatores condicionantes:* Momento em que a rotina de backup é implementada em relação à entrada em produção; volume e criticidade dos dados de cada usuário armazenados.

---

## 4. Priorização — riscos que exigem atenção imediata

Os riscos classificados como **Crítico** concentram-se em três frentes que, se não tratadas cedo, comprometem o projeto como um todo:

1. **Definição e alinhamento de requisitos** (RI-11, RI-13, RI-14, RI-12) — antes de qualquer código, formalizar critérios de aceitação e métricas mensuráveis com o solicitante.
2. **Redesenho arquitetural** (RI-01, RI-02, RI-03, RI-18) — decidir a nova arquitetura (API, banco, hospedagem) antes de estender a base atual.
3. **Segurança multiusuário** (RI-08) e **capacidade da equipe** (RI-15, RI-16) — sem autenticação/autorização e sem clareza de papéis e dedicação, as demais entregas ficam bloqueadas ou mal estimadas.

Os riscos **Altos** (RI-04, RI-06, RI-09, RI-10, RI-17, RI-19) devem ser monitorados e endereçados no planejamento detalhado de cada frente, mas não bloqueiam o início do projeto da mesma forma que os críticos.

O risco **Muito Baixo** (RI-07) pode ser aceito ou tratado com uma ação simples (ex.: hospedar o recurso localmente), sem necessidade de planejamento aprofundado.

---

## Observação

Esta análise qualitativa parte da lista de riscos registrada em `riscos/identificacao.md` (Etapa 1) e não substitui uma análise quantitativa (estimativas de custo/prazo por risco) nem o plano de respostas aos riscos, que correspondem a etapas subsequentes do processo de gerenciamento de riscos.
