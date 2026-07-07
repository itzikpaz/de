### CREATE Snowflake user

### Create PAT user (Programmatic Access Token)
```sql
USE ROLE ACCOUNTADMIN;

-- Optional: verify available warehouses
SHOW WAREHOUSES;

-- Remove the user if it already exists (optional)
DROP USER IF EXISTS USR_TEST;

CREATE USER IF NOT EXISTS USR_TEST
  TYPE = SERVICE
  DEFAULT_ROLE = <role_name>
  DEFAULT_WAREHOUSE = <warehouse_name>
  NETWORK_POLICY = '<network_policy_name>';  -- usually required for service users

GRANT ROLE <role_name> TO USER USR_TEST;

-- Create a programmatic access token for the service user.
-- The generated token value is displayed only once.
ALTER USER IF EXISTS USR_TEST
  ADD PROGRAMMATIC ACCESS TOKEN USR_TEST_TOKEN
  DAYS_TO_EXPIRY = 30
  ROLE_RESTRICTION = '<role_name>';

-- USR_TEST_TOKEN is the token name; choose any identifier you prefer.
```