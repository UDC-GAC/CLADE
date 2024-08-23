# Benchmark set up

The aim of this guide is to outline the most common tasks required to implement the TPC-DS benchmark. The [official TPC webpage](https://www.tpc.org/tpcds/) provides access to all the code, but the documentation contains several errors.

## Installation

CLADE compiles the DSGen for TPC-DS using Linux. Therefore, this guide is intended for Linux users. Prerequisites:

- gcc-9
- make
- yacc

You can install everything with: `apt install gcc-9 make bison flex`

## DSGen code structure

The data generation tool DSGen include the next folders and files:

- **query_templates**: folder with templates for the 99 queries in TPC-DS. Also, it include templates to complete the queries in different dialects: ansi, db2, netezza and oracle. For Snowflake and Databricks, we use Netezza.

- **tools**: folder with the code in C to compile **dsdgen**, for generating data, and **dsqgen**, for generating queries. There is also an official guide titled **How_To_Guide-DS-V2.0.0.doc** for installing and using the software. It should be noted that the guide appears to be a draft as it includes crossed-out text and notes.

## Build the binaries

 1. Copy `Makefile.suite` to `Makefile`, you can use `cp Makefile.suite Makefile`
 2. Open `Makefile` and edit the line with OS (line 42), for example in linux it must be `OS	=	LINUX`.
 3. Additionally, it is possible to modify CFLAGS to suppress some warnings during compilation. This is recommended as there can be an excessive number of warnings which can be irritating. For instance, on Linux, you can change `LINUX_CFLAGS = -g -Wall` to `LINUX_CFLAGS = -g -w` (line 58).
 4. To run the command, use `make CC=gcc-9`. Although the official documentation does not require the use of gcc-9, we have found that it is currently necessary on Linux. If needed, you can change the compiler in the Makefile on line 50 by setting `LINUX_CC = gcc-9` to run `make`.

## Generate data and queries

The previous steps have created the binaries **dsdgen** and **dsqgen**. The first is needed to create the data and the second is needed for the queries, but before the latter can be used, errors in the query templates must be corrected using the scripts described in [Scripts to fix TPC-DS queries](#scripts-to-fix-tpc-ds-queries).

### Dsdgen

The **dsdgen** utility generates input data for loading into the data warehouse. To obtain help information, run `./dsdgen -h`. An example of generating 1 GB (Scale Factor 1) in an output directory:

````
./dsdgen -scale 1 -dir path_output_dir
````

To generate large amounts of data efficiently, it is recommended to run multiple parallel streams. This will help to manage the huge amount of GBs that need to be generated. Here is an example using 4 parallel streams simultaneously on Unix:

````
./dsdgen -scale 1 -dir /tmp/data_partitions/ -parallel 4 -child 1 &
./dsdgen -scale 1 -dir /tmp/data_partitions/ -parallel 4 -child 2 &
./dsdgen -scale 1 -dir /tmp/data_partitions/ -parallel 4 -child 3 &
./dsdgen -scale 1 -dir /tmp/data_partitions/ -parallel 4 -child 4 &
````

### Dsqgen

The **dsqgen** utility is used to transform the query templates in the directory **query_templates** into executable SQL queries using a specific dialect (available in the same directory). For assistance, run `./dsqgen -h`. The following example generates a SQL file from the query99 template for a 1GB database using Oracle syntax.

````
./dsqgen -directory path_to_query_templates -template query99.tpl -dialect oracle –scale 1 -output_dir path_output_dir
````

### Scripts to fix TPC-DS queries

The query templates have some bugs that can be fixed using the scripts in the **Scripts** folder. The first, **fix_netezza_template.sh**, must be run before the **dsqgen** tool as it fixes the template for Netezza SQL. Usage:

````
./scripts/fix_netezza_template.sh <netezza_template_path>
````
The other scripts have been moved to the [TPC-DS repository](https://github.com/torusware/clade-tpcds).