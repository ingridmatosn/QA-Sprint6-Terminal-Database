# QA Sprint 6 — Terminal e Banco de Dados

Investigação de falhas de aplicação em **logs de servidor Apache** pelo terminal e **consultas SQL** em base PostgreSQL, no bootcamp de Analista de QA da TripleTen.

Esta é a parte do trabalho de QA que acontece depois que o bug é relatado: ir aos dados para entender o tamanho e a origem do problema.

## Documentação

- **[Console — análise de logs](console-analise-de-logs.md)** — comandos usados, com os resultados obtidos.
- **[Banco de dados — consultas SQL](banco-de-dados-sql.md)** — as quatro consultas, com a saída de cada uma.
- Documento original: `Ingrid Matos - QA34 » Sprint 6.docx`

## Objetivo

Localizar e quantificar erros de aplicação a partir dos logs do servidor, e extrair informação de uma base real de corridas de táxi de Chicago usando SQL.

## Parte 1 — Console (Cygwin/Bash)

Busca e filtragem de logs Apache por IP, período e código de status HTTP.

**Resultados quantificados em um único dia de log (30/12/2019):**

| Tipo de erro | Ocorrências |
|---|---|
| HTTP 400 (erro do cliente) | 172 |
| HTTP 500 (erro do servidor) | 156 |

Comandos aplicados: `grep`, `grep -R`, redirecionamento para arquivo, `wc`, `head`, `tail`, `mkdir`, `cp`.

## Parte 2 — Banco de dados (PostgreSQL)

Base **Chicago Taxi Trips**, com as tabelas `cabs`, `trips` e `weather_records`.

| Tarefa | Técnica SQL | Resultado |
|---|---|---|
| Total de táxis cadastrados | `COUNT(*)` | 5.529 táxis |
| Empresas com menos de 100 carros | `GROUP BY` + `HAVING` | 51 empresas |
| Classificar o clima por hora em Good/Bad | `CASE WHEN` + `LIKE` | 24 registros de 05/11/2017 |
| Corridas por empresa em um período | `INNER JOIN` + `GROUP BY` + `ORDER BY` | 64 empresas, liderada por Flash Cab com 19.558 corridas |

As consultas completas estão em [banco-de-dados-sql.md](banco-de-dados-sql.md).

## Ferramentas

Cygwin / Bash · grep e utilitários de linha de comando · PostgreSQL · SQL

## Estrutura do repositório

- `console-analise-de-logs.md` — comandos e resultados da análise de logs
- `banco-de-dados-sql.md` — as 4 consultas SQL com as saídas
- `Ingrid Matos - QA34 » Sprint 6.docx` — documento original
- `README.md`

## O que aprendi

- Log responde a pergunta que a tela não responde: com quantos usuários aquilo aconteceu, em que horário, e se é um caso isolado ou um padrão. Saber que foram 172 erros 400 e 156 erros 500 num único dia muda a prioridade de um defeito.
- `grep` com redirecionamento resolve em uma linha o que levaria muito tempo abrindo arquivo por arquivo — e o resultado é reproduzível, porque o comando fica registrado.
- SQL me deixa validar dado direto na fonte, sem depender do que a interface mostra. Em teste, isso serve para confirmar se o problema está na tela ou nos dados.

## Melhorias a fazer

- Aprofundar em `JOIN` de mais de duas tabelas e em subconsultas.
- Aplicar a análise de logs a um defeito real do portfólio, ligando o erro visto na tela ao registro no servidor.
- Documentar um passo a passo de como reproduzir as consultas em uma base local.

---

**Ingrid Matos** — Analista de QA Júnior
[LinkedIn](https://www.linkedin.com/in/ingridmatosn/) · [Portfólio de QA](https://github.com/ingridmatosn/QA-Portfolio-Main)
