# Duck db cli demo

## 1. Install the cli
Goto the download [page](https://duckdb.org/docs/installation/index?version=stable&environment=cli&platform=linux&download_method=direct)

```shell
cd /home/pengfei/Tools/DuckDB

wget https://github.com/duckdb/duckdb/releases/download/v0.10.2/duckdb_cli-linux-amd64.zip

# unzip the bin
unzip 
```

## Start a duckdb instance

```shell
# if you skip the path to db, the duckdb will run in memory mode
./duckdb

# If DuckDB is already running, use the attach command to connect to a database.
ATTACH DATABASE '/home/pengfei/data_set/demo_chu/demo_base/mydb.db' AS mydb;

# show available database
show databases;

# show available tables
show tables;

# get table details
select table_name, table_type from INFORMATION_SCHEMA.TABLES;

# run a on disk mode duck db
./duckdb /home/pengfei/data_set/demo_chu/demo_base/mydb.db
```

## Examine the existing table

```shell

```

## Create tables

```shell
# create a base table from a csv file
CREATE TABLE sf_fire AS SELECT * FROM read_csv_auto('/home/pengfei/data_set/sf_fire/sf_fire.csv');

CREATE OR REPLACE VIEW sf_fire_parquet AS SELECT * from read_parquet('/home/pengfei/data_set/sf_fire/sf_fire_snappy.parquet');
```

## Better sql experience

### no select required

```shell
# no select required,
from sf_fire limit 10;

# revert select
from sf_fire select * limit 10;

```

### exclude from select

```shell
from sf_fire select * exclude ("Unit ID","Call Number", "Call Type") limit 2;
```

### groupBy all

WE can use keyword `All` to replace all column names in the select statement which are not aggregation.
This can save us a lot of time, if the select is long

```shell
# classic sql
select City, CallType, count(*) as call_count  from sf_fire_parquet group by City, CallType limit 10;

# duck db simple sql
select City, CallType, count(*) as call_count  from sf_fire_parquet group by all limit 10;

```

### Various display mode

```shell
# line mode
.mode line

from sf_fire select "Call Number", "Call Type" limit 1;

# mode markdown
.mode markdown
```

The `.mode` command may be used to change the appearance of the tables returned in the terminal output. 
These include the default :
- `line` mode: print data in one line
- `duckbox` mode: default display mode
- `csv/json` mode: for ingestion by other tools, 
- `markdown/latex` mode: for generating documents
- `insert` mode for generating SQL statements.

### output mode

By default, the output is printed on the stdout, we can change it to redirect output to a file

```shell
# output to a file
.output /tmp/out.md

# change it back to stdout
.output
```

