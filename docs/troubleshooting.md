# Troubleshooting

## Pages stay empty / no data appears

Check the following:

1. Confirm at least one table or view has been granted under **Admin → Connections**.
2. Confirm a warehouse has been granted and can resume.
3. Confirm `EXECUTE TASK` has been granted so scheduled checks can run — or run
   **Admin → Assessment → Run assessment** for an immediate, on-demand result.
4. Confirm your selected time range contains activity.

Snowflake system views can have ingestion latency, so very recent activity may not
immediately appear.

## Scheduled checks never run

1. Go to **Admin → Schedules** and confirm checks have been **resumed** — nothing
   runs on a schedule, and no credits are spent, until this is turned on.
2. Confirm the `EXECUTE TASK` privilege has been granted to the application.
3. Confirm the granted warehouse exists and has not been dropped.

## Cost or governance pages are empty

These pages rely on `IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE`. Confirm this grant
has been made (Snowsight: *Data Products → Apps → Qualix → Privileges*). Some
`SNOWFLAKE` database views also have ingestion latency, so very recent activity may
not appear immediately.

## AI features show a one-line command instead of results

AI features require `SNOWFLAKE.CORTEX_USER` to be granted to the application:

```sql
GRANT DATABASE ROLE SNOWFLAKE.CORTEX_USER
TO APPLICATION QUALIX;
```

This cannot be requested through the install-time manifest, so an administrator must
grant it by hand. Every AI feature still works without it — rule suggestions fall back
to column statistics, and PII detection falls back to value patterns.

## Qualix feels slow

Try the following:

- Narrow the selected time range.
- Filter to one or more relevant tables or warehouses.
- Use a larger virtual warehouse for Qualix.
- Reduce the number of monitored tables analyzed simultaneously.

## Warehouse error

If Qualix cannot use the granted warehouse:

1. Confirm the warehouse name.
2. Confirm the warehouse has not been dropped.
3. Verify the application has the `USAGE` privilege on it.
4. Confirm the warehouse can auto-resume, or manually resume it.

## Permission error

Ask a Snowflake administrator to review the privileges granted to the application and
compare them with the privileges requested by Qualix during installation. See
[Required Privileges](privileges.md).
