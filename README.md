# SQL for Economists — Joins, Windows and the Cardinality Trap

## Objective
I translated common pandas workflows into SQL with DuckDB and tested where the two give different results, especially after joins and with missing values.

## Methodology
- Rewrote pandas select, filter, sort, groupby and merge operations as SQL queries in DuckDB, and checked that each pair returned the same rows.
- Ran a one-to-many LEFT JOIN of customers to a second table, compared row counts before and after, and ran the same Welch t-test on revenue in both versions of the data.
- Used window functions (`AVG() OVER (PARTITION BY ...)` and `RANK()`) to add group averages and ranks to each row without collapsing the table.
- Applied the filter `WHERE income != 50000` to a 5-row survey table in SQL and the equivalent filter in pandas, then compared which rows each kept.
- Queried a public Parquet file of about 3 million NYC taxi trips over HTTP without downloading it, so DuckDB read only the columns the query needed.
- Asked an AI to translate three of my pandas steps into SQL, revised my prompt once, and compared the row counts of its queries with my pandas results.

## Key Findings
- **Pandas and SQL agree on basic operations.** Select, filter, sort, groupby and merge returned the same rows in both.
- **Joins can change statistical results.** The one-to-many LEFT JOIN turned [YOUR VALUE] customer rows into [YOUR VALUE] rows. The Welch t-test on the same revenue figures went from t = [YOUR VALUE] (p = [YOUR VALUE]) to t = [YOUR VALUE] (p = [YOUR VALUE]). The repeated rows shrank the standard error and moved the difference, so checking row counts after every join is necessary before running any test.
- **Window functions keep the row-level data.** Group averages and ranks sat next to each original row, which a groupby does not do.
- **SQL and pandas handle missing values differently.** `WHERE income != 50000` kept [YOUR VALUE] of 5 survey rows in SQL because [YOUR one-line reason], while the same filter in pandas kept [YOUR VALUE].
- **DuckDB can query large remote files directly.** I queried about 3 million taxi trips without downloading the file.
- **AI-generated SQL needs checking.** Comparing the row counts of the AI's queries against my pandas results was how I verified its three translations after one prompt revision.
