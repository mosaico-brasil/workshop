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
