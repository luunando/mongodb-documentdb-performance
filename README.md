# Performance de MongoDB 4.0 e Amazon DocumentDB 4.0

Curadoria de skills oficiais e documentação para diagnóstico e tuning, com atenção à compatibilidade com a versão 4.0.

**Data da pesquisa:** 9 de novembro de 2026.

O principal cuidado ao utilizar esses materiais é conferir a versão: os guias e as skills atuais incluem recursos que não existem no MongoDB ou no Amazon DocumentDB 4.0.

## Skills oficiais

| Skill | O que cobre | Aplicação à versão 4.0 |
| --- | --- | --- |
| [MongoDB Query Optimizer](https://github.com/mongodb/agent-skills/blob/main/skills/mongodb-query-optimizer/SKILL.md) | Queries lentas, índices, `explain` e recomendações do Atlas Performance Advisor. | Primeira opção para análise de queries. A compatibilidade das integrações atuais com a versão 4.0 precisa ser validada. |
| [MongoDB Connection](https://github.com/mongodb/agent-skills/blob/main/plugins/mongodb/skills/mongodb-connection/SKILL.md) | Connection pools, timeouts, concorrência e esgotamento de conexões. | Útil quando a latência envolve a aplicação e o driver. |
| [MongoDB Schema Design](https://github.com/mongodb/agent-skills/blob/main/skills/mongodb-schema-design/SKILL.md) | Modelagem, arrays sem limite, excesso de índices e uso frequente de `$lookup`. | Indicada para problemas de performance ligados ao modelo de dados. |
| [Amazon DocumentDB Toolkit](https://github.com/aws/agent-toolkit-for-aws/blob/main/skills/specialized-skills/database-skills/amazon-documentdb/SKILL.md) | Diagnóstico, índices, CPU, conexões e revisão operacional. | Primeira opção para DocumentDB. O arquivo [performance.md](https://github.com/aws/agent-toolkit-for-aws/blob/main/skills/specialized-skills/database-skills/amazon-documentdb/references/performance.md) concentra o tuning. |

Esses materiais estão nos repositórios oficiais da MongoDB e da AWS, mas abrangem versões atuais. Na pesquisa, não foi encontrada indicação de que sejam skills dedicadas exclusivamente à versão 4.0.

Referências gerais:

- [Documentação oficial das MongoDB Agent Skills](https://www.mongodb.com/docs/agent-skills/).
- [Repositório oficial mongodb/agent-skills](https://github.com/mongodb/agent-skills).
- [Repositório oficial aws/agent-toolkit-for-aws](https://github.com/aws/agent-toolkit-for-aws).

## Documentação específica do MongoDB 4.0

Os arquivos abaixo foram confirmados no repositório oficial, na branch `v4.0`. Os links abrem o texto-fonte da documentação, em formato reStructuredText, preservando a referência histórica da versão.

| Documento da versão 4.0 | Para que consultar |
| --- | --- |
| [Analyze Query Performance](https://raw.githubusercontent.com/mongodb/docs/v4.0/source/tutorial/analyze-query-plan.txt) | Interpretar `COLLSCAN`, `IXSCAN`, documentos examinados e resultados retornados. |
| [Optimize Query Performance](https://raw.githubusercontent.com/mongodb/docs/v4.0/source/tutorial/optimize-query-performance-with-indexes-and-projections.txt) | Índices compostos, projeções e otimização das consultas. |
| [Database Profiler](https://raw.githubusercontent.com/mongodb/docs/v4.0/source/tutorial/manage-the-database-profiler.txt) | Capturar operações lentas, configurar profiling e analisar `system.profile`. |
| [Aggregation Pipeline Optimization](https://raw.githubusercontent.com/mongodb/docs/v4.0/source/core/aggregation-pipeline-optimization.txt) | Otimização e reorganização dos estágios de agregação. |
| [WiredTiger Storage Engine](https://raw.githubusercontent.com/mongodb/docs/v4.0/source/core/wiredtiger.txt) | Cache, memória, compressão e comportamento do storage engine. |

Há também o [portal oficial de documentação legada](https://www.mongodb.com/docs/legacy/). A MongoDB informa que esses manuais históricos não recebem manutenção.

Durante a pesquisa, um link antigo com `/v4.0/` redirecionou para o manual atual. Por isso, não se deve presumir que uma URL antiga ainda exiba o conteúdo daquela versão. Os arquivos da branch `v4.0` ajudam a manter a referência correta.

## Documentação para Amazon DocumentDB 4.0

A documentação da AWS utiliza o caminho `latest` e reúne funcionalidades de diferentes versões. Para aplicar uma recomendação ao DocumentDB 4.0, é necessário conferir as notas de versão e a coluna correspondente na matriz de compatibilidade.

| Documento oficial AWS | Conteúdo mais útil |
| --- | --- |
| [Best practices](https://docs.aws.amazon.com/documentdb/latest/devguide/best_practices.html) | Dimensionamento de memória, working set, cache, índices e distribuição de leituras. |
| [Troubleshooting performance and resource utilization](https://docs.aws.amazon.com/documentdb/latest/devguide/user_diagnostics.html) | Investigação de CPU, cache, queries demoradas, cursores e utilização dos índices. |
| [Querying in Amazon DocumentDB](https://docs.aws.amazon.com/documentdb/latest/devguide/querying.html) | Planos de execução, `explain("executionStats")` e cache de planos. |
| [Profiling Amazon DocumentDB operations](https://docs.aws.amazon.com/documentdb/latest/devguide/profiling.html) | Captura de queries lentas e análise dos logs no CloudWatch Logs Insights. |
| [Functional differences](https://docs.aws.amazon.com/documentdb/latest/devguide/functional-differences.html) | Particularidades de índices, `$regex`, `$lookup` e comportamento das consultas. |
| [Supported MongoDB APIs](https://docs.aws.amazon.com/documentdb/latest/devguide/mongo-apis.html) | Matriz por versão para validar comandos, operadores e tipos de índice. |

Os quatro primeiros documentos formam uma sequência útil de diagnóstico: identificar a operação cara, examinar o plano e correlacionar o comportamento com memória, CPU e I/O.

## Diferenças importantes para a versão 4.0

### Planos de execução e métricas

O Amazon DocumentDB usa um engine próprio. A compatibilidade com a API MongoDB não garante os mesmos planos de execução nem a mesma estrutura de saída do `explain`.

Referência: [Troubleshooting performance and resource utilization](https://docs.aws.amazon.com/documentdb/latest/devguide/user_diagnostics.html).

### Documentos examinados no explain do DocumentDB

O guia atual da AWS informa que `docsExamined` e `totalDocsExamined` no `explain` do DocumentDB estão disponíveis a partir das versões **5.0.1 e 8.0.0**. Esses campos não devem ser exigidos no diagnóstico do DocumentDB 4.0.

Referência: [Query plan analysis — Understanding docsExamined](https://docs.aws.amazon.com/documentdb/latest/devguide/performance-query-plan-analysis.html).

### Índices e manutenção no DocumentDB 4.0

Segundo a matriz de compatibilidade consultada, o Amazon DocumentDB 4.0 não suporta:

- Índices parciais.
- Índices textuais.
- O comando `reIndex`.

Uma recomendação genérica para MongoDB pode depender desses recursos. A compatibilidade deve ser confirmada antes de aplicar a recomendação ao DocumentDB 4.0.

Referência: [Supported MongoDB APIs, operations, and data types](https://docs.aws.amazon.com/documentdb/latest/devguide/mongo-apis.html).

## Recomendação de uso

- **MongoDB 4.0:** começar pelo MongoDB Query Optimizer e pelo manual histórico da branch `v4.0`.
- **Amazon DocumentDB 4.0:** começar pelo `performance.md` da AWS, pelo guia de troubleshooting e pela matriz de compatibilidade.

Essa combinação oferece um roteiro prático e permite conferir cada recomendação contra a versão utilizada. A pesquisa identifica referências; a adequação de um ajuste depende das consultas, dos índices, do volume de dados e do comportamento observado no ambiente.
