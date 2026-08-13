# Domain 5: Data Collaboration — Practice Questions

## Section A: Secure Data Sharing (15 Questions)

### Question 1
What type of view is REQUIRED when sharing data through Snowflake Secure Data Sharing?

A) Materialized view  
B) Standard view  
C) Secure view  
D) Any view type is acceptable  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Snowflake mandates that all views included in a share must be secure views. This requirement prevents the consumer from accessing the underlying view definition and prevents query optimizer behaviors from leaking information about filtered data. Standard views cannot be added to shares — Snowflake will return an error.

</details>

---

### Question 2
In a direct share, who pays for the compute resources when a consumer queries shared data?

A) The provider pays for all compute  
B) The consumer pays using their own warehouse  
C) Costs are split 50/50 between provider and consumer  
D) Snowflake absorbs the compute cost  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** In Snowflake's zero-copy sharing model, the consumer uses their own virtual warehouse to query shared data. The provider pays only for storage of the original data. The consumer bears all compute costs for their queries, giving them full control over performance and spend.

</details>

---

### Question 3
A provider shares a database with a consumer. The consumer wants to create a new table inside the shared database. What happens?

A) The table is created successfully but only visible to the consumer  
B) The table is created and visible to both provider and consumer  
C) The operation fails — shared databases are read-only  
D) The operation succeeds only if the provider grants CREATE TABLE privilege  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Databases created from shares are entirely read-only. Consumers cannot create, modify, or delete any objects within the shared database. If the consumer needs to create derived tables or views, they must do so in a separate database within their own account.

</details>

---

### Question 4
Which Snowflake edition is required for a provider to use Secure Data Sharing?

A) Standard Edition or higher  
B) Enterprise Edition or higher  
C) Business Critical Edition or higher  
D) Virtual Private Snowflake only  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**  
**Explanation:** Secure Data Sharing is available on ALL Snowflake editions, including Standard. This is a core platform feature, not a premium add-on. However, certain advanced features like cross-region/cross-cloud sharing and failover groups require Business Critical or higher.

</details>

---

### Question 5
What does CURRENT_ACCOUNT() return when used inside a secure view that has been shared with a consumer?

A) The provider's account identifier  
B) The consumer's account identifier  
C) NULL when accessed by a consumer  
D) An error because context functions are disabled in shares  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** When a consumer queries a secure view, CURRENT_ACCOUNT() returns the consumer's account identifier. This enables row-level security where the secure view filters data based on which consumer is querying. This is a key pattern for multi-tenant sharing from a single share object.

</details>

---

### Question 6
A provider revokes a consumer's access to a share. What happens to the consumer's database created from that share?

A) The database remains with a static snapshot of the last available data  
B) The database is automatically dropped from the consumer's account  
C) The database remains but all queries return errors — no data is accessible  
D) The database enters a 90-day retention period before being dropped  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** When access is revoked, the database object remains in the consumer's account but becomes completely inaccessible. Any queries against it fail immediately. The consumer would need to drop the dangling database object manually. No data snapshot is retained because there was never a physical copy.

</details>

---

### Question 7
Which objects can be included in a Snowflake share? (Choose the BEST answer)

A) Tables, secure views, secure UDFs, and stored procedures  
B) Tables, secure views, secure materialized views, and secure UDFs  
C) Tables, views, UDFs, stages, and file formats  
D) Only tables and secure views  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Shares can include: tables, external tables, secure views, secure materialized views, and secure UDFs/UDTFs. Stored procedures CANNOT be shared. Stages, file formats, sequences, and pipes also cannot be directly shared. All views and UDFs in a share must be secure.

</details>

---

### Question 8
How does a consumer create a local database from a share?

A) `CREATE DATABASE mydb CLONE provider_acct.share_name`  
B) `CREATE DATABASE mydb FROM SHARE provider_acct.share_name`  
C) `IMPORT DATABASE FROM SHARE provider_acct.share_name AS mydb`  
D) `ALTER DATABASE mydb ADD SHARE provider_acct.share_name`  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The correct syntax is `CREATE DATABASE <name> FROM SHARE <provider_account>.<share_name>`. This creates a read-only database in the consumer's account that references the provider's data. CLONE creates a physical copy (different concept). IMPORT and ADD SHARE are not valid syntax.

</details>

---

### Question 9
What is a Reader Account in the context of Snowflake data sharing?

A) An account type for consumers who only need SELECT access  
B) A managed account created by the provider for consumers without Snowflake accounts  
C) A limited account that can only read from the Marketplace  
D) An account with read-only access to the provider's entire account  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Reader accounts (also called managed accounts) are created by providers to enable sharing with consumers who do not have their own Snowflake account. The provider manages and pays for the reader account's compute resources. Reader accounts can ONLY access data shared by the provider that created them.

</details>

---

### Question 10
A secure view in a share uses a WHERE clause filtering on a column. The consumer runs EXPLAIN on their query. What can they see about the filter?

A) The full WHERE clause and filter values  
B) Only that a filter exists, but not the column or values  
C) The query plan does not reveal secure view internals  
D) The same information as a regular view's EXPLAIN output  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Secure views intentionally disable certain optimizer behaviors and hide internal details from EXPLAIN plans and query profiles. This prevents consumers from inferring filtered data through plan analysis, error messages, or timing attacks. This is a core security property of secure views, though it can impact performance.

</details>

---

### Question 11
What happens to Time Travel on data that has been shared with a consumer?

A) Consumer can use Time Travel on shared data using their own retention settings  
B) Consumer uses the provider's Time Travel retention settings  
C) Time Travel is not available on shared data for the consumer  
D) Time Travel works but only up to 1 day regardless of edition  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Consumers cannot use Time Travel on shared data. Time Travel requires access to historical micro-partitions, which is managed by the provider's account. The consumer always sees the current state of the data as it exists in the provider's account. If the consumer needs historical access, the provider must share historical views or tables.

</details>

---

### Question 12
A provider wants to share data with 50 different consumer accounts but restrict each consumer to see only their own rows. What is the MOST efficient approach?

A) Create 50 separate shares, each with a dedicated view for that consumer  
B) Create one share with a secure view using CURRENT_ACCOUNT() to filter rows  
C) Create 50 reader accounts, each with custom views  
D) Use a data exchange with 50 members  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The most efficient approach is a single share with a secure view that dynamically filters rows based on CURRENT_ACCOUNT(). This avoids managing 50 separate shares or views. A mapping table maps account identifiers to allowed data, and the secure view joins against it. All 50 consumers are added to the same share.

</details>

---

### Question 13
Which statement about shares and the objects within them is TRUE?

A) A table can belong to multiple shares simultaneously  
B) A table can only belong to one share at a time  
C) Shares can include objects from multiple databases  
D) A share can contain both secure and non-secure views  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**  
**Explanation:** A single table (or any sharable object) can be granted to multiple different shares simultaneously. However, each share can only contain objects from a SINGLE database (not multiple). All views in a share must be secure — you cannot include non-secure views.

</details>

---

### Question 14
What privilege is required to create a share in Snowflake?

A) ACCOUNTADMIN role only  
B) CREATE SHARE privilege (grantable to any role)  
C) SYSADMIN role or higher  
D) MANAGE SHARES global privilege  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The CREATE SHARE privilege can be granted to any role — it is not restricted to ACCOUNTADMIN. However, only ACCOUNTADMIN (or a role with IMPORT SHARE privilege) can create databases from incoming shares on the consumer side. By default, ACCOUNTADMIN has CREATE SHARE privilege.

</details>

---

### Question 15
A provider shares a table. Later, they add a new column to that table. What does the consumer see?

A) The consumer must re-create the database from share to see the new column  
B) The new column is automatically visible to the consumer  
C) The provider must update the share grant for the consumer to see it  
D) The consumer sees the new column only after their next query cache expires  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Because sharing is zero-copy (no physical data movement), schema changes by the provider are immediately reflected for the consumer. Adding columns, modifying data, or other DDL changes are instantly visible. The consumer does not need to take any action to see updated schemas or data.

</details>

---

## Section B: Marketplace & Data Exchange (12 Questions)

### Question 16
What is the primary difference between the Snowflake Marketplace and a Private Data Exchange?

A) Marketplace is free; Data Exchange is paid  
B) Marketplace is publicly discoverable; Data Exchange is invitation-only  
C) Marketplace only supports structured data; Data Exchange supports all types  
D) Marketplace is cross-region; Data Exchange is single-region only  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The Marketplace is a public catalog discoverable by all Snowflake customers. A Private Data Exchange is an invitation-only group where an administrator controls membership. Both support cross-region (via listings/auto-fulfillment), both support free and paid arrangements, and both support all data types.

</details>

---

### Question 17
A data provider publishes a free listing on the Snowflake Marketplace. A consumer in a different region wants to access it. What mechanism enables this?

A) The consumer must create an account in the provider's region  
B) Cross-region replication configured manually by the provider  
C) Auto-fulfillment — Snowflake automatically replicates to the consumer's region  
D) The consumer queries the data cross-region with higher latency  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Auto-fulfillment is the mechanism that enables cross-region and cross-cloud Marketplace delivery. When enabled by the provider, Snowflake automatically replicates the shared data to the consumer's region. The provider pays for replication costs. This is transparent to the consumer.

</details>

---

### Question 18
In a Private Data Exchange, which role manages membership and governance?

A) The first provider who creates the exchange  
B) The Data Exchange Administrator  
C) Snowflake Support  
D) ACCOUNTADMIN of any member account  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Each Private Data Exchange has a designated administrator who controls membership (inviting/removing members), sets governance policies, and manages the exchange. This role is separate from provider/consumer roles within the exchange. Members can be providers, consumers, or both.

</details>

---

### Question 19
A company wants to monetize their data on the Snowflake Marketplace. Which listing type should they use for custom, per-customer pricing?

A) Free Listing  
B) Standard Paid Listing  
C) Personalized Listing  
D) Private Listing  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Personalized Listings allow the provider to set custom pricing, terms, and access on a per-consumer basis. Standard Paid Listings have uniform pricing for all consumers. Free Listings have no cost. Personalized Listings require the consumer to "Request" access rather than immediately "Get" the data.

</details>

---

### Question 20
Which Snowflake edition is required for a provider to publish PAID listings on the Marketplace?

A) Standard Edition  
B) Enterprise Edition  
C) Business Critical Edition  
D) Any edition can publish paid listings  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Publishing paid listings on the Snowflake Marketplace requires Business Critical edition or higher for the provider account. Free listings can be published from any edition. This requirement exists because paid listings often involve cross-region fulfillment and advanced governance features.

</details>

---

### Question 21
A consumer "gets" a free Marketplace listing. What is created in their account?

A) A new database with a full copy of the provider's data  
B) A read-only database referencing the provider's shared data  
C) A set of tables imported into an existing database  
D) A link object that must be activated before use  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Getting a Marketplace listing creates a read-only database in the consumer's account — identical to creating a database from a direct share. The data is zero-copy (no physical replication within the same region). The consumer uses their own warehouse to query. For cross-region, auto-fulfillment creates a local replica.

</details>

---

### Question 22
What types of data products can be published on the Snowflake Marketplace?

A) Only tables and views  
B) Tables, secure views, secure UDFs, entire databases, and Snowflake Native Apps  
C) Any Snowflake object including stored procedures and tasks  
D) Only pre-built dashboards and notebooks  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Marketplace listings can include standard shared data (tables, secure views, secure UDFs packaged as a shared database) as well as Snowflake Native Apps (application packages). Stored procedures, tasks, pipes, stages, and other non-sharable objects cannot be directly listed. Native Apps can contain procedures internally.

</details>

---

### Question 23
How many Data Exchanges can a single Snowflake account participate in?

A) One as provider, one as consumer  
B) One total  
C) Multiple — there is no hard limit on membership  
D) Up to 5 exchanges per account  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** A single Snowflake account can participate in multiple Data Exchanges simultaneously without a fixed limit. Within each exchange, the account can serve as a provider, consumer, or both. This enables organizations to participate in multiple industry consortiums or partner groups.

</details>

---

### Question 24
A provider publishes a listing on the Marketplace with auto-fulfillment. Who pays for the cross-region data replication?

A) The consumer pays replication costs  
B) The provider pays replication costs  
C) Snowflake absorbs replication costs  
D) Costs are shared equally between provider and consumer  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The provider pays for data replication and storage costs associated with auto-fulfillment to other regions. The consumer only pays for their compute (warehouse) when querying. This cost model incentivizes providers to only enable auto-fulfillment for regions where they expect consumer demand.

</details>

---

### Question 25
What is the difference between a "Get" and "Request" button on a Marketplace listing?

A) "Get" is for paid; "Request" is for free  
B) "Get" provides immediate access; "Request" requires provider approval  
C) "Get" is for same-region; "Request" is for cross-region  
D) "Get" is for tables; "Request" is for Native Apps  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** "Get" means the consumer can immediately access the data — typical for free or standard paid listings. "Request" means the consumer must ask for access and the provider must approve — used for personalized listings where custom terms, pricing, or eligibility verification is needed.

</details>

---

### Question 26
A data provider wants to track how many consumers have installed their Marketplace listing. Where can they find this?

A) In the SNOWFLAKE.ACCOUNT_USAGE schema  
B) In the Provider Studio within Snowsight  
C) By querying INFORMATION_SCHEMA in the shared database  
D) This information is not available to providers  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The Provider Studio (accessible through Snowsight under Data Products → Provider Studio) shows providers analytics about their listings including: number of consumers, daily queries, which regions consumers are in, and usage trends. Providers cannot see individual query details but can see aggregate metrics.

</details>

---

### Question 27
In the context of the Marketplace, what is a "sample" or "trial" dataset?

A) A dataset with randomly generated fake data  
B) A free listing that provides a limited subset of the full paid dataset  
C) A temporary 30-day access to the full dataset  
D) A metadata-only preview with no actual query capability  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Providers often publish a free sample listing alongside their paid full listing. The sample contains real but limited data (e.g., one month instead of five years, or aggregated instead of granular) so consumers can validate quality and compatibility before purchasing. It provides full query capability on the limited dataset.

</details>

---

## Section C: Replication & Cross-Region (13 Questions)

### Question 28
What is the key difference between a Replication Group and a Failover Group?

A) Replication Groups are for databases; Failover Groups are for accounts  
B) Replication Groups copy data; Failover Groups only copy metadata  
C) Replication Groups provide read-only replicas; Failover Groups allow promotion to primary  
D) Replication Groups are cross-cloud; Failover Groups are same-cloud only  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Both replicate objects to secondary accounts. The key difference is that Failover Groups include the ability to PROMOTE the secondary to become the new primary (failover capability). Replication Groups create read-only replicas only. Both can work cross-cloud and include the same object types.

</details>

---

### Question 29
Which Snowflake edition is required for database replication?

A) Standard Edition  
B) Enterprise Edition  
C) Business Critical Edition  
D) Any edition supports replication  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Database replication and failover require Business Critical edition or higher on BOTH the source and target accounts. This is a frequently tested fact on the exam. Standard and Enterprise editions do not support replication or failover groups.

</details>

---

### Question 30
A company replicates a database from Account A (primary) to Account B (secondary). Can users in Account B write to the replicated database?

A) Yes, with WRITE privilege granted by Account A  
B) Yes, but only for INSERT operations  
C) No, the replicated database is read-only until failover  
D) No, replication is for backup only and cannot be queried  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** A replicated database in the secondary account is read-only. Users can query it but cannot perform any DML (INSERT, UPDATE, DELETE) or DDL operations. The replica becomes writable ONLY if the secondary is promoted to primary through a failover operation.

</details>

---

### Question 31
Which objects can be replicated using replication/failover groups? (Choose the MOST complete answer)

A) Databases only  
B) Databases and shares  
C) Databases, shares, users, roles, warehouses, network policies, integrations  
D) Everything in the account including query history and billing  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Replication/failover groups can include: DATABASES, SHARES, USERS, ROLES, WAREHOUSES, RESOURCE MONITORS, NETWORK POLICIES, INTEGRATIONS, and CONNECTIONS. Query history, billing data, session state, and some metadata are NOT replicated. Each object type is opt-in when configuring the group.

</details>

---

### Question 32
What is the replication lag in Snowflake database replication?

A) Always real-time (zero lag)  
B) Exactly 1 hour  
C) Depends on refresh schedule and data volume — typically minutes to hours  
D) Fixed at 24 hours  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Replication lag depends on the configured refresh schedule (can be as frequent as every few minutes) and the volume of changes to replicate. Initial replication of a large database takes longer. Subsequent refreshes are incremental (only changed micro-partitions). There is always SOME lag — it is never truly real-time.

</details>

---

### Question 33
How is replication configured between two accounts?

A) Both accounts must be in the same Snowflake organization  
B) Accounts must be in the same cloud provider  
C) Accounts must be in the same region  
D) Any two Snowflake accounts can replicate regardless of organization  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**  
**Explanation:** Replication (and failover) can only be configured between accounts within the SAME Snowflake organization. The accounts can be in different regions and different cloud providers (AWS, Azure, GCP), but they must belong to the same organization. This is a key exam point.

</details>

---

### Question 34
A global company has accounts in AWS US-East, Azure West Europe, and GCP Asia. They want all three to stay in sync. How many replication relationships are needed?

A) 1 (primary replicates to both)  
B) 2 (primary → secondary, primary → tertiary)  
C) 3 (each replicates to the other two)  
D) 6 (bidirectional between all pairs)  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Replication follows a primary → secondary model. One account is designated as primary, and it replicates to one or more secondary accounts. You need one replication relationship per secondary. With one primary and two secondaries, that's 2 relationships. Secondaries do NOT replicate to each other.

</details>

---

### Question 35
What command promotes a secondary (failover) database to become the primary?

A) `ALTER DATABASE mydb PROMOTE`  
B) `ALTER DATABASE mydb PRIMARY`  
C) `ALTER FAILOVER GROUP mygroup PRIMARY`  
D) `SELECT SYSTEM$PRIMARY_FAILOVER('mygroup')`  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** Failover is performed at the group level, not the individual database level. The correct command is `ALTER FAILOVER GROUP <name> PRIMARY` executed in the secondary account. This promotes all objects in the group simultaneously. The old primary automatically becomes a secondary.

</details>

---

### Question 36
What is Client Redirect in the context of Snowflake replication?

A) A DNS-based mechanism to route client connections to the current primary account  
B) A proxy server that load-balances queries across replicas  
C) A connection parameter that switches between reader and writer endpoints  
D) A Snowflake feature that redirects failed queries to the secondary  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A)**  
**Explanation:** Client Redirect (using Connection objects) allows applications to use a single connection URL that automatically points to whichever account is currently the primary. After a failover event, the connection URL redirects to the new primary without client-side configuration changes. This enables transparent failover for applications.

</details>

---

### Question 37
What data transfer costs are associated with replication?

A) No cost — replication is included in the Snowflake subscription  
B) Inter-region data transfer costs only when replicating across regions  
C) Per-byte costs for all replicated data regardless of region  
D) A flat monthly fee per replication group  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Replication incurs: (1) data transfer costs when replicating across regions or clouds, and (2) storage costs for the replica in the target account. Within the same region (but different accounts), transfer costs are minimal or zero. Compute for the replication process is also charged. There is no flat fee — costs scale with data volume.

</details>

---

### Question 38
A company configures a failover group with DATABASES and ROLES but NOT WAREHOUSES. After failover, what happens?

A) Failover fails because all object types must be included  
B) Failover succeeds — users have roles but no warehouses, so they cannot query  
C) Failover succeeds — Snowflake auto-creates default warehouses  
D) Failover succeeds — warehouses from the secondary account's existing config are used  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**  
**Explanation:** Failover group object types are independently configurable. If WAREHOUSES is not included, the secondary account uses whatever warehouses already exist there (which could be pre-provisioned for DR). If no warehouses exist in the secondary, users would need to create them. Snowflake does NOT auto-create warehouses during failover.

</details>

---

### Question 39
How does Snowflake handle replication of shared data (data shared TO the primary account from a third party)?

A) Shared data is automatically replicated to the secondary  
B) Shared data is NOT replicated — the secondary must establish its own share from the third party  
C) Only metadata about the share is replicated; data remains in the original provider  
D) Shared data is replicated but becomes read-write in the secondary  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Data that is shared INTO an account (inbound shares/imported databases) is NOT replicated. The secondary account must independently establish sharing relationships with the same third-party providers. This is because the secondary does not own that data — it belongs to the external provider. Only data owned by the primary is replicated.

</details>

---

### Question 40
What is the maximum number of secondary accounts for a single replication or failover group?

A) 1  
B) 3  
C) No hard limit (within organization account limits)  
D) 10  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** There is no fixed maximum number of secondary accounts for a replication or failover group. You can replicate to as many secondary accounts as exist in your organization. Practical limits are driven by cost (each secondary incurs storage and transfer charges) and by the total number of accounts in the organization.

</details>

---

### Question 41
Which function can you use to monitor replication lag for a database?

A) `REPLICATION_USAGE_HISTORY()`  
B) `SNOWFLAKE.ACCOUNT_USAGE.REPLICATION_GROUP_REFRESH_HISTORY`  
C) `SYSTEM$GET_REPLICATION_STATUS()`  
D) `SHOW REPLICATION DATABASES`  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The `REPLICATION_GROUP_REFRESH_HISTORY` view in `SNOWFLAKE.ACCOUNT_USAGE` provides details on replication refreshes including timing, bytes transferred, and status. `SHOW REPLICATION DATABASES` shows configuration but not detailed lag metrics. Multiple monitoring options exist, but the Account Usage view provides the most comprehensive history.

</details>

---

### Question 42
A database in a failover group has a refresh schedule of every 10 minutes. The primary fails. What is the maximum data loss (RPO)?

A) Zero — replication is synchronous  
B) Up to 10 minutes of data  
C) Depends on whether the last refresh completed before failure  
D) Up to 24 hours  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** RPO (Recovery Point Objective) depends on when the last successful refresh completed relative to the failure. If the refresh runs every 10 minutes and the last one completed 9 minutes before failure, RPO is approximately 9 minutes. If the last refresh failed or was still in progress, RPO could be longer (up to 10 min + duration of previous refresh cycle). Replication is asynchronous, so zero data loss is not guaranteed.

</details>

---

### Question 43
Cross-cloud replication is configured between AWS and Azure accounts. What formats does Snowflake use for data in transit?

A) Provider's cloud-native format (S3 format for AWS, Blob format for Azure)  
B) Snowflake's proprietary micro-partition format — cloud-agnostic  
C) Apache Parquet for cross-cloud compatibility  
D) CSV/JSON intermediate format  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Snowflake uses its own proprietary micro-partition format for replication regardless of the underlying cloud. Data does not need to be converted between cloud-specific formats. This is part of Snowflake's cloud-agnostic architecture — the storage format is the same on AWS, Azure, and GCP.

</details>

---

### Question 44
Which statement about replication and Snowflake objects is TRUE?

A) A database can belong to multiple replication groups simultaneously  
B) A database can belong to only ONE replication or failover group  
C) Replication groups can include databases from different accounts  
D) You can replicate individual tables without replicating the full database  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** A database can belong to only ONE replication or failover group at a time. You cannot replicate the same database through multiple groups. Replication operates at the database level (not individual tables). All objects within the database are replicated together. The group is defined within a single primary account.

</details>

---

### Question 45
A company needs to share data cross-region but wants to minimize replication costs. The data is 5TB but the consumer only queries a specific 200GB subset. What should they do?

A) Replicate the full 5TB database and let the consumer filter  
B) Create a separate database with only the needed 200GB, then replicate or share that  
C) Use a secure view to filter, then share — cross-region sharing only replicates queried data  
D) Use Snowpipe to stream only the subset to the consumer's region  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** The most cost-effective approach is to isolate the needed subset into its own database and replicate only that. Replication copies the entire database — you cannot replicate a subset. Secure views filter at query time but don't reduce replication volume (the underlying tables are still fully replicated). Creating a purpose-built database for sharing is a common best practice.

</details>

---

## Bonus Questions — Mixed Topics

### Question 46
Which of the following is NOT possible with Snowflake Secure Data Sharing?

A) Sharing data across different cloud providers  
B) Sharing a stored procedure with a consumer  
C) Sharing a secure UDF with a consumer  
D) Sharing data with a consumer who has no Snowflake account (via reader account)  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** Stored procedures CANNOT be shared through Secure Data Sharing. Shareable objects are: tables, external tables, secure views, secure materialized views, and secure UDFs/UDTFs. Cross-cloud sharing is possible via listings/auto-fulfillment. Reader accounts enable sharing with non-Snowflake users. Secure UDFs are shareable.

</details>

---

### Question 47
What happens to a share when the provider account is suspended or terminated?

A) The share continues to work from cached data  
B) Consumer access is immediately revoked — shared databases become inaccessible  
C) The consumer keeps access for 45 days (data retention period)  
D) Ownership transfers to the consumer  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** If a provider account is suspended or dropped, all shares from that account become immediately inaccessible. Since sharing is zero-copy (no physical data in consumer's account), there is nothing to fall back on. Consumer databases from share return errors. There is no grace period or data retention for shared data on the consumer side.

</details>

---

### Question 48
A multinational bank uses Snowflake with Business Critical edition. They want to implement disaster recovery with <5 minute RPO across AWS regions. Is this achievable with Snowflake native features?

A) Yes — configure failover group with 5-minute refresh schedule  
B) Yes — use synchronous replication for zero RPO  
C) No — minimum RPO with Snowflake replication is approximately 1 hour  
D) Partially — <5 minute RPO is achievable but not guaranteed due to asynchronous nature  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D)**  
**Explanation:** Snowflake replication is asynchronous with configurable refresh schedules. You CAN set a very frequent refresh (e.g., every few minutes), but actual RPO depends on data volume, network conditions, and whether the last refresh completed. Near-5-minute RPO is achievable for smaller datasets but is NOT guaranteed. Snowflake does not offer synchronous replication.

</details>

---

### Question 49
What is the relationship between a share object and the data it contains?

A) The share contains a physical copy of the data  
B) The share is a named object that holds grants/references to the underlying objects  
C) The share is an encrypted container of data files  
D) The share is a virtual database that wraps the original tables  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B)**  
**Explanation:** A share is a Snowflake object that encapsulates grants (privileges) on database objects. It doesn't contain data — it references the underlying tables/views and specifies which accounts can access them. This is why sharing is "zero-copy" — the data stays in place and the share is just a permission envelope.

</details>

---

### Question 50
When using SHOW SHARES, what types of shares can you see?

A) Only shares you have created (outbound)  
B) Only shares available to you (inbound)  
C) Both OUTBOUND shares (you created) and INBOUND shares (shared to you)  
D) All shares in the Snowflake deployment  

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C)**  
**Explanation:** `SHOW SHARES` displays both OUTBOUND shares (created by your account, shared to others) and INBOUND shares (shared to your account by other providers). The output includes a KIND column that distinguishes them. You cannot see shares between other accounts that don't involve you.

</details>

---

## Bonus: Advanced Scenario Questions

### Question 1
A provider creates a share containing a standard (non-secure) view that filters sensitive salary data. They attempt to add consumer account 'ACME_CORP' to the share. What happens?
- A) The share is created successfully but the consumer sees unfiltered data
- B) The ALTER SHARE ADD ACCOUNTS command fails with an error — only secure views can be shared
- C) The share works but the consumer can see the view definition via SHOW VIEWS
- D) The consumer can use the view but EXPLAIN plans reveal the filter logic

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The ALTER SHARE ADD ACCOUNTS command fails with an error — only secure views can be shared**
**Explanation:** Snowflake enforces that ALL views in a share must be secure views. Attempting to add a non-secure view to a share (via GRANT on the share) produces an error. This prevents information leakage through view definitions, query plans, and optimizer behaviors.
**Exam Trap:** The error occurs at GRANT time (adding the view to the share), not when adding consumer accounts — the share itself can exist, but non-secure views cannot be granted to it.

</details>

---

### Question 2
A provider creates a reader account for a partner company. The partner's analysts run complex queries consuming significant warehouse credits. Who receives the bill for this compute?
- A) The partner company (reader account holder)
- B) The provider who created the reader account
- C) Split between provider and consumer based on usage
- D) Snowflake absorbs reader account compute costs

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The provider who created the reader account**
**Explanation:** Reader accounts (managed accounts) are fully funded by the provider who created them. All compute, storage, and transfer costs for reader accounts are billed to the provider's account. This is a key cost consideration — providers often set resource monitors on reader accounts to control spend.
**Exam Trap:** Regular sharing (consumer has their own account) = consumer pays compute. Reader accounts = provider pays EVERYTHING.

</details>

---

### Question 3
A US-based provider on AWS US-East-1 wants to share data with a consumer on Azure West Europe. They attempt a direct share (CREATE SHARE, ADD ACCOUNTS). What happens?
- A) The share works seamlessly — Snowflake handles cross-cloud sharing transparently
- B) The share fails — direct sharing only works within the same cloud region; they need database replication or a Marketplace listing with auto-fulfillment
- C) The share works but with high latency for the consumer's queries
- D) The provider must first replicate their account to Azure

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The share fails — direct sharing only works within the same cloud region; they need database replication or a Marketplace listing with auto-fulfillment**
**Explanation:** Direct shares (CREATE SHARE) only work between accounts in the same region. For cross-region or cross-cloud sharing, the provider must either: (1) replicate the database to the consumer's region first, or (2) create a Marketplace listing with auto-fulfillment enabled. Direct sharing is regional only.
**Exam Trap:** "Same region" means same cloud provider AND same region — AWS US-East-1 cannot directly share with AWS EU-West-1.

</details>

---

### Question 4
A healthcare company wants to monetize de-identified patient analytics data. They want any Snowflake customer to discover and access it with uniform pricing. Which Marketplace listing type is appropriate?
- A) Free Listing
- B) Standard Paid Listing (publicly discoverable, uniform pricing)
- C) Personalized Listing (request-based with custom pricing)
- D) Private Data Exchange listing

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Standard Paid Listing (publicly discoverable, uniform pricing)**
**Explanation:** Standard Paid Listings are publicly discoverable on the Marketplace with uniform pricing for all consumers. Consumers click "Get" and are immediately charged the listed price. Personalized Listings require provider approval per consumer. A Private Data Exchange is invitation-only, not publicly discoverable.
**Exam Trap:** Standard Paid = "Get" button (immediate access). Personalized = "Request" button (provider approval required).

</details>

---

### Question 5
A company on Enterprise Edition wants to configure a Failover Group to replicate their database to a DR account in another region. They attempt `CREATE FAILOVER GROUP`. What happens?
- A) The command succeeds — Failover Groups are available on Enterprise Edition
- B) The command fails — Failover Groups require Business Critical Edition or higher on BOTH source and target accounts
- C) The command succeeds but failover (promotion) is disabled until upgrade
- D) Only database replication works on Enterprise; Failover Groups need VPS edition

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The command fails — Failover Groups require Business Critical Edition or higher on BOTH source and target accounts**
**Explanation:** Both replication groups AND failover groups require Business Critical Edition on BOTH the primary and secondary accounts. Enterprise Edition does not support any form of database replication or failover. This is one of the most commonly tested edition requirements.
**Exam Trap:** BOTH accounts must be Business Critical — if either account is Enterprise or lower, replication/failover will fail.

</details>

---

### Question 6
A secondary (replicated) database exists in a DR account. A developer in the DR account attempts: `INSERT INTO replicated_db.schema.table VALUES (1, 'test')`. What happens?
- A) The INSERT succeeds and the data is replicated back to the primary
- B) The INSERT fails — replicated databases are read-only until promoted to primary
- C) The INSERT succeeds but the row is lost on the next replication refresh
- D) The INSERT is queued until the next failover event

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The INSERT fails — replicated databases are read-only until promoted to primary**
**Explanation:** Secondary/replicated databases are strictly read-only. No DML (INSERT, UPDATE, DELETE) or DDL (ALTER, CREATE) is permitted. The database becomes writable ONLY after promotion to primary via ALTER FAILOVER GROUP ... PRIMARY. Queries (SELECT) work normally.
**Exam Trap:** Read-only means ALL writes are blocked — not just data modifications but also DDL like adding views or granting privileges.

</details>

---

### Question 7
An industry consortium of 15 pharmaceutical companies wants to share clinical trial metadata among themselves but NOT with the general public. No single company should control the data exchange, and membership should be governed collectively. What is the best solution?
- A) One company creates 14 direct shares (one to each partner)
- B) A Private Data Exchange with a neutral administrator and all 15 companies as members
- C) Each company publishes to the public Marketplace with access restricted by account
- D) Create 15 reader accounts managed by a central entity

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) A Private Data Exchange with a neutral administrator and all 15 companies as members**
**Explanation:** A Private Data Exchange provides: invitation-only membership, a designated administrator for governance, and the ability for all members to be both providers AND consumers. Direct shares are 1-to-1 and would require N*(N-1)/2 relationships. The Marketplace is public. Reader accounts are for non-Snowflake users.
**Exam Trap:** Data Exchange = governed, multi-directional sharing among a group. Marketplace = public, one-to-many discovery. Direct Share = point-to-point.

</details>

---

### Question 8
A provider runs: `GRANT SELECT ON TABLE sensitive_data TO SHARE my_share`. They have NOT created a secure view. The consumer creates a database from the share and queries the table. Can the consumer see the raw data?
- A) Yes — tables can be shared directly without secure views, exposing all rows and columns
- B) No — tables shared directly are automatically wrapped in a secure view
- C) Yes, but only columns that were explicitly listed in the GRANT
- D) No — sharing tables directly is not permitted; only views can be shared

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) Yes — tables can be shared directly without secure views, exposing all rows and columns**
**Explanation:** Tables CAN be shared directly via GRANT to a share. The consumer sees all rows and all columns with no filtering. Secure views are only REQUIRED when you need to restrict what the consumer sees or when sharing views. If sharing a full table is acceptable, no secure view is needed.
**Exam Trap:** Secure views are required for VIEWS in a share, not for tables — tables can be shared directly with full access.

</details>

---

### Question 9
A provider has a secure view that uses `CURRENT_ACCOUNT()` for row filtering. The view references a mapping table that maps consumer account identifiers to allowed data. A new consumer is added to the share. What must the provider do to enable data access for the new consumer?
- A) Nothing — the view automatically filters based on the new consumer's account
- B) INSERT the new consumer's account identifier and allowed data mappings into the mapping table
- C) Recreate the share with the new consumer included
- D) Grant the new consumer a new role with specific privileges

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) INSERT the new consumer's account identifier and allowed data mappings into the mapping table**
**Explanation:** The secure view filters dynamically using CURRENT_ACCOUNT(), but it needs the mapping table to contain entries for the new consumer. Adding the account to the share grants access to the view, but without a mapping table entry, the WHERE clause filters out all rows (returning empty results).
**Exam Trap:** Adding a consumer to the share gives them access to the view object, but the DATA they see depends on the mapping table — forgetting to update it results in empty query results, not an error.

</details>

---

### Question 10
A share contains objects from database `ANALYTICS_DB`. The provider wants to add a table from `MARKETING_DB` to the same share. Is this possible?
- A) Yes — shares can span multiple databases
- B) No — a share can only contain objects from a SINGLE database
- C) Yes, but only if both databases are in the same schema
- D) No, but you can create a cross-database secure view in ANALYTICS_DB that references MARKETING_DB

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) No, but you can create a cross-database secure view in ANALYTICS_DB that references MARKETING_DB**
**Explanation:** A share is limited to objects from a single database. However, you can create a secure view in the share's database that queries tables from another database. This view can then be added to the share, effectively making cross-database data available through a single share.
**Exam Trap:** The workaround (cross-database secure view) is a key pattern — the exam tests whether you know both the limitation AND the solution.

</details>

---

### Question 11
A company uses a Marketplace listing with auto-fulfillment. Their data is 500GB and they enable fulfillment to 8 regions. Approximately how much additional storage cost do they incur?
- A) Zero — auto-fulfillment doesn't replicate data until a consumer requests it
- B) Up to 8x their storage cost (500GB replicated to each region)
- C) Storage only for regions where consumers have actually "gotten" the listing
- D) A flat fee per region regardless of data size

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Storage only for regions where consumers have actually "gotten" the listing**
**Explanation:** Auto-fulfillment replicates data to a region only when a consumer in that region requests (gets) the listing. Enabling auto-fulfillment for 8 regions doesn't immediately replicate — it only provisions when demand exists. Storage costs are per-region where data is actually replicated.
**Exam Trap:** Enabling auto-fulfillment is not the same as replicating — replication only happens on consumer demand, not on enablement.

</details>

---

### Question 12
A provider shares data with Consumer A. Consumer A creates a database from the share. Consumer A then wants to share a secure view (built on the shared data) with Consumer B. Can they?
- A) Yes — Consumer A can reshare by creating their own share containing the view
- B) No — databases created from shares are read-only and cannot have objects granted to new shares
- C) Yes, but only with the original provider's explicit permission
- D) No — resharing is blocked because Consumer A doesn't own the underlying data

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No — databases created from shares are read-only and cannot have objects granted to new shares**
**Explanation:** Databases created from shares are read-only — you cannot create views, tables, or other objects within them. Since Consumer A can't create a secure view IN the shared database, they can't reshare. They would need to copy data to their own database first, then share from there (which creates a physical copy, breaking zero-copy efficiency).
**Exam Trap:** Zero-copy sharing is provider-to-consumer only — "resharing" or "chaining" shares is not architecturally possible without data duplication.

</details>

---

### Question 13
A provider's Snowflake account is in AWS US-East-1. They create a direct share with Consumer X (also AWS US-East-1) and Consumer Y (AWS US-West-2). What happens for each consumer?
- A) Both consumers can access the share successfully
- B) Consumer X can access the share; Consumer Y's ADD ACCOUNTS fails because it's cross-region
- C) Both fail because direct shares require Business Critical edition
- D) Consumer X succeeds; Consumer Y gets read access with higher latency

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Consumer X can access the share; Consumer Y's ADD ACCOUNTS fails because it's cross-region**
**Explanation:** Direct shares (non-listing) work only within the same region. Consumer X (same region) can be added. Consumer Y (different region) cannot be added to a direct share. For Consumer Y, the provider must use a Marketplace listing with auto-fulfillment or replicate the database to US-West-2 first.
**Exam Trap:** "Same region" for sharing is exact — even different availability zones within the same cloud are fine, but different regions are not.

</details>

---

### Question 14
A provider grants SELECT on a table to a share, then later ALTERs the table to add a NOT NULL constraint on an existing column. Some existing rows have NULLs in that column. What does the consumer experience?
- A) The consumer immediately sees the constraint error on their queries
- B) The consumer sees the new constraint reflected but can still query all rows (constraints are informational in Snowflake)
- C) The consumer's database becomes invalid and must be recreated
- D) The ALTER fails because the table is part of a share

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The consumer sees the new constraint reflected but can still query all rows (constraints are informational in Snowflake)**
**Explanation:** In Snowflake, most constraints (NOT NULL, UNIQUE, FOREIGN KEY) are informational/declarative — they are NOT enforced (except NOT NULL which IS enforced for INSERTs). Since the data already exists with NULLs, and the consumer is read-only, the constraint metadata changes but existing data remains accessible.
**Exam Trap:** Snowflake constraints are largely informational for optimization hints — only NOT NULL is enforced on new data; existing NULL values persist.

</details>

---

### Question 15
A global organization has 3 Snowflake accounts: Primary (AWS US-East), DR1 (Azure EU-West), DR2 (GCP Asia). They configure a Failover Group. A catastrophic failure occurs in AWS US-East. Who can promote DR1 to primary?
- A) Any ACCOUNTADMIN in DR1 can run ALTER FAILOVER GROUP ... PRIMARY
- B) Only the original primary account's ACCOUNTADMIN can authorize promotion
- C) Snowflake Support must perform the promotion
- D) Promotion happens automatically when the primary is unreachable

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) Any ACCOUNTADMIN in DR1 can run ALTER FAILOVER GROUP ... PRIMARY**
**Explanation:** Failover promotion is initiated FROM the secondary account by running ALTER FAILOVER GROUP <name> PRIMARY. The secondary account's ACCOUNTADMIN executes this command. No authorization from the (potentially unavailable) primary is needed — that's the entire point of DR.
**Exam Trap:** Failover is initiated from the SECONDARY, not the primary — if the primary is down, you need access to a secondary account to promote it.

</details>

---

### Question 16
A provider publishes a paid listing on the Snowflake Marketplace. A consumer in the same region "gets" the listing. How is the data delivered to the consumer?
- A) The data is physically copied to the consumer's account storage
- B) A share is created behind the scenes — the consumer gets zero-copy access to the provider's data
- C) The consumer downloads a snapshot file
- D) A dedicated replication stream is established between accounts

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) A share is created behind the scenes — the consumer gets zero-copy access to the provider's data**
**Explanation:** Marketplace listings in the same region use the same zero-copy sharing mechanism as direct shares. No data is physically moved or duplicated. The consumer's "database from listing" is functionally identical to a "database from share." Cross-region listings use auto-fulfillment (replication) instead.
**Exam Trap:** Same-region Marketplace access is zero-copy (like direct sharing); cross-region requires auto-fulfillment (replication with provider-paid storage).

</details>

---

### Question 17
A company creates a share and grants SELECT on table T1. Later they DROP table T1 and CREATE a new table with the same name T1. What does the consumer see?
- A) The consumer automatically sees the new T1
- B) The consumer still sees the old T1 data (share retains a reference to the dropped table)
- C) The consumer's queries on T1 fail — the original grant is invalid after DROP
- D) The consumer sees an empty table

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) The consumer's queries on T1 fail — the original grant is invalid after DROP**
**Explanation:** The share's grant references the original table object (by internal ID, not just name). When that table is dropped, the grant becomes invalid. Creating a new table with the same name doesn't restore the grant. The provider must re-grant the new table to the share.
**Exam Trap:** Shares reference objects by internal ID — DROP + recreate with the same name breaks the share grant and requires re-granting.

</details>

---

### Question 18
A consumer queries a shared secure view and attempts `DESCRIBE VIEW shared_db.schema.secure_view` to understand its columns. What information do they receive?
- A) Full view definition including SQL text, column names, and types
- B) Column names and data types only — the SQL definition is hidden
- C) Nothing — DESCRIBE is blocked on shared objects
- D) Only the view name and creation date

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Column names and data types only — the SQL definition is hidden**
**Explanation:** Consumers can see the column structure (names, types) of secure views via DESCRIBE, which is needed for writing queries. However, the view's SQL definition (the SELECT statement with filters, joins, etc.) is hidden. This is a key security property of secure views — consumers know the output schema but not the logic.
**Exam Trap:** DESCRIBE works but shows structure only — GET_DDL() on a shared secure view returns NULL or an error for the consumer.

</details>

---

### Question 19
A provider in a regulated industry wants to share data only with pre-approved partners, negotiate individual pricing per partner, and require each partner to accept custom terms before accessing data. Which Marketplace approach fits?
- A) Standard Paid Listing with terms of service
- B) Personalized Listing — each consumer must "Request" and the provider approves with custom terms
- C) Free Listing with a separate legal agreement outside Snowflake
- D) Private Data Exchange with custom membership rules

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Personalized Listing — each consumer must "Request" and the provider approves with custom terms**
**Explanation:** Personalized Listings enable per-consumer approval, custom pricing, and individualized terms. The consumer clicks "Request" (not "Get"), the provider reviews the request, negotiates terms, and grants access manually. This is ideal for regulated or high-value datasets requiring bilateral agreements.
**Exam Trap:** Personalized Listing = "Request" button, per-customer pricing, provider approval. Standard Paid = "Get" button, uniform pricing, instant access.

</details>

---

### Question 20
A company configures a Failover Group that includes DATABASES and SHARES. After a failover event (secondary promoted to primary), what happens to the outbound shares that were being served from the original primary?
- A) Consumers immediately connect to the new primary — shares transfer seamlessly
- B) Shares stop working until the provider manually reconfigures them on the new primary
- C) Shares in the failover group are replicated — after promotion, the new primary serves the shares and consumers reconnect
- D) Consumer access is permanently lost until the original primary comes back online

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: C) Shares in the failover group are replicated — after promotion, the new primary serves the shares and consumers reconnect**
**Explanation:** When SHARES are included in a Failover Group, the share definitions and grants are replicated to the secondary. Upon promotion, the new primary account can serve those shares. Consumers may need to update their account references (unless Client Redirect is configured), but the share infrastructure transfers.
**Exam Trap:** Including SHARES in the failover group is critical for DR — without it, consumers lose access even after successful failover.

</details>

---

## Bonus: Advanced Scenario Questions

### Question 51
A provider creates a reader account for a small partner company that doesn't have Snowflake. The partner's analysts run queries on shared data. At month-end, the provider is surprised by a $5,000 compute bill for the reader account. Who pays and why?

- A) The partner pays because they consumed the compute
- B) The PROVIDER pays all compute costs — reader accounts are billed entirely to the provider that created them
- C) Snowflake absorbs reader account costs as part of the sharing feature
- D) Costs are split based on query volume

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The PROVIDER pays all compute costs — reader accounts are billed entirely to the provider that created them**

**Explanation:** Reader accounts (managed accounts) are fully funded by the provider. The provider pays for all compute (warehouse), storage, and data transfer in the reader account. The partner/consumer has no billing relationship with Snowflake. This is why providers should set resource monitors on reader accounts to control costs.

**Exam Trap:** Reader accounts = provider pays EVERYTHING (compute + storage). This is a major cost consideration for providers sharing via reader accounts.

</details>

---

### Question 52
A provider shares a table using a secure view with `WHERE mapping.account_id = CURRENT_ACCOUNT()` for row-level filtering. Consumer A can see 1,000 rows. Consumer A then creates a reader account for their downstream partner. Can the partner's reader account see Consumer A's 1,000 rows?

- A) Yes — the reader account inherits Consumer A's access
- B) No — the reader account has its own CURRENT_ACCOUNT() value, so it sees only rows mapped to its own account identifier in the mapping table
- C) No — reader accounts cannot access shared secure views
- D) Yes — but only if Consumer A grants access explicitly

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No — the reader account has its own CURRENT_ACCOUNT() value, so it sees only rows mapped to its own account identifier in the mapping table**

**Explanation:** Each account (including reader accounts) has a unique account identifier. CURRENT_ACCOUNT() returns the querying account's ID. The reader account's ID would need to be explicitly added to the provider's mapping table for it to see any rows. Reader accounts don't inherit the parent consumer's access — they're independent entities.

**Exam Trap:** CURRENT_ACCOUNT() returns the actual querying account's ID. Reader accounts are separate accounts with their own identifiers.

</details>

---

### Question 53
A provider shares a database containing 100 tables. The consumer only needs 5 tables. The provider wants to minimize what's exposed. What is the recommended approach?

- A) Share the full database and tell the consumer to only query 5 tables
- B) Create a share with a secure view that joins the 5 needed tables, or grant only those 5 specific tables to the share
- C) Create 5 separate shares — one per table
- D) Use a network policy to restrict access to other tables

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Create a share with a secure view that joins the 5 needed tables, or grant only those 5 specific tables to the share**

**Explanation:** Shares allow granular control — you grant specific objects (tables, secure views) to the share. You don't have to share an entire database. Grant only the 5 needed tables, or create a secure view combining them. This follows the principle of least privilege. Network policies don't control object-level access.

**Exam Trap:** Shares grant specific objects, not entire databases. You control exactly what's exposed at the table/view level.

</details>

---

### Question 54
A consumer queries a shared table and notices the data is 2 hours stale compared to what the provider reported. The share is a direct share (same region, same cloud). What's the most likely explanation?

- A) Replication lag between provider and consumer
- B) Zero-copy sharing has NO lag in same-region direct shares — the consumer always sees the provider's current data. The 2-hour "staleness" is likely in the provider's source pipeline, not the sharing mechanism
- C) The consumer needs to refresh their database from share
- D) Result cache is serving old data

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Zero-copy sharing has NO lag in same-region direct shares — the consumer always sees the provider's current data. The 2-hour "staleness" is likely in the provider's source pipeline, not the sharing mechanism**

**Explanation:** Same-region direct sharing is truly zero-copy with zero lag — the consumer reads the same physical micro-partitions as the provider. If data appears stale, the issue is upstream (the provider's ETL hasn't loaded recent data yet). There's no "refresh" needed for direct shares — changes are instantly visible.

**Exam Trap:** Same-region direct shares = zero lag, instant visibility. Staleness always means the provider hasn't updated their table yet.

</details>

---

### Question 55
A provider creates a share and adds a table. They then ADD a consumer to the share. The consumer creates a database from the share. Later, the provider REMOVES the table from the share and adds a different table. What does the consumer see?

- A) Both the old and new tables
- B) Only the new table — the share is dynamic and changes are reflected immediately
- C) Only the old table until they recreate the database
- D) An error — tables cannot be removed from active shares

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Only the new table — the share is dynamic and changes are reflected immediately**

**Explanation:** Shares are dynamic — when the provider adds or removes objects from a share, the consumer's view updates immediately. The consumer sees whatever is currently granted to the share. No action is needed by the consumer — their database from share automatically reflects the current share contents.

**Exam Trap:** Shares are LIVE — adding/removing objects or changing data is immediately visible to consumers without any consumer action.

</details>

---

### Question 56
A company wants to share data cross-region (provider in US-East-1, consumer in EU-West-1) using a direct share. Is this possible?

- A) Yes — direct shares work cross-region natively
- B) No — direct shares only work within the same region. For cross-region, use a Marketplace listing with auto-fulfillment or manually replicate the database first
- C) Yes — but only with Business Critical edition
- D) No — cross-region sharing is not supported in any form

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No — direct shares only work within the same region. For cross-region, use a Marketplace listing with auto-fulfillment or manually replicate the database first**

**Explanation:** Direct shares (CREATE SHARE) only work within the same cloud region. For cross-region sharing, the provider must either: (1) publish a listing with auto-fulfillment enabled, (2) replicate their database to the consumer's region first then share locally, or (3) use a data exchange with cross-region capabilities.

**Exam Trap:** Direct shares = same region only. Cross-region requires replication or Marketplace auto-fulfillment.

</details>

---

### Question 57
A failover group is configured between Account A (primary, US-East) and Account B (secondary, US-West). The group includes DATABASES and SHARES. Account A shares data with Consumer C. After failover to Account B, can Consumer C still access the shared data?

- A) Yes — automatically, because SHARES are included in the failover group
- B) No — Consumer C's database from share points to Account A, which is no longer primary
- C) Yes — but only if Client Redirect is configured
- D) Consumer C must manually recreate their database from the share pointing to Account B

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: A) Yes — automatically, because SHARES are included in the failover group**

**Explanation:** When SHARES are included in the failover group, the share definitions are replicated to the secondary. After failover, Account B becomes the new primary and the replicated shares become active. Consumer C's access is maintained (especially with Client Redirect or updated connection). This is the purpose of including SHARES in failover groups.

**Exam Trap:** Including SHARES in a failover group ensures data sharing continues after DR failover. Without it, consumers lose access.

</details>

---

### Question 58
A provider publishes a free Marketplace listing. A consumer "Gets" the listing and creates a database. The provider then modifies data in their source table (adds 1M rows). When does the consumer see the new rows?

- A) After the consumer refreshes their database
- B) Immediately — same-region listings use the same zero-copy mechanism as direct shares
- C) After the Marketplace syncs (typically 15-30 minutes)
- D) Only after the provider publishes a new version of the listing

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Immediately — same-region listings use the same zero-copy mechanism as direct shares**

**Explanation:** Within the same region, Marketplace listings use the identical zero-copy sharing mechanism as direct shares. Data changes by the provider are immediately visible to all consumers who have installed the listing. There's no sync delay, no versioning required, and no consumer-side refresh needed.

**Exam Trap:** Marketplace listings (same region) = zero-copy, instant updates. Cross-region with auto-fulfillment has replication lag.

</details>

---

### Question 59
A company has Snowflake accounts in 3 regions for their global operations. They want Account B (EU) to be a readable replica of Account A (US). Account C (APAC) should be a full failover target. What configuration is needed?

- A) One replication group (A→B) and one failover group (A→C)
- B) One failover group containing both B and C
- C) Two failover groups — one for B and one for C
- D) A replication group for B (read-only) and a failover group for C (promotable), from the same primary A

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: D) A replication group for B (read-only) and a failover group for C (promotable), from the same primary A**

**Explanation:** Replication groups create read-only replicas (Account B can query but never become primary). Failover groups allow promotion to primary (Account C can take over during DR). Using both group types from the same primary serves different purposes: B for read scaling, C for disaster recovery. A database can only belong to ONE group, so plan which databases go where.

**Exam Trap:** Replication group = read-only replica only. Failover group = read-only + promotable to primary. Choose based on DR vs read-scaling needs.

</details>

---

### Question 60
Account A and Account B are in the same Snowflake organization but different cloud providers (A on AWS, B on Azure). Can A replicate to B?

- A) No — replication only works within the same cloud provider
- B) Yes — replication works cross-cloud as long as accounts are in the same organization
- C) Yes — but only with Virtual Private Snowflake
- D) No — cross-cloud requires a third-party ETL tool

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Yes — replication works cross-cloud as long as accounts are in the same organization**

**Explanation:** Snowflake replication works cross-cloud AND cross-region, provided both accounts are in the same Snowflake organization. The proprietary micro-partition format is cloud-agnostic, so data can be replicated from AWS to Azure or GCP seamlessly. This is a key differentiator — no data format conversion needed.

**Exam Trap:** Replication requirement = same ORGANIZATION. Cross-region and cross-cloud are both supported within the same org.

</details>

---

### Question 61
A consumer creates a database from a share and runs `ALTER DATABASE shared_db SET DATA_RETENTION_TIME_IN_DAYS = 30`. What happens?

- A) Time Travel is enabled for the shared data with 30-day retention
- B) The command FAILS — databases created from shares are read-only and their properties cannot be modified
- C) The setting is accepted but has no effect on shared data
- D) Time Travel works but only for the consumer's local queries

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The command FAILS — databases created from shares are read-only and their properties cannot be modified**

**Explanation:** Databases created from shares are completely read-only. You cannot alter their properties, create objects within them, or enable Time Travel. The consumer sees the provider's current data only. For historical access, the consumer must create local copies (e.g., `CREATE TABLE local_copy AS SELECT * FROM shared_db.schema.table`).

**Exam Trap:** Shared databases = fully read-only. No DDL, no DML, no property changes. Consumers must create local objects for any modifications.

</details>

---

### Question 62
A data provider wants to monitor how consumers are using their shared data — specifically query counts and which tables are most accessed. Where can they find this information?

- A) ACCOUNT_USAGE.QUERY_HISTORY (filtering for consumer queries)
- B) Provider Studio analytics in Snowsight — shows aggregate consumer usage metrics
- C) By querying the shared database's INFORMATION_SCHEMA from the provider side
- D) This information is not available to providers — consumer queries are private

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Provider Studio analytics in Snowsight — shows aggregate consumer usage metrics**

**Explanation:** The Provider Studio (Data Products → Provider Studio in Snowsight) provides aggregate analytics about listing usage: number of consumers, daily query counts, regional distribution. Providers cannot see individual queries or data accessed — only aggregate metrics. This balances transparency with consumer privacy.

**Exam Trap:** Providers see AGGREGATE usage metrics via Provider Studio. They CANNOT see individual consumer queries or query details.

</details>

---

### Question 63
An organization wants to share a secure UDF that performs a proprietary calculation. The UDF references a lookup table. What must be true for the share to work?

- A) Only the UDF needs to be in the share — it automatically accesses the lookup table
- B) Both the secure UDF AND the lookup table must be granted to the share — the UDF needs access to its dependencies within the share context
- C) UDFs cannot be shared
- D) The consumer must have a copy of the lookup table

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Both the secure UDF AND the lookup table must be granted to the share — the UDF needs access to its dependencies within the share context**

**Explanation:** When sharing a secure UDF that references other objects (tables, views), those dependent objects must also be included in the share. The UDF executes in the consumer's context, and it needs access to all objects it references. The share must contain the complete dependency chain.

**Exam Trap:** Shared UDFs need their dependencies (tables, views) also included in the share. Missing dependencies = runtime errors for consumers.

</details>

---

### Question 64
A Snowflake account participates as a consumer in 15 different data shares from various providers. An admin notices the account's storage costs are unusually high. Could the shared data be contributing to storage costs?

- A) Yes — each shared database consumes storage proportional to the data size
- B) No — shared data (inbound shares, same region) incurs ZERO storage cost for the consumer. Storage is paid by the provider only
- C) Yes — but only for cross-region shares with auto-fulfillment
- D) Storage costs are split 50/50

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) No — shared data (inbound shares, same region) incurs ZERO storage cost for the consumer. Storage is paid by the provider only**

**Explanation:** For same-region direct shares, the consumer incurs ZERO storage cost — they're reading the provider's physical data via zero-copy. The provider pays all storage. The consumer only pays for warehouse compute when querying. Cross-region with auto-fulfillment creates a local replica, which DOES incur storage costs — but that's paid by the provider.

**Exam Trap:** Consumer costs for shared data: compute (warehouse) = YES. Storage = NO (same region). Cross-region storage = provider pays.

</details>

---

### Question 65
A provider accidentally shares a table containing sensitive PII that should not have been exposed. They immediately revoke the consumer's access by removing them from the share. Is the data safe?

- A) No — the consumer may have already copied the data locally
- B) Yes — since sharing is zero-copy, the consumer never had a physical copy of the data and loses access immediately upon revocation
- C) Partially — the consumer keeps cached query results
- D) No — there's a 24-hour grace period before revocation takes effect

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Yes — since sharing is zero-copy, the consumer never had a physical copy of the data and loses access immediately upon revocation**

**Explanation:** Zero-copy sharing means no data was ever physically in the consumer's account. Revoking access immediately prevents all future queries. However, if the consumer ran queries and stored results locally (e.g., INSERT INTO local_table SELECT * FROM shared_table) BEFORE revocation, that copied data would persist. The sharing mechanism itself provides immediate revocation.

**Exam Trap:** Revoking share access is IMMEDIATE — but cannot undo data the consumer may have already copied into their own tables.

</details>

---

### Question 66
Replication is configured between Account A (primary) and Account B (secondary) with a 10-minute refresh schedule. The latest successful refresh completed 5 minutes ago. Account A now fails catastrophically. What is the data loss?

- A) Zero — replication is synchronous
- B) Up to 5 minutes of data (since last successful refresh)
- C) Up to 10 minutes of data (one full refresh interval)
- D) Cannot be determined without knowing data volume

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) Up to 5 minutes of data (since last successful refresh)**

**Explanation:** RPO (Recovery Point Objective) equals the time since the last successful replication refresh. If the last refresh completed 5 minutes ago, any data written to Account A in those 5 minutes is lost. Snowflake replication is ASYNCHRONOUS — there is always some lag. RPO varies based on when the failure occurs relative to the last refresh.

**Exam Trap:** Snowflake replication = asynchronous. RPO = time since last successful refresh (not the schedule interval). Zero RPO is NOT guaranteed.

</details>

---

### Question 67
A company creates a share and adds 3 tables from Database_A and 2 tables from Database_B. What happens?

- A) The share is created with all 5 tables successfully
- B) The share creation FAILS — a share can only contain objects from a SINGLE database
- C) The share works but consumers see two separate databases
- D) The tables from Database_B are replicated into Database_A first

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The share creation FAILS — a share can only contain objects from a SINGLE database**

**Explanation:** A share can only include objects from ONE database. This is a hard architectural constraint. To share objects from multiple databases, you must either: (1) create separate shares per database, or (2) create a dedicated "sharing" database with secure views that reference the source tables across databases.

**Exam Trap:** One share = one database. To share from multiple databases, use views in a single "sharing database" that reference source tables cross-database.

</details>

---

### Question 68
A provider shares data via a listing with auto-fulfillment enabled. The listing has consumers in 5 regions. The provider's source data is 10TB. Approximately how much total storage does the provider pay for?

- A) 10TB — auto-fulfillment is free
- B) ~60TB — the original 10TB plus one replica (~10TB) in each of the 5 consumer regions
- C) 10TB plus data transfer costs only
- D) 50TB (10TB × 5 regions, original not counted)

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) ~60TB — the original 10TB plus one replica (~10TB) in each of the 5 consumer regions**

**Explanation:** Auto-fulfillment creates a complete replica of the shared data in each consumer region. With 5 regions, the provider stores: 10TB original + 5 × 10TB replicas = 60TB total storage. Plus ongoing data transfer costs for keeping replicas current. This is why providers carefully select which regions to enable for auto-fulfillment.

**Exam Trap:** Auto-fulfillment = full replica per region. Provider pays ALL replication storage + transfer. Costs scale linearly with regions.

</details>

---

### Question 69
A company has database replication configured (Account A → Account B). They also share data from Account A to Consumer C. After a failover where Account B becomes primary, Consumer C cannot access the shared data. What's wrong?

- A) Consumer C's database from share is permanently broken
- B) The failover group did NOT include SHARES. Without replicating share definitions, consumer access breaks after failover
- C) Account B needs to re-create the share manually
- D) Consumer C must recreate their database from the share

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The failover group did NOT include SHARES. Without replicating share definitions, consumer access breaks after failover**

**Explanation:** For shares to survive failover, the SHARES object type must be included in the failover group alongside DATABASES. If only DATABASES are replicated, the share definitions don't exist on Account B after promotion, breaking consumer access. This is a critical DR planning consideration.

**Exam Trap:** Include SHARES in failover groups to maintain consumer access after DR failover. Missing this = sharing breaks during disaster recovery.

</details>

---

### Question 70
A Snowflake Native App is published on the Marketplace. A consumer installs it. Unlike a standard data share, what additional capability does the Native App provide?

- A) Better compression of shared data
- B) The ability to include stored procedures, tasks, and application logic that runs in the consumer's account — not just passive data
- C) Faster query performance than regular shares
- D) The ability to write data back to the provider

<details>
<summary>🔑 Click to reveal answer</summary>

**Answer: B) The ability to include stored procedures, tasks, and application logic that runs in the consumer's account — not just passive data**

**Explanation:** Native Apps can package code (stored procedures, UDFs, Streamlit dashboards, tasks) alongside data — providing functionality, not just passive data access. Standard shares only provide read-only data access. Native Apps enable providers to distribute complete data applications with business logic.

**Exam Trap:** Native Apps = data + code (procedures, UDFs, UI). Standard shares = data only (tables, views, UDFs but no procedures/tasks).

</details>
