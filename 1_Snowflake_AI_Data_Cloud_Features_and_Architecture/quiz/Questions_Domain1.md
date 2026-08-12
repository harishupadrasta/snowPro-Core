# <center>SnowPro Core (COF-C03) - Domain 1 Quiz</center>

<center>

![Questions](https://img.shields.io/badge/Questions-63-blue)
![Domain Weight](https://img.shields.io/badge/Domain%20Weight-31%25-orange)
![Difficulty](https://img.shields.io/badge/Difficulty-Mixed-yellow)

</center>

---

## Overview

**Domain 1: Snowflake AI Data Cloud Features and Architecture (31%)**

This quiz covers the foundational architecture concepts of Snowflake including its multi-cluster shared data architecture, cloud services layer, storage layer, virtual warehouses, caching mechanisms, and edition differences.

### Instructions
- Select the single best answer for each question unless otherwise stated.
- Questions marked with a scenario require you to apply concepts, not just recall definitions.
- After answering, check the explanation to reinforce your understanding.
- Target score: 80%+ before moving to the next domain.

---

## Topic 1.1: Architecture

### Question 1
What type of architecture does Snowflake use?

- A) Shared-disk architecture
- B) Shared-nothing architecture
- C) Multi-cluster, shared data architecture (hybrid)
- D) Traditional MPP architecture

**Answer: C) Multi-cluster, shared data architecture (hybrid)**

**Explanation:** Snowflake uses a unique hybrid architecture that combines the benefits of shared-disk (centralized storage accessible by all compute nodes) and shared-nothing (independent compute clusters that don't compete for resources). This is neither purely shared-disk nor shared-nothing, making it a multi-cluster shared data architecture.

---

### Question 2
Which of the following are the three main layers of Snowflake's architecture? (Choose the best answer)

- A) Presentation Layer, Application Layer, Database Layer
- B) Cloud Services Layer, Query Processing Layer (Compute), Storage Layer
- C) Ingestion Layer, Processing Layer, Output Layer
- D) Metadata Layer, Compute Layer, Storage Layer

**Answer: B) Cloud Services Layer, Query Processing Layer (Compute), Storage Layer**

**Explanation:** Snowflake's architecture consists of three distinct layers: the Cloud Services Layer (brain - handles authentication, metadata, optimization), the Query Processing Layer (muscle - virtual warehouses that execute queries), and the Storage Layer (centralized, compressed columnar storage). Each layer scales independently.

---

### Question 3
**Scenario:** A company is evaluating Snowflake and wants to understand how it differs from traditional on-premises data warehouses. Which statement BEST describes a key architectural advantage?

- A) Snowflake requires capacity planning for storage and compute together
- B) Storage and compute are tightly coupled for maximum performance
- C) Storage and compute scale independently, allowing optimization of each
- D) Snowflake uses the same shared-nothing architecture as Teradata

**Answer: C) Storage and compute scale independently, allowing optimization of each**

**Explanation:** The decoupling of storage and compute is Snowflake's core architectural differentiator. You can scale storage without adding compute (pay only for what you store) and scale compute without adding storage (spin up warehouses only when needed). Traditional systems couple these, requiring over-provisioning of one to scale the other.

---

### Question 4
Which statement about Snowflake's architecture is FALSE?

- A) Multiple virtual warehouses can access the same data simultaneously
- B) The Cloud Services Layer manages authentication and access control
- C) Each virtual warehouse has its own dedicated storage
- D) Snowflake stores data in a columnar format

**Answer: C) Each virtual warehouse has its own dedicated storage**

**Explanation:** Virtual warehouses do NOT have dedicated storage. They are pure compute resources that read from the centralized storage layer. This is what enables multiple warehouses to query the same data concurrently without contention, and why warehouses can be started/stopped without any data movement.

---

### Question 5
In Snowflake's architecture, what happens to running queries if the Cloud Services Layer experiences a brief disruption?

- A) All queries immediately fail and must be resubmitted
- B) Queries continue executing because compute and cloud services are independent
- C) The system automatically pauses all warehouses
- D) Data in the storage layer becomes temporarily inaccessible

**Answer: A) All queries immediately fail and must be resubmitted**

**Explanation:** The Cloud Services Layer is essential for query coordination, optimization, and transaction management. If it is disrupted, queries cannot be coordinated or completed. However, Snowflake's Cloud Services Layer runs across multiple availability zones with redundancy, making such failures extremely rare. The storage layer data remains safe regardless.

---

### Question 6
Which Snowflake architectural component is responsible for query optimization and generating execution plans?

- A) Virtual Warehouse
- B) Storage Layer
- C) Cloud Services Layer
- D) Result Cache

**Answer: C) Cloud Services Layer**

**Explanation:** The Cloud Services Layer handles query parsing, optimization, and execution plan generation before dispatching work to virtual warehouses. It uses metadata about micro-partitions (min/max values, row counts) to perform pruning and create efficient plans. The virtual warehouse simply executes the plan it receives.

---

### Question 7
**Scenario:** Your organization runs Snowflake on AWS in US-East-1. A colleague claims they can query data stored in a Snowflake account on Azure in West Europe without any data replication. Is this correct?

- A) Yes, Snowflake's architecture allows cross-cloud querying natively
- B) No, data must be replicated using Snowflake's database replication feature
- C) Yes, but only if both accounts are on the Business Critical edition
- D) No, cross-cloud access is impossible in Snowflake

**Answer: B) No, data must be replicated using Snowflake's database replication feature**

**Explanation:** Snowflake accounts are region and cloud-specific. To access data across clouds or regions, you must use database replication or data sharing with replication. Snowflake does not natively allow cross-cloud querying without first replicating the data to the target region/cloud. Listings and data sharing can facilitate this with auto-fulfillment.

---

### Question 8
How does Snowflake handle metadata management in its architecture?

- A) Metadata is stored within the same micro-partitions as the data
- B) Metadata is managed by the Cloud Services Layer in a highly available, distributed key-value store
- C) Each virtual warehouse maintains its own copy of metadata
- D) Metadata is stored in external object storage alongside data files

**Answer: B) Metadata is managed by the Cloud Services Layer in a highly available, distributed key-value store**

**Explanation:** Snowflake's Cloud Services Layer maintains all metadata (table definitions, micro-partition statistics, access control, query history) in a transactional, highly available key-value store separate from the data storage layer. This metadata enables intelligent query pruning and is not stored in S3/Azure Blob/GCS with the data files.

---

### Question 9
Which cloud platforms does Snowflake currently run on?

- A) AWS only
- B) AWS and Azure only
- C) AWS, Microsoft Azure, and Google Cloud Platform
- D) AWS, Azure, GCP, and Oracle Cloud

**Answer: C) AWS, Microsoft Azure, and Google Cloud Platform**

**Explanation:** Snowflake is available on all three major public cloud platforms: Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP). Each deployment uses the native object storage of that cloud (S3, Azure Blob, GCS) for its storage layer, but Snowflake itself provides the same experience across all three.

---

### Question 10
**Scenario:** A data engineer notices that two different virtual warehouses are querying the same large table simultaneously, yet neither is experiencing performance degradation. What architectural principle explains this?

- A) The queries are automatically load-balanced to a single warehouse
- B) Snowflake's shared data architecture allows concurrent access to centralized storage without contention
- C) One warehouse is reading from cache while the other reads from storage
- D) Snowflake automatically duplicates the table for each warehouse

**Answer: B) Snowflake's shared data architecture allows concurrent access to centralized storage without contention**

**Explanation:** Because storage is decoupled from compute, multiple warehouses read from the same centralized data files independently. There is no locking or contention at the storage layer since data files are immutable. Each warehouse has its own compute resources and local cache, so concurrent access does not cause performance interference.

---

## Topic 1.2: Cloud Services Layer

### Question 11
Which of the following is NOT a function of the Cloud Services Layer?

- A) Authentication and access control
- B) Query execution and data scanning
- C) Metadata management
- D) Infrastructure management

**Answer: B) Query execution and data scanning**

**Explanation:** Query execution and data scanning are performed by the Query Processing Layer (virtual warehouses). The Cloud Services Layer handles the "brains" of the operation: authentication, access control, metadata management, query optimization/compilation, infrastructure management, and transaction management. It plans the query; warehouses execute it.

---

### Question 12
How is the Cloud Services Layer billed?

- A) A flat monthly fee regardless of usage
- B) Customers pay for all Cloud Services compute consumed
- C) Only usage exceeding 10% of daily warehouse consumption is billed
- D) It is included free with every Snowflake account

**Answer: C) Only usage exceeding 10% of daily warehouse consumption is billed**

**Explanation:** Snowflake includes Cloud Services compute at no additional charge up to 10% of the daily total warehouse compute consumption. Only the amount that exceeds this 10% threshold is billed. For most workloads, Cloud Services costs remain within this allowance. Heavy metadata operations or many small queries can push past this threshold.

---

### Question 13
Which Cloud Services function would be responsible for determining which micro-partitions to scan for a query with a WHERE clause?

- A) Authentication Service
- B) Query Optimizer (pruning)
- C) Transaction Manager
- D) Result Cache Manager

**Answer: B) Query Optimizer (pruning)**

**Explanation:** The Query Optimizer, part of the Cloud Services Layer, uses metadata about micro-partitions (including min/max values stored for each column in each partition) to perform partition pruning. It determines which partitions can be skipped entirely because they cannot contain matching rows, dramatically reducing the amount of data scanned.

---

### Question 14
**Scenario:** A developer runs `SHOW TABLES` followed by `DESCRIBE TABLE`, then checks `INFORMATION_SCHEMA.COLUMNS`. None of these operations use a virtual warehouse. Why?

- A) These are DDL operations which never require compute
- B) These operations are served entirely by the Cloud Services Layer using metadata
- C) Snowflake uses a hidden system warehouse for these queries
- D) The results are always returned from the result cache

**Answer: B) These operations are served entirely by the Cloud Services Layer using metadata**

**Explanation:** Metadata operations like SHOW, DESCRIBE, and INFORMATION_SCHEMA queries are handled directly by the Cloud Services Layer without requiring a virtual warehouse. The Cloud Services Layer maintains all structural metadata and can serve these requests from its distributed metadata store, which is why no warehouse needs to be running.

---

### Question 15
What role does the Cloud Services Layer play in Snowflake's security model?

- A) It only handles network-level security (firewalls)
- B) It manages authentication, authorization (RBAC), encryption key management, and data governance
- C) Security is handled by the underlying cloud provider, not Cloud Services
- D) It only manages row-level security policies

**Answer: B) It manages authentication, authorization (RBAC), encryption key management, and data governance**

**Explanation:** The Cloud Services Layer is the central security authority in Snowflake. It handles all aspects of security including user authentication (MFA, SSO, key-pair), role-based access control (RBAC), encryption key management (for data at rest and in transit), masking policies, row access policies, and network policies. Security is not delegated to the cloud provider.

---

### Question 16
Which component of the Cloud Services Layer ensures ACID compliance for concurrent transactions?

- A) Query Optimizer
- B) Transaction Manager
- C) Result Cache
- D) Metadata Store

**Answer: B) Transaction Manager**

**Explanation:** The Transaction Manager within the Cloud Services Layer ensures ACID (Atomicity, Consistency, Isolation, Durability) compliance across all operations. It manages concurrent transactions, handles commit/rollback operations, and maintains isolation between concurrent DML operations even when multiple warehouses are accessing the same objects.

---

### Question 17
**Scenario:** An administrator notices that Cloud Services charges have exceeded the 10% threshold. Which activities are MOST likely contributing to excessive Cloud Services consumption?

- A) Large table scans on a medium warehouse
- B) Thousands of small, simple queries and heavy use of SHOW/DESCRIBE commands
- C) Running complex analytical queries with multiple joins
- D) Loading large files via COPY INTO

**Answer: B) Thousands of small, simple queries and heavy use of SHOW/DESCRIBE commands**

**Explanation:** Cloud Services consumption is driven by query compilation/optimization overhead, metadata operations, and authentication. Thousands of small queries each incur compilation costs relative to their warehouse compute usage, pushing the ratio above 10%. Large analytical queries have high warehouse consumption, keeping Cloud Services proportionally low. Excessive API calls and metadata operations also drive Cloud Services costs.

---

### Question 18
The Cloud Services Layer operates across how many availability zones in a given region?

- A) Single availability zone for cost efficiency
- B) Multiple availability zones for high availability
- C) It depends on the Snowflake edition
- D) Across all regions within a cloud provider

**Answer: B) Multiple availability zones for high availability**

**Explanation:** The Cloud Services Layer is deployed across multiple availability zones within a region to ensure high availability and fault tolerance. This is true regardless of edition. If one availability zone experiences an issue, the Cloud Services Layer continues operating from other zones, providing resilience without requiring customer action.

---

### Question 19
Which statement about Global Services in Snowflake is TRUE?

- A) Global Services only work with Enterprise edition or higher
- B) Snowflake's Global Services include features like database replication and failover across regions
- C) Global Services replace the need for the Cloud Services Layer
- D) Global Services are only available on AWS

**Answer: B) Snowflake's Global Services include features like database replication and failover across regions**

**Explanation:** Global Services operate above the regional Cloud Services Layer and enable cross-region and cross-cloud capabilities such as database replication, failover/failback, and data sharing across regions. These services coordinate between Snowflake deployments in different regions. They complement (not replace) the regional Cloud Services Layer.

---

### Question 20
What happens in the Cloud Services Layer when you execute a CREATE TABLE statement?

- A) A new micro-partition is created in object storage
- B) The metadata store is updated with the table definition; no data is written to storage
- C) A virtual warehouse is automatically started to create the table
- D) The table is created in both the metadata store and storage simultaneously

**Answer: B) The metadata store is updated with the table definition; no data is written to storage**

**Explanation:** DDL statements like CREATE TABLE only modify metadata in the Cloud Services Layer. No data is written to the storage layer because the table is empty. No warehouse is needed for DDL operations. The metadata store records the table's structure, owner, privileges, and other properties. Data is only written to storage when rows are inserted.

---

## Topic 1.3: Storage and Micro-Partitions

### Question 21
What is the approximate size of each micro-partition in Snowflake (before compression)?

- A) 1-5 MB
- B) 16 MB (uncompressed)
- C) 50-500 MB (uncompressed, with each storing 50-500 MB of data)
- D) 1 GB

**Answer: C) 50-500 MB (uncompressed, with each storing 50-500 MB of data)**

**Explanation:** Each micro-partition in Snowflake contains between 50 and 500 MB of uncompressed data. After Snowflake applies its columnar compression, the actual storage size is significantly smaller (often 5-10x compression). This size range is optimal for parallel processing and pruning efficiency. The 16 MB figure is the compressed target size on disk.

---

### Question 22
How does Snowflake organize data within micro-partitions?

- A) Row-oriented format optimized for transactional workloads
- B) Columnar format with metadata about min/max values per column
- C) Key-value pairs similar to NoSQL databases
- D) Unstructured format that relies on external indexing

**Answer: B) Columnar format with metadata about min/max values per column**

**Explanation:** Snowflake stores data in a columnar (hybrid-columnar) format within each micro-partition. Each partition also stores metadata including the range of values (min/max) for each column, the number of distinct values, and NULL counts. This metadata enables partition pruning without reading the actual data, and columnar storage enables efficient compression and analytical query patterns.

---

### Question 23
**Scenario:** A data engineer inserts 1 million rows into a table, then updates 100 rows. How does Snowflake handle this at the storage level?

- A) It updates the 100 rows in place within the existing micro-partitions
- B) It creates entirely new micro-partitions containing the modified rows and marks old partitions as deleted
- C) It creates a separate change log that is merged during compaction
- D) It locks the affected micro-partitions during the update

**Answer: B) It creates entirely new micro-partitions containing the modified rows and marks old partitions as deleted**

**Explanation:** Snowflake uses immutable storage. Micro-partitions are never modified in place. For an UPDATE, Snowflake recreates the affected micro-partitions with the new values and marks the old versions for deletion (retained during Time Travel period). This copy-on-write approach enables Time Travel, zero-copy cloning, and eliminates locking at the storage level.

---

### Question 24
What is the primary benefit of Snowflake's automatic micro-partition pruning?

- A) It reduces storage costs by compressing data
- B) It eliminates the need for indexes by skipping irrelevant partitions during query execution
- C) It automatically backs up data for disaster recovery
- D) It improves write performance for INSERT operations

**Answer: B) It eliminates the need for indexes by skipping irrelevant partitions during query execution**

**Explanation:** Partition pruning allows Snowflake to skip entire micro-partitions that cannot contain relevant data based on the query's filter predicates and the partition metadata (min/max values). This eliminates the need for traditional indexes (B-tree, bitmap) while achieving similar or better performance for analytical queries. It reduces the amount of data scanned, improving both performance and cost.

---

### Question 25
Which storage format does Snowflake use for its micro-partitions?

- A) Standard Apache Parquet files
- B) Apache ORC files
- C) A proprietary columnar format (FDN format)
- D) CSV files with custom compression

**Answer: C) A proprietary columnar format (FDN format)**

**Explanation:** Snowflake uses its own proprietary columnar file format, not standard open formats like Parquet or ORC. This proprietary format is optimized for Snowflake's specific query engine, compression algorithms, and metadata needs. While Snowflake can read external Parquet/ORC files (via external tables), its internal storage uses this custom format for optimal performance.

---

### Question 26
**Scenario:** A table has 1000 micro-partitions. A query filters on a column where all values in the table are between 1 and 100, but each partition stores a random distribution. How effective will pruning be?

- A) Highly effective - Snowflake will prune most partitions
- B) Ineffective - overlapping ranges mean most partitions must be scanned
- C) Moderately effective - exactly 50% of partitions will be pruned
- D) Pruning is not attempted on numeric columns

**Answer: B) Ineffective - overlapping ranges mean most partitions must be scanned**

**Explanation:** Partition pruning relies on non-overlapping value ranges across partitions. If data is randomly distributed, each partition's min/max range likely spans most of the value domain (1-100), meaning few partitions can be eliminated. This is why natural clustering (e.g., data loaded in date order) or explicit clustering keys improve pruning effectiveness by reducing range overlap.

---

### Question 27
What type of object storage does Snowflake use for its storage layer?

- A) Snowflake's proprietary distributed file system
- B) The native object storage of the underlying cloud provider (S3, Azure Blob, GCS)
- C) Network-attached storage (NAS)
- D) Local SSD storage on compute nodes

**Answer: B) The native object storage of the underlying cloud provider (S3, Azure Blob, GCS)**

**Explanation:** Snowflake leverages the native object storage service of whichever cloud it runs on: Amazon S3 on AWS, Azure Blob Storage on Azure, and Google Cloud Storage on GCP. This provides virtually unlimited scalability, high durability (11 nines), and cost-effective storage. Snowflake manages the data within these services transparently.

---

### Question 28
How does Snowflake handle data compression in its storage layer?

- A) Users must specify compression algorithms when creating tables
- B) Snowflake automatically compresses data using optimal algorithms per column, requiring no user configuration
- C) Only specific data types support compression
- D) Compression is only applied to cold/archived data

**Answer: B) Snowflake automatically compresses data using optimal algorithms per column, requiring no user configuration**

**Explanation:** Snowflake automatically selects and applies the best compression algorithm for each column based on the data type and distribution. This is done transparently without any user configuration. Different columns may use different compression methods (LZ4, ZSTD, etc.) depending on what achieves the best ratio. Typical compression achieves 4-10x reduction.

---

### Question 29
What is the relationship between clustering and micro-partitions?

- A) Clustering replaces micro-partitions with indexed data blocks
- B) Clustering defines how data is physically organized across micro-partitions to improve pruning
- C) Clustering is only used for semi-structured data
- D) Clustering creates additional copies of micro-partitions

**Answer: B) Clustering defines how data is physically organized across micro-partitions to improve pruning**

**Explanation:** Clustering determines the physical ordering of data across micro-partitions. A well-clustered table has micro-partitions with non-overlapping ranges on the clustering key columns, enabling effective pruning. Snowflake automatically clusters data on ingestion order, but you can define explicit clustering keys for tables where the natural ingestion order doesn't align with common query patterns.

---

### Question 30
Which statement about Snowflake's storage billing is TRUE?

- A) You are billed for uncompressed data size
- B) You are billed for compressed data size plus Time Travel and Fail-safe storage
- C) Storage is free; you only pay for compute
- D) Storage costs are included in warehouse credits

**Answer: B) You are billed for compressed data size plus Time Travel and Fail-safe storage**

**Explanation:** Snowflake bills storage based on the average compressed size of data stored, including active data, Time Travel data (retained versions after changes), and Fail-safe data (7-day recovery period after Time Travel expires). Storage is billed separately from compute (credits), and the cost varies by cloud provider and region. On-demand storage is billed per TB per month.

---

## Topic 1.4: Virtual Warehouses

### Question 31
What is the smallest virtual warehouse size available in Snowflake?

- A) X-Small (1 credit/hour)
- B) Small (2 credits/hour)
- C) Micro (0.5 credits/hour)
- D) Nano (0.25 credits/hour)

**Answer: A) X-Small (1 credit/hour)**

**Explanation:** The smallest warehouse size is X-Small, which consumes 1 credit per hour when running. Each subsequent size doubles the compute resources and credits: Small (2), Medium (4), Large (8), X-Large (16), 2X-Large (32), 3X-Large (64), 4X-Large (128), 5X-Large (256), and 6X-Large (512 credits/hour).

---

### Question 32
When you increase a virtual warehouse from Medium to Large, what happens to currently executing queries?

- A) All running queries are cancelled and must be restarted
- B) Running queries complete on the existing resources; new queries use the larger size
- C) Running queries are automatically distributed across the new resources
- D) The warehouse must be suspended first before resizing

**Answer: B) Running queries complete on the existing resources; new queries use the larger size**

**Explanation:** Resizing a warehouse is a non-disruptive operation. Queries that are already executing continue on the resources they were allocated. The additional compute resources from the resize become available for new queries. You do not need to suspend the warehouse to resize it, and no queries are cancelled.

---

### Question 33
**Scenario:** A warehouse is configured with AUTO_SUSPEND = 300 and AUTO_RESUME = TRUE. A user submits a query after the warehouse has been idle for 6 minutes. What happens?

- A) The query fails because the warehouse is suspended
- B) The warehouse automatically resumes, and the query is executed after a brief startup delay
- C) The user must manually resume the warehouse before querying
- D) The query is queued indefinitely until an admin resumes the warehouse

**Answer: B) The warehouse automatically resumes, and the query is executed after a brief startup delay**

**Explanation:** With AUTO_SUSPEND = 300 (seconds = 5 minutes), the warehouse suspended after 5 minutes of inactivity. With AUTO_RESUME = TRUE, when the user submits a query, Snowflake automatically resumes the warehouse. There is a brief provisioning delay (typically seconds), after which the query executes normally. The user does not need to take any manual action.

---

### Question 34
What is the minimum AUTO_SUSPEND setting for a virtual warehouse?

- A) 0 seconds (immediate)
- B) 60 seconds (1 minute)
- C) 300 seconds (5 minutes)
- D) 600 seconds (10 minutes)

**Answer: B) 60 seconds (1 minute)**

**Explanation:** The minimum AUTO_SUSPEND value is 60 seconds (1 minute) when set through the UI, though it can be set to 0 via SQL to mean "never auto-suspend." Snowflake recommends keeping AUTO_SUSPEND low for cost savings but notes that frequent suspend/resume cycles can impact the local disk cache (warehouse cache). The value cannot be set between 1-59 seconds.

---

### Question 35
How does multi-cluster warehousing work in Snowflake?

- A) It splits a single large query across multiple warehouses
- B) It automatically adds or removes clusters within a warehouse to handle concurrency
- C) It replicates the warehouse across multiple regions
- D) It combines multiple warehouses into a single logical unit

**Answer: B) It automatically adds or removes clusters within a warehouse to handle concurrency**

**Explanation:** Multi-cluster warehouses (available in Enterprise edition and above) automatically scale out by adding additional clusters when query concurrency increases, and scale back in when demand decreases. Each cluster is the same size as the original. This handles concurrency (many users querying simultaneously) not complexity (a single large query). A single query runs on one cluster.

---

### Question 36
**Scenario:** An X-Large warehouse is running a complex query that scans 2TB of data. If you resize the warehouse to 2X-Large while the query is running, will the query run faster?

- A) Yes, the running query immediately gets double the resources
- B) No, the running query continues on the X-Large resources it was allocated
- C) Yes, but only after the query completes its current execution stage
- D) The query will be cancelled and automatically resubmitted on the larger warehouse

**Answer: B) No, the running query continues on the X-Large resources it was allocated**

**Explanation:** As noted earlier, warehouse resize does not affect currently executing queries. The running query will continue to execute using the X-Large resources it was initially allocated. Only new queries submitted after the resize completes will benefit from the 2X-Large compute resources. To get better performance, you would need to cancel and resubmit the query.

---

### Question 37
What is the billing minimum when a warehouse starts (resumes)?

- A) 30 seconds minimum
- B) 60 seconds (1 minute) minimum
- C) 5 minutes minimum
- D) No minimum; billed per second from the start

**Answer: B) 60 seconds (1 minute) minimum**

**Explanation:** When a warehouse is started or resumes, there is a minimum billing charge of 60 seconds (1 minute). After the first minute, billing continues per second of usage. This means even if a warehouse runs a query in 10 seconds and then suspends, you are charged for the full 60 seconds. This applies each time the warehouse resumes.

---

### Question 38
Which warehouse scaling policy starts additional clusters only when queries are queuing?

- A) Standard scaling policy
- B) Economy scaling policy
- C) Aggressive scaling policy
- D) Conservative scaling policy

**Answer: B) Economy scaling policy**

**Explanation:** The Economy scaling policy is conservative - it only starts additional clusters when the system estimates that enough queries are queuing to keep the new cluster busy for at least 6 minutes. The Standard policy is more aggressive, starting new clusters as soon as queries begin queuing. Economy saves credits but may result in some queries waiting; Standard minimizes wait time but costs more.

---

### Question 39
A 4X-Large warehouse consumes how many credits per hour?

- A) 32 credits/hour
- B) 64 credits/hour
- C) 128 credits/hour
- D) 256 credits/hour

**Answer: C) 128 credits/hour**

**Explanation:** Warehouse sizes double in credits with each step: X-Small=1, Small=2, Medium=4, Large=8, X-Large=16, 2X-Large=32, 3X-Large=64, 4X-Large=128, 5X-Large=256, 6X-Large=512. A 4X-Large warehouse provides massive compute power at 128 credits/hour and is appropriate for extremely large or complex workloads.

---

### Question 40
**Scenario:** Your multi-cluster warehouse has MIN_CLUSTER_COUNT=1 and MAX_CLUSTER_COUNT=3 with Standard scaling policy. During peak hours, you observe all 3 clusters running. Which action would you take if query performance is still slow for individual complex queries?

- A) Increase MAX_CLUSTER_COUNT to 6
- B) Resize the warehouse to a larger size
- C) Switch to Economy scaling policy
- D) Add more users to distribute the load

**Answer: B) Resize the warehouse to a larger size**

**Explanation:** Multi-cluster warehouses handle concurrency (many simultaneous queries) by adding clusters. However, individual query performance is determined by the warehouse size. If individual complex queries are slow, you need more compute per query, which means resizing to a larger warehouse. Adding more clusters would only help if the issue were queue wait time due to concurrent users, not single-query execution speed.

---

## Topic 1.5: Caching

### Question 41
Which three types of caching does Snowflake employ?

- A) Browser cache, CDN cache, Database cache
- B) Result cache, Local disk cache (warehouse cache), Metadata cache
- C) L1 cache, L2 cache, L3 cache
- D) Query cache, Table cache, Column cache

**Answer: B) Result cache, Local disk cache (warehouse cache), Metadata cache**

**Explanation:** Snowflake uses three caching layers: (1) Result Cache - stores complete query results in Cloud Services for 24 hours, (2) Local Disk Cache (Warehouse Cache) - stores raw data from micro-partitions on local SSD of warehouse nodes, and (3) Metadata Cache - stores metadata about objects for quick access to table statistics, schema info, etc.

---

### Question 42
How long does the Result Cache persist after a query is executed?

- A) 1 hour
- B) 4 hours
- C) 24 hours from last access
- D) 7 days

**Answer: C) 24 hours from last access**

**Explanation:** The Result Cache retains query results for 24 hours. Importantly, the timer resets each time the cached result is accessed, so frequently-run identical queries can stay cached indefinitely (up to 31 days maximum). If 24 hours pass without the same query being submitted, the cached result expires and is purged.

---

### Question 43
**Scenario:** A user runs the exact same SELECT query twice within 5 minutes. The underlying table has not changed. The second execution returns instantly with zero warehouse compute used. Which cache served this result?

- A) Local Disk Cache
- B) Metadata Cache
- C) Result Cache
- D) Operating system page cache

**Answer: C) Result Cache**

**Explanation:** The Result Cache stores complete query results in the Cloud Services Layer. When an identical query is submitted and the underlying data hasn't changed, Snowflake returns the cached result directly without activating the warehouse (zero compute cost). This is why the second query returns instantly with no warehouse usage. Local disk cache would still require the warehouse to be active.

---

### Question 44
Which condition would INVALIDATE the Result Cache for a previously cached query?

- A) A different user running the same query
- B) The underlying table's data being modified (INSERT, UPDATE, DELETE)
- C) Running the query from a different warehouse
- D) Changing the session timezone

**Answer: B) The underlying table's data being modified (INSERT, UPDATE, DELETE)**

**Explanation:** The Result Cache is invalidated when the underlying data changes. DML operations (INSERT, UPDATE, DELETE, MERGE) on any table referenced in the query invalidate the cached result. Notably, the Result Cache is available across users and warehouses - a different user or warehouse can benefit from it. However, session-level settings that affect results (like timezone for CURRENT_TIMESTAMP) also affect cache matching.

---

### Question 45
What happens to the Local Disk Cache (Warehouse Cache) when a warehouse is suspended?

- A) It is preserved for when the warehouse resumes
- B) It is dropped/cleared and must be rebuilt when the warehouse resumes
- C) It is moved to the Result Cache
- D) It is written back to object storage

**Answer: B) It is dropped/cleared and must be rebuilt when the warehouse resumes**

**Explanation:** The Local Disk Cache exists on the local SSD storage of the compute nodes that make up the warehouse. When a warehouse is suspended, those compute nodes are released, and the local disk cache is lost. When the warehouse resumes, the cache starts empty ("cold") and rebuilds as queries read data from remote storage. This is why very aggressive AUTO_SUSPEND can impact performance.

---

### Question 46
Which statement about the Result Cache is FALSE?

- A) It requires no warehouse to serve cached results
- B) It is available across different warehouses for the same query
- C) It works only for SELECT statements, not SHOW commands
- D) It persists for up to 24 hours since last access

**Answer: C) It works only for SELECT statements, not SHOW commands**

**Explanation:** The Result Cache works for SELECT queries, SHOW commands, and other query types that produce results. It is not limited to SELECT statements. The other statements are all true: no warehouse is needed (results come from Cloud Services), it works across warehouses, and it persists 24 hours from last access.

---

### Question 47
**Scenario:** A warehouse was recently started and is executing queries against a large table for the first time. Query performance is initially slow but improves significantly for subsequent queries against the same table. What explains this?

- A) The Query Optimizer is learning the data distribution
- B) The Local Disk Cache (Warehouse Cache) is being populated with micro-partition data from remote storage
- C) Snowflake is building indexes on the table
- D) The Result Cache is being populated

**Answer: B) The Local Disk Cache (Warehouse Cache) is being populated with micro-partition data from remote storage**

**Explanation:** When a warehouse first queries a table, it must read micro-partitions from remote object storage (S3/Azure Blob/GCS), which incurs network latency. As data is read, it is cached on the local SSD of the warehouse nodes. Subsequent queries against the same data can read from local SSD (much faster) instead of remote storage. This is the "warming" of the warehouse cache.

---

### Question 48
Can the Result Cache be explicitly disabled?

- A) No, it is always active and cannot be disabled
- B) Yes, by setting the session parameter USE_CACHED_RESULT = FALSE
- C) Yes, but only by an ACCOUNTADMIN
- D) It can only be disabled at the account level

**Answer: B) Yes, by setting the session parameter USE_CACHED_RESULT = FALSE**

**Explanation:** You can disable the Result Cache for a session by running `ALTER SESSION SET USE_CACHED_RESULT = FALSE`. This forces Snowflake to re-execute queries even if matching results exist in cache. This is useful for benchmarking or when you need guaranteed fresh execution. Any user can set this at the session level without requiring ACCOUNTADMIN.

---

### Question 49
How does the Metadata Cache benefit query performance?

- A) It stores frequently accessed rows in memory
- B) It allows the Cloud Services Layer to answer metadata queries and perform pruning without accessing storage
- C) It pre-computes aggregation results for common queries
- D) It caches connection credentials to speed up authentication

**Answer: B) It allows the Cloud Services Layer to answer metadata queries and perform pruning without accessing storage**

**Explanation:** The Metadata Cache stores information about tables, schemas, micro-partition statistics (min/max, row counts, NULL counts), and other object properties. This allows the Cloud Services Layer to serve SHOW/DESCRIBE commands instantly and perform partition pruning without reading from object storage. It is essential for the query optimizer to build efficient execution plans.

---

### Question 50
**Scenario:** Two users with identical roles run the exact same query. User A uses Warehouse WH1, and User B uses Warehouse WH2. User A ran the query 10 minutes ago. Will User B benefit from any caching?

- A) No, caches are isolated per warehouse and per user
- B) Yes, User B will benefit from the Result Cache since it is shared across warehouses
- C) Only if both warehouses are the same size
- D) Only if the users are in the same role

**Answer: B) Yes, User B will benefit from the Result Cache since it is shared across warehouses**

**Explanation:** The Result Cache is maintained in the Cloud Services Layer and is shared across all warehouses and users (provided the query is identical and the user has appropriate privileges). User B will receive the cached result without activating WH2, paying zero compute. The Local Disk Cache is warehouse-specific, but the Result Cache is global within the account.

---

## Topic 1.6: Editions and Features

### Question 51
Which Snowflake edition is required for multi-cluster virtual warehouses?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

**Answer: B) Enterprise**

**Explanation:** Multi-cluster warehouses require Enterprise edition or higher. Standard edition only supports single-cluster warehouses. Enterprise adds multi-cluster warehouses, materialized views, column-level security, search optimization, and up to 90 days of Time Travel. Business Critical and VPS include Enterprise features plus additional security.

---

### Question 52
What is the maximum Time Travel retention period for the Standard edition?

- A) 0 days (not available)
- B) 1 day
- C) 7 days
- D) 90 days

**Answer: B) 1 day**

**Explanation:** Standard edition supports Time Travel for 0 or 1 day only. Enterprise edition and above extend this to up to 90 days for permanent tables (transient and temporary tables are still limited to 0 or 1 day regardless of edition). The 90-day retention allows querying historical data and undropping objects for up to 90 days after changes or drops.

---

### Question 53
**Scenario:** A healthcare company must comply with HIPAA regulations and requires support for PHI (Protected Health Information). They also need data encryption with customer-managed keys. Which is the MINIMUM Snowflake edition they should select?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

**Answer: C) Business Critical**

**Explanation:** Business Critical edition (formerly Enterprise for Sensitive Data / ESD) is designed for organizations with strict compliance requirements like HIPAA, PCI-DSS, and SOC 2 Type II. It includes Tri-Secret Secure (customer-managed keys), enhanced security, HIPAA and HITRUST CSF support, and AWS/Azure PrivateLink support. Standard and Enterprise do not meet these compliance requirements.

---

### Question 54
Which feature is exclusive to the Enterprise edition (not available in Standard)?

- A) Time Travel (1 day)
- B) Data Sharing
- C) Materialized Views
- D) Fail-safe

**Answer: C) Materialized Views**

**Explanation:** Materialized Views are available starting with Enterprise edition. Standard edition includes Time Travel (1 day), Data Sharing, Fail-safe, and many other features. Enterprise adds materialized views, multi-cluster warehouses, column-level security (masking policies), search optimization service, 90-day Time Travel, and resource monitors with notification capabilities.

---

### Question 55
How long is the Fail-safe period in Snowflake, and can it be configured?

- A) 7 days; it is configurable per table
- B) 7 days; it is NOT configurable and is managed by Snowflake
- C) 30 days; it is configurable by ACCOUNTADMIN
- D) 1 day for Standard, 7 days for Enterprise

**Answer: B) 7 days; it is NOT configurable and is managed by Snowflake**

**Explanation:** Fail-safe provides a non-configurable 7-day period of data recovery AFTER Time Travel expires. It is managed entirely by Snowflake and cannot be adjusted by users. Data in Fail-safe can only be recovered by Snowflake support. Fail-safe exists for all editions (Standard, Enterprise, Business Critical, VPS) and applies to permanent tables only (not transient or temporary).

---

### Question 56
Which Snowflake edition provides the highest level of security with a dedicated, isolated environment?

- A) Enterprise
- B) Business Critical
- C) Virtual Private Snowflake (VPS)
- D) Government edition

**Answer: C) Virtual Private Snowflake (VPS)**

**Explanation:** Virtual Private Snowflake (VPS) provides a completely separate Snowflake deployment with dedicated infrastructure (compute, storage, and Cloud Services). It offers the highest level of isolation for organizations with the most stringent security requirements (financial institutions, government agencies). It includes all Business Critical features plus complete physical isolation.

---

### Question 57
**Scenario:** A company on Standard edition wants to implement column-level masking policies to protect sensitive PII data. Can they do this?

- A) Yes, column-level masking is available on all editions
- B) No, they must upgrade to Enterprise edition or higher
- C) Yes, but only through custom UDFs
- D) Yes, but only for specific data types

**Answer: B) No, they must upgrade to Enterprise edition or higher**

**Explanation:** Dynamic Data Masking (column-level security through masking policies) is an Enterprise edition feature. Standard edition does not support masking policies. The company would need to upgrade to Enterprise (or higher) to use CREATE MASKING POLICY to protect PII. Standard does support basic RBAC with roles and privileges but not policy-based masking.

---

### Question 58
What is the credit cost difference between Snowflake editions for the same warehouse size?

- A) All editions cost the same credits per hour
- B) Higher editions cost more credits per compute hour
- C) Higher editions have lower credit costs due to volume discounts
- D) Credit costs vary only by cloud provider, not edition

**Answer: B) Higher editions cost more credits per compute hour**

**Explanation:** The credit price (in dollars) increases with higher editions. While a Medium warehouse always consumes 4 credits/hour regardless of edition, the dollar cost per credit is higher for Enterprise vs Standard, Business Critical vs Enterprise, and VPS vs Business Critical. This reflects the additional services, compliance certifications, and infrastructure provided by higher editions.

---

### Question 59
Which feature is available in ALL Snowflake editions?

- A) Materialized views
- B) Multi-cluster warehouses
- C) Secure data sharing
- D) Tri-Secret Secure encryption

**Answer: C) Secure data sharing**

**Explanation:** Secure Data Sharing is available across all Snowflake editions (Standard, Enterprise, Business Critical, VPS). It allows sharing live data between accounts without copying. Materialized views and multi-cluster warehouses require Enterprise+. Tri-Secret Secure requires Business Critical+. Data sharing is a foundational Snowflake capability regardless of edition.

---

### Question 60
**Scenario:** An organization requires database failover and disaster recovery capabilities with the ability to fail over to a different region. What is the minimum edition required?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

**Answer: C) Business Critical**

**Explanation:** Database failover/failback and Client Redirect (allowing applications to automatically connect to a secondary deployment) are Business Critical edition features. While Enterprise supports database replication, the automatic failover and business continuity features (disaster recovery) require Business Critical or higher. Standard supports neither replication nor failover across accounts.

---

### Question 61
Which statement accurately compares Snowflake editions?

- A) Standard edition does not support any form of data encryption
- B) Enterprise adds features primarily focused on performance and governance over Standard
- C) Business Critical and Enterprise have identical features except for marketing name
- D) VPS is the only edition that supports RBAC

**Answer: B) Enterprise adds features primarily focused on performance and governance over Standard**

**Explanation:** Enterprise adds performance features (multi-cluster warehouses, materialized views, search optimization) and governance features (column-level security, masking policies, row access policies, 90-day Time Travel, object tagging). All editions support encryption (AES-256 at rest, TLS in transit) and RBAC. Business Critical adds compliance and enhanced security beyond Enterprise.

---

### Question 62
What is the Time Travel retention for TRANSIENT tables, regardless of edition?

- A) 0 days only
- B) 0 or 1 day maximum
- C) Up to 90 days on Enterprise
- D) Same as permanent tables for the edition

**Answer: B) 0 or 1 day maximum**

**Explanation:** Transient tables have a maximum Time Travel retention of 0 or 1 day, regardless of the Snowflake edition. Even on Enterprise edition (which supports up to 90 days for permanent tables), transient tables are capped at 1 day. Additionally, transient tables have NO Fail-safe period. This makes them suitable for ETL staging data where recovery is not critical.

---

### Question 63
**Scenario:** A financial services firm needs all of the following: HIPAA compliance, SOC 1 Type II certification, AWS PrivateLink connectivity, and customer-managed encryption keys. They also want to minimize cost. Which edition should they choose?

- A) Enterprise (cheapest option that meets most needs)
- B) Business Critical (minimum edition meeting all requirements)
- C) Virtual Private Snowflake (only option for financial services)
- D) Standard with add-on security package

**Answer: B) Business Critical (minimum edition meeting all requirements)**

**Explanation:** Business Critical is the minimum edition that provides HIPAA compliance support, SOC 1/2 Type II, AWS/Azure PrivateLink, and Tri-Secret Secure (customer-managed keys). Enterprise lacks these compliance certifications and PrivateLink support. VPS would also meet the requirements but at significantly higher cost (the question asks to minimize cost). There is no "add-on security package."

---

## Summary Table

| Topic | Questions | Coverage |
|-------|-----------|----------|
| 1.1 Architecture | Q1-Q10 | Layers, hybrid architecture, independence, cross-cloud |
| 1.2 Cloud Services | Q11-Q20 | Functions, billing, optimization, metadata, security |
| 1.3 Storage/Micro-Partitions | Q21-Q30 | Size, format, pruning, immutability, clustering, billing |
| 1.4 Virtual Warehouses | Q31-Q40 | Sizing, scaling, billing, multi-cluster, policies |
| 1.5 Caching | Q41-Q50 | Result cache, warehouse cache, metadata cache, invalidation |
| 1.6 Editions | Q51-Q63 | Feature comparison, Time Travel limits, compliance, costs |

---

<center>

## Navigation

[Back to Domain 1 Study Guide](../notes/) | [Answer Key (Quick Reference)](#summary-table) | [Domain 2 Quiz →](../../2_Account_Access_and_Security/quiz/)

---

*Generated for SnowPro Core COF-C03 preparation. Last updated: 2026.*
*Always verify against the latest Snowflake documentation for exam accuracy.*

</center>
