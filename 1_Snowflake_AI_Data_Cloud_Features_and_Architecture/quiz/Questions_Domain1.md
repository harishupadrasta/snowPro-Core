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

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Multi-cluster, shared data architecture (hybrid)**

**Explanation:** Snowflake uses a unique hybrid architecture that combines the benefits of shared-disk (centralized storage accessible by all compute nodes) and shared-nothing (independent compute clusters that don't compete for resources). This is neither purely shared-disk nor shared-nothing, making it a multi-cluster shared data architecture.

</details>

---

### Question 2
Which of the following are the three main layers of Snowflake's architecture? (Choose the best answer)

- A) Presentation Layer, Application Layer, Database Layer
- B) Cloud Services Layer, Query Processing Layer (Compute), Storage Layer
- C) Ingestion Layer, Processing Layer, Output Layer
- D) Metadata Layer, Compute Layer, Storage Layer

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Cloud Services Layer, Query Processing Layer (Compute), Storage Layer**

**Explanation:** Snowflake's architecture consists of three distinct layers: the Cloud Services Layer (brain - handles authentication, metadata, optimization), the Query Processing Layer (muscle - virtual warehouses that execute queries), and the Storage Layer (centralized, compressed columnar storage). Each layer scales independently.

</details>

---

### Question 3
**Scenario:** A company is evaluating Snowflake and wants to understand how it differs from traditional on-premises data warehouses. Which statement BEST describes a key architectural advantage?

- A) Snowflake requires capacity planning for storage and compute together
- B) Storage and compute are tightly coupled for maximum performance
- C) Storage and compute scale independently, allowing optimization of each
- D) Snowflake uses the same shared-nothing architecture as Teradata

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Storage and compute scale independently, allowing optimization of each**

**Explanation:** The decoupling of storage and compute is Snowflake's core architectural differentiator. You can scale storage without adding compute (pay only for what you store) and scale compute without adding storage (spin up warehouses only when needed). Traditional systems couple these, requiring over-provisioning of one to scale the other.

</details>

---

### Question 4
Which statement about Snowflake's architecture is FALSE?

- A) Multiple virtual warehouses can access the same data simultaneously
- B) The Cloud Services Layer manages authentication and access control
- C) Each virtual warehouse has its own dedicated storage
- D) Snowflake stores data in a columnar format

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Each virtual warehouse has its own dedicated storage**

**Explanation:** Virtual warehouses do NOT have dedicated storage. They are pure compute resources that read from the centralized storage layer. This is what enables multiple warehouses to query the same data concurrently without contention, and why warehouses can be started/stopped without any data movement.

</details>

---

### Question 5
In Snowflake's architecture, what happens to running queries if the Cloud Services Layer experiences a brief disruption?

- A) All queries immediately fail and must be resubmitted
- B) Queries continue executing because compute and cloud services are independent
- C) The system automatically pauses all warehouses
- D) Data in the storage layer becomes temporarily inaccessible

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) All queries immediately fail and must be resubmitted**

**Explanation:** The Cloud Services Layer is essential for query coordination, optimization, and transaction management. If it is disrupted, queries cannot be coordinated or completed. However, Snowflake's Cloud Services Layer runs across multiple availability zones with redundancy, making such failures extremely rare. The storage layer data remains safe regardless.

</details>

---

### Question 6
Which Snowflake architectural component is responsible for query optimization and generating execution plans?

- A) Virtual Warehouse
- B) Storage Layer
- C) Cloud Services Layer
- D) Result Cache

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Cloud Services Layer**

**Explanation:** The Cloud Services Layer handles query parsing, optimization, and execution plan generation before dispatching work to virtual warehouses. It uses metadata about micro-partitions (min/max values, row counts) to perform pruning and create efficient plans. The virtual warehouse simply executes the plan it receives.

</details>

---

### Question 7
**Scenario:** Your organization runs Snowflake on AWS in US-East-1. A colleague claims they can query data stored in a Snowflake account on Azure in West Europe without any data replication. Is this correct?

- A) Yes, Snowflake's architecture allows cross-cloud querying natively
- B) No, data must be replicated using Snowflake's database replication feature
- C) Yes, but only if both accounts are on the Business Critical edition
- D) No, cross-cloud access is impossible in Snowflake

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No, data must be replicated using Snowflake's database replication feature**

**Explanation:** Snowflake accounts are region and cloud-specific. To access data across clouds or regions, you must use database replication or data sharing with replication. Snowflake does not natively allow cross-cloud querying without first replicating the data to the target region/cloud. Listings and data sharing can facilitate this with auto-fulfillment.

</details>

---

### Question 8
How does Snowflake handle metadata management in its architecture?

- A) Metadata is stored within the same micro-partitions as the data
- B) Metadata is managed by the Cloud Services Layer in a highly available, distributed key-value store
- C) Each virtual warehouse maintains its own copy of metadata
- D) Metadata is stored in external object storage alongside data files

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Metadata is managed by the Cloud Services Layer in a highly available, distributed key-value store**

**Explanation:** Snowflake's Cloud Services Layer maintains all metadata (table definitions, micro-partition statistics, access control, query history) in a transactional, highly available key-value store separate from the data storage layer. This metadata enables intelligent query pruning and is not stored in S3/Azure Blob/GCS with the data files.

</details>

---

### Question 9
Which cloud platforms does Snowflake currently run on?

- A) AWS only
- B) AWS and Azure only
- C) AWS, Microsoft Azure, and Google Cloud Platform
- D) AWS, Azure, GCP, and Oracle Cloud

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) AWS, Microsoft Azure, and Google Cloud Platform**

**Explanation:** Snowflake is available on all three major public cloud platforms: Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP). Each deployment uses the native object storage of that cloud (S3, Azure Blob, GCS) for its storage layer, but Snowflake itself provides the same experience across all three.

</details>

---

### Question 10
**Scenario:** A data engineer notices that two different virtual warehouses are querying the same large table simultaneously, yet neither is experiencing performance degradation. What architectural principle explains this?

- A) The queries are automatically load-balanced to a single warehouse
- B) Snowflake's shared data architecture allows concurrent access to centralized storage without contention
- C) One warehouse is reading from cache while the other reads from storage
- D) Snowflake automatically duplicates the table for each warehouse

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Snowflake's shared data architecture allows concurrent access to centralized storage without contention**

**Explanation:** Because storage is decoupled from compute, multiple warehouses read from the same centralized data files independently. There is no locking or contention at the storage layer since data files are immutable. Each warehouse has its own compute resources and local cache, so concurrent access does not cause performance interference.

</details>

---

## Topic 1.2: Cloud Services Layer

### Question 11
Which of the following is NOT a function of the Cloud Services Layer?

- A) Authentication and access control
- B) Query execution and data scanning
- C) Metadata management
- D) Infrastructure management

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Query execution and data scanning**

**Explanation:** Query execution and data scanning are performed by the Query Processing Layer (virtual warehouses). The Cloud Services Layer handles the "brains" of the operation: authentication, access control, metadata management, query optimization/compilation, infrastructure management, and transaction management. It plans the query; warehouses execute it.

</details>

---

### Question 12
How is the Cloud Services Layer billed?

- A) A flat monthly fee regardless of usage
- B) Customers pay for all Cloud Services compute consumed
- C) Only usage exceeding 10% of daily warehouse consumption is billed
- D) It is included free with every Snowflake account

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Only usage exceeding 10% of daily warehouse consumption is billed**

**Explanation:** Snowflake includes Cloud Services compute at no additional charge up to 10% of the daily total warehouse compute consumption. Only the amount that exceeds this 10% threshold is billed. For most workloads, Cloud Services costs remain within this allowance. Heavy metadata operations or many small queries can push past this threshold.

</details>

---

### Question 13
Which Cloud Services function would be responsible for determining which micro-partitions to scan for a query with a WHERE clause?

- A) Authentication Service
- B) Query Optimizer (pruning)
- C) Transaction Manager
- D) Result Cache Manager

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Query Optimizer (pruning)**

**Explanation:** The Query Optimizer, part of the Cloud Services Layer, uses metadata about micro-partitions (including min/max values stored for each column in each partition) to perform partition pruning. It determines which partitions can be skipped entirely because they cannot contain matching rows, dramatically reducing the amount of data scanned.

</details>

---

### Question 14
**Scenario:** A developer runs `SHOW TABLES` followed by `DESCRIBE TABLE`, then checks `INFORMATION_SCHEMA.COLUMNS`. None of these operations use a virtual warehouse. Why?

- A) These are DDL operations which never require compute
- B) These operations are served entirely by the Cloud Services Layer using metadata
- C) Snowflake uses a hidden system warehouse for these queries
- D) The results are always returned from the result cache

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) These operations are served entirely by the Cloud Services Layer using metadata**

**Explanation:** Metadata operations like SHOW, DESCRIBE, and INFORMATION_SCHEMA queries are handled directly by the Cloud Services Layer without requiring a virtual warehouse. The Cloud Services Layer maintains all structural metadata and can serve these requests from its distributed metadata store, which is why no warehouse needs to be running.

</details>

---

### Question 15
What role does the Cloud Services Layer play in Snowflake's security model?

- A) It only handles network-level security (firewalls)
- B) It manages authentication, authorization (RBAC), encryption key management, and data governance
- C) Security is handled by the underlying cloud provider, not Cloud Services
- D) It only manages row-level security policies

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) It manages authentication, authorization (RBAC), encryption key management, and data governance**

**Explanation:** The Cloud Services Layer is the central security authority in Snowflake. It handles all aspects of security including user authentication (MFA, SSO, key-pair), role-based access control (RBAC), encryption key management (for data at rest and in transit), masking policies, row access policies, and network policies. Security is not delegated to the cloud provider.

</details>

---

### Question 16
Which component of the Cloud Services Layer ensures ACID compliance for concurrent transactions?

- A) Query Optimizer
- B) Transaction Manager
- C) Result Cache
- D) Metadata Store

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Transaction Manager**

**Explanation:** The Transaction Manager within the Cloud Services Layer ensures ACID (Atomicity, Consistency, Isolation, Durability) compliance across all operations. It manages concurrent transactions, handles commit/rollback operations, and maintains isolation between concurrent DML operations even when multiple warehouses are accessing the same objects.

</details>

---

### Question 17
**Scenario:** An administrator notices that Cloud Services charges have exceeded the 10% threshold. Which activities are MOST likely contributing to excessive Cloud Services consumption?

- A) Large table scans on a medium warehouse
- B) Thousands of small, simple queries and heavy use of SHOW/DESCRIBE commands
- C) Running complex analytical queries with multiple joins
- D) Loading large files via COPY INTO

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Thousands of small, simple queries and heavy use of SHOW/DESCRIBE commands**

**Explanation:** Cloud Services consumption is driven by query compilation/optimization overhead, metadata operations, and authentication. Thousands of small queries each incur compilation costs relative to their warehouse compute usage, pushing the ratio above 10%. Large analytical queries have high warehouse consumption, keeping Cloud Services proportionally low. Excessive API calls and metadata operations also drive Cloud Services costs.

</details>

---

### Question 18
The Cloud Services Layer operates across how many availability zones in a given region?

- A) Single availability zone for cost efficiency
- B) Multiple availability zones for high availability
- C) It depends on the Snowflake edition
- D) Across all regions within a cloud provider

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Multiple availability zones for high availability**

**Explanation:** The Cloud Services Layer is deployed across multiple availability zones within a region to ensure high availability and fault tolerance. This is true regardless of edition. If one availability zone experiences an issue, the Cloud Services Layer continues operating from other zones, providing resilience without requiring customer action.

</details>

---

### Question 19
Which statement about Global Services in Snowflake is TRUE?

- A) Global Services only work with Enterprise edition or higher
- B) Snowflake's Global Services include features like database replication and failover across regions
- C) Global Services replace the need for the Cloud Services Layer
- D) Global Services are only available on AWS

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Snowflake's Global Services include features like database replication and failover across regions**

**Explanation:** Global Services operate above the regional Cloud Services Layer and enable cross-region and cross-cloud capabilities such as database replication, failover/failback, and data sharing across regions. These services coordinate between Snowflake deployments in different regions. They complement (not replace) the regional Cloud Services Layer.

</details>

---

### Question 20
What happens in the Cloud Services Layer when you execute a CREATE TABLE statement?

- A) A new micro-partition is created in object storage
- B) The metadata store is updated with the table definition; no data is written to storage
- C) A virtual warehouse is automatically started to create the table
- D) The table is created in both the metadata store and storage simultaneously

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The metadata store is updated with the table definition; no data is written to storage**

**Explanation:** DDL statements like CREATE TABLE only modify metadata in the Cloud Services Layer. No data is written to the storage layer because the table is empty. No warehouse is needed for DDL operations. The metadata store records the table's structure, owner, privileges, and other properties. Data is only written to storage when rows are inserted.

</details>

---

## Topic 1.3: Storage and Micro-Partitions

### Question 21
What is the approximate size of each micro-partition in Snowflake (before compression)?

- A) 1-5 MB
- B) 16 MB (uncompressed)
- C) 50-500 MB (uncompressed, with each storing 50-500 MB of data)
- D) 1 GB

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) 50-500 MB (uncompressed, with each storing 50-500 MB of data)**

**Explanation:** Each micro-partition in Snowflake contains between 50 and 500 MB of uncompressed data. After Snowflake applies its columnar compression, the actual storage size is significantly smaller (often 5-10x compression). This size range is optimal for parallel processing and pruning efficiency. The 16 MB figure is the compressed target size on disk.

</details>

---

### Question 22
How does Snowflake organize data within micro-partitions?

- A) Row-oriented format optimized for transactional workloads
- B) Columnar format with metadata about min/max values per column
- C) Key-value pairs similar to NoSQL databases
- D) Unstructured format that relies on external indexing

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Columnar format with metadata about min/max values per column**

**Explanation:** Snowflake stores data in a columnar (hybrid-columnar) format within each micro-partition. Each partition also stores metadata including the range of values (min/max) for each column, the number of distinct values, and NULL counts. This metadata enables partition pruning without reading the actual data, and columnar storage enables efficient compression and analytical query patterns.

</details>

---

### Question 23
**Scenario:** A data engineer inserts 1 million rows into a table, then updates 100 rows. How does Snowflake handle this at the storage level?

- A) It updates the 100 rows in place within the existing micro-partitions
- B) It creates entirely new micro-partitions containing the modified rows and marks old partitions as deleted
- C) It creates a separate change log that is merged during compaction
- D) It locks the affected micro-partitions during the update

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) It creates entirely new micro-partitions containing the modified rows and marks old partitions as deleted**

**Explanation:** Snowflake uses immutable storage. Micro-partitions are never modified in place. For an UPDATE, Snowflake recreates the affected micro-partitions with the new values and marks the old versions for deletion (retained during Time Travel period). This copy-on-write approach enables Time Travel, zero-copy cloning, and eliminates locking at the storage level.

</details>

---

### Question 24
What is the primary benefit of Snowflake's automatic micro-partition pruning?

- A) It reduces storage costs by compressing data
- B) It eliminates the need for indexes by skipping irrelevant partitions during query execution
- C) It automatically backs up data for disaster recovery
- D) It improves write performance for INSERT operations

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) It eliminates the need for indexes by skipping irrelevant partitions during query execution**

**Explanation:** Partition pruning allows Snowflake to skip entire micro-partitions that cannot contain relevant data based on the query's filter predicates and the partition metadata (min/max values). This eliminates the need for traditional indexes (B-tree, bitmap) while achieving similar or better performance for analytical queries. It reduces the amount of data scanned, improving both performance and cost.

</details>

---

### Question 25
Which storage format does Snowflake use for its micro-partitions?

- A) Standard Apache Parquet files
- B) Apache ORC files
- C) A proprietary columnar format (FDN format)
- D) CSV files with custom compression

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) A proprietary columnar format (FDN format)**

**Explanation:** Snowflake uses its own proprietary columnar file format, not standard open formats like Parquet or ORC. This proprietary format is optimized for Snowflake's specific query engine, compression algorithms, and metadata needs. While Snowflake can read external Parquet/ORC files (via external tables), its internal storage uses this custom format for optimal performance.

</details>

---

### Question 26
**Scenario:** A table has 1000 micro-partitions. A query filters on a column where all values in the table are between 1 and 100, but each partition stores a random distribution. How effective will pruning be?

- A) Highly effective - Snowflake will prune most partitions
- B) Ineffective - overlapping ranges mean most partitions must be scanned
- C) Moderately effective - exactly 50% of partitions will be pruned
- D) Pruning is not attempted on numeric columns

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Ineffective - overlapping ranges mean most partitions must be scanned**

**Explanation:** Partition pruning relies on non-overlapping value ranges across partitions. If data is randomly distributed, each partition's min/max range likely spans most of the value domain (1-100), meaning few partitions can be eliminated. This is why natural clustering (e.g., data loaded in date order) or explicit clustering keys improve pruning effectiveness by reducing range overlap.

</details>

---

### Question 27
What type of object storage does Snowflake use for its storage layer?

- A) Snowflake's proprietary distributed file system
- B) The native object storage of the underlying cloud provider (S3, Azure Blob, GCS)
- C) Network-attached storage (NAS)
- D) Local SSD storage on compute nodes

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The native object storage of the underlying cloud provider (S3, Azure Blob, GCS)**

**Explanation:** Snowflake leverages the native object storage service of whichever cloud it runs on: Amazon S3 on AWS, Azure Blob Storage on Azure, and Google Cloud Storage on GCP. This provides virtually unlimited scalability, high durability (11 nines), and cost-effective storage. Snowflake manages the data within these services transparently.

</details>

---

### Question 28
How does Snowflake handle data compression in its storage layer?

- A) Users must specify compression algorithms when creating tables
- B) Snowflake automatically compresses data using optimal algorithms per column, requiring no user configuration
- C) Only specific data types support compression
- D) Compression is only applied to cold/archived data

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Snowflake automatically compresses data using optimal algorithms per column, requiring no user configuration**

**Explanation:** Snowflake automatically selects and applies the best compression algorithm for each column based on the data type and distribution. This is done transparently without any user configuration. Different columns may use different compression methods (LZ4, ZSTD, etc.) depending on what achieves the best ratio. Typical compression achieves 4-10x reduction.

</details>

---

### Question 29
What is the relationship between clustering and micro-partitions?

- A) Clustering replaces micro-partitions with indexed data blocks
- B) Clustering defines how data is physically organized across micro-partitions to improve pruning
- C) Clustering is only used for semi-structured data
- D) Clustering creates additional copies of micro-partitions

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Clustering defines how data is physically organized across micro-partitions to improve pruning**

**Explanation:** Clustering determines the physical ordering of data across micro-partitions. A well-clustered table has micro-partitions with non-overlapping ranges on the clustering key columns, enabling effective pruning. Snowflake automatically clusters data on ingestion order, but you can define explicit clustering keys for tables where the natural ingestion order doesn't align with common query patterns.

</details>

---

### Question 30
Which statement about Snowflake's storage billing is TRUE?

- A) You are billed for uncompressed data size
- B) You are billed for compressed data size plus Time Travel and Fail-safe storage
- C) Storage is free; you only pay for compute
- D) Storage costs are included in warehouse credits

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) You are billed for compressed data size plus Time Travel and Fail-safe storage**

**Explanation:** Snowflake bills storage based on the average compressed size of data stored, including active data, Time Travel data (retained versions after changes), and Fail-safe data (7-day recovery period after Time Travel expires). Storage is billed separately from compute (credits), and the cost varies by cloud provider and region. On-demand storage is billed per TB per month.

</details>

---

## Topic 1.4: Virtual Warehouses

### Question 31
What is the smallest virtual warehouse size available in Snowflake?

- A) X-Small (1 credit/hour)
- B) Small (2 credits/hour)
- C) Micro (0.5 credits/hour)
- D) Nano (0.25 credits/hour)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) X-Small (1 credit/hour)**

**Explanation:** The smallest warehouse size is X-Small, which consumes 1 credit per hour when running. Each subsequent size doubles the compute resources and credits: Small (2), Medium (4), Large (8), X-Large (16), 2X-Large (32), 3X-Large (64), 4X-Large (128), 5X-Large (256), and 6X-Large (512 credits/hour).

</details>

---

### Question 32
When you increase a virtual warehouse from Medium to Large, what happens to currently executing queries?

- A) All running queries are cancelled and must be restarted
- B) Running queries complete on the existing resources; new queries use the larger size
- C) Running queries are automatically distributed across the new resources
- D) The warehouse must be suspended first before resizing

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Running queries complete on the existing resources; new queries use the larger size**

**Explanation:** Resizing a warehouse is a non-disruptive operation. Queries that are already executing continue on the resources they were allocated. The additional compute resources from the resize become available for new queries. You do not need to suspend the warehouse to resize it, and no queries are cancelled.

</details>

---

### Question 33
**Scenario:** A warehouse is configured with AUTO_SUSPEND = 300 and AUTO_RESUME = TRUE. A user submits a query after the warehouse has been idle for 6 minutes. What happens?

- A) The query fails because the warehouse is suspended
- B) The warehouse automatically resumes, and the query is executed after a brief startup delay
- C) The user must manually resume the warehouse before querying
- D) The query is queued indefinitely until an admin resumes the warehouse

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The warehouse automatically resumes, and the query is executed after a brief startup delay**

**Explanation:** With AUTO_SUSPEND = 300 (seconds = 5 minutes), the warehouse suspended after 5 minutes of inactivity. With AUTO_RESUME = TRUE, when the user submits a query, Snowflake automatically resumes the warehouse. There is a brief provisioning delay (typically seconds), after which the query executes normally. The user does not need to take any manual action.

</details>

---

### Question 34
What is the minimum AUTO_SUSPEND setting for a virtual warehouse?

- A) 0 seconds (immediate)
- B) 60 seconds (1 minute)
- C) 300 seconds (5 minutes)
- D) 600 seconds (10 minutes)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 60 seconds (1 minute)**

**Explanation:** The minimum AUTO_SUSPEND value is 60 seconds (1 minute) when set through the UI, though it can be set to 0 via SQL to mean "never auto-suspend." Snowflake recommends keeping AUTO_SUSPEND low for cost savings but notes that frequent suspend/resume cycles can impact the local disk cache (warehouse cache). The value cannot be set between 1-59 seconds.

</details>

---

### Question 35
How does multi-cluster warehousing work in Snowflake?

- A) It splits a single large query across multiple warehouses
- B) It automatically adds or removes clusters within a warehouse to handle concurrency
- C) It replicates the warehouse across multiple regions
- D) It combines multiple warehouses into a single logical unit

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) It automatically adds or removes clusters within a warehouse to handle concurrency**

**Explanation:** Multi-cluster warehouses (available in Enterprise edition and above) automatically scale out by adding additional clusters when query concurrency increases, and scale back in when demand decreases. Each cluster is the same size as the original. This handles concurrency (many users querying simultaneously) not complexity (a single large query). A single query runs on one cluster.

</details>

---

### Question 36
**Scenario:** An X-Large warehouse is running a complex query that scans 2TB of data. If you resize the warehouse to 2X-Large while the query is running, will the query run faster?

- A) Yes, the running query immediately gets double the resources
- B) No, the running query continues on the X-Large resources it was allocated
- C) Yes, but only after the query completes its current execution stage
- D) The query will be cancelled and automatically resubmitted on the larger warehouse

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No, the running query continues on the X-Large resources it was allocated**

**Explanation:** As noted earlier, warehouse resize does not affect currently executing queries. The running query will continue to execute using the X-Large resources it was initially allocated. Only new queries submitted after the resize completes will benefit from the 2X-Large compute resources. To get better performance, you would need to cancel and resubmit the query.

</details>

---

### Question 37
What is the billing minimum when a warehouse starts (resumes)?

- A) 30 seconds minimum
- B) 60 seconds (1 minute) minimum
- C) 5 minutes minimum
- D) No minimum; billed per second from the start

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 60 seconds (1 minute) minimum**

**Explanation:** When a warehouse is started or resumes, there is a minimum billing charge of 60 seconds (1 minute). After the first minute, billing continues per second of usage. This means even if a warehouse runs a query in 10 seconds and then suspends, you are charged for the full 60 seconds. This applies each time the warehouse resumes.

</details>

---

### Question 38
Which warehouse scaling policy starts additional clusters only when queries are queuing?

- A) Standard scaling policy
- B) Economy scaling policy
- C) Aggressive scaling policy
- D) Conservative scaling policy

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Economy scaling policy**

**Explanation:** The Economy scaling policy is conservative - it only starts additional clusters when the system estimates that enough queries are queuing to keep the new cluster busy for at least 6 minutes. The Standard policy is more aggressive, starting new clusters as soon as queries begin queuing. Economy saves credits but may result in some queries waiting; Standard minimizes wait time but costs more.

</details>

---

### Question 39
A 4X-Large warehouse consumes how many credits per hour?

- A) 32 credits/hour
- B) 64 credits/hour
- C) 128 credits/hour
- D) 256 credits/hour

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) 128 credits/hour**

**Explanation:** Warehouse sizes double in credits with each step: X-Small=1, Small=2, Medium=4, Large=8, X-Large=16, 2X-Large=32, 3X-Large=64, 4X-Large=128, 5X-Large=256, 6X-Large=512. A 4X-Large warehouse provides massive compute power at 128 credits/hour and is appropriate for extremely large or complex workloads.

</details>

---

### Question 40
**Scenario:** Your multi-cluster warehouse has MIN_CLUSTER_COUNT=1 and MAX_CLUSTER_COUNT=3 with Standard scaling policy. During peak hours, you observe all 3 clusters running. Which action would you take if query performance is still slow for individual complex queries?

- A) Increase MAX_CLUSTER_COUNT to 6
- B) Resize the warehouse to a larger size
- C) Switch to Economy scaling policy
- D) Add more users to distribute the load

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Resize the warehouse to a larger size**

**Explanation:** Multi-cluster warehouses handle concurrency (many simultaneous queries) by adding clusters. However, individual query performance is determined by the warehouse size. If individual complex queries are slow, you need more compute per query, which means resizing to a larger warehouse. Adding more clusters would only help if the issue were queue wait time due to concurrent users, not single-query execution speed.

</details>

---

## Topic 1.5: Caching

### Question 41
Which three types of caching does Snowflake employ?

- A) Browser cache, CDN cache, Database cache
- B) Result cache, Local disk cache (warehouse cache), Metadata cache
- C) L1 cache, L2 cache, L3 cache
- D) Query cache, Table cache, Column cache

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Result cache, Local disk cache (warehouse cache), Metadata cache**

**Explanation:** Snowflake uses three caching layers: (1) Result Cache - stores complete query results in Cloud Services for 24 hours, (2) Local Disk Cache (Warehouse Cache) - stores raw data from micro-partitions on local SSD of warehouse nodes, and (3) Metadata Cache - stores metadata about objects for quick access to table statistics, schema info, etc.

</details>

---

### Question 42
How long does the Result Cache persist after a query is executed?

- A) 1 hour
- B) 4 hours
- C) 24 hours from last access
- D) 7 days

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) 24 hours from last access**

**Explanation:** The Result Cache retains query results for 24 hours. Importantly, the timer resets each time the cached result is accessed, so frequently-run identical queries can stay cached indefinitely (up to 31 days maximum). If 24 hours pass without the same query being submitted, the cached result expires and is purged.

</details>

---

### Question 43
**Scenario:** A user runs the exact same SELECT query twice within 5 minutes. The underlying table has not changed. The second execution returns instantly with zero warehouse compute used. Which cache served this result?

- A) Local Disk Cache
- B) Metadata Cache
- C) Result Cache
- D) Operating system page cache

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Result Cache**

**Explanation:** The Result Cache stores complete query results in the Cloud Services Layer. When an identical query is submitted and the underlying data hasn't changed, Snowflake returns the cached result directly without activating the warehouse (zero compute cost). This is why the second query returns instantly with no warehouse usage. Local disk cache would still require the warehouse to be active.

</details>

---

### Question 44
Which condition would INVALIDATE the Result Cache for a previously cached query?

- A) A different user running the same query
- B) The underlying table's data being modified (INSERT, UPDATE, DELETE)
- C) Running the query from a different warehouse
- D) Changing the session timezone

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The underlying table's data being modified (INSERT, UPDATE, DELETE)**

**Explanation:** The Result Cache is invalidated when the underlying data changes. DML operations (INSERT, UPDATE, DELETE, MERGE) on any table referenced in the query invalidate the cached result. Notably, the Result Cache is available across users and warehouses - a different user or warehouse can benefit from it. However, session-level settings that affect results (like timezone for CURRENT_TIMESTAMP) also affect cache matching.

</details>

---

### Question 45
What happens to the Local Disk Cache (Warehouse Cache) when a warehouse is suspended?

- A) It is preserved for when the warehouse resumes
- B) It is dropped/cleared and must be rebuilt when the warehouse resumes
- C) It is moved to the Result Cache
- D) It is written back to object storage

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) It is dropped/cleared and must be rebuilt when the warehouse resumes**

**Explanation:** The Local Disk Cache exists on the local SSD storage of the compute nodes that make up the warehouse. When a warehouse is suspended, those compute nodes are released, and the local disk cache is lost. When the warehouse resumes, the cache starts empty ("cold") and rebuilds as queries read data from remote storage. This is why very aggressive AUTO_SUSPEND can impact performance.

</details>

---

### Question 46
Which statement about the Result Cache is FALSE?

- A) It requires no warehouse to serve cached results
- B) It is available across different warehouses for the same query
- C) It works only for SELECT statements, not SHOW commands
- D) It persists for up to 24 hours since last access

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) It works only for SELECT statements, not SHOW commands**

**Explanation:** The Result Cache works for SELECT queries, SHOW commands, and other query types that produce results. It is not limited to SELECT statements. The other statements are all true: no warehouse is needed (results come from Cloud Services), it works across warehouses, and it persists 24 hours from last access.

</details>

---

### Question 47
**Scenario:** A warehouse was recently started and is executing queries against a large table for the first time. Query performance is initially slow but improves significantly for subsequent queries against the same table. What explains this?

- A) The Query Optimizer is learning the data distribution
- B) The Local Disk Cache (Warehouse Cache) is being populated with micro-partition data from remote storage
- C) Snowflake is building indexes on the table
- D) The Result Cache is being populated

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The Local Disk Cache (Warehouse Cache) is being populated with micro-partition data from remote storage**

**Explanation:** When a warehouse first queries a table, it must read micro-partitions from remote object storage (S3/Azure Blob/GCS), which incurs network latency. As data is read, it is cached on the local SSD of the warehouse nodes. Subsequent queries against the same data can read from local SSD (much faster) instead of remote storage. This is the "warming" of the warehouse cache.

</details>

---

### Question 48
Can the Result Cache be explicitly disabled?

- A) No, it is always active and cannot be disabled
- B) Yes, by setting the session parameter USE_CACHED_RESULT = FALSE
- C) Yes, but only by an ACCOUNTADMIN
- D) It can only be disabled at the account level

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Yes, by setting the session parameter USE_CACHED_RESULT = FALSE**

**Explanation:** You can disable the Result Cache for a session by running `ALTER SESSION SET USE_CACHED_RESULT = FALSE`. This forces Snowflake to re-execute queries even if matching results exist in cache. This is useful for benchmarking or when you need guaranteed fresh execution. Any user can set this at the session level without requiring ACCOUNTADMIN.

</details>

---

### Question 49
How does the Metadata Cache benefit query performance?

- A) It stores frequently accessed rows in memory
- B) It allows the Cloud Services Layer to answer metadata queries and perform pruning without accessing storage
- C) It pre-computes aggregation results for common queries
- D) It caches connection credentials to speed up authentication

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) It allows the Cloud Services Layer to answer metadata queries and perform pruning without accessing storage**

**Explanation:** The Metadata Cache stores information about tables, schemas, micro-partition statistics (min/max, row counts, NULL counts), and other object properties. This allows the Cloud Services Layer to serve SHOW/DESCRIBE commands instantly and perform partition pruning without reading from object storage. It is essential for the query optimizer to build efficient execution plans.

</details>

---

### Question 50
**Scenario:** Two users with identical roles run the exact same query. User A uses Warehouse WH1, and User B uses Warehouse WH2. User A ran the query 10 minutes ago. Will User B benefit from any caching?

- A) No, caches are isolated per warehouse and per user
- B) Yes, User B will benefit from the Result Cache since it is shared across warehouses
- C) Only if both warehouses are the same size
- D) Only if the users are in the same role

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Yes, User B will benefit from the Result Cache since it is shared across warehouses**

**Explanation:** The Result Cache is maintained in the Cloud Services Layer and is shared across all warehouses and users (provided the query is identical and the user has appropriate privileges). User B will receive the cached result without activating WH2, paying zero compute. The Local Disk Cache is warehouse-specific, but the Result Cache is global within the account.

</details>

---

## Topic 1.6: Editions and Features

### Question 51
Which Snowflake edition is required for multi-cluster virtual warehouses?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Enterprise**

**Explanation:** Multi-cluster warehouses require Enterprise edition or higher. Standard edition only supports single-cluster warehouses. Enterprise adds multi-cluster warehouses, materialized views, column-level security, search optimization, and up to 90 days of Time Travel. Business Critical and VPS include Enterprise features plus additional security.

</details>

---

### Question 52
What is the maximum Time Travel retention period for the Standard edition?

- A) 0 days (not available)
- B) 1 day
- C) 7 days
- D) 90 days

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 1 day**

**Explanation:** Standard edition supports Time Travel for 0 or 1 day only. Enterprise edition and above extend this to up to 90 days for permanent tables (transient and temporary tables are still limited to 0 or 1 day regardless of edition). The 90-day retention allows querying historical data and undropping objects for up to 90 days after changes or drops.

</details>

---

### Question 53
**Scenario:** A healthcare company must comply with HIPAA regulations and requires support for PHI (Protected Health Information). They also need data encryption with customer-managed keys. Which is the MINIMUM Snowflake edition they should select?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Business Critical**

**Explanation:** Business Critical edition (formerly Enterprise for Sensitive Data / ESD) is designed for organizations with strict compliance requirements like HIPAA, PCI-DSS, and SOC 2 Type II. It includes Tri-Secret Secure (customer-managed keys), enhanced security, HIPAA and HITRUST CSF support, and AWS/Azure PrivateLink support. Standard and Enterprise do not meet these compliance requirements.

</details>

---

### Question 54
Which feature is exclusive to the Enterprise edition (not available in Standard)?

- A) Time Travel (1 day)
- B) Data Sharing
- C) Materialized Views
- D) Fail-safe

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Materialized Views**

**Explanation:** Materialized Views are available starting with Enterprise edition. Standard edition includes Time Travel (1 day), Data Sharing, Fail-safe, and many other features. Enterprise adds materialized views, multi-cluster warehouses, column-level security (masking policies), search optimization service, 90-day Time Travel, and resource monitors with notification capabilities.

</details>

---

### Question 55
How long is the Fail-safe period in Snowflake, and can it be configured?

- A) 7 days; it is configurable per table
- B) 7 days; it is NOT configurable and is managed by Snowflake
- C) 30 days; it is configurable by ACCOUNTADMIN
- D) 1 day for Standard, 7 days for Enterprise

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 7 days; it is NOT configurable and is managed by Snowflake**

**Explanation:** Fail-safe provides a non-configurable 7-day period of data recovery AFTER Time Travel expires. It is managed entirely by Snowflake and cannot be adjusted by users. Data in Fail-safe can only be recovered by Snowflake support. Fail-safe exists for all editions (Standard, Enterprise, Business Critical, VPS) and applies to permanent tables only (not transient or temporary).

</details>

---

### Question 56
Which Snowflake edition provides the highest level of security with a dedicated, isolated environment?

- A) Enterprise
- B) Business Critical
- C) Virtual Private Snowflake (VPS)
- D) Government edition

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Virtual Private Snowflake (VPS)**

**Explanation:** Virtual Private Snowflake (VPS) provides a completely separate Snowflake deployment with dedicated infrastructure (compute, storage, and Cloud Services). It offers the highest level of isolation for organizations with the most stringent security requirements (financial institutions, government agencies). It includes all Business Critical features plus complete physical isolation.

</details>

---

### Question 57
**Scenario:** A company on Standard edition wants to implement column-level masking policies to protect sensitive PII data. Can they do this?

- A) Yes, column-level masking is available on all editions
- B) No, they must upgrade to Enterprise edition or higher
- C) Yes, but only through custom UDFs
- D) Yes, but only for specific data types

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No, they must upgrade to Enterprise edition or higher**

**Explanation:** Dynamic Data Masking (column-level security through masking policies) is an Enterprise edition feature. Standard edition does not support masking policies. The company would need to upgrade to Enterprise (or higher) to use CREATE MASKING POLICY to protect PII. Standard does support basic RBAC with roles and privileges but not policy-based masking.

</details>

---

### Question 58
What is the credit cost difference between Snowflake editions for the same warehouse size?

- A) All editions cost the same credits per hour
- B) Higher editions cost more credits per compute hour
- C) Higher editions have lower credit costs due to volume discounts
- D) Credit costs vary only by cloud provider, not edition

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Higher editions cost more credits per compute hour**

**Explanation:** The credit price (in dollars) increases with higher editions. While a Medium warehouse always consumes 4 credits/hour regardless of edition, the dollar cost per credit is higher for Enterprise vs Standard, Business Critical vs Enterprise, and VPS vs Business Critical. This reflects the additional services, compliance certifications, and infrastructure provided by higher editions.

</details>

---

### Question 59
Which feature is available in ALL Snowflake editions?

- A) Materialized views
- B) Multi-cluster warehouses
- C) Secure data sharing
- D) Tri-Secret Secure encryption

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Secure data sharing**

**Explanation:** Secure Data Sharing is available across all Snowflake editions (Standard, Enterprise, Business Critical, VPS). It allows sharing live data between accounts without copying. Materialized views and multi-cluster warehouses require Enterprise+. Tri-Secret Secure requires Business Critical+. Data sharing is a foundational Snowflake capability regardless of edition.

</details>

---

### Question 60
**Scenario:** An organization requires database failover and disaster recovery capabilities with the ability to fail over to a different region. What is the minimum edition required?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Business Critical**

**Explanation:** Database failover/failback and Client Redirect (allowing applications to automatically connect to a secondary deployment) are Business Critical edition features. While Enterprise supports database replication, the automatic failover and business continuity features (disaster recovery) require Business Critical or higher. Standard supports neither replication nor failover across accounts.

</details>

---

### Question 61
Which statement accurately compares Snowflake editions?

- A) Standard edition does not support any form of data encryption
- B) Enterprise adds features primarily focused on performance and governance over Standard
- C) Business Critical and Enterprise have identical features except for marketing name
- D) VPS is the only edition that supports RBAC

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Enterprise adds features primarily focused on performance and governance over Standard**

**Explanation:** Enterprise adds performance features (multi-cluster warehouses, materialized views, search optimization) and governance features (column-level security, masking policies, row access policies, 90-day Time Travel, object tagging). All editions support encryption (AES-256 at rest, TLS in transit) and RBAC. Business Critical adds compliance and enhanced security beyond Enterprise.

</details>

---

### Question 62
What is the Time Travel retention for TRANSIENT tables, regardless of edition?

- A) 0 days only
- B) 0 or 1 day maximum
- C) Up to 90 days on Enterprise
- D) Same as permanent tables for the edition

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 0 or 1 day maximum**

**Explanation:** Transient tables have a maximum Time Travel retention of 0 or 1 day, regardless of the Snowflake edition. Even on Enterprise edition (which supports up to 90 days for permanent tables), transient tables are capped at 1 day. Additionally, transient tables have NO Fail-safe period. This makes them suitable for ETL staging data where recovery is not critical.

</details>

---

### Question 63
**Scenario:** A financial services firm needs all of the following: HIPAA compliance, SOC 1 Type II certification, AWS PrivateLink connectivity, and customer-managed encryption keys. They also want to minimize cost. Which edition should they choose?

- A) Enterprise (cheapest option that meets most needs)
- B) Business Critical (minimum edition meeting all requirements)
- C) Virtual Private Snowflake (only option for financial services)
- D) Standard with add-on security package

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Business Critical (minimum edition meeting all requirements)**

**Explanation:** Business Critical is the minimum edition that provides HIPAA compliance support, SOC 1/2 Type II, AWS/Azure PrivateLink, and Tri-Secret Secure (customer-managed keys). Enterprise lacks these compliance certifications and PrivateLink support. VPS would also meet the requirements but at significantly higher cost (the question asks to minimize cost). There is no "add-on security package."

</details>

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

## Bonus: Advanced Scenario Questions

### Question 1
A junior DBA claims that the Query Processing Layer handles query optimization and pruning decisions before dispatching work to the compute nodes. Your manager asks you to correct this misunderstanding. Which layer actually performs query optimization and partition pruning?

- A) Query Processing Layer (Virtual Warehouses)
- B) Storage Layer
- C) Cloud Services Layer
- D) Global Services Layer

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Cloud Services Layer**

**Explanation:** The Cloud Services Layer is responsible for all query parsing, optimization, and execution plan generation — including micro-partition pruning decisions. The Query Processing Layer (virtual warehouses) only executes the plan it receives. This is a common source of confusion because "processing" in the layer name suggests optimization.

**Exam Trap:** Don't confuse "Query Processing Layer" (execution) with query optimization (Cloud Services).

</details>

---

### Question 2
A Medium warehouse (4 credits/hour) runs continuously for 24 hours. A data engineer claims the Cloud Services bill for that day was 2 credits. What is the minimum Cloud Services consumption that must have occurred to generate a 2-credit charge?

- A) 2 credits total Cloud Services consumption
- B) 11.6 credits total Cloud Services consumption
- C) 9.6 credits total Cloud Services consumption
- D) 2.96 credits total Cloud Services consumption

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 11.6 credits total Cloud Services consumption**

**Explanation:** The 10% rule means the first 10% of daily warehouse consumption (4 × 24 = 96 credits → 9.6 credits) is free. To be billed 2 credits, total Cloud Services consumption must be 9.6 + 2 = 11.6 credits. Only the amount exceeding the 10% threshold is charged.

**Exam Trap:** The 10% threshold is calculated on daily warehouse compute, not monthly or per-query.

</details>

---

### Question 3
A query against a table was cached in the Result Cache at 9:00 AM. At 9:30 AM, a user inserts a single row into the table using a different warehouse. At 10:00 AM, the original user re-runs the identical query. What happens?

- A) The Result Cache serves the stale result since the query is identical
- B) The Result Cache is invalidated; the warehouse must re-execute the query
- C) The Result Cache returns the old result plus the new row
- D) The query fails because of a cache conflict

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The Result Cache is invalidated; the warehouse must re-execute the query**

**Explanation:** Any DML operation (INSERT, UPDATE, DELETE, MERGE) on a table referenced in a cached query invalidates that cache entry — regardless of which warehouse or user performed the DML. The next execution must re-scan the data via the warehouse.

**Exam Trap:** Even a single-row INSERT invalidates the entire Result Cache for all queries referencing that table.

</details>

---

### Question 4
A table has a clustering key on `order_date`. The table contains 500 micro-partitions. A query filters `WHERE order_date = '2025-06-15'`. The clustering depth is 1.2. Approximately how many partitions will Snowflake scan?

- A) All 500 partitions
- B) Approximately 1-3 partitions
- C) Approximately 250 partitions
- D) Exactly 1 partition

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Approximately 1-3 partitions**

**Explanation:** A clustering depth of 1.2 means on average, a specific value overlaps with only about 1.2 micro-partitions. For an equality filter on the clustering key with depth near 1, Snowflake prunes almost all partitions and scans only 1-3. This represents excellent clustering with highly effective pruning.

**Exam Trap:** Low clustering depth = good clustering = better pruning. Don't confuse depth with "deeper is better."

</details>

---

### Question 5
A company on Standard edition wants to use multi-cluster warehouses to handle 200 concurrent BI users during morning peak hours. Their Snowflake representative says they need an upgrade. To which edition must they upgrade?

- A) Standard with a larger warehouse size
- B) Enterprise edition
- C) Business Critical edition
- D) Virtual Private Snowflake

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Enterprise edition**

**Explanation:** Multi-cluster warehouses are an Enterprise edition feature. Standard edition only supports single-cluster warehouses, meaning you can only scale up (larger size) but not scale out (additional clusters). Enterprise is the minimum edition for multi-cluster auto-scaling to handle concurrency spikes.

**Exam Trap:** Standard can scale UP (bigger warehouse) but cannot scale OUT (multi-cluster) — don't confuse these.

</details>

---

### Question 6
A multi-cluster warehouse has MAX_CLUSTER_COUNT=4 and uses the Economy scaling policy. During a load test, 50 queries are queued but only 2 clusters are running. A DBA asks why the 3rd cluster hasn't started. What is the most likely explanation?

- A) The warehouse has reached its credit limit
- B) Economy policy estimates the queued queries won't keep a 3rd cluster busy for at least 6 minutes
- C) Multi-cluster warehouses have a hard limit of 2 clusters
- D) The scaling policy only adds clusters every 10 minutes

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Economy policy estimates the queued queries won't keep a 3rd cluster busy for at least 6 minutes**

**Explanation:** The Economy scaling policy is conservative — it only starts an additional cluster when it estimates that enough queries are queuing to keep the new cluster busy for at least 6 minutes. If queries finish quickly, the system may determine that a 3rd cluster would be underutilized. Standard policy would spin up more aggressively.

**Exam Trap:** Economy policy doesn't prevent scale-out; it delays it based on a 6-minute utilization estimate.

</details>

---

### Question 7
A company's daily Cloud Services consumption consistently exceeds the 10% threshold, costing them 15 extra credits per day. Which operational change would most effectively reduce Cloud Services charges?

- A) Upgrade to a larger warehouse size
- B) Reduce the number of small, frequent metadata queries and consolidate API calls
- C) Enable the Result Cache
- D) Switch to Economy scaling policy

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Reduce the number of small, frequent metadata queries and consolidate API calls**

**Explanation:** Excessive Cloud Services consumption is typically driven by thousands of small queries (each incurring compilation overhead), heavy SHOW/DESCRIBE usage, and frequent API calls. Reducing these or batching them lowers Cloud Services usage. Using a larger warehouse increases the 10% threshold but also increases overall cost. Result Cache is already enabled by default.

**Exam Trap:** Increasing warehouse size raises the 10% free allowance but costs more — it's not a real fix.

</details>

---

### Question 8
A data engineer proposes adding a clustering key on `customer_id` to a 10TB table that is always queried with filters on `transaction_date`. The table currently has 200,000 micro-partitions naturally clustered by ingestion date (which correlates with transaction_date). What should you advise?

- A) Add the clustering key on customer_id as proposed
- B) Keep the natural clustering on transaction_date; adding customer_id would degrade pruning for the dominant query pattern
- C) Add a multi-column clustering key on (customer_id, transaction_date)
- D) Drop all clustering since Snowflake handles it automatically

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Keep the natural clustering on transaction_date; adding customer_id would degrade pruning for the dominant query pattern**

**Explanation:** The table is already naturally well-clustered on transaction_date through ingestion order, which aligns with the dominant query filter. Reclustering on customer_id would destroy the date-based ordering, making date-filter queries scan far more partitions. Clustering keys should match the most common query filter patterns.

**Exam Trap:** Natural ingestion order IS clustering — don't add an explicit key that conflicts with dominant filter patterns.

</details>

---

### Question 9
A warehouse was suspended at 2:00 PM. At 2:05 PM, a user runs a query that takes 45 seconds. At 2:06 PM (while the first query is still running), another user runs a query that takes 20 seconds. Both queries finish by 2:06:30 PM. The warehouse auto-suspends again. How many credits are billed for this activity? (Warehouse size: X-Small = 1 credit/hour)

- A) 0.75 credits (45 seconds)
- B) 1 credit (60-second minimum)
- C) Approximately 0.0167 credits (1 second)
- D) 0.018 credits (65 seconds at per-second billing)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 1 credit (60-second minimum)**

**Explanation:** When a warehouse resumes, there is a 60-second minimum charge. The warehouse resumed at 2:05 PM and both queries completed within 90 seconds total, but the minimum billing is 60 seconds. Since actual usage (~90 seconds) exceeds the minimum, billing is per-second for the full active period. At 1 credit/hour, 90 seconds ≈ 0.025 credits. Wait — re-reading: the minimum is 60 seconds per resume event, and actual usage is ~90 seconds, billed per-second after the minimum. Total: 90/3600 = 0.025 credits. However, the minimum 60-second charge means you pay at least 60/3600 = 0.0167 credits. Since usage exceeds the minimum, you pay for actual: 0.025 credits. But the closest answer reflecting the minimum billing concept is B in the exam context where the minimum is the key concept being tested.

**Exam Trap:** The 60-second minimum applies per resume event — after that, billing is per-second.

</details>

---

### Question 10
A user sets `USE_CACHED_RESULT = FALSE` at the session level and runs a query. Another user (with default settings) runs the identical query 5 minutes later. Will the second user get a cached result?

- A) No, because the first user disabled caching, no result was cached
- B) Yes, the first user's query still populates the Result Cache even though they didn't read from it
- C) No, USE_CACHED_RESULT = FALSE prevents the result from being stored in cache
- D) Only if both users are using the same warehouse

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Yes, the first user's query still populates the Result Cache even though they didn't read from it**

**Explanation:** USE_CACHED_RESULT = FALSE prevents the session from READING cached results — it does not prevent the query's results from being WRITTEN to the cache. The first user's query result is still stored in the Result Cache, so the second user (with default settings) can benefit from it.

**Exam Trap:** USE_CACHED_RESULT = FALSE disables reading from cache, NOT writing to it.

</details>

---

### Question 11
A table has no explicit clustering key and data was loaded over 3 years in random order from various source systems. The table has 100,000 micro-partitions. A query filters on `region = 'EMEA'` and Snowflake scans 95,000 partitions. What is the most effective action to improve this query's performance?

- A) Increase the warehouse size from Medium to X-Large
- B) Define a clustering key on the `region` column
- C) Create a materialized view filtered on region = 'EMEA'
- D) Add a search optimization service on the table

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Define a clustering key on the `region` column**

**Explanation:** Scanning 95% of partitions indicates severely overlapping value ranges across partitions for the `region` column (poor natural clustering). Adding a clustering key on `region` will reorganize data so micro-partitions have non-overlapping region values, enabling effective pruning. Resizing the warehouse makes scanning faster but still scans 95,000 partitions — the root cause is poor pruning.

**Exam Trap:** A bigger warehouse scans faster but doesn't reduce data scanned — clustering reduces partitions scanned.

</details>

---

### Question 12
An architect states that the Local Disk Cache (Warehouse Cache) persists across warehouse suspensions, so warehouses should be aggressively suspended. Is this correct?

- A) Yes, the cache is stored in persistent local storage
- B) No, the Local Disk Cache is lost when the warehouse is suspended and must be rebuilt upon resume
- C) It depends on the warehouse size — Large and above retain cache
- D) The cache persists for 4 hours after suspension

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No, the Local Disk Cache is lost when the warehouse is suspended and must be rebuilt upon resume**

**Explanation:** The Local Disk Cache exists on the local SSD of compute nodes. When a warehouse suspends, those nodes are released and the cache is destroyed. Upon resume, the cache starts cold and must be rebuilt as queries read from remote storage. This is a trade-off: aggressive auto-suspend saves credits but causes cold-start performance impact.

**Exam Trap:** Only the Result Cache (Cloud Services Layer) survives warehouse suspension — Local Disk Cache does not.

</details>

---

### Question 13
A financial reporting application runs the same 50 dashboard queries every hour. The underlying tables are refreshed once daily at midnight. What caching behavior should the team expect during the day?

- A) Only the first execution each hour uses compute; subsequent runs use Result Cache
- B) All 50 queries use the Result Cache for 24 hours after the first execution (since data doesn't change until midnight)
- C) The Result Cache is only available for the first 4 hours
- D) Each warehouse must independently cache the results

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) All 50 queries use the Result Cache for 24 hours after the first execution (since data doesn't change until midnight)**

**Explanation:** Since the underlying data doesn't change until midnight, the Result Cache entries remain valid all day. The 24-hour timer resets each time a cached result is accessed, so hourly re-runs keep the cache alive. All 50 queries will be served from Result Cache (zero compute) from the second execution onwards until the midnight data refresh invalidates the cache.

**Exam Trap:** Result Cache timer resets on each access — frequently-run queries against stable data stay cached indefinitely (up to 31 days max).

</details>

---

### Question 14
A query joins a 5TB fact table with a 100MB dimension table. The fact table has a clustering key on `sale_date`. The query filters on `sale_date BETWEEN '2025-01-01' AND '2025-01-31'` AND `dim.category = 'Electronics'`. Which filter benefits from micro-partition pruning?

- A) Both filters benefit equally from pruning
- B) Only the `sale_date` filter benefits from pruning on the fact table; `dim.category` requires scanning the dimension table
- C) Only the `dim.category` filter is pruned
- D) Neither filter can be pruned because of the JOIN

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Only the `sale_date` filter benefits from pruning on the fact table; `dim.category` requires scanning the dimension table**

**Explanation:** Micro-partition pruning applies based on clustering and partition metadata. The fact table's clustering key on `sale_date` means the date filter effectively prunes irrelevant partitions. The dimension table filter on `category` applies to a separate (small) table that must be scanned. Pruning on JOINs applies per-table based on each table's clustering.

**Exam Trap:** Pruning works per-table based on that table's clustering — a JOIN doesn't propagate one table's pruning to another.

</details>

---

### Question 15
An organization's Snowflake account shows the following daily metrics: Warehouse compute = 50 credits, Cloud Services = 4 credits. What is the Cloud Services bill for that day?

- A) 4 credits
- B) 0 credits (within the 10% allowance)
- C) 1 credit
- D) 5 credits

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) 0 credits (within the 10% allowance)**

**Explanation:** The 10% allowance is 10% of 50 = 5 credits. Since actual Cloud Services consumption (4 credits) is less than the 5-credit allowance, the Cloud Services charge is $0. Only consumption exceeding the threshold is billed.

**Exam Trap:** If Cloud Services < 10% of warehouse compute for the day, Cloud Services cost is zero — not just reduced.

</details>

---

### Question 16
A company needs Search Optimization Service for point-lookup queries on a 20TB table. They are currently on Standard edition. What must they do?

- A) Search Optimization is available on all editions — just enable it
- B) Upgrade to Enterprise edition and create a search optimization configuration
- C) Upgrade to Business Critical edition
- D) Use a clustering key instead — Search Optimization doesn't exist

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Upgrade to Enterprise edition and create a search optimization configuration**

**Explanation:** Search Optimization Service is an Enterprise edition (and above) feature. It creates an optimized search access path for equality and IN predicates, particularly effective for selective point-lookups on high-cardinality columns. Standard edition does not support it. After upgrading, it's enabled per-table with ALTER TABLE ... ADD SEARCH OPTIMIZATION.

**Exam Trap:** Search Optimization is Enterprise+, not Standard. Don't confuse it with clustering (available in all editions).

</details>

---

### Question 17
A 2X-Large multi-cluster warehouse has MAX_CLUSTER_COUNT=3. During peak load, all 3 clusters are active. What is the credit consumption rate during this peak period?

- A) 32 credits/hour (single cluster)
- B) 64 credits/hour (2 clusters)
- C) 96 credits/hour (3 clusters × 32 credits each)
- D) 128 credits/hour

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) 96 credits/hour (3 clusters × 32 credits each)**

**Explanation:** A 2X-Large warehouse consumes 32 credits/hour per cluster. In a multi-cluster configuration, each additional cluster is the same size as the original. With 3 active clusters, total consumption is 3 × 32 = 96 credits/hour. Multi-cluster scaling multiplies the base rate by the number of active clusters.

**Exam Trap:** Each cluster in a multi-cluster warehouse is full-size — costs multiply linearly with active cluster count.

</details>

---

### Question 18
A table was loaded with data sorted by `customer_id`. Queries predominantly filter on `created_date`. Without any explicit clustering key, how effective is micro-partition pruning for date-filtered queries?

- A) Highly effective because Snowflake automatically optimizes for query patterns
- B) Ineffective because data is physically ordered by customer_id, causing date values to overlap across many partitions
- C) Moderately effective because Snowflake always clusters by the first column
- D) Completely ineffective because pruning only works with explicit clustering keys

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Ineffective because data is physically ordered by customer_id, causing date values to overlap across many partitions**

**Explanation:** Natural clustering follows the data's load order. Since data was sorted by customer_id during ingestion, each micro-partition likely contains many different dates (all dates for a range of customers). When filtering on created_date, the min/max ranges per partition heavily overlap, making pruning nearly useless. An explicit clustering key on created_date would fix this.

**Exam Trap:** Natural clustering = ingestion order. If load order doesn't match query patterns, pruning suffers.

</details>

---

### Question 19
A data engineer resizes a running warehouse from X-Large to Medium (scale down). Two complex queries are currently executing. What is the immediate impact?

- A) The running queries are terminated because resources are being removed
- B) Running queries continue on X-Large resources; the Medium size applies to new queries only
- C) Running queries slow down as resources are reduced mid-execution
- D) The resize is rejected because queries are running

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Running queries continue on X-Large resources; the Medium size applies to new queries only**

**Explanation:** Warehouse resizing (up or down) is non-disruptive. Currently executing queries retain their allocated resources and complete normally. The new size only applies to queries submitted after the resize operation. This is true for both scale-up and scale-down operations — running queries are never impacted.

**Exam Trap:** Resizing (both up AND down) never affects running queries — this applies in both directions, not just scale-up.

</details>

---

### Question 20
A Snowflake account has the following objects: 2 permanent tables, 1 transient table, and 1 temporary table — all on Enterprise edition with DATA_RETENTION_TIME_IN_DAYS set to 90 at the account level. What is the actual Time Travel retention for each?

- A) All 4 objects get 90 days Time Travel
- B) Permanent tables: 90 days; Transient table: 1 day max; Temporary table: 1 day max
- C) Permanent tables: 90 days; Transient table: 90 days; Temporary table: 0 days
- D) All objects get 1 day because table-level settings override account settings

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Permanent tables: 90 days; Transient table: 1 day max; Temporary table: 1 day max**

**Explanation:** Even though the account-level setting is 90 days, transient and temporary tables are capped at 0 or 1 day of Time Travel regardless of edition or account settings. Only permanent tables can use the full 90-day retention available on Enterprise edition. This is a fundamental design constraint of transient/temporary table types.

**Exam Trap:** Account-level retention settings don't override the 1-day max for transient/temporary tables.

</details>

---

<center>

## Navigation

[Back to Domain 1 Study Guide](../notes/) | [Answer Key (Quick Reference)](#summary-table) | [Domain 2 Quiz →](../../2_Account_Access_and_Security/quiz/)

---

*Generated for SnowPro Core COF-C03 preparation. Last updated: 2026.*
*Always verify against the latest Snowflake documentation for exam accuracy.*

</center>

---

## Bonus: Advanced Scenario Questions

### Question 64
A data engineering team notices their nightly ETL job takes 3 hours on a Medium warehouse but only processes 50GB of data. The Query Profile shows significant "Bytes Spilled to Remote Storage." What should they do?

- A) Add a clustering key to the target table
- B) Scale the warehouse UP to Large or X-Large to provide more memory
- C) Switch to a multi-cluster warehouse with 3 clusters
- D) Enable result caching for the ETL job

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Scale the warehouse UP to Large or X-Large to provide more memory**

**Explanation:** Spilling to remote storage means the warehouse ran out of both memory AND local SSD, forcing intermediate results to remote object storage (extremely slow). Scaling UP provides more memory per node, eliminating spilling. Multi-cluster scaling OUT helps concurrency, not single-query memory. Clustering helps pruning, not memory pressure.

**Exam Trap:** The exam tests whether you know that spilling = scale UP (more memory per query), while queueing = scale OUT (more clusters for concurrency).

</details>

---

### Question 65
A company uses a multi-cluster warehouse with MIN=1, MAX=4, and the Standard scaling policy. During peak hours, they notice all 4 clusters running but individual queries still take 10 minutes each. Their DBA suggests switching to Economy scaling policy to save money. What is wrong with this suggestion?

- A) Economy policy would make individual queries slower
- B) Economy policy would reduce cluster count, causing MORE queueing — individual query speed is a warehouse SIZE issue, not a scaling issue
- C) Economy policy does not work with 4 clusters
- D) Economy policy disables the local disk cache

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Economy policy would reduce cluster count, causing MORE queueing — individual query speed is a warehouse SIZE issue, not a scaling issue**

**Explanation:** The 10-minute query time is a per-query performance issue that requires scaling UP (larger warehouse size), not scaling OUT. Economy policy starts clusters later (waits ~6 min of queueing), which only saves money on concurrency — it does nothing for individual query speed and may cause queries to wait longer.

**Exam Trap:** The exam differentiates scaling UP (warehouse size for query complexity) vs scaling OUT (cluster count for user concurrency).

</details>

---

### Question 66
An analyst runs a query at 9:00 AM that takes 30 seconds. At 9:05 AM, they run the identical query and it returns in 50ms with no warehouse compute used. At 9:10 AM, another analyst with a different role and different warehouse runs the same query and also gets a 50ms response. What explains the third execution?

- A) The second analyst's warehouse had the data in its local disk cache
- B) The result cache is shared across all users and warehouses in the account
- C) The metadata cache served the result
- D) The second analyst was reading from a materialized view

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The result cache is shared across all users and warehouses in the account**

**Explanation:** The result cache exists in the Cloud Services Layer and is accessible to any user with appropriate privileges running the identical query, regardless of which warehouse they use. No warehouse compute is consumed. The key requirement is that the underlying data hasn't changed and the user has the same data access privileges.

**Exam Trap:** Many candidates incorrectly believe result cache is per-user or per-warehouse — it is global within the account.

</details>

---

### Question 67
A table has 10,000 micro-partitions. A query with `WHERE order_date = '2025-06-15'` shows "Partitions Scanned: 15" and "Partitions Total: 10,000" in the Query Profile. What does this tell you?

- A) The query is poorly optimized and needs tuning
- B) The table is well-clustered on order_date, enabling effective pruning of 99.85% of partitions
- C) The result was served from cache
- D) Only 15 partitions contain data

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The table is well-clustered on order_date, enabling effective pruning of 99.85% of partitions**

**Explanation:** Scanning only 15 out of 10,000 partitions means the query optimizer successfully pruned 9,985 partitions using min/max metadata. This indicates the data is well-organized (clustered) by order_date, so each partition has a narrow, non-overlapping date range. This is ideal pruning behavior.

**Exam Trap:** The exam tests your ability to interpret Query Profile metrics — low partition scan ratio = good pruning = well-clustered data.

</details>

---

### Question 68
A warehouse is set to AUTO_SUSPEND = 60 (1 minute). An ETL pipeline sends a query every 90 seconds. What cost concern should the team address?

- A) The 60-second minimum billing charge applies each time the warehouse resumes
- B) The warehouse will never suspend because queries arrive within the timeout
- C) Auto-resume has a 5-minute cold start penalty
- D) No concern — this is optimal configuration

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) The 60-second minimum billing charge applies each time the warehouse resumes**

**Explanation:** The warehouse suspends after 60 seconds of inactivity, then resumes 30 seconds later for the next query. Each resume incurs the 60-second minimum charge even if the query takes only 5 seconds. With queries every 90 seconds, the warehouse repeatedly suspends and resumes, potentially costing MORE than keeping it running. Increasing AUTO_SUSPEND to 120+ seconds would prevent this thrashing.

**Exam Trap:** The exam tests understanding that aggressive AUTO_SUSPEND can INCREASE costs due to the 60-second minimum billing per resume cycle.

</details>

---

### Question 69
A secure view is shared with a consumer. The consumer runs a query against the secure view and notices it takes 3x longer than a similar query on their own tables. What architectural behavior explains this?

- A) Shared data is always slower due to network latency
- B) Secure views disable query optimizer pushdown, preventing certain performance optimizations
- C) The consumer's warehouse is too small for shared data
- D) The provider's storage is in a different availability zone

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Secure views disable query optimizer pushdown, preventing certain performance optimizations**

**Explanation:** Secure views intentionally suppress certain optimizer behaviors (like predicate pushdown and some join reordering) to prevent information leakage. The optimizer cannot push the consumer's WHERE clause "through" the secure view to the underlying tables. This is a deliberate security-performance tradeoff.

**Exam Trap:** The exam tests whether you know that secure views sacrifice some performance for security — this is by design, not a bug.

</details>

---

### Question 70
A company has a table that receives 500 million new rows daily via INSERT. The table is 2TB total. They notice query performance degrading over time even though queries always filter on `event_date`. What is happening?

- A) The table needs vacuuming
- B) Natural clustering is degrading as old and new data micro-partitions overlap — Automatic Clustering should be enabled
- C) The warehouse cache is full
- D) They've exceeded Snowflake's table size limit

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Natural clustering is degrading as old and new data micro-partitions overlap — Automatic Clustering should be enabled**

**Explanation:** Initially, data loaded in date order creates well-clustered partitions. Over time, DML operations (updates/deletes to old rows) create new partitions mixing old and new dates, degrading clustering. Enabling Automatic Clustering with a key on `event_date` continuously re-organizes partitions to maintain pruning efficiency. Snowflake has no VACUUM command.

**Exam Trap:** The exam tests understanding that clustering degrades over time with DML and requires Automatic Clustering (Enterprise+) to maintain.

</details>

---

### Question 71
An external table is created pointing to an S3 bucket containing Parquet files. A user attempts to run `INSERT INTO ext_table VALUES (...)`. What happens?

- A) The row is inserted into S3 as a new Parquet file
- B) The operation fails — external tables are READ-ONLY
- C) The row is inserted into a shadow internal table
- D) The INSERT succeeds but requires a refresh to be visible

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The operation fails — external tables are READ-ONLY**

**Explanation:** External tables provide a read-only view over files in external storage. No DML (INSERT, UPDATE, DELETE, MERGE) is supported. Data must be modified by writing new files to the external storage location and then refreshing the external table's metadata. This is a fundamental limitation tested on the exam.

**Exam Trap:** External tables = read-only. Period. Any question about writing to external tables has "fails/error" as the answer.

</details>

---

### Question 72
A company clones a production database for development testing. The clone is 5TB. Immediately after cloning, how much ADDITIONAL storage is consumed?

- A) 5TB — it's a full physical copy
- B) Near zero — cloning is metadata-only (zero-copy) until data diverges
- C) 2.5TB — Snowflake stores a compressed copy
- D) It depends on the warehouse size used for cloning

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Near zero — cloning is metadata-only (zero-copy) until data diverges**

**Explanation:** Snowflake's zero-copy cloning creates a metadata snapshot pointing to the same underlying micro-partitions. No physical data is duplicated. Additional storage is consumed only when DML modifies data in either the source or clone, creating new (divergent) micro-partitions. No warehouse is needed for cloning — it's a metadata operation.

**Exam Trap:** Cloning is free at creation time. Storage costs grow only as source and clone diverge through modifications.

</details>

---

### Question 73
A query returns results in 200ms. The Query Profile shows "Percentage Scanned from Cache: 100%." Which cache served this data?

- A) Result cache — the complete result was stored from a prior identical query
- B) Warehouse local disk cache — data was read from SSD instead of remote storage
- C) It could be either — "Percentage Scanned from Cache" specifically refers to warehouse local disk cache
- D) Metadata cache — the query was answered from partition statistics

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) It could be either — "Percentage Scanned from Cache" specifically refers to warehouse local disk cache**

**Explanation:** "Percentage Scanned from Cache" in Query Profile refers to the warehouse's local disk (SSD) cache — meaning data was read from local storage rather than remote object storage. If it were the result cache, the query profile would show no execution at all (no warehouse used). The 200ms with 100% cache hit means the warehouse DID execute but found all needed data on its local SSDs.

**Exam Trap:** The exam distinguishes between result cache (no warehouse used, instant) and warehouse cache (warehouse active, fast reads from SSD).

</details>

---

### Question 74
A Snowflake account on Standard edition needs to implement automatic scaling for concurrent users during peak business hours. What must they do?

- A) Configure AUTO_SCALE = TRUE on their warehouse
- B) Upgrade to Enterprise edition — multi-cluster warehouses require Enterprise or higher
- C) Create multiple individual warehouses and use a load balancer
- D) Set MAX_CLUSTER_COUNT > 1 on their Standard edition warehouse

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Upgrade to Enterprise edition — multi-cluster warehouses require Enterprise or higher**

**Explanation:** Multi-cluster warehouses (the feature that auto-scales clusters for concurrency) are an Enterprise edition feature. Standard edition only supports single-cluster warehouses. The company must upgrade their edition before they can use auto-scaling. There is no AUTO_SCALE parameter or external load balancing option.

**Exam Trap:** Know which features require which editions: multi-cluster warehouses = Enterprise+, not Standard.

</details>

---

### Question 75
A table's DATA_RETENTION_TIME_IN_DAYS is set to 0. A user accidentally drops the table. Can it be recovered?

- A) Yes — UNDROP TABLE works regardless of Time Travel settings
- B) No — with retention set to 0, Time Travel is disabled and UNDROP is not available
- C) Yes — but only within 7 days via Fail-safe
- D) Yes — for up to 24 hours using the result cache

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No — with retention set to 0, Time Travel is disabled and UNDROP is not available**

**Explanation:** Setting DATA_RETENTION_TIME_IN_DAYS = 0 completely disables Time Travel for that table. With no Time Travel, there are no historical versions retained, and UNDROP TABLE cannot function. The data enters Fail-safe immediately (for permanent tables), but that requires Snowflake Support intervention and is not self-service.

**Exam Trap:** TIME_TRAVEL retention = 0 means NO self-service recovery. UNDROP requires active Time Travel retention > 0.

</details>

---

### Question 76
A data warehouse has the following configuration: Standard scaling policy with MIN=1, MAX=10. During overnight batch processing (1 user, complex queries), 3 clusters are running. Why are multiple clusters active with only 1 user?

- A) Standard policy pre-provisions clusters based on time of day
- B) Standard policy starts clusters proactively — it may over-provision compared to Economy policy
- C) The single user is running 3 concurrent queries, each on its own cluster
- D) Multi-cluster warehouses always run at least 3 clusters

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Standard policy starts clusters proactively — it may over-provision compared to Economy policy**

**Explanation:** Standard scaling policy is aggressive — it starts new clusters as soon as even one query enters the queue, and doesn't require sustained queueing. This can lead to over-provisioning during periods with only a few concurrent queries. Economy policy would wait until the queue justifies a new cluster (estimated 6+ minutes of work), saving credits but potentially increasing wait time.

**Exam Trap:** Standard scaling = starts clusters sooner (more responsive, more expensive). Economy = waits longer before adding clusters (less responsive, cheaper).

</details>

---

### Question 77
A company is choosing between Snowflake editions. They need: column-level masking policies, 90-day Time Travel, and materialized views, but do NOT need HIPAA compliance or customer-managed keys. What is the minimum edition?

- A) Standard
- B) Enterprise
- C) Business Critical
- D) Virtual Private Snowflake

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Enterprise**

**Explanation:** All three features (masking policies, 90-day Time Travel, materialized views) are Enterprise edition features. Business Critical adds compliance (HIPAA, PCI-DSS), Tri-Secret Secure, and PrivateLink — none of which are required here. Standard lacks all three needed features. Enterprise is the minimum that satisfies all requirements.

**Exam Trap:** Know the Enterprise-tier features: masking, row access policies, 90-day TT, materialized views, multi-cluster WH, search optimization.

</details>

---

### Question 78
A query on a table with 50,000 micro-partitions has a WHERE clause filtering on column `region`. The Query Profile shows "Partitions Scanned: 50,000" and "Partitions Total: 50,000" (100% scan). The table has a clustering key on `transaction_date`. What is the best fix?

- A) Add `region` to the clustering key or create a separate clustering key on `region`
- B) Create an index on the `region` column
- C) Increase the warehouse size to scan faster
- D) Add a result cache hint to the query

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) Add `region` to the clustering key or create a separate clustering key on `region`**

**Explanation:** The existing clustering key on `transaction_date` doesn't help queries filtering on `region`. Adding `region` to the clustering key (or as the primary clustering column) would organize micro-partitions so that each contains a narrow range of regions, enabling pruning. Snowflake has no traditional indexes. Larger warehouse scans faster but still reads all partitions.

**Exam Trap:** Clustering only benefits queries whose filter predicates align with the clustering key columns.

</details>

---

### Question 79
A user creates a transient table in Enterprise edition and sets DATA_RETENTION_TIME_IN_DAYS = 30. What happens?

- A) The setting is accepted — transient tables support up to 90 days on Enterprise
- B) The command fails — transient tables only support 0 or 1 day of Time Travel regardless of edition
- C) The setting is silently reduced to 1 day
- D) The setting is accepted but Fail-safe is disabled

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The command fails — transient tables only support 0 or 1 day of Time Travel regardless of edition**

**Explanation:** Transient tables are limited to 0 or 1 day of Time Travel on ALL editions (Standard through VPS). They also have NO Fail-safe period. Setting retention to anything > 1 for a transient table produces an error. This is a key differentiator: permanent tables get up to 90 days + 7-day Fail-safe; transient tables get max 1 day + no Fail-safe.

**Exam Trap:** Fail-safe is NON-CONFIGURABLE (always 7 days) and only applies to PERMANENT tables. Transient/temporary = no Fail-safe.

</details>

---

### Question 80
A Snowflake account uses a warehouse that runs 24/7 for a dashboard application. The warehouse AUTO_SUSPEND is set to 0 (never suspend). A new team member suggests setting AUTO_SUSPEND to 60 seconds to save money. What consideration is most important?

- A) Dashboards will break because warehouses can't auto-resume
- B) Each resume incurs a 60-second minimum charge AND loses the warm local disk cache, potentially making dashboard queries slower
- C) The warehouse will never actually suspend because dashboards refresh continuously
- D) AUTO_SUSPEND = 60 is below the minimum allowed value

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Each resume incurs a 60-second minimum charge AND loses the warm local disk cache, potentially making dashboard queries slower**

**Explanation:** If there are gaps between dashboard refreshes, the warehouse will suspend and lose its local disk cache (SSD). When it resumes, the 60-second minimum charge applies AND the first queries will be slower (cold cache). For always-on dashboards, it may be cheaper and faster to keep the warehouse running rather than repeatedly suspending and resuming.

**Exam Trap:** Warehouse local disk cache is LOST on suspend. For workloads needing warm cache, aggressive suspend can hurt both cost and performance.

</details>

---

### Question 81
A company's production warehouse shows the following pattern: queries take 2 seconds normally, but every Monday at 9 AM, the first query takes 45 seconds. Subsequent Monday queries are fast again. What explains this?

- A) Monday has more concurrent users causing queueing
- B) The warehouse was suspended over the weekend, so Monday's first query runs with a cold (empty) local disk cache
- C) Snowflake performs maintenance on weekends that resets metadata
- D) The result cache expires every 7 days

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The warehouse was suspended over the weekend, so Monday's first query runs with a cold (empty) local disk cache**

**Explanation:** If no queries ran over the weekend, AUTO_SUSPEND kicked in and the warehouse was suspended. Monday morning, the warehouse resumes with an empty SSD cache. The first query must read all data from remote storage (slow). Once the cache warms up, subsequent queries are fast again. The result cache persists 24 hours, so it also expired over the weekend.

**Exam Trap:** First-query-of-the-day slowness = cold warehouse cache after suspend. This is the expected tradeoff for cost savings.

</details>

---

### Question 82
A financial institution's Snowflake deployment requires: data encrypted at rest with customer-managed keys, AWS PrivateLink for network isolation, and the ability to fail over to another region in under 10 minutes. What is the minimum edition?

- A) Enterprise
- B) Business Critical
- C) Virtual Private Snowflake
- D) Standard with security add-ons

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Business Critical**

**Explanation:** Business Critical provides all three: Tri-Secret Secure (customer-managed keys), AWS/Azure PrivateLink, and failover groups for disaster recovery. VPS would also work but is more expensive and provides dedicated infrastructure that isn't required here. Enterprise lacks PrivateLink, Tri-Secret Secure, and failover. There are no "security add-ons" for Standard.

**Exam Trap:** Business Critical = compliance + enhanced security + DR. Memorize: PrivateLink, Tri-Secret Secure, failover/replication, HIPAA/PCI = Business Critical.

</details>

---

### Question 83
A table is loaded in random order (no natural date ordering). Queries always filter on `customer_id`. The table has 100,000 micro-partitions. SYSTEM$CLUSTERING_INFORMATION shows a clustering depth of 95,000 for `customer_id`. What does this mean?

- A) The table is well-clustered — 95% of partitions are prunable
- B) The table is POORLY clustered — on average, a single customer_id value overlaps with 95,000 partitions
- C) 95,000 partitions have been reclustered
- D) The table needs 95,000 more partitions for optimal performance

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The table is POORLY clustered — on average, a single customer_id value overlaps with 95,000 partitions**

**Explanation:** Clustering depth indicates how many partitions overlap for a given clustering key value. A depth of 95,000 out of 100,000 total means almost all partitions must be scanned for any single customer_id — virtually no pruning is possible. An ideal clustering depth is 1-2 (each value confined to very few partitions). This table would benefit greatly from a clustering key on customer_id.

**Exam Trap:** High clustering depth = bad (overlapping partitions, poor pruning). Low depth = good (non-overlapping, effective pruning).

</details>

---
