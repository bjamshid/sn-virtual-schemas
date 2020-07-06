# Virtual Schemas 

<img alt="virtual-schemas logo" src="doc/images/virtual-schemas_128x128.png" style="float:left; padding:0px 10px 10px 10px;"/>

Build Result:
[![Build Status](https://travis-ci.org/bjamshid/sn-virtual-schemas.svg?branch=dev)](https://travis-ci.org/bjamshid/sn-virtual-schemas)

SonarCloud Result:
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=areto%3Asnowflake-virtual-schema&metric=alert_status)](https://sonarcloud.io/dashboard?id=areto%3Asnowflake-virtual-schema)


# Overview

**Snowflake's Virtual Schema** is an implementation based on Exasol's Virtual Schema Open Source project. The adaptor is an abstraction layer that makes Snowlflake tables accessible in Exasol instance through regular SQL commands. The contents of the external data sources are mapped to virtual tables which look like and can be queried as any regular Exasol table.
**Original Virtual Schemas Repository:**[Exasol Virtual Schemas] (https://github.com/exasol/virtual-schemas)

## Version Requirements

Snowflake Virtual Schema  | Snowflake's JDBC driver version | Required Java Version | Lifecycle
--------------------------|---------------------------------|-----------------------|--------------------------------
0.1.0                     | 3.12.5                          |  11                   | supported, active development


Exasol Version  | Java Version Installed by Default in Language Container
----------------|--------------------------------------------------------
6.2             | 11

Please contact the [Areto Support Team](https://www.areto.de/kontakt/) if you need help installing the Virtual Schema.

## Features

* Read access to data in Snowflake.
* Source tables appear as tables inside Exasol and can be queried using regular SQL statements.
* Pushes down queries to the remote source
* Allows limiting metadata mapping to selected tables and / or schemas
* Allows redirecting log output to a remote machine

## Customer Support

This is an open source project which is officially supported by Areto GMBH. For any question, you can contact our support team.

# Table of Contents

## Information for Users
 
Please refer to the user-guide for usage [Snowflake SQL dialects](doc/dialects/snowflake.md)


## Dependencies

### Run Time Dependencies

Running the Virtual Schema requires a Java Runtime version 11 or later.

| Dependency                                                                          | Purpose                                                | License                          |
|-------------------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------|
| [JSON-P](https://javaee.github.io/jsonp/)                                           | JSON Processing                                        | CDDL-1.0                         |
| [Exasol Script API](https://docs.exasol.com/database_concepts/udf_scripts.htm)      | Accessing Exasol features                              | MIT License                      |
| [Exasol Virtual Schema Common](https://github.com/exasol/virtual-schema-common-java)| Common module of Exasol Virtual Schemas adapters       | MIT License                      |
| [Exasol Virtual Schema JDBC](https://github.com/exasol/virtual-schema-common-jdbc)  | Common JDBC functions for Virtual Schemas adapters     | MIT License                      |
| JDBC driver(s), depending on data source                                            | Connecting to the data source                          | Check driver documentation       |

### Test Dependencies

| Dependency                                                                           | Purpose                                                | License                          |
|--------------------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------|
| [Apache Maven](https://maven.apache.org/)                                            | Build tool                                             | Apache License 2.0               |
| [Exasol JDBC Driver][exasol-jdbc-driver]                                             | JDBC driver for Exasol database                        | MIT License                      |
| [Exasol Testcontainers][exasol-testcontainers]                                       | Exasol extension for the Testcontainers framework      | MIT License                      |
| [HikariCP](https://github.com/brettwooldridge/HikariCP)                              | JDBC connection pool                                   | Apache License 2.0               |
| [Java Hamcrest](http://hamcrest.org/JavaHamcrest/)                                   | Checking for conditions in code via matchers           | BSD License                      |
| [JSONassert](http://jsonassert.skyscreamer.org/)                                     | Compare JSON documents for semantic equality           | Apache License 2.0               |
| [JUnit](https://junit.org/junit5)                                                    | Unit testing framework                                 | Eclipse Public License 1.0       |
| [J5SE](https://github.com/itsallcode/junit5-system-extensions)                       | JUnit5 extensions to test Java System.x functions      | Eclipse Public License 2.0       |
| [Mockito](http://site.mockito.org/)                                                  | Mocking framework                                      | MIT License                      |
| [Testcontainers](https://www.testcontainers.org/)                                    | Container-based integration tests                      | MIT License                      |
| [Snowflake JDBC Driver](https://repo1.maven.org/maven2/net/snowflake/snowflake-jdbc/)| JDBC Driver for Snowflake database                     | Apache License 2.0               |

### Maven Plug-ins

| Plug-in                                                                             | Purpose                                                | License                          |
|-------------------------------------------------------------------------------------|--------------------------------------------------------|----------------------------------|
| [Maven Compiler Plugin](https://maven.apache.org/plugins/maven-compiler-plugin/)    | Setting required Java version                          | Apache License 2.0               |
| [Maven Exec Plugin](https://www.mojohaus.org/exec-maven-plugin/)                    | Executing external applications                        | Apache License 2.0               |
| [Maven GPG Plugin](https://maven.apache.org/plugins/maven-gpg-plugin/)              | Code signing                                           | Apache License 2.0               |
| [Maven Failsafe Plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)   | Integration testing                                    | Apache License 2.0               |
| [Maven Javadoc Plugin](https://maven.apache.org/plugins/maven-javadoc-plugin/)      | Creating a Javadoc JAR                                 | Apache License 2.0               |
| [Maven Jacoco Plugin](https://www.eclemma.org/jacoco/trunk/doc/maven.html)          | Code coverage metering                                 | Eclipse Public License 2.0       |
| [Maven Source Plugin](https://maven.apache.org/plugins/maven-source-plugin/)        | Creating a source code JAR                             | Apache License 2.0               |
| [Maven Surefire Plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)   | Unit testing                                           | Apache License 2.0               |


[exasol-jdbc-driver]: https://www.exasol.com/portal/display/DOWNLOAD/Exasol+Download+Section
[exasol-testcontainers]: https://github.com/exasol/exasol-testcontainers
