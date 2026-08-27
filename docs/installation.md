# Installation

## Prerequisites

Before installing Qualix, ensure that you have:

- A Snowflake account
- Sufficient privileges to install a Snowflake Native App
- A Snowflake virtual warehouse that Qualix can use to run scheduled checks and
  power the app
- One or more tables (or views) you want Qualix to monitor
- Permission to grant the application access to the Snowflake system database where
  required

An **XS** or **S** warehouse is sufficient for many accounts. Larger or high-volume
Snowflake environments may benefit from a larger warehouse.

## Install from Snowflake Marketplace

1. Sign in to **Snowsight**.
2. Open **Marketplace**.
3. Search for **QUALIX**.
4. Open the Qualix listing.
5. Select **Get**.
6. Follow the Snowflake installation prompts.
7. Complete any requested privilege grants.

## Post-installation configuration

1. Open the app and go to **Admin → Connections**, then grant:
   - **Tables to monitor** — the data you want Qualix to check (read-only).
   - **Views to monitor** — optional, read-only.
   - **A warehouse** — used to run scheduled checks and power the app.
2. Grant Qualix permission to **run scheduled checks** and to **read account
   usage** (in Snowsight: *Data Products → Apps → Qualix → Privileges*). These power
   automatic monitoring and the cost/usage pages.

```sql
-- Allow Qualix to use a warehouse.
GRANT USAGE
ON WAREHOUSE <your_warehouse>
TO APPLICATION QUALIX;

-- Allow Qualix to run scheduled data-quality checks.
GRANT EXECUTE TASK
ON ACCOUNT
TO APPLICATION QUALIX;

-- Allow Qualix to read Snowflake account usage: cost/usage insights,
-- governance analysis, and the column metadata behind rule and PII suggestions.
GRANT IMPORTED PRIVILEGES
ON DATABASE SNOWFLAKE
TO APPLICATION QUALIX;
```

!!! warning
    Replace `<your_warehouse>` with the warehouse you want Qualix to use, and
    `QUALIX` with the name you installed the application under.

!!! note
    The exact privileges available to a Snowflake Native App depend on the
    application's manifest, requested references, Snowflake's Native App security
    model, and the privileges approved during installation. Follow the privileges
    shown by Snowflake during the installation flow if they differ from the examples
    above.

## Verify installation

After configuration:

1. Open **Apps** in Snowsight.
2. Select **Qualix**.
3. Launch the application.
4. Go to **Admin → Assessment** and select **Run assessment** — this runs every
   detector once, straight away, so you get results in the first few minutes rather
   than after the first scheduled cycle.
5. Go to **Admin → Schedules** and **resume automatic checks** if you want checks to
   run on a schedule.

If the dashboard remains empty, see [Troubleshooting](troubleshooting.md).
