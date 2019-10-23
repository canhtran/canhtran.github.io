---
layout:     post
published: false
title:      "Hive and HBase Integration"
subtitle:   "Mapping Hive and Hbase for complex queries"
date:       2016-11-08
author:     "Canh Tran"
tags:
    - Hive
    - HBase
---
### Objective
Integrate Hive and Hbase, data stores in Hbase and use HiveSQL to query the results.

### Why should we do it ?

My scenario, I have to build a big data system that receive real-time data from sensor. A prediction engine is built on top of it to query and process the data every 15 minutes. Here is the problems:
- The prediction is built by spark and Python with Machine Learning and I need to perform a complex queries, Hive is a good choice with the support from HiveSQL but Hive is batch processing, it's slow down performance.
- Hbase is good for real time queries. But Hbase is very limited in complex queries.

If you have the same problem with me, this is my architect for you.

### `Let's begin`

The example dataset which I used in this post.

| studentid | name  | state | street           | zipcode |
|-----------|-------|-------|------------------|---------|
| student1  | Alice | CA    | 123 Ballmer Av   | 12345   |
| student2  | Bob   | CA    | 1 Infinite Loop  | 12345   |
| student3  | Frank | CA    | 435 Walker Ct    | 12345   |
| student4  | Mary  | CA    | 56 Southern Pkwy | 12345   |

And for HBase storage I define two columns family call **Account** and **Address** which look like.

| row_key  | account          | address                                                      |
|----------|------------------|--------------------------------------------------------------|
| student1 | {"name":"Alice"} | {"state":"CA","street":"123 Ballmer Av","zipcode":"12345"}   |
| student2 | {"name":"Frank"} | {"state":"CA","street":"1 Infinite Loop","zipcode":"12345"}  |
| student3 | {"name":"Bob"}   | {"state":"CA","street":"435 Walker Ct","zipcode":"12345"}    |
| student4 | {"name":"Mary"}  | {"state":"CA","street":"56 Southern Pkwy","zipcode":"12345"} |


&nbsp;

#### <i class="fa fa-server" aria-hidden="true"></i> Senario 1: Create Hive and HBase from scratch

In this senario, I don't have any existing Hive or HBase tables, I need to integrate and make it from beginning.

In order to create a new HBase table which is to be managed by Hive, I use the `STORED BY` clause on `CREATE TABLE`

```sql
CREATE TABLE students(studentid string, state string, name string, street string, zipcode int)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key, account:name, address:state, address:street, address:zipcode")
TBLPROPERTIES ("hbase.table.name" = "students", "hbase.mapred.output.outputtable" = "students");
```

After execute the command above, I should be able to see the new (empty) table in HBase shell and then I insert some data using HBase command.

```bash
put 'students','student1','account:name','Alice'
put 'students','student1','address:street','123 Ballmer Av'
put 'students','student1','address:zipcode','12345'
put 'students','student1','address:state','CA'
```

And I query it back via Hive with the `SELECT` command

```sql
hive> select * from students;
OK
student1    Alice   CA  123 Ballmer Av  12345
Time taken: 0.156 seconds, Fetched: 1 row(s)
```

Then I do the reverse way. From Hive shell, I insert new records

```sql
INSERT INTO TABLE students VALUES ('student2', 'Bob', 'CA', '1 Infinite Loop', 12345);
INSERT INTO TABLE students VALUES ('student3', 'Frank', 'CA', '435 Walker Ct', 12345);
```

And use HBase shell to verify that the data actually got loaded:

```bash
hbase(main):020:0> scan "students"
ROW           COLUMN+CELL
student1      column=account:name, timestamp=1478591477045, value="Alice"
student1      column=address:state, timestamp=1478591459611, value="CA"
student1      column=address:street, timestamp=1478591487312, value="123 Ballmer Av"
student1      column=address:zipcode, timestamp=1478591482494, value="12345"
student2      column=account:name, timestamp=1478595281380, value="Bob"
student2      column=address:state, timestamp=1478595281380, value="CA"
student2      column=address:street, timestamp=1478595281380, value="435 Walker Ct"
student2      column=address:zipcode, timestamp=1478595281380, value="12345"
student3      column=account:name, timestamp=1478595383043, value="Frank"
student3      column=address:state, timestamp=1478595383043, value="CA"
student3      column=address:street, timestamp=1478595383043, value="435 Walker Ct"
student3      column=address:zipcode, timestamp=1478595383043, value="12345"
3 row(s) in 0.0540 seconds
```

&nbsp;

#### <i class="fa fa-server" aria-hidden="true"></i> Senario 2: Integrated Hive with existing HBase.
In the second senario, I create a new table call **students2** in Hbase and then map it to Hive to manage.

```bash
echo "create 'students2','account','address'" | hbase shell
```

And insert the data

```bash
put 'students2','student1','account:name','Alice'
put 'students2','student1','address:street','123 Ballmer Av'
put 'students2','student1','address:zipcode','12345'
put 'students2','student1','address:state','CA'
put 'students2','student2','account:name','Bob'
put 'students2','student2','address:street','1 Infinite Loop'
put 'students2','student2','address:zipcode','12345'
put 'students2','student2','address:state','CA'
put 'students2','student3','account:name','Frank'
put 'students2','student3','address:street','435 Walker Ct'
put 'students2','student3','address:zipcode','12345'
put 'students2','student3','address:state','CA'
put 'students2','student4','account:name','Mary'
put 'students2','student4','address:street','56 Southern Pkwy'
put 'students2','student4','address:zipcode','12345'
put 'students2','student4','address:state','CA'
```

Via Hive, I create a new table also name **students2** by using the `CREATE EXTERNAL TABLE` command and link it with existing Hbase table.

```bash
CREATE EXTERNAL TABLE students2(studentid string, state string, name string, street string, zipcode int)
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key, account:name, address:state, address:street, address:zipcode")
TBLPROPERTIES ("hbase.table.name" = "students2", "hbase.mapred.output.outputtable" = "students2");
```

After execute the command above, I retrieve the data from **student2** table

```sql
hive> select * from students2;
OK
student1    Alice   CA  123 Ballmer Av  12345
student2    Bob CA  1 Infinite Loop 12345
student3    Frank   CA  435 Walker Ct   12345
student4    Mary    CA  56 Southern Pkwy    12345
Time taken: 1.289 seconds, Fetched: 4 row(s)
```

&nbsp;

#### <i class="fa fa-server" aria-hidden="true"></i> Wrap it up!
One of the notice in here, the `hbase.columns.mapping` is required and will be validated against the existing HBase table's column families, whereas `hbase.table.name` and `hbase.mapred.output.outputtable` are optional.

__Update__ I have found out that [Apache Drill](https://drill.apache.org/docs/querying-hbase/) can connect to Hbase data source and query data using Drill.
