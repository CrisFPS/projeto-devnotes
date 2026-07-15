# Contexto: 

Atuo como gerente de projetos e recebi a incumbência de ser o responsável pelo desenvolvimento de uma Aplicação web local chamada DevNotes que serve para organizar, pesquisar e reutilizar conteúdos técnicos: snippets, SQLs, scripts, anotações, arquivos de sistemas legados e regras de negócio. Não conheço a aplicação e as únicas informações que me foram passadas foram a pasta do projeto e mais o Objetivo da aplicação DevNotes e as Funcionalidades do MVP da aplicação DevNotes que estão descritas logo abaixo. Preciso que entenda o projeto e me auxilie a expandir suas funcionalidades assim como passá-lo para a plataforma web. 

O projeto encontra-se com o primeiro MVP entregue mas foi-me solicitado inicialmente as seguintes demandas:
- fluidez e modernização na interface;
- interface arrojada; 
- 2000 conexões simultâneas;
- usuários guardarem seus documentos em banco de dados.

Considere ainda que: 
- a equipe é composta por 1 desenvolvedor .Net dedicado, 1 desenvolvedor python dedicado, 1 desenvolvedor angular part time e 1 tester part time 

## Objetivo da aplicação DevNotes

Centralizar conteúdos técnicos dispersos em um repositório local simples,
pesquisável e com preservação da formatação original.

O DevNotes Local atende especialmente cenários de estudo, manutenção de sistemas
legados, consulta rápida de snippets, organização de regras de negócio e
reaproveitamento de comandos ou scripts usados no dia a dia.

---

## Funcionalidades do MVP da aplicação DevNotes

- Cadastrar conteúdos técnicos manualmente.
- Editar e excluir conteúdos.
- Listar conteúdos com paginação de 20 itens por página.
- Fazer upload simples de um arquivo por vez, até 12 MB.
- Aceitar as extensões `.py`, `.java`, `.sql`, `.md`, `.txt`, `.srw`, `.sru`,
  `.srd`, `.srm`, `.srf`, `.sra`, `.srs`.
- Classificar automaticamente arquivos PowerBuilder pela extensão.
- Ler arquivos com fallback de encoding: `utf-8` → `latin-1` → `cp1252`.
- Pesquisar conteúdos com SQLite FTS5.
- Filtrar por categoria, linguagem, sistema, domínio, tags e regra de negócio.
- Visualizar conteúdo com formatação preservada usando `<pre><code>`.
- Destacar sintaxe com Highlight.js.

## Objetivo do Prompt

### Etapa 1 — Identificação de Riscos  

Com base no contexto apresentado, e que o projeto DevNotes encotra-se na pasta C:\Projetos_Pos\DevNotes, identifique os riscos associados ao projeto. 
### Entregável esperado
- lista de riscos identificados; 
- descrição breve de cada risco; 
- contexto em que podem ocorrer. 