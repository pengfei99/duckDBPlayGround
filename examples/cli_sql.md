# Duck db cli demo

## 1. Install the cli
Goto the download [page](https://duckdb.org/docs/installation/index?version=stable&environment=cli&platform=linux&download_method=direct)

```shell
cd /home/pengfei/Tools/DuckDB

wget https://github.com/duckdb/duckdb/releases/download/v0.10.2/duckdb_cli-linux-amd64.zip

# unzip the bin
unzip duckdb_cli-linux-amd64.zip
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

```

We can also run the duckdb directly on disk mode

```shell
# run a on disk mode duck db
./duckdb /home/pengfei/data_set/demo_chu/demo_base/mydb.db
```

## Create tables

```shell
# create a base table from a csv file
CREATE OR REPLACE TABLE patho_csv AS SELECT * FROM read_csv_auto('/home/pengfei/data_set/demo_chu/pathologies.csv');

CREATE OR REPLACE VIEW patho_parquet AS SELECT * from read_parquet('/home/pengfei/data_set/demo_chu/pathologies.parquet');
```

## Better sql experience

### no select required

```shell
# no select required,
from patho_parquet limit 10;

# revert select
from patho_parquet select * limit 10;

```

### exclude from select

```shell
from patho_parquet select * exclude ("patho_niv2", "patho_niv3") limit 2;
```

### Replace column

We can use the `replace` keyword to replace old column with new columns

```shell
select * exclude ('patho_niv2', 'patho_niv3') replace(CAST(annee AS INT) AS annee) FROM patho_parquet;

```

### Create a new view 

```shell
create or replace view patho_clean as select * exclude ("patho_niv2", "patho_niv3") replace(CAST(annee AS INT) AS annee) FROM patho_parquet;
```

### groupBy all

WE can use keyword `All` to replace all column names in the select statement which are not aggregation.
This can save us a lot of time, if the select is long

```shell
# classic sql
SELECT dept, sexe, annee, patho_niv1, SUM(ntop), AVG(npop)
        FROM patho_clean
        GROUP BY dept, sexe, annee, patho_niv1;

# duck db simple sql
SELECT dept, sexe, annee, patho_niv1, SUM(ntop), AVG(npop)
        FROM patho_clean
        GROUP BY ALL;
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

### Drop tables

In memory mode, no need to drop tables

```shell
drop table patho_csv;
```

