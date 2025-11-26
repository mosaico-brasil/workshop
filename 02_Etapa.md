# Projeto Mosaico Brasil

## Workshop Análise de Dados Públicos com o projeto Base de Dados e o Google Cloud

Segunda etapa do Workshop: Saneamento Básico por Município

Iremos utilizar duas tabelas:

- Serviços de Água e Esgoto nos Municípios - [link](https://basedosdados.org/dataset/2a543ad8-3cdb-4047-9498-efe7fb8ed697?table=df7cf198-4889-4baf-bb77-4e0e28eb90ca)
- Diretórios Brasileiros - Municipio - [link](https://basedosdados.org/dataset/33b49786-fb5f-496f-bb7c-9811c985af8e?table=dffb65ac-9df9-4151-94bf-88c45bfcb056)

Dados da tabela `municipio_agua_esgoto`

```sql
SELECT
  ano,
  id_municipio,
  sigla_uf,
  populacao_urbana_atendida_agua,
  volume_agua_produzido,
  volume_agua_tratada_eta,
  extensao_rede_agua
FROM
  `basedosdados.br_mdr_snis.municipio_agua_esgoto` LIMIT 1000
```

Explorando a tabela `prestador_agua_esgoto`

```sql
SELECT
  ano,
  id_municipio,
  id_prestador,
  prestador,
  sigla_prestador,
  abrangencia,
  tipo_servico
FROM
  `basedosdados.br_mdr_snis.prestador_agua_esgoto` LIMIT 1000
```

Utilizando o `where` com o `like` para pesquisar a Sigla do Prestador

```sql
SELECT
  ano,
  id_municipio,
  sigla_uf,
  id_prestador,
  prestador,
  sigla_prestador,
  abrangencia,
  tipo_servico
FROM
  `basedosdados.br_mdr_snis.prestador_agua_esgoto`
where
  sigla_prestador like 'SANEATINS%';
```

Pesquisa pelo Id do Município Itaituba, PA

```sql
SELECT
  ano,
  id_municipio,
  sigla_uf,
  id_prestador,
  prestador,
  sigla_prestador,
  abrangencia,
  tipo_servico
FROM
  `basedosdados.br_mdr_snis.prestador_agua_esgoto`
-- where id_municipio = '1502772' -- Curionópolis, PA
where id_municipio = '1503606' -- Itaituba, PA
```

Preparando os dados para realizar o `inner join`

```sql
-- tabela municipio_agua_esgoto
select
  ano,
  id_municipio,
  sigla_uf,
  populacao_urbana_atendida_agua,
  volume_agua_produzido,
  volume_agua_tratada_eta,
  extensao_rede_agua
from
  `basedosdados.br_mdr_snis.municipio_agua_esgoto` LIMIT 1000;


-- tabela municipio
select
  id_municipio,
  nome,
  sigla_uf,
  nome_uf
from
  `basedosdados.br_bd_diretorios_brasil.municipio` LIMIT 1000;
```

Estrutura dos dados após o `inner join` entre `municipio_agua_esgoto` e `municipio`

```sql
select
  mag.ano,
  m.id_municipio,
  m.nome as `Município`,
  m.sigla_uf,
  mag.populacao_urbana_atendida_agua,
  mag.volume_agua_produzido,
  mag.volume_agua_tratada_eta,
  mag.extensao_rede_agua
from
  `basedosdados.br_mdr_snis.municipio_agua_esgoto` as mag
inner join
  `basedosdados.br_bd_diretorios_brasil.municipio` as m
on
  mag.id_municipio = m.id_municipio
order by ano;
```

Criaçao do dataset `saneamento_por_municipio`

```sql
-- Cria um novo dataset
CREATE SCHEMA IF NOT EXISTS saneamento_por_municipio
OPTIONS(
  location="US"
);

-- Alterado o dataset adicionando uma descrição
ALTER SCHEMA saneamento_por_municipio
SET OPTIONS(
  description="Dataset criado durante o Workshop de Análise de dados públicos.",
  default_table_expiration_days=60
);

-- Pesquisa os datasets em um Projeto
SELECT
  schema_name
FROM
  mosaico-brasil-[codigo].`region-US`.INFORMATION_SCHEMA.SCHEMATA;
```

Cria a Consulta `produzida_por_municipio`. Essa será utilizada pelo Looker.

```sql
-- Cria uma nova View/Consulta
create or replace view saneamento_por_municipio.produzida_por_municipio as

select
  mag.ano,
  m.id_municipio,
  m.nome as `Município`,
  m.sigla_uf,
  mag.populacao_urbana_atendida_agua,
  mag.volume_agua_produzido,
  mag.volume_agua_tratada_eta,
  mag.extensao_rede_agua
from
  `basedosdados.br_mdr_snis.municipio_agua_esgoto` as mag
inner join
  `basedosdados.br_bd_diretorios_brasil.municipio` as m
on
  mag.id_municipio = m.id_municipio
order by ano;
```

Renomear os campos `sigra_uf` e `ano` e melhorar a apresentação no Looker.

```sql
-- Cria uma nova View/Consulta
create or replace view saneamento_por_municipio.produzida_por_municipio as

select
  mag.ano as `Ano`,
  m.id_municipio,
  m.nome as `Município`,
  m.sigla_uf as `Estado`,
  mag.populacao_urbana_atendida_agua,
  mag.volume_agua_produzido,
  mag.volume_agua_tratada_eta,
  mag.extensao_rede_agua
from
  `basedosdados.br_mdr_snis.municipio_agua_esgoto` as mag
inner join
  `basedosdados.br_bd_diretorios_brasil.municipio` as m
on
  mag.id_municipio = m.id_municipio
order by ano;
```

#### Nome do Relatório:

Workshop - Volume de água por município

### Formatação dos gráficos

#### Gráfico em Barras

Título do gráfico:

Volume de água produzido (dam³), por Ano

Legenda da Métrica:

Volume de Água Produzido (dam³)

Aumentar o número de Barras de 10 para 30.

Ordenar Por Ano.
Classificar por Ano.

Eixo X, Rotação em 90 graus.

#### Donuts gráfico

Título do gráfico: Proporção por Estado

### Link público do Looker:

https://lookerstudio.google.com/reporting/64409d66-9def-48db-9c17-c611eeb55fad
