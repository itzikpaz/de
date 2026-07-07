# check if database cdc enabled

```sql
SELECT name AS Database_Name,
       is_cdc_enabled
FROM sys.databases
WHERE name = DB_NAME();
```

# get list tables with cdc enabled:

```sql
SELECT s.name AS Schema_Name,
       t.name AS Table_Name,
       t.is_tracked_by_cdc
FROM sys.tables t
JOIN sys.schemas s ON t.schema_id = s.schema_id
WHERE t.is_tracked_by_cdc = 1;
```

# enable cdc to a table:

```sql
USE OLA;
EXEC sys.sp_cdc_enable_table @source_schema = 'dbo', @source_name = 'FNCBAL', @role_name = NULL;
EXEC sys.sp_cdc_enable_table @source_schema = 'dbo', @source_name = 'GENLEDGERS', @role_name = NULL;
EXEC sys.sp_cdc_enable_table @source_schema = 'dbo', @source_name = 'FNCBAL5', @role_name = NULL;
```
