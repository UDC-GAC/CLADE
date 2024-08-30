# LST Bench

LST-Bench is a framework that allows users to run benchmarks specifically designed for evaluating Log-Structured Tables (LSTs) such as Delta Lake, Apache Hudi, and Apache Iceberg [@camacho2024lst]. It is available for use with Spark and Trino and can be extended to other engines as an open source project from Microsoft. The source code is available on GitHub: [https://github.com/microsoft/lst-bench](https://github.com/microsoft/lst-bench).

Support for Snowflake has been added from CLADE, with both native and Iceberg tables, and the changes have been proposed to the official Microsoft repository. These have been accepted and added to the project, providing support for this new engine. Figure 1 shows the architecture of LST-Bench, the framework being a Java program. It is to this that the SQL queries and changes needed to work with Snowflake have been added. The benchmark uses the TPC-DS data that can be generated using the instructions in [TPC DS set up](3_set-up.md).

<figure>
  <img src="/benchmarking/assets/lst-bench-arch.png" alt="TPC-DS excerpt schema" style="width:90%; margin-left:5%; margin-right:5%;">
  <figcaption>Fig. 1 - LST-Bench architecture.</figcaption>
</figure>