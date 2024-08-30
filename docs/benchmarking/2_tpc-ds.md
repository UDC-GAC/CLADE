# TPC-DS

## Context
Evaluating computer performance proves to be a significant challenge. The only reliable metrics are the cost (price/performance ratio) and data processing capacity. Attempts to supply these metrics come in the form of industry-standard benchmarks, which package multiple micro-benchmarks and workload simulations[@gregg2014systems]. This is where TPC comes in, a non-profit organisation established to determine transaction processing and database benchmarks and distribute fair and verifiable performance information to the industry. The metrics developed by TPC enable system performance comparisons and are widely used to evaluate the performance of computer systems, publishing the results on the [TPC website](https://www.tpc.org/). These benchmarks offer vendors the opportunity to improve their products and publish their results online for competitive comparisons, usually at a fee. However, they must provide precise information overseen by a TPC auditor regarding their testing environment to enable a third party to verify this data subsequently.

## Benchmark Characteristics
TPC-DS models a data warehouse, with a focus on online analytical processing (OLAP) tasks. It models the decision support functions of a retail product supplier, allowing users to relate easily to the benchmark components[@nambiar2006making]. The retailer, albeit fictitious, sells its products through three distribution channels: store, catalogue and Internet.

## Dataset
TPC-DS implements a multiple snowflake schema, which is a "hybrid approach between a 3NF and a pure star schema"[@nambiar2006making].

<figure>
  <img src="/benchmarking/assets/tpcds_schema.png" alt="TPC-DS excerpt schema" style="width:90%; margin-left:5%; margin-right:5%;">
  <figcaption>Fig. 1 - TPC-DS excerpt schema.</figcaption>
</figure>

The diagram of Figure 1[@menzler2019summary] displays a portion of the TPC-DS schema through an entity-relationship format. It comprises of two fact tables, *Store_Sales* and *Store_Returns*, alongside the associated dimension tables such as *Customer* and *Store*.

The green and blue relations illustrate the conventional star schema approach, while the orange relations display the normalization of dimension tables, which was introduced by the snowflake schema approach. Normalized tables allow for relations between themselves, and can also be separated from fact tables, as demonstrated by the *Income_Band* table.

Therefore, TPC-DS employs a complex snowflake schema comprised of 24 tables in total.

After designing the test database schema, it is essential to populate the tables with data. To determine the size of the database, TPC-DS introduces the concept of scale factors, which consists of discrete values measurement of the data volume in gigabytes. The scale factors range from 1 (1 GB, only suitable for qualification) to 100,000 (100 TB). In this context, it's essential to note that benchmark results are limited to specific scale factors and are not comparable to results obtained from other scale factors.

How does TPC-DS facilitate scaling? Initially, it is crucial to differentiate domain and tuple scaling. The latter relates to the number of tuples present in fact tables, which scale linearly with the scale factor in TPC-DS.

However, domain scaling refers to the dimension tables, which generally do not scale linearly. TPC-DS scales its domains sub-linearly and avoids the unrealistic table ratios present in TPC-H (a former decision support benchmark for TPC-DS)[@nambiar2006making]. Furthermore, the ratio of customers, items, stores, and other dimensions remains realistic, even with high-scale factors.

The generation of values in TPC-DS using mathematical models and well-studied distributions (such as the Normal or Poisson distributions) is also of interest. Synthetic datasets resulting from these distributions possess several benefits for decision support benchmarks, but they also present a notable drawback: they are not compatible with a binding variable technique employed in TPC-DS to reduce predictability[@nambiar2006making].

Thus, TPC-DS synthesizes real-world data for several critical distributions to circumvent this issue. For further details regarding this process, please refer to the paper titled "The making of TPC-DS"[@nambiar2006making].


## Queries
TPC-DS defines a set of 99 different SQL queries (including OLAP extensions) designed to cover the entire dataset[@nambiar2006making]. The schema design allows for two distinct query types: a reporting section (catalogue sales channel, which accounts for 40% of the dataset) and an ad-hoc segment (internet and store sales channels). The query workload contains four query classes which include pure reporting queries, pure ad-hoc queries, iterative OLAP queries, and extraction or data mining queries. Reporting queries are well-known beforehand, allowing for the use of optimisation methods such as data placement and auxiliary data structures (e.g. materialised views and indexes), as stated by the [TPC-DS specification](https://www.tpc.org/TPC_Documents_Current_Versions/pdf/TPC-DS_v3.2.0.pdf).

Ad-hoc queries, on the other hand, are designed to simulate user queries which are not pre-determined. Explicit optimizations through auxiliary data structures (such as DDL, session options, and global configuration parameters) are not allowed for the ad-hoc section of the schema.

Queries involving both the ad-hoc and reporting sections of the schema, such as iterative OLAP and data mining queries, are categorized as hybrid queries and fall under both types. Technical terminology abbreviations will be explained when first used[@nambiar2006making]. 

Interestingly, none of the queries are completely static, not even the ones used for reporting. Instead, a query template model with pseudo-random substitutions is used to create all queries to model the dynamic use of DSS. However, the model is closely linked to the data generator to ensure query comparability across substitutions[@nambiar2006making].

## Metrics
TPC benchmarks offer uncomplicated metrics for comparing system performance, requiring the measurement of execution times to calculate them. For TPC-DS, four specific time intervals are of importance.

<figure>
  <img src="/benchmarking/assets/tpc-ds-workflow.png" alt="TPC-DS execution order" style="width:90%; margin-left:5%; margin-right:5%;">
  <figcaption>Fig. 2 - TPC-DS execution order.</figcaption>
</figure>

Figure 2[@menzler2019summary] demonstrates effectively the periods within which TPC-DS evaluates execution times. The Load Test phase encompasses preparations for the performance test, including table creation, data population and auxiliary data structure setup, amongst other tasks. Following this, in the Performance Test phase, all 99 queries are executed twice, with intervening data maintenance operations.

Once the necessary timings have been recorded, the performance measurement for Decision Support, expressed as Queries per hour, can be computed using the following method:

<figure>
  <img src="/benchmarking/assets/tpcds_formula.png" alt="TPC-DS performance calculation" style="width:90%; margin-left:5%; margin-right:5%;">
  <figcaption>Fig. 3 - TPC-DS performance metric.</figcaption>
</figure>

Worth mentioning: SF stands for scale factor, while Q refers to the number of streams used in the Throughput Tests multiplyed by 99, the number of queries for each stream. The divisor elements are the Total Time of execution of each of the phases of the benchmark: Power Test, Troughput Tests, Data Maintenance and Load Test.

## Implementation Considerations

### Strengths & Advantages
One significant benefit of TPC-DS is its high utilization rate. Both platforms examined in this study offer an out-of-the-box implementation: Databricks possesses a [GitHub repository](https://github.com/databricks/spark-sql-perf) containing the necessary code (in the form of notebooks) to perform the benchmark, and Snowflake provides the full dataset and all 99 queries as sample data[@snowflake2023sample], making them quickly executable.

### Challenges & Limitations
The main drawback of TPC-DS for this project is its limitation to structured data. TPC-DS exclusively employs structured data, whereas Data Lakehouses can manage semi-structured and non-structured data. As a result, more comprehensive benchmarks, such as TPCx-BB[@tpcxbb2023], are available for testing capabilities to handle not only structured data but also semi-structured (such as weblogs) and non-structured data (such as reviews).
