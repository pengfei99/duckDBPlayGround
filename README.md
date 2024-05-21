# duckDBPlayGround

This repo is for playing with duckdb and demo

## What is duckDB?

DuckDB is a `light-weight, embedded(in-process), relational, column oriented, OLAP DataBase Management System (DBMS)`, it can run `in-memory` or `on-disk` mode: 

- embedded, in-process: means the DBMS features are running from within the application you’re trying to access from.
         You don't need to create a standalone dbms server.
- OLAP/column oriented: means the database is designed for data analysis. It can not handle large transactional data.
- vectorized processing?
- MVCC(Multiversion concurrency control):
- It provides command line client
- API for python, R, Java, c/c++, node.js, etc.
- Read various data format: parquet, csv, etc.
- for python users, it has connectors with pandas and arrow
- support os: linux, windows, mac

> duckDB is the sqlite for analytics

| mode       | Transactional     | Analytical            |
|------------|-------------------|-----------------------|
| Embedded   | SQLite, SolidDB   | DuckDB                |
| Standalone | Mysql, Postgresql | Snowflake, clickhouse |

## What are the limits of duckdb

- runs on single machine(single process per db), not a distributed system. As a result, no horizontal scalability
- meant for single player experience, not design for team collaboration(e.g. share tables/views)
- not design for storing transactions.
- If we have concurrent connections on the same duckDB, these connections must be `read-only`.
- 


## How to download

https://duckdb.org/docs/installation/?version=stable&environment=cli&platform=win&download_method=package_manager