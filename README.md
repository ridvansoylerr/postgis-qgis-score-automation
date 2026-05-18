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
