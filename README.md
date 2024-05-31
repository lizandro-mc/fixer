
# Nectarvet "Fixer" Project
*By Lizandro Ramirez, May 30, 2024*

## Study Case with 'TPVC'

### Step #1: Move Raw Migration Data to Snowflake
- Move TPVC raw migration models to Snowflake:
  - **Destination:** `dbt_dev_db.tpvc_raw_migration_[model_name]`

### Step #2: Replicate Mongo Production DB to Snowflake
- Using Fivetran, replicate the TPVC Mongo production database to Snowflake:
  - **Destination:** `PC_FIVETRAN_DB.NECTARVET_PROD_TPVC_TPVC`
- Verify the updated data in Snowflake against the TPVC Mongo database. Ensuring no data is lost from the production database is a critical step in this process.

### Step #3: Create dbt Core Project "Fixer"
- Configure the dbt project to work on `dbt_dev_db`, `dbt_prod_db`, and local Postgres.
- Copy from existing metrics and data migration projects.
- **Clean Layer:**
  - Clean staging models in `NECTARVET_PROD_TPVC_TPCV`.
  - Clean Fivetran metadata in the 'fixer' models.
- **Fixer Layer:**
  - Perform all cleaning and fixing of data for production based on Roberlina's list of problems.
  - **Destination for fixer models:** `dbt_prod_db.fixer_[mongo_db_name]` (e.g., `dbt_prod_db.fixer_tpcv`).

### Step #4: Move Snowflake Fixer Models to Local Postgres
- Download fixer models from Snowflake to local Postgres (can be in .csv files).
- In the final layer (gold), perform transformations over casting data to confirm valid data types.
- From Postgres, move the models to MongoDB 'TPCV' fixer models.
  - Confirm the number of rows and IDs before deleting old models.

### Note
- TODO: Explore the possibility of moving data directly from Snowflake, eliminating the need for Postgres in all processes. Check Snowpipe capabilities in Snowflake.