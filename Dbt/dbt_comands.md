# Dbt command

dbt run-operation stage_external_sources 
dbt run --select my_model+          # select
dbt run --select +my_model          # select my_model and all parents
dbt run --select +my_model+         # select
dbt run --full-refresh -s @stg_brd_partnerships.sql -t dev

# dbt test

## running test
on models with tag:mongo_bi excluding models with tag=dev

```bash
dbt test --exclude tag:dev -s tag:mongo_bi
```

## running test on specific module
revenue_monthly_by_customer_and_product

```bash
dbt test -s revenue_monthly_by_customer_and_product
```

## run all
models under folder marts/priority

```bash
dbt run --models marts.priority -t prod
```

```bash
dbt run --models +marts.priority -t prod  # all models' children
```

# run external table on selected table:
bi_pg_hubspot_extbl.hubspot_extbl_companies

## https://github.com/dbt-labs/dbt-external-tables

```bash
dbt run-operation stage_external_sources \
  --vars "ext_full_refresh: true" \
  --args "select: bi_pg_hubspot_extbl.hubspot_extbl_companies" -t prod
```

```bash
dbt run --select my_model+          # select my_model and all children
```

```bash
dbt run --select +my_model          # select my_model and all parents
```

```bash
dbt run --select +my_model+         # select my_model and all parents and children
```

## running with vars

```bash
dbt run -s stg_brd_partnerships.sql --vars '{"logical_date":"2023-10-01"}' -t dev
```

```bash
dbt run --full-refresh -s @stg_brd_partnerships.sql -t dev
```

## list models

```bash
dbt list -s tag:integrity_check --resource-types=model --exclude tag:dev
```

quarterly_nrr_forecast_by_groups

## generate DBT DOC

```bash
dbt docs generate
```

```bash
dbt docs serve
```

# run model model1 in stage (-t stg) but getting the data for that table from production tables

```bash
dbt run -s model1 --vars '{"logical_date": "'$YESTERDAY_DATE'"}' -t stg --defer --favor-state --state dbt_prod_artifacts
```

```bash
dbt run -s model1 -t stg --defer --favor-state --state dbt_prod_artifacts
```

1. --defer

--state dbt_prod_artifacts

- Points to the directory containing the manifest.json from a previous production run.
- Your documentation at README.md indicates that artifacts are copied to dbt_prod_artifacts during CI/CD.
     This tells dbt "where to look" to find the production versions
     of the models for deferral.

--favor-state

- If a model exists in both your current project and the
     provided --state manifest,
     this flag tells dbt to prefer the definition in the state manifest for
     deferral purposes. This ensures consistency with the production state when
     bridging environments.

## get results in format database.schema.table

```bash
dbt --quiet list --select "path:models/staging/s3/s3_brd_logzio"+ -t prod --resource-type model --output json | jq -r '.config.database + "." + .config.schema + "." + .name'
```