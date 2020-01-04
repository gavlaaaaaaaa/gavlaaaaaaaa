--- 
layout: post 
title: Apache Sqoop Advanced
author: Lewis Gavin
tags: 
- bigdata 
- sqoop 
- ingestion 
--- 

Here we are going to look at using Sqoop to do a little more than simply import or export data, including ensuring data imported is in a suitable format for use with Hive. 

For the basics see the initial post here: [Apache Sqoop for data ingestion](http://gavlaaaaaaaa.github.io/Data-Ingestion-SQOOP/)


## Importing LOBs
Large objects can be imported using Sqoop but their size will determine how they are handled.

By default Large objects over 16MB will be spilled to disk into a directory named _lobs in the target dir. Otherwise they will be materialised in memory and imported with the rest of the data. 

To change the memory size for importing large objects, use the following parameter:

`--inline-lob-limit`

### Using imported LOBs with Hive 
Something to consider when importing large objects that are going to be used with Hive is their content. Especially the inclusion of delimiter characters or newline characters.

Large objects are usually used to store text or documents that often contain more exotic characters. If one of these characters happens to be the delimiter used by Hive then you may experience issues when viewing data. 

**Solutions:**

1. Rather obvious, but just change your hive delimiter to something that's not in the data

2. If you can't guarantee a delimiter won't be in the data, then utilise the ^A character as your hive and import delimiter. This can be set using `\\001`, which is the octal representation for the ^A Utf-8 character. Use this in conjunction with the solution below. 

Similarly, if the large object data contains new line characters then data will spill onto new lines meaning this spilled data will be treated as it's own row by hive. This will cause extra rows that will not be in the same layout as the rest of the data. 

**Solutions:** 

1. Use Sqoops `--hive-drop-import-delims` or `--hive-delims-replacement` parameters to remove newline or ^A characters from the data or replace them with your own defined string. 

2. Make your hive table an Avro table and use the `--as-avrodatafile` parameter when importing. Avro doesn't use newline characters as row delimiters like the text file format, meaning new lines can be preserved within data columns. 

## Incremental loads
If you want to keep loading new changes to the source data on a regular basis, then you should make use of Sqoops Incremental load feature. You pass Sqoop the name of the column to check against, the last value imported and the Incremental mode, then Sqoop will bring in anything newer than this value. 

**Note: the Sqoop docs state the column should not be of type CHAR/NCHAR/VARCHAR/VARNCHAR/ LONGVARCHAR/LONGNVARCHAR**

### Using a last modified timestamp 
If the source data can have new records inserted or have the current records updated, and your data contains a last modified timestamp then you can use Sqoops last modified mode. 

`--incremental lastmodified --check-column update_timestamp_col --last-value "2015-01-01 12:01:00"`

This will bring in all data with a date newer than the first of Jan 2015 within the update_timestamp_col column. 

### Using append mode
If your source data uses an insert only model then you can utilise the append model. This requires a column with an incrementing value within the data, preferably a key column. 

`--incremental append --check-column order_id --last-value 27`

This will import all rows that contain a value greater than 27 within the order_id column. 


## Type Mapping
By default Sqoop will create a Java class to represent the data being imported and map each columns type from its SQL type to its Java equivalent, it can also do this for Hive types too if you're importing straight into Hive.

Sometimes you may find the chosen mapping to be inappropriate and may want to overwrite it. To do this you use a parameter that accepts a comma separated list in the format of column_name=type. 

#### Java Type mapping
`--map-column-java order_id=String,amount=Double`

This would imoort the order_id column as a String and the amount column as a Double. 

#### Hive Type mapping
`--map-column-hive order_id=STRING,amount=DECIMAL`

This would import the order_id as a Hive string and the amount column as a Decimal. 

## Validating Imports
Sqoop offers the ability to validate your Imports and exports by performing a count comparison to ensure all rows of data were transferred successfully. 

This is especially useful to let you know if data has been imported into text files that contains newline characters as this would cause the counts to be mismatched. 

To utilise this feature simply supply the `--validate` parameter at the end of your sqoop import or export statement.
