# Projeto Mosaico Brasil

## Workshop Análise de Dados Públicos com o projeto Base de Dados e o Google Cloud

Primeira etapa do Workshop: Quantas pessoas com o seu nome na sua cidade?

### Consultas na tabela `basedosdados.br_ibge_nomes_brasil.quantidade_municipio_nome_2010`

1a consulta

```sql
select
  id_municipio,
  nome,
  quantidade_nascimentos_ate_2010
FROM
  `basedosdados.br_ibge_nomes_brasil.quantidade_municipio_nome_2010` LIMIT 1000
```

2a consulta

```sql
select
  id_municipio,
  nome,
  quantidade_nascimentos_ate_2010
from
  `basedosdados.br_ibge_nomes_brasil.quantidade_municipio_nome_2010`
where nome = 'Tamara'
```

### Consultas na tabela `basedosdados.br_bd_diretorios_brasil.municipio`

```sql
select
  id_municipio,
  nome,
  sigla_uf,
  nome_uf
from
  `basedosdados.br_bd_diretorios_brasil.municipio` LIMIT 1000
```

### Junção entre as duas tabelas, `quantidade_municipio_nome_2010` e `municipio` utilizando o `inner join`

```sql
-- Usando o inner join
-- Filtro por Nome
select
  m.id_municipio,
  m.nome as nome_municipio,
  m.sigla_uf,
  m.nome_uf,
  qtn.nome,
  qtn.quantidade_nascimentos_ate_2010
FROM
  `basedosdados.br_ibge_nomes_brasil.quantidade_municipio_nome_2010` as qtn
inner join `basedosdados.br_bd_diretorios_brasil.municipio` as m
  on qtn.id_municipio = m.id_municipio
where
  qtn.nome = 'Tamara'
```

Aplicando o filtro por Cidade

```sql
-- Usando o inner join
-- Filtro por Nome e Cidade
select
  m.id_municipio,
  m.nome as nome_municipio,
  m.sigla_uf,
  m.nome_uf,
  qtn.nome,
  qtn.quantidade_nascimentos_ate_2010
FROM
  `basedosdados.br_ibge_nomes_brasil.quantidade_municipio_nome_2010` as qtn
inner join `basedosdados.br_bd_diretorios_brasil.municipio` as m
  on qtn.id_municipio = m.id_municipio
where
  qtn.nome = 'Tamara'
  and m.nome = 'Itaituba' -- Filtro por Cidade
```
