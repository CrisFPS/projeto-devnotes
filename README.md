# Gestão de Riscos e Comunicação — Projeto DevNotes

Exercício de especialização do microcurso **Gerência de Projetos de Software Apoiada por Inteligência Artificial Generativa** — **Unidade III: Riscos, Comunicação e Documentação Inteligente**.

---

## 📌 Descrição do projeto

Este repositório reúne os artefatos de **gerenciamento de riscos, comunicação e documentação** produzidos para um cenário fictício de projeto de software: a **expansão da aplicação DevNotes**.

O **DevNotes** é uma aplicação originalmente concebida como ferramenta **local e de uso individual** para organizar, pesquisar e reutilizar conteúdos técnicos (snippets, SQLs, scripts, anotações, arquivos de sistemas legados e regras de negócio). A partir de novas demandas — interface moderna, suporte a **2.000 conexões simultâneas** e armazenamento de documentos dos usuários em banco de dados — surge a necessidade de **migrá-la para uma plataforma web multiusuário**.

Essa mudança de propósito, que rompe com as premissas originais do MVP, é o pano de fundo sobre o qual foram conduzidas as atividades de **identificação, análise e resposta a riscos**, além da **comunicação aos stakeholders**.

> ℹ️ Este repositório **não contém o código-fonte** da aplicação DevNotes. Ele concentra exclusivamente os **entregáveis de gerência de projeto** (documentação de riscos e comunicação) e os **prompts de IA generativa** utilizados para produzi-los.

---

## 🎯 Objetivo do exercício

Aplicar, de forma prática e apoiada por **IA generativa**, os conceitos da Unidade III do microcurso:

- **Riscos** — identificar, analisar qualitativamente e propor estratégias de resposta aos riscos do projeto;
- **Comunicação** — elaborar comunicação de status adequada a diferentes stakeholders (Diretoria, PO, Desenvolvedores, DBA), traduzindo aspectos técnicos em impacto de negócio;
- **Documentação inteligente** — utilizar prompts estruturados como instrumento de geração e organização dos artefatos, demonstrando o uso da IA como apoio ao gerente de projetos.

O foco não está no desenvolvimento da aplicação, mas em **como a IA generativa pode apoiar o gerente de projetos** nas atividades de gestão de riscos, comunicação e documentação.

---

## 🗂️ Organização do repositório

```
projeto-devnotes/
├── README.md                      # Este arquivo
├── prompts/                       # Prompts de IA generativa que geraram os artefatos
│   ├── Prompt_gerenciamento_do_projeto.md        # Contexto e identificação de riscos
│   ├── Prompt_analise_riscos.md                  # Análise qualitativa dos riscos
│   ├── Prompt_definicao_de_estrategias_de_resposta.md  # Estratégias de resposta
│   └── Prompt_comunicacao_stakeholders.md        # Comunicação aos stakeholders
├── riscos/                        # Entregáveis do processo de gestão de riscos
│   ├── identificacao.md           # Etapa 1 — Identificação de riscos
│   ├── analise.md                 # Etapa 2 — Análise qualitativa (probabilidade × impacto)
│   └── respostas.md               # Etapa 3 — Estratégias de resposta (evitar/mitigar/transferir/aceitar)
└── comunicacao/                   # Entregáveis de comunicação
    └── status-stakeholders.md     # Relatório de status para stakeholders (padrão de mercado)
```

Cada artefato em `riscos/` e `comunicacao/` foi gerado a partir do prompt correspondente em `prompts/`, seguindo um fluxo encadeado em que a saída de uma etapa serve de insumo para a próxima:

**Identificação → Análise → Estratégias de resposta → Comunicação**

---

## 🤖 Nota sobre o uso de IA generativa

Os documentos deste repositório foram elaborados com apoio de IA generativa a partir dos prompts em [prompts/](prompts/). Os prompts são parte integrante do entregável: demonstram como o direcionamento (persona, tarefa, contexto e formato de saída) influencia a qualidade e a adequação dos artefatos produzidos.
