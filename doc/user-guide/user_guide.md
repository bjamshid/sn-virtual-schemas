# User Guide

The purpose of this page is to provide detailed information for the supported Snowflake dialect. Typical questions are
* Which files have to be uploaded and included when creating the adapter script.
* How does the **CREATE VIRTUAL SCHEMA** statement look like, i.e. which properties are required.
* supported capabilities or things to consider regarding the data type mapping.

## Prerequisites

Before you can start using Snowflake Virtual Schemas you should know:

* How to [Create a bucket in BucketFS](https://docs.exasol.com/administration/on-premise/bucketfs/create_new_bucket_in_bucketfs_service.htm) 
* How to [Upload the driver to BucketFS](https://docs.exasol.com/administration/on-premise/bucketfs/accessfiles.htm) 

# JDBC Adapter for Virtual Schemas

## Overview

The JDBC adapter for `Snowflake` virtual schemas allows you to connect to Snowflake JDBC data source.
It uses the well proven ```IMPORT FROM JDBC``` Exasol statement behind the scenes to obtain the requested data, when running a query on a virtual table. 
The JDBC adapter also serves as the reference adapter for the Exasol virtual schema framework.

## If you are interested in an introduction to Snowflake virtual schema please contact Areto Support (supportteam@areto.de)

## Before you Start

Please note that the syntax for creating adapter scripts is not recognized by all SQL clients. 
[DBeaver](https://dbeaver.io/) for example. If you encounter such a problem, try a different client.

## Getting Started

This page contains common information applicable to Snowflake dialect.

Before you can start using the JDBC adapter for virtual schemas you have to deploy the adapter and the JDBC driver of your data source in your Exasol database.

for creating a Snowflake virtual schema please see /doc/dialects/snowflake.md

## Deploying JDBC Driver Files

You have to upload the JDBC driver files of your remote database **twice** (except for the Exasol and BigQuery dialects):

* Upload all files of the JDBC driver into a bucket of your choice, so that they can be accessed from the adapter script.
  
* Upload all files of the JDBC driver as a JDBC driver in EXAOperation
  - In EXAOperation go to Software -> JDBC Drivers
  - Add the JDBC driver by specifying the JDBC main class and the prefix of the JDBC connection string
  - Upload all files (one by one) to the specific JDBC to the newly added JDBC driver.

Note that some JDBC drivers consist of several files and that you have to upload all of them. 
To find out which JAR you need, check the individual dialects' documentation pages.

To explore the tables in the virtual schema, just like for a regular schema:

```sql
OPEN SCHEMA SNOWLFAKE_VIRTUAL_SCHEMA;
SELECT * FROM cat;
DESCRIBE clicks;
```

And we can run arbitrary queries on the virtual tables:

```sql
SELECT count(*) FROM clicks;
SELECT DISTINCT USER_ID FROM clicks;
```

Behind the scenes the Exasol command `IMPORT FROM JDBC` will be executed to obtain the data needed from the data source to fulfil the query. 
The Exasol database interacts with the adapter to pushdown as much as possible to the data source (e.g. filters, aggregations or `ORDER BY` / `LIMIT`), 
while considering the capabilities of the data source.

Let's combine a virtual and a native tables in a query:

```sql
SELECT * from clicks JOIN native_schema.users on clicks.userid = users.id;
```

You can refresh the schemas metadata, e.g. if tables were added in the remote system:

```sql
ALTER VIRTUAL SCHEMA SNOWLFAKE_VIRTUAL_SCHEMA;
ALTER VIRTUAL SCHEMA SNOWLFAKE_VIRTUAL_SCHEMA REFRESH TABLES t1 t2; -- refresh only these tables
```

Or set properties. Depending on the adapter and the property you set this might update the metadata or not. 
In our example the metadata are affected, because afterwards the virtual schema will only expose two virtual tables.

```sql
ALTER VIRTUAL SCHEMA SNOWLFAKE_VIRTUAL_SCHEMA SET TABLE_FILTER='CUSTOMERS, CLICKS';
```

Finally, you can unset properties:

```sql
ALTER VIRTUAL SCHEMA SNOWLFAKE_VIRTUAL_SCHEMA SET TABLE_FILTER=null;
```

Or drop the virtual schema:

```sql
DROP VIRTUAL SCHEMA SNOWLFAKE_VIRTUAL_SCHEMA CASCADE;
```

### Adapter Properties

The following properties can be used to control the behavior of the JDBC adapter. 
As you see above, these properties can be defined in `CREATE VIRTUAL SCHEMA` or changed afterwards via `ALTER VIRTUAL SCHEMA SET`. 
Note that properties are always strings, like `TABLE_FILTER='T1,T2'`.

#### Mandatory Properties

Property                    | Value
--------------------------- | -----------
**SQL_DIALECT**             | Name of the SQL dialect: SNOWFLAKE (case insensitive). If you try generating a virtual schema without specifying this property you will see all available dialects in the error message.
**CONNECTION_NAME**         | Name of the connection created with `CREATE CONNECTION` which contains the JDBC connection string, the username and password.

#### Common Optional Properties

Property                    | Value
--------------------------- | -----------
**SCHEMA_NAME**             | The name of the remote JDBC schema. This is usually case-sensitive, depending on the dialect. It depends on the dialect whether you have to specify this or not. Usually you have to specify it if the data source JDBC driver supports the concepts of schemas.
**TABLE_FILTER**            | A comma-separated list of table names (case sensitive). Only these tables will be available as virtual tables, other tables are ignored. Use this if you don't want to have all remote tables in your virtual schema.

## Unrecognized and Unsupported Data Types

Not all data types present in a source database have a matching equivalent in Exasol. Also software updates on the source database can introduce new data type that the Snowflake Virtual schema does not recognize.

There are a few important things you need to know about those data types.

1. Columns of an unrecognized / unsupported data type are not mapped in a Virtual Schema. From Exasol's perspective those columns do not exist on a table. This is done so that tables containing those columns can still be mapped and do not have to be rejected as a whole.
2. You can't query columns of an unrecognized / unsupported data type. If the source table contains them, you have to *explicitly* exclude them from the query. You can for example not use the asterisk (`*`) on a table that contains one or more of those columns. This will result in an error issued by the Virtual schema.
3. If you want to query all columns except unsupported, add `1` to the columns list. Otherwise you will see the same error as if you query with the asterisk (`*`).
    For example, a table contains 3 columns: `bool_column` BOOLEAN, `timestamp_column` TIMESTAMP, `blob_column` BLOB. The column BLOB is not supported. If you want to query two other columns, use: `SELECT "bool_column", "timestamp_column", 1 FROM table_name;` .
4. You can't use functions that result in an unsupported / unknown data type. 


[virtual-schemas-repository]: https://github.com/exasol/virtual-schemas
[virtual-schemas-releases]: https://github.com/exasol/virtual-schemas/releases
[exasol-virtual-schema-repository]: https://github.com/exasol/exasol-virtual-schema
[exasol-virtual-schema-releases]: https://github.com/exasol/exasol-virtual-schema/releases