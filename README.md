# duckDBPlayGround

This repo is for playing with duckdb and demo

## 1. What is duckDB?

DuckDB is a `light-weight, embedded(in-process), relational, column oriented, OLAP DataBase Management System (DBMS)`, it can run `in-memory` or `on-disk` mode: 

- embedded, in-process: means the DBMS features are running from within the application you’re trying to access from.
         You don't need to create a standalone dbms server.
- OLAP/column oriented: means the database is designed for data analysis. It can not handle large transactional data.
- vectorized processing: Predicate pushdown and projection pushdown
- MVCC(Multiversion concurrency control):  The MVCC implementation is inspired by the paper `Fast Serializable Multi-Version Concurrency Control for Main-Memory Database Systems` by Thomas Neumann, Tobias Mühlbauer and Alfons Kemper.
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

## 2. What are the limits of duckdb

- runs on single machine(single process per db), not a distributed system. As a result, no horizontal scalability
- meant for single player experience, not design for team collaboration(e.g. share tables/views)
- not design for storing transactions.
- If we have concurrent connections on the same DB file, these connections must be `read-only`.



## 3. How to play the examples

For the command line client demo. Just follow the instructions in [01.duckdb_cli.md](examples/01.duckdb_cli.md)

For the jupyter notebook demo. Follow the below instructions

### Get the demo data

The demo data can be downloaded from the below link:
- The [Pathologies by region and age](https://www.data.gouv.fr/fr/datasets/pathologies-effectif-de-patients-par-pathologie-sexe-classe-dage-et-territoire-departement-region/)
- The [region shape file](https://geodata.ucdavis.edu/gadm/gadm4.1/shp/gadm41_FRA_shp.zip)


### 3.1 Get the project source

```shell
git clone https://github.com/pengfei99/duckDBPlayGround.git
```

### Install a python virtual environment

Our code runs on python `3.11.8`. So we recommend you to use the same python version

```shell
# create a virtual env
python -m venv $VENV_PATH

# activate virtual env
source $VENV_PATH/bin/activate
```


### Install the project dependencies

```shell
cd /path/to/duckDBPlayGround

pip install -r requirements.txt 
```

### Run the notebook

```shell
# go to the project source folder, and run
jupyter notebook
```