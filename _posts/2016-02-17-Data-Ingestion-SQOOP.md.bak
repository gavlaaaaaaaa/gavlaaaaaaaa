---
layout: post
title: Apache Sqoop for data ingestion
author: Lewis Gavin
tags:
- bigdata
- sqoop
- ingestion
---

## What is apache SQOOP?

SQOOP is an open source tool that allows you to ingest data from many different types of databases into hdfs. It also has the ability to export data from hdfs back into an external database.


## Importing a single table 

I will be using a MySQL database as my external source to import data into hdfs.

To import a single table, as a minimum, SQOOP requires two parameters: a connection string and a table. To go with the connection string, you may need to specify a username and password.

`sqoop-import --connect jdbc:mysql://localhost:3306/test --username user --password 1234 --table test_table`

This will import all rows and columns in this table and import it into the current user base directory within HDFS as delimited text files, the number of mappers used (default 4) determines the number of output data files.


### Specifying the target directory 

To specify a target directory for the import, SQOOP has two options. --target-dir or --warehouse-dir. The difference is that --target-dir is a full directory path and the data files will be created directly inside the specified folder. --warehouse-dir is used to specify a base directory within hdfs where SQOOP will create a sub folder inside with the name of the source table, and import the data files into that folder.

`sqoop-import --connect jdbc:mysql://localhost:3306/test --username user --password 1234 --table test_table --target-dir /user/abc/test`

or

`sqoop-import --connect jdbc:mysql://localhost:3306/test --username user --password 1234 --table test_table --warehouse-dir /user/abc `

**Note: The target directory MUST NOT already exist, otherwise SQOOP will fail.**


### Specifying the file formats

There are a number of file formats available when writing data files to hdfs using SQOOP. These include *Avro, Parquet, Sequence and Text files*. There are parameters for each that can be added to the above SQOOP commands, these are:

`--as-avrodatafile` `--as-parquetfile` `--as-sequencefile` `--as-textfile`

By default SQOOP will use --as-textfile if no parameter is specified.

**If you use `--as-avrodatafile` then SQOOP will create an Avro schema to represent this data within the users current working directory.**

**`--as-parquetfile` is only available in sqoop 1.4.5 onwards.**


### Specifying the delimiter

By default SQOOP will use a comma as the delimiter for the text file. If you wish to change this you can do so by using the `--fields-terminated-by` parameter.

`--fields-terminated-by \|`

**Note: you may need to escape the delimiter using a back slash**


### Importing into Hive

There are a number of ways to import data directly into Hive using SQOOP. You can have SQOOP create a hive table for you converting the source schema into an appropriate hive schema, or you can SQOOP into an already created hive table. If you allow SQOOP to create you a table it will be a Hive managed table and *not* an external one.

To have SQOOP create you a table use the following syntax.

`sqoop-import --connect jdbc:mysql://localhost:3306/test --username user --password 1234 --table test_table --target-dir /user/abc/test --hive-import`

To import into an already created hive table use the following.

`sqoop-import --connect jdbc:mysql://localhost:3306/test --username user --password 1234 --table test_table --target-dir /user/abc/test --hive-table hive_table_name --hive-database hive_db`

**Note: it is recommended to also use and set the `--null-string '\\N'` and `--null-non-string '\\N'` parameters. This tells SQOOP that nulls should be converted to a \N character, which is the default null character used by Hive, allowing null operations such as `IS NULL` to be used on the data.**


## Import all tables

To import all tables from a database into HDFS using SQOOP, you can use the `sqoop-import-all-tables` command. This requires the same connect, username and password variables however when specifying a the target directory within hdfs, **you must use** `--warehouse-dir`, as SQOOP will make you sub folders per table. 

`sqoop-import-all-tables --connect jdbc:mysql://localhost:3306/test --username user --password 1234 --fields-terminated-by \| --warehouse-dir /user/abc `

