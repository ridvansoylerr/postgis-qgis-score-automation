# PostGIS QGIS Score Automation

This project demonstrates an automated GIS production score reporting workflow using QGIS, PostGIS, SQL, ODBC and Excel Power Query.

## Purpose

The purpose of this workflow is to automate the daily calculation of GIS digitization scores from line geometry data stored in a PostGIS database.

In many GIS production workflows, daily production scores are calculated manually. This project shows how line geometries can be measured, grouped by user and date, and automatically transferred into an Excel-based score report.

## Workflow

1. Line features are digitized in QGIS.
2. Geometry data is stored in PostGIS tables.
3. SQL calculates daily kilometer-based production values.
4. Excel Power Query connects to the database through ODBC.
5. The score table is refreshed automatically in Excel.

## Technologies Used

- QGIS
- PostgreSQL
- PostGIS
- SQL
- ODBC
- Excel Power Query
- Excel Dashboard

## Example Output

| date | user_a_km | user_a_drawing | user_b_km | user_b_drawing | total_km | total_drawing |
|---|---:|---:|---:|---:|---:|---:|
| 2026-05-01 | 12.50 | 3.25 | 9.80 | 2.10 | 22.30 | 5.35 |
| 2026-05-02 | 14.20 | 4.10 | 11.40 | 2.85 | 25.60 | 6.95 |

## Workflow Diagram

```text
QGIS Digitization
        ↓
PostGIS Database
        ↓
SQL Daily KM Calculation
        ↓
ODBC / Power Query
        ↓
Excel Score Dashboard
## How to Use

This repository is a template project. To use it with your own GIS data, you need to update the sample table names, column names, user IDs and ODBC connection name according to your own PostGIS database.

### 1. Prepare your PostGIS data

Your PostGIS database should contain line geometry tables. These tables should include at least:

- a geometry column
- a user/operator ID column
- a date column

Example table structure:

```text
sample_schema.tracking_line_table
- id
- user_id
- digitized_date
- geom

sample_schema.road_marking_line_table
- id
- created_by
- created_date
- geom
```

The geometry column should contain line geometries. The SQL query uses `ST_Length(geom::geography)` to calculate line length in meters and converts it to kilometers.

### 2. Update the SQL query

Open the SQL file:

```text
sql/daily_score_query.sql
```

Replace the sample schema, table and column names with your own database structure.

For example:

```sql
sample_schema.tracking_line_table
```

should be replaced with your own schema and table name.

Also update the user IDs according to your own users or operators:

```sql
WHERE user_id IN (1, 2)
```

and:

```sql
WHERE created_by IN (1001, 1002)
```

### 3. Test the SQL query

Before connecting the query to Excel, test it in a database client such as DBeaver or pgAdmin.

The expected output should look like this:

```text
report_date | user_a_km | user_a_drawing | user_b_km | user_b_drawing | total_km | total_drawing
```

If the query works correctly, it should return daily kilometer-based production scores.

### 4. Create an ODBC connection

Create an ODBC connection to your PostgreSQL/PostGIS database.

Example DSN name:

```text
your_odbc_dsn_name
```

This DSN name will be used inside the Power Query script.

### 5. Update the Power Query script

Open the Power Query file:

```text
powerquery/excel_powerquery_score.m
```

Find this line:

```powerquery
Source = Odbc.Query("dsn=your_odbc_dsn_name", SQL)
```

Replace `your_odbc_dsn_name` with your own ODBC DSN name.

Example:

```powerquery
Source = Odbc.Query("dsn=postgis_score_report", SQL)
```

### 6. Use the query in Excel

In Excel, go to:

```text
Data → Get Data → From Other Sources → Blank Query
```

Then open:

```text
Advanced Editor
```

Paste the Power Query code from:

```text
powerquery/excel_powerquery_score.m
```

Then click:

```text
Done → Close & Load
```

The query result will be loaded into Excel as a table.

### 7. Refresh the report

After the table is loaded into Excel, the report can be refreshed anytime with:

```text
Data → Refresh All
```

You can also enable automatic refresh from the query properties.

### 8. Recommended Excel settings

For safer and more stable reporting, it is recommended to:

```text
Enable: Refresh data when opening the file
Disable: Enable background refresh
```

This ensures that the report is fully refreshed before it is used.

## Safety Notes

This workflow only reads data from the database.

The SQL query uses `SELECT` statements only.

It does not use:

```sql
INSERT
UPDATE
DELETE
ALTER
DROP
TRUNCATE
```

For production usage, it is recommended to use a read-only database user with permission only to select from the required tables.

Example:

```sql
GRANT SELECT ON sample_schema.tracking_line_table TO reporting_user;
GRANT SELECT ON sample_schema.road_marking_line_table TO reporting_user;
```

This helps prevent accidental changes to production GIS data.
