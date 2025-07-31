# Duck DB

Installation: https://duckdb.org/docs/installation/.

```bash
winget install DuckDB.cli
```

We can create a database to persist. Make sure you are in right directory:

```bash
cd data
duckdb mydb.db

D SHOW DATABASES;
```

Create table in database:

```bash
D CREATE TABLE mydb.phones AS SELECT 15 AS version, 'iPhone' AS model;
FROM mydb.phones;
```

You can also launch DuckDB with a database in read-only mode to avoid modifying the database:

```bash
duckdb -readonly /data/myawesomedb.db
```

If DuckDB is already running, use the `attach` command to connect to a database at the specified file path.

```bash
ATTACH DATABASE '/data/myawesomedb.db' AS mydb;
```

MotherDuck as could server as a cloud data warehouse, it will automatically manage the DuckDB databases for you.

## Reading and Display Data

We can read directly files.

Let's read the CSV file.

```bash
SELECT * FROM read_csv_auto('data/orders.csv');
```

We can create a table, use a `CREATE TABLE x AS` (CTAS) statement and it will save data in database.

```bash
CREATE TABLE mydb.netflix_top10 AS SELECT * FROM read_csv_auto('data/orders.csv');
```

To write data to a CSV file, use the `COPY` command and specify the delimiter. For Parquet files, simply specify the file format:

```bash
COPY 'data/orders.csv' TO 'data/orders.parquet' WITH (FORMAT 'PARQUET');
```

To read data from a Parquet file, use the `read_parquet` command:

```bash
SELECT * FROM read_parquet('data/orders.parquet');
```

We can read these files from your local filesystem, a http endpoint or a cloud blob store like AWS S3,Azure Blob Storage or Google Cloud Storage.

## Display modes, output options

DuckDB CLI offers various ways to enhance your experience by customizing the data display and output options.

We can use the `.mode` command to change the appearance of tables returned in the terminal output.

If we are dealing with long nested JSON, we can change the mode to `line` or `JSON` to have a better view of data.

```bash
.mode line
SELECT * FROM 'data/namesfile.json';
```

We can output the result to a Markdown file, by setting the display mode to Markdown with `.mode markdown`.

Combine this with the `.output` or `.once` command to write the result directly to a specific file. The `.output` command writes all the output of the different results you run, while `.once` does it just once.

```bash
.mode markdown
.output myfile.md
```

## Running commands and exiting

DuckDB CLI allows you to run a SQL statement and exit using the `-c` option parameter. For example, if you use a `SELECT` statement to read a Parquet file:

```bash
duckdb -c "SELECT * FROM read_parquet('data/orders.parquet');"
```