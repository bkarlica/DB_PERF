# B*-Tree Index Performance

An individual academic project focused on experimentally evaluating the performance of B-tree indexes in PostgreSQL, including their impact on query execution time, data modification operations, disk space consumption, and index creation time.

## Project Overview

The goal of this project was to investigate how B-tree indexes affect database performance under different conditions, including table size, query selectivity, and indexed column data types. The experiments were conducted using PostgreSQL 18 and a dataset representing a crocheter's yarn inventory.

The project also examined the accuracy of PostgreSQL's query optimizer estimates and the trade-offs between faster data retrieval and the additional storage and maintenance costs associated with indexes.

## Technologies Used

- **PostgreSQL 18** – relational database management system
- **SQL** – table creation, synthetic data generation, indexing, and performance testing
- **PL/pgSQL** – implementation of a reusable data generation function
- **EXPLAIN ANALYZE** – query execution plan analysis and performance measurement
- **psql** – execution of SQL commands and timing measurements
- **Data visualization** – charts for comparing experimental results

## Methodology

1. **Database design** – created a `yarn_stash` table with 14 columns covering different data types, including `VARCHAR`, `TEXT`, `INTEGER`, `REAL`, `DATE`, and `SERIAL`.

2. **Synthetic data generation** – implemented a reusable PostgreSQL function, `generate_yarn_data(n)`, to generate random records. The experiments used up to 100,000 rows.

3. **Index creation** – created six B-tree indexes on columns of different data types and on a column expression, `UPPER(color)`.

4. **Query performance evaluation** – compared indexed queries with sequential scans to investigate the effects of table size, predicate selectivity, and indexed column type.

5. **Query optimizer analysis** – compared estimated and actual row counts using `EXPLAIN ANALYZE`, including the effect of running `ANALYZE` to update database statistics.

6. **DML performance testing** – measured the execution times of `INSERT`, `UPDATE`, and `DELETE` operations with and without indexes.

7. **Storage and index creation analysis** – measured index disk space consumption and index build times across different table sizes and column types.

8. **Result visualization** – created charts to compare execution times, storage requirements, and index creation performance.

## Results

- **Query performance:** Indexes provided approximately 4–7× speedups as table size increased from 10,000 to 100,000 rows for the tested color lookup. The expression index on `UPPER(color)` achieved an approximately 9× speedup in a separate comparison.
- **Query selectivity:** For predicates matching approximately 90% of rows, PostgreSQL switched to a sequential scan because retrieving such a large proportion of the table through an index was less efficient.
- **Optimizer accuracy:** After running `ANALYZE`, estimated row counts were generally close to actual counts, although less accurate estimates occurred for very low-selectivity queries.
- **DML operations:** Inserting 10,000 rows was approximately 3.2× slower with six indexes. However, indexed `UPDATE` and `DELETE` operations were faster in the tested scenarios because indexes helped locate matching rows.
- **Disk space:** At 100,000 rows, the six indexes occupied approximately 11 MB compared with 30 MB of table data.
- **Index creation:** Index build times increased with table size. Numeric and date indexes were generally faster to create than text and expression indexes.

## Key Takeaways

The experiments demonstrated that B-tree indexes can substantially improve query performance when a relatively small proportion of rows matches a condition. However, indexes also introduce storage overhead and increase the cost of inserting data.

The results highlight the importance of query selectivity, appropriate index selection, and up-to-date database statistics when optimizing relational database performance.

## Author

Karla Budimir
