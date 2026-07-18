---
tipo: Estratégias de Resposta a Riscos
projeto: DevNotes — Expansão para Plataforma Web
etapa: 3 — Definição de Estratégias de Resposta
fonte: Prompt_definicao_de_estrategias_de_resposta.md
insumo: riscos/analise.md
natureza: exploratório e qualitativo (não constitui escolha definitiva)
---

# Estratégias de Resposta a Riscos — Expansão do DevNotes para Web

> **Aviso de escopo.** Este documento tem caráter **exploratório e qualitativo**. Para cada risco relevante são apresentadas as quatro estratégias clássicas (evitar, mitigar, transferir, aceitar) com suas implicações, **sem eleger uma resposta definitiva**. A escolha final depende de validação com *stakeholders* e de informações ainda pendentes (ver Seção 3). São priorizados os riscos classificados como **Crítico** e **Alto** em `riscos/analise.md`; os de menor exposição são tratados de forma condensada ao final.

---

## 1. Estratégias Possíveis por Risco

### 3.1 Requisitos e Escopo (tratar primeiro — condicionam todos os demais)

---

**Risco:** RI-11 — Demandas recebidas ainda não traduzidas em requisitos *(Crítico)*

- **Descrição:** As quatro demandas iniciais (interface moderna, 2.000 conexões, armazenamento em banco) não possuem critérios de aceitação formalizados, abrindo espaço para interpretação divergente e retrabalho transversal.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Estabelecer um *gate* formal — nenhum desenvolvimento inicia antes de requisitos com critérios de aceitação aprovados pelo solicitante. Elimina o risco de construir sobre premissa errada, ao custo de adiar o início do projeto.
    2. **Mitigar:** Conduzir workshops curtos de levantamento e registrar histórias de usuário com critérios verificáveis, começando pelas frentes de maior incerteza; entregar em fatias validadas incrementalmente para reduzir o tamanho do retrabalho quando houver divergência.
    3. **Transferir:** Atribuir a formalização e o aceite dos requisitos a um analista de negócio/*product owner* designado pelo solicitante, deslocando a responsabilidade pela definição para quem detém o conhecimento do negócio.
    4. **Aceitar:** Iniciar apenas tarefas de baixo risco e independentes de requisito (ex.: preparação de ambiente) enquanto os requisitos amadurecem, aceitando conscientemente a incerteza residual nessas frentes menores.

---

**Risco:** RI-13 — Salto de escopo em relação ao propósito didático original *(Crítico)*

- **Descrição:** Planejar prazo/esforço no ritmo de um MVP acadêmico para um objetivo próximo de produção corporativa tende a gerar subestimação sistemática.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Reposicionar formalmente a iniciativa como um **novo projeto** (e não uma "evolução" do MVP), com planejamento, orçamento e cronograma próprios, evitando que a âncora do esforço anterior contamine as estimativas.
    2. **Mitigar:** Adotar estimativas por analogia com projetos web multiusuário reais, planejar por releases e incluir margem de contingência explícita; tornar visível a diferença de complexidade por meio de um comparativo MVP × plataforma.
    3. **Transferir:** Submeter as estimativas a validação de um consultor/arquiteto externo experiente em aplicações web de produção, compartilhando o risco de subestimação.
    4. **Aceitar:** Assumir e comunicar formalmente ao patrocinador que o cronograma inicial é uma hipótese com alta incerteza, aceitando revisões de baseline à medida que o escopo se detalha.

---

**Risco:** RI-14 — Ausência de meta clara de não funcional para 2.000 conexões *(Crítico)*

- **Descrição:** Sem definir se são usuários ativos, conexões HTTP concorrentes ou picos momentâneos, o dimensionamento pode ser sub ou superdimensionado.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Exigir do solicitante a definição mensurável da métrica (tipo de carga, perfil de uso, SLA) como pré-condição para o desenho de arquitetura, evitando dimensionamento sobre premissa ambígua.
    2. **Mitigar:** Traduzir o número em um requisito não funcional testável (ex.: "X req/s com tempo de resposta ≤ Y ms no percentil 95"), definir metas provisórias conservadoras e validá-las com testes de carga antes do go-live.
    3. **Transferir:** Terceirizar o dimensionamento de capacidade e teste de performance a especialista/serviço de *performance engineering*, compartilhando o risco de erro de dimensionamento.
    4. **Aceitar:** Adotar uma meta assumida documentada e projetar a arquitetura para escalar elasticamente, aceitando ajustar a capacidade conforme a demanda real observada em produção.

---

**Risco:** RI-12 — Ambiguidade sobre "armazenar documentos em banco de dados" *(Alto)*

- **Descrição:** Modelagem construída sobre premissa equivocada (BLOB no banco vs. storage externo, quotas por usuário) pode exigir redesenho já em ambiente multiusuário.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Esclarecer com o solicitante a intenção exata ("no banco" literal vs. "gerenciado pela aplicação") antes de qualquer modelagem, evitando decidir a persistência sobre suposição.
    2. **Mitigar:** Desacoplar a camada de persistência de arquivos por trás de uma interface/abstração, permitindo trocar a estratégia (banco ↔ storage externo) com impacto localizado; prototipar ambas antes de fixar.
    3. **Transferir:** Delegar a decisão de arquitetura de dados a um DBA/arquiteto de dados, compartilhando a responsabilidade técnica pela escolha.
    4. **Aceitar:** Se o volume inicial for pequeno, aceitar temporariamente a solução mais simples e reavaliar quando o uso crescer, assumindo o custo de eventual migração futura.

---

### 3.2 Arquitetura e Tecnologia

---

**Risco:** RI-01 — Arquitetura atual incompatível com o novo objetivo *(Crítico)*

- **Descrição:** Presumir que a base local do MVP pode ser apenas "estendida" pode levar ao descarte de trabalho já iniciado quando a incompatibilidade for percebida.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Inserir uma etapa formal de reavaliação arquitetural (arquitetura-alvo definida) antes de escrever código de produção, evitando construir sobre uma base inadequada.
    2. **Mitigar:** Aproveitar seletivamente apenas os componentes reutilizáveis (regras de classificação, FTS, parsing de encoding) sob nova arquitetura, provando as decisões com uma *walking skeleton*/prova de conceito antes de comprometer o cronograma.
    3. **Transferir:** Contratar revisão de arquitetura (*architecture review*) por especialista externo para validar a arquitetura-alvo, compartilhando o risco da decisão estrutural.
    4. **Aceitar:** Aceitável apenas para módulos isolados e de baixo acoplamento cujo eventual retrabalho seria pequeno; não recomendável para o núcleo da aplicação.

---

**Risco:** RI-02 — SQLite incompatível com alta concorrência *(Crítico)*

- **Descrição:** O modelo de bloqueio do SQLite compromete o requisito de alta concorrência, podendo gerar indisponibilidade/lentidão e migração de dados sob pressão.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Decidir cedo pela migração para um SGBD cliente-servidor (ex.: PostgreSQL), eliminando o gargalo de concorrência do SQLite antes que ele se manifeste em produção.
    2. **Mitigar:** Isolar o acesso a dados por uma camada de repositório/ORM que torne a troca de banco de baixo impacto; planejar migração de dados com antecedência e validar concorrência com testes de carga.
    3. **Transferir:** Adotar um banco gerenciado em nuvem (DBaaS), transferindo ao provedor a operação, escalabilidade e disponibilidade da camada de dados.
    4. **Aceitar:** Só sustentável se a análise de RI-14 concluir que a carga real é baixa e atendível por instância única; caso contrário, aceitar o risco significa aceitar falha provável em produção — não recomendado.

---

**Risco:** RI-03 — Ausência de API JSON para o novo frontend *(Crítico)*

- **Descrição:** O frontend Angular fica bloqueado até a construção de uma API completa, hoje inexistente; alto risco de subestimação de esforço.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Não é um risco "evitável" no sentido de eliminar a necessidade — a API é requisito. A leitura de "evitar" aqui é remover a dependência bloqueante priorizando a API como primeira entrega de backend.
    2. **Mitigar:** Definir cedo um contrato de API (OpenAPI) e disponibilizar *mocks*/stubs para o frontend trabalhar em paralelo; fatiar a API por funcionalidade para desbloquear incrementalmente o desenvolvedor Angular.
    3. **Transferir:** Reforçar temporariamente a frente de backend (realocação/contratação) para não concentrar toda a API no único desenvolvedor Python, distribuindo o esforço.
    4. **Aceitar:** Aceitar sequenciamento parcial (frontend aguarda endpoints não críticos) apenas para funcionalidades secundárias, mantendo o caminho crítico priorizado.

---

**Risco:** RI-18 — Ausência de infraestrutura de hospedagem, CI/CD e monitoramento *(Crítico)*

- **Descrição:** Toda a infraestrutura de deploy, integração contínua e observabilidade precisa ser criada do zero antes da exposição pública, com equipe sem histórico nesse tipo de operação.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Adotar plataformas gerenciadas (PaaS) que embutem deploy, escalabilidade e monitoramento, evitando construir e operar infraestrutura de baixo nível internamente.
    2. **Mitigar:** Estabelecer pipeline de CI/CD e observabilidade mínimos já no início ("infra desde o dia 1"), amadurecendo-os iterativamente; capacitar a equipe em DevOps antes do go-live.
    3. **Transferir:** Terceirizar a montagem e operação da infraestrutura a um parceiro de DevOps/*managed services*, transferindo a responsabilidade operacional.
    4. **Aceitar:** Aceitável apenas para ambientes de teste internos sem exposição pública; inaceitável para produção, onde a ausência de monitoramento/deploy confiável compromete a operação.

---

**Risco:** RI-04 — Servidor de aplicação em modo de desenvolvimento *(Alto)*

- **Descrição:** Executar em servidor de desenvolvimento (reload automático, sem múltiplos workers, sem balanceamento) gera instabilidade e insegurança sob carga real.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Definir por padrão um servidor de produção (ex.: WSGI/ASGI com múltiplos workers atrás de proxy reverso) na configuração-alvo, impedindo que o modo dev chegue a produção.
    2. **Mitigar:** Parametrizar configurações por ambiente e incluir *checklist* de prontidão para produção no pipeline; validar comportamento sob carga antes da exposição.
    3. **Transferir:** Delegar a configuração de runtime a plataforma gerenciada/parceiro de infraestrutura que já entrega o servidor em modo produção.
    4. **Aceitar:** Apenas em ambientes internos de desenvolvimento/homologação; não aceitável em produção.

---

**Risco:** RI-06 — Ausência de estratégia de migração de schema *(Alto)*

- **Descrição:** Alterações no modelo de dados para multiusuário podem causar perda/inconsistência dos dados já existentes em `devnotes.db`.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Adotar uma ferramenta de versionamento de schema (ex.: Alembic) antes de qualquer alteração estrutural, evitando migrações manuais e ad hoc.
    2. **Mitigar:** Versionar migrações, testá-las em cópia dos dados e exigir backup verificado antes de cada aplicação em produção; automatizar rollback.
    3. **Transferir:** Atribuir a governança de migrações a um DBA responsável, compartilhando a responsabilidade sobre a integridade dos dados.
    4. **Aceitar:** Aceitável apenas se o volume de dados a preservar for irrelevante (ex.: base de MVP descartável), assumindo eventual recriação em vez de migração.

---

**Risco:** RI-05 — Armazenamento de arquivos em disco local não escalável *(Médio)*

- **Descrição:** Em múltiplas instâncias, cada uma teria visão distinta dos arquivos, gerando inconsistência para o usuário.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Adotar armazenamento externo compartilhado (object storage) desde o início, eliminando a dependência do disco local.
    2. **Mitigar:** Abstrair o acesso a arquivos por uma interface de storage, permitindo migrar de disco local para storage compartilhado com impacto localizado.
    3. **Transferir:** Utilizar serviço de object storage gerenciado (ex.: S3-compatível), transferindo durabilidade e escalabilidade ao provedor.
    4. **Aceitar:** Aceitável se a meta de carga (RI-14) for atendida por instância única robusta (escala vertical), assumindo a limitação de escala horizontal.

---

### 3.3 Segurança e Dados

---

**Risco:** RI-08 — Ausência de autenticação e autorização *(Crítico)*

- **Descrição:** Sem essa camada é impossível isolar dados por usuário — requisito central; sua ausência bloqueia todas as funcionalidades multiusuário.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Não expor a aplicação na web enquanto não houver autenticação/autorização implementada e testada, evitando qualquer janela de acesso não controlado.
    2. **Mitigar:** Implementar autenticação/autorização como uma das primeiras entregas, com isolamento de dados por usuário verificado por testes automatizados de autorização.
    3. **Transferir:** Adotar um provedor de identidade gerenciado (IdP/OAuth2/OIDC — ex.: Keycloak, Auth0, Entra ID), transferindo a complexidade e a manutenção da segurança de identidade a solução especializada.
    4. **Aceitar:** Inaceitável para uma plataforma multiusuário exposta; só admissível em ambiente de teste isolado sem dados reais.

---

**Risco:** RI-09 — Exposição de conteúdo sensível em ambiente compartilhado *(Alto)*

- **Descrição:** Regras de negócio e trechos de sistemas legados, antes protegidos por estarem em máquina local, passam a ser potencialmente acessíveis a terceiros.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Definir política de classificação de conteúdo e vedar o armazenamento de dados altamente sensíveis na plataforma, evitando a exposição na origem.
    2. **Mitigar:** Combinar autenticação/autorização (RI-08), criptografia em trânsito e em repouso, e trilhas de auditoria de acesso, reduzindo probabilidade e impacto de exposição indevida.
    3. **Transferir:** Contratar seguro cibernético e/ou apoiar-se em provedor de nuvem com certificações de conformidade, compartilhando o risco residual e as garantias de segurança.
    4. **Aceitar:** Aceitável apenas para conteúdo público/não sensível, com o solicitante ciente e formalmente responsável por essa classificação.

---

**Risco:** RI-10 — Upload de arquivos sem validação de segurança aprofundada *(Alto)*

- **Descrição:** Em ambiente web multiusuário, um upload malicioso pode comprometer outros usuários ou a infraestrutura; hoje só extensão e tamanho são validados.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Restringir drasticamente os tipos aceitos e tratar todo arquivo como texto inerte (sem execução/renderização ativa), removendo a superfície de ataque de conteúdo executável.
    2. **Mitigar:** Validar tipo real (magic number), aplicar antivírus/sandbox, servir downloads com cabeçalhos seguros e isolar o storage de uploads, reduzindo a probabilidade de exploração.
    3. **Transferir:** Utilizar serviço de varredura de malware/*content scanning* gerenciado, transferindo a detecção de conteúdo malicioso a um especialista.
    4. **Aceitar:** Aceitável apenas com base fechada de usuários confiáveis e conteúdo controlado; arriscado para acesso aberto.

---

**Risco:** RI-19 — Ausência de rotina de backup e recuperação *(Alto)*

- **Descrição:** Com documentos de múltiplos usuários, uma falha sem backup afeta muito mais pessoas, com potencial perda irreversível de dados.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Não permitir entrada em produção sem rotina de backup e teste de restauração validados, evitando operar sem rede de proteção.
    2. **Mitigar:** Implementar backups automáticos com política de retenção, verificação periódica de restauração e definição de RPO/RTO, reduzindo o impacto de incidentes.
    3. **Transferir:** Usar backup gerenciado do provedor de nuvem/DBaaS, transferindo a execução e a durabilidade das cópias ao serviço.
    4. **Aceitar:** Aceitável apenas na fase pré-produção sem dados reais de usuários; inaceitável após o go-live.

---

### 3.4 Equipe e Recursos

---

**Risco:** RI-15 — Descompasso entre stack atual e composição da equipe *(Crítico)*

- **Descrição:** Sem clareza de papéis, a reescrita da API e a construção do frontend podem ficar sem dono efetivo, gerando atraso e retrabalho de coordenação.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Redefinir a divisão de trabalho alinhando frentes às competências existentes (ex.: dedicar o dev Python ao backend/API) antes de iniciar, evitando frentes sem dono.
    2. **Mitigar:** Formalizar papéis e responsabilidades (matriz RACI), promover *pairing*/capacitação cruzada e nomear um responsável técnico por frente, reduzindo a coordenação improvisada.
    3. **Transferir:** Contratar/alocar profissionais com a stack necessária (ex.: reforço backend web ou frontend Angular), transferindo a lacuna de competência para fora da equipe atual.
    4. **Aceitar:** Aceitar temporariamente uma curva de aprendizado planejada, assumindo produtividade menor no início, apenas se o cronograma comportar essa folga.

---

**Risco:** RI-16 — Capacidade reduzida da equipe frente ao aumento de escopo *(Crítico)*

- **Descrição:** Cronograma planejado com premissa de capacidade full-time tende a atrasar ou sacrificar qualidade, já que só dois membros são dedicados integralmente.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Ajustar o escopo do release à capacidade real da equipe (MVP web enxuto), evitando comprometer-se com um volume incompatível com a força de trabalho disponível.
    2. **Mitigar:** Priorizar rigorosamente o backlog (MoSCoW), planejar por capacidade real e proteger as frentes part-time do caminho crítico, reduzindo o efeito da limitação.
    3. **Transferir:** Ampliar a capacidade via contratação, alocação adicional ou terceirização de frentes específicas, transferindo parte da carga.
    4. **Aceitar:** Aceitar prazo mais longo e entregas faseadas como consequência da capacidade atual, desde que negociado e aprovado pelo patrocinador.

---

**Risco:** RI-17 — Cobertura de testes insuficiente para os novos requisitos *(Alto)*

- **Descrição:** Validar 2.000 conexões e isolamento de dados exige testes de carga, concorrência e segurança, hoje ausentes; o tester é part-time.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Tornar critérios de qualidade (cobertura mínima, testes de carga e de autorização) um *gate* obrigatório de release, evitando entregar sem validação dos requisitos críticos.
    2. **Mitigar:** Automatizar testes na pipeline (unitários, integração, carga, segurança) e capacitar/instrumentar a equipe com ferramentas de teste de carga, reduzindo a dependência de esforço manual.
    3. **Transferir:** Contratar serviço especializado de teste de carga/*pentest* para as validações não funcionais e de segurança, transferindo essa competência.
    4. **Aceitar:** Aceitar cobertura reduzida apenas em áreas de baixo risco/baixa criticidade, nunca nas frentes de concorrência, dados e segurança.

---

### 3.5 Riscos de menor exposição (tratamento condensado)

**Risco:** RI-07 — Dependência de CDN externo para recursos de interface *(Muito Baixo)*

- **Descrição:** Indisponibilidade ou bloqueio do CDN afetaria destaque de sintaxe/formatação para todos, sem comprometer funcionalidades essenciais.
- **Estratégias de resposta possíveis:**
    1. **Evitar:** Hospedar os recursos localmente (self-hosting dos assets), eliminando a dependência externa — ação simples e de baixo custo.
    2. **Mitigar:** Manter *fallback* local caso o CDN falhe, reduzindo o impacto de indisponibilidade.
    3. **Transferir:** Manter-se em CDN de provedor com SLA, delegando a disponibilidade ao fornecedor.
    4. **Aceitar:** Dado o impacto muito baixo, **aceitar** é a resposta mais proporcional, com monitoramento leve — eventualmente combinada com o self-hosting por ser trivial.

---

## 2. Considerações sobre a aplicação das estratégias

### Quando cada estratégia tende a ser mais adequada

- **Evitar** — indicada para riscos **Críticos** cujo impacto é inaceitável e cuja causa pode ser removida por uma decisão estrutural (ex.: não expor sem autenticação — RI-08; migrar de SQLite — RI-02; reavaliar arquitetura — RI-01). Costuma implicar mudança de escopo, cronograma ou abordagem.
- **Mitigar** — a resposta "padrão" quando o risco não pode ser eliminado mas pode ser reduzido a nível tolerável (aplicável à maioria dos riscos deste projeto: RI-03, RI-06, RI-09, RI-10, RI-17). Atua sobre probabilidade e/ou impacto por meio de boas práticas de engenharia.
- **Transferir** — adequada quando existe um terceiro mais capacitado para assumir o risco (nuvem gerenciada, IdP, DBaaS, parceiro DevOps, seguro) — especialmente forte para RI-08, RI-18, RI-19, dada a inexperiência da equipe nessas frentes.
- **Aceitar** — reservada para riscos de **baixa exposição** (RI-07) ou para riscos residuais após mitigação, sempre com ciência formal do patrocinador. Perigosa se aplicada a riscos críticos apenas por conveniência de prazo.

### Limitações e trade-offs por abordagem

- **Evitar** remove o risco, mas frequentemente ao custo de prazo, orçamento ou redução de escopo; pode adiar o início do projeto.
- **Mitigar** raramente elimina o risco por completo — resta risco residual — e consome esforço/recursos que competem com as entregas de funcionalidade.
- **Transferir** não elimina o risco, apenas o desloca; introduz custo financeiro (contratos, seguros, serviços gerenciados) e nova dependência de fornecedor (*lock-in*, SLA), além de não transferir a responsabilidade reputacional.
- **Aceitar** só é responsável quando o impacto é comprovadamente baixo ou o custo de tratar supera o do risco; aplicada indevidamente a riscos críticos, transforma-se em omissão.

Observa-se ainda que os riscos são **interdependentes**: RI-08 (autenticação) condiciona RI-09; RI-14 (métrica de carga) condiciona RI-02 e RI-05; RI-11/RI-12/RI-14 (requisitos) condicionam praticamente todas as decisões arquiteturais. Estratégias tendem a ser **combinadas** (ex.: mitigar + transferir) e não excludentes.

---

## 3. Observações gerais

### Dependência de contexto adicional

As sugestões acima assumem premissas que precisam ser confirmadas antes de qualquer decisão definitiva:

- Definição mensurável da carga esperada (RI-14) — essencial para escolher entre evitar/mitigar/aceitar em RI-02 e RI-05.
- Intenção real de "armazenar documentos em banco" (RI-12) — condiciona a modelagem de dados e a estratégia de storage.
- Volume e sensibilidade dos dados a migrar/preservar — condicionam RI-06, RI-09 e RI-19.
- Orçamento disponível para transferência de risco (nuvem gerenciada, IdP, terceirização) e para reforço de equipe (RI-15, RI-16).
- Apetite a risco do patrocinador — define o limite entre "aceitar" e "mitigar/evitar".

### Pontos que exigem validação com *stakeholders*

- Formalização e aceite dos requisitos e critérios de aceitação (RI-11) junto ao solicitante/*product owner*.
- Reposicionamento da iniciativa como novo projeto com baseline própria (RI-13).
- Aprovação de eventual ampliação de equipe ou terceirização (RI-15, RI-16, RI-17, RI-18).
- Classificação de sensibilidade do conteúdo e políticas de segurança/conformidade (RI-09, RI-10).
- Definição de metas de RPO/RTO e política de backup (RI-19).

### Natureza deste documento

Esta é uma análise **exploratória e qualitativa**: apresenta alternativas e implicações, **não** a seleção final de resposta por risco, tampouco planos de ação detalhados, responsáveis, prazos ou custos — que correspondem a etapas subsequentes (plano de resposta a riscos e sua execução/monitoramento). As sugestões devem ser lidas como apoio à decisão, não como decisão tomada.
