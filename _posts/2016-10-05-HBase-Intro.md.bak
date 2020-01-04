--- 
layout: post 
title:  Intro to Apache HBase
author: Lewis Gavin 
comments: true 
tags: 
- bigdata 
- hadoop
- hbase 
---

![Intro to Apache HBase](../images/hbase_intro.png)

Apache HBase is a distributed, NoSQL database designed specifically to sit on top of Hadoop and is modeled closely on Google's BigTable.

**Designing an HBase application requires developers to engineer the system using a data-centric approach - not a relationship-centric approach.** This is obviously a lot different than the approach you would take coming from an standard SQL background - *so what are the benefits?*

## When to use HBase

HBase as a technology has a number of features that give it a wide range of use cases. Due to it sitting on Hadoop, an obvious use case is when you have large amounts of data that continuously grows.

You might ask why not use Hive to solve the same problem, well unlike Hive, HBase is designed to be faster to access to smaller, more specific data sets. Rather than using bulky map reduce jobs to churn through lots of data, it focuses on writing lots of data fast and reading small amounts very fast. 
Facebook managed to get speeds of around 1.5m operations per second.

The way it can randomly read and write data so fast is due to how it stores data. Data is stored in HBase with a key. It can then use this key to lookup items quickly, and due to there being no penalty for sparse columns, you can store as much data as you want on a single row.

It also has in-memory caching and will allow certain nodes or blocks to be more favorable for caching too. This means less disk I/O, which we all know is one of the downfalls of MapReduce on Hadoop. It also means if you scale your cluster and add new nodes - you get more cache.

| Do use HBase if... | Do not use HBase if... |
|---|---|
| you have random read and writes | you are only appending to a dataset |
| you have thousands of operations per second on lots of data | your primary usage is ad-hoc analytics - non-deterministic access patterns |
| your access patterns are well known and relatively simple | your data easily fits on one large node |

It has no integrated SQL support as it is a NoSQL database - however Hive and Impala can be used with HBase if you require this flexibility. 

HBase also has no transaction support or no multiple index tables. So this shouldn't be replacing use cases where these are important.

## HBase Tables

A HBase table has the following properties:
1. **Rows, columns and column families**
2. **Row key** per row for fast lookup
3. Sorted for speed
4. Columns hold the data
5. Each **column** has **column family** (there can be one or more column family per table)

![HBase table conceptual view](https://www.tutorialspoint.com/hbase/images/table.jpg)

*Image taken from [Tutorials Point](https://www.tutorialspoint.com/hbase/hbase_overview.htm)*

Within a HBase table, data stored in HDFS so is therefore split into blocks across nodes within the cluster. It is essentially a distributed sorted map, with the key being the row key and the value being the whole row of data. This means adjacent keys are stored next to each other on disk making them quicker to access. The cells within a table are just arbitrary arrays of bytes - this means you can store anything that can be serialized into a byte-array. However the cell size is the practical limit on the size of the values - its recommended that this should not be consistently above 10MB for performance reasons. Data is physically stored on disk on a per column family basis.

Column families are logical groups of columns (look at it like a table within a table). Separate column families are useful for frequently accessed groups of columns and if you want to compress certain columns but not others. Compression is recommended for most column families apart from those storing already compressed data e.g. JPEG data. **Column family names need to be printable as they are used as the directory name in HDFS** and these names are stored in each row within the HFile (the data file HBase uses to write data) - so if these names are long then it could potentially start to waste space over a long period of time.

Cloudera initially recommend 3 column families per table and they should be designed such that data being accessed simultaneously is in the same column family.

## Differences Between HBase and RDBMS

| | RDBMS| HBase |
|---|---|---|
| Data Layout | Row or column oriented | Column family oriented |
| Transactions | Yes | Single row only |
| Query language | SQL | get/put/scan |
| Security | Authentication | Access control at per-cell/cluster/table/row level |
| Indexes | Yes | Row Key only |
| Max Data Size | Terabites | Petabites+ |
| Read/write throughput | 1000s queries per second | Millions of queries per second |

Replacing an RDBMS application with HBase requires significant re-architecture. You would need to consider:
- Data layout due to HBase being column oriented
- How to support no transactions
- How will data be split into column families 
- What will the row key be and how will it be sorted
- What are the access patterns for data retrieval - as there is no sql
- What data is stored together - as there are no explicit joins

## HBase Key Operations

- **Get** retrieves a single row using the row key
 - It retrieves the most recent version (one with the largest timestamp)
 - `hbase> get ‘tablename’, ‘rowkey’ [,options]`
 - `hbase> get ‘movie’, ‘row1’, {COLUMN => ‘desc:title’, VERSIONS => 2}`
- **Scan** retrieves all rows or rows between two specific keys when the exact key is not known, or a group of rows needs to be accessed
 - Can set a start and end row key - the start key is displayed in the results but the stop row isn’t
 - Can be limited to certain column families or descriptors
 - When no start and stop rowkey is used, the whole table will be scanned
 - `hbase> scan ‘tablename’ [,options]`
 - `hbase> scan ‘movie’, {LIMIT =>10}`
 - `hbase> scan ‘movie’, {STARTROW => ‘row1’, STOPROW => ‘row5’}`
 - `hbase> scan ‘movie’, {COLUMNS => [‘desc:title’, ‘media:type’]}`
- **Put** adds a new row identified by row key
 - Will add or alter existing data - basically inserts a new version of a cell and adds a timestamp to it using ‘currentTimeMillis’
 - Can override the timestamp with your own (in the past or future) or assign a value other than time
 - To overwrite use Put to the same row, column and version as the cell to overwrite
 - multiple put calls can be run to insert multiple rows with different row keys
 - if the row key does exists - it updates existing value and its timestamp
  - if we reach the version limit, the oldest version is removed to make space for the new value
 - Updating specific column descriptors leave the other columns unchanged
 - `hbase> put ‘tablename’, ‘rowkey’, ‘colfam:col’, ‘value’ [,timestamp]`
- **Delete** removes a row by row key
 - Data isn't deleted at call time, but marked for deletion - rows are selected for deletion using the row key
 - Deletes can be performed on column families or specific column descriptors - using descriptors will leave all  - others intact
 - Physical deletion from HDFS happens later
 - adding a timestamp to the delete will delete that version and all previous versions
 - `hbase> delete ‘tablename’, ‘rowkey’, ‘colfam:col’ [,timestamp]`
 - `hbase> deleteall ‘movie’, ‘row1’ will delete entire row`
 - `hbase> truncate ‘movie’ to delete rows but not schema`

## Wrap Up

Thats all for this introductory post on Apache HBase - I'll be writing a post on the Architectural Fundamentals of HBase later on this month. As always, I hope you have found this interesting and would love to hear your thoughts below!



