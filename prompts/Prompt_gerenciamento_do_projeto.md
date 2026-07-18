# Contexto

Atuo como gerente de projetos e recebi a responsabilidade de conduzir o desenvolvimento de uma aplicação web local chamada **DevNotes**, cujo propósito é organizar, pesquisar e reutilizar conteúdos técnicos: snippets, SQLs, scripts, anotações, arquivos de sistemas legados e regras de negócio.

Não conheço a aplicação em profundidade. As únicas informações que me foram repassadas até o momento são a pasta do projeto, o objetivo da aplicação DevNotes e as funcionalidades do MVP, descritos logo abaixo. Preciso que você entenda o projeto e me ajude a expandir suas funcionalidades, além de conduzir sua migração para a plataforma web.

O projeto já teve seu primeiro MVP entregue, mas as seguintes demandas foram solicitadas inicialmente:

- Fluidez e modernização na interface;
- Interface arrojada;
- Suporte a 2.000 conexões simultâneas;
- Permitir que os usuários armazenem seus documentos em banco de dados.

Considere ainda que a equipe é composta por:

- 1 desenvolvedor .NET dedicado;
- 1 desenvolvedor Python dedicado;
- 1 desenvolvedor Angular em regime part-time;
- 1 tester em regime part-time.

## Objetivo da aplicação DevNotes

Centralizar conteúdos técnicos dispersos em um repositório local simples, pesquisável e com preservação da formatação original.

O DevNotes Local atende especialmente a cenários de estudo, manutenção de sistemas legados, consulta rápida de snippets, organização de regras de negócio e reaproveitamento de comandos ou scripts usados no dia a dia.

---

## Funcionalidades do MVP da aplicação DevNotes

- Cadastrar conteúdos técnicos manualmente.
- Editar e excluir conteúdos.
- Listar conteúdos com paginação de 20 itens por página.
- Fazer upload simples de um arquivo por vez, com até 12 MB.
- Aceitar as extensões `.py`, `.java`, `.sql`, `.md`, `.txt`, `.srw`, `.sru`, `.srd`, `.srm`, `.srf`, `.sra`, `.srs`.
- Classificar automaticamente arquivos PowerBuilder pela extensão.
- Ler arquivos com fallback de encoding: `utf-8` → `latin-1` → `cp1252`.
- Pesquisar conteúdos com SQLite FTS5.
- Filtrar por categoria, linguagem, sistema, domínio, tags e regra de negócio.
- Visualizar conteúdo com formatação preservada usando `<pre><code>`.
- Destacar sintaxe com Highlight.js.

---

## Seu Objetivo é:

### Etapa 1 — Identificação de Riscos

Com base no contexto apresentado, e considerando que o projeto DevNotes se encontra na pasta `C:\Projetos_Pos\DevNotes`, identifique os riscos associados ao projeto.

### Entregável esperado

- Lista de riscos identificados;
- Descrição breve de cada risco;
- Contexto em que podem ocorrer;
- arquivo markdowm riscos\identificacao.md.