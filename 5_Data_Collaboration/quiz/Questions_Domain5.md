# Domain 5: Data Collaboration — Practice Questions

## Section A: Secure Data Sharing (15 Questions)

### Question 1
What type of view is REQUIRED when sharing data through Snowflake Secure Data Sharing?

A) Materialized view  
B) Standard view  
C) Secure view  
D) Any view type is acceptable  

**Answer: C)**  
**Explanation:** Snowflake mandates that all views included in a share must be secure views. This requirement prevents the consumer from accessing the underlying view definition and prevents query optimizer behaviors from leaking information about filtered data. Standard views cannot be added to shares — Snowflake will return an error.

---

### Question 2
In a direct share, who pays for the compute resources when a consumer queries shared data?

A) The provider pays for all compute  
B) The consumer pays using their own warehouse  
C) Costs are split 50/50 between provider and consumer  
D) Snowflake absorbs the compute cost  

**Answer: B)**  
**Explanation:** In Snowflake's zero-copy sharing model, the consumer uses their own virtual warehouse to query shared data. The provider pays only for storage of the original data. The consumer bears all compute costs for their queries, giving them full control over performance and spend.

---

### Question 3
A provider shares a database with a consumer. The consumer wants to create a new table inside the shared database. What happens?

A) The table is created successfully but only visible to the consumer  
B) The table is created and visible to both provider and consumer  
C) The operation fails — shared databases are read-only  
D) The operation succeeds only if the provider grants CREATE TABLE privilege  

**Answer: C)**  
**Explanation:** Databases created from shares are entirely read-only. Consumers cannot create, modify, or delete any objects within the shared database. If the consumer needs to create derived tables or views, they must do so in a separate database within their own account.

---

### Question 4
Which Snowflake edition is required for a provider to use Secure Data Sharing?

A) Standard Edition or higher  
B) Enterprise Edition or higher  
C) Business Critical Edition or higher  
D) Virtual Private Snowflake only  

**Answer: A)**  
**Explanation:** Secure Data Sharing is available on ALL Snowflake editions, including Standard. This is a core platform feature, not a premium add-on. However, certain advanced features like cross-region/cross-cloud sharing and failover groups require Business Critical or higher.

---

### Question 5
What does CURRENT_ACCOUNT() return when used inside a secure view that has been shared with a consumer?

A) The provider's account identifier  
B) The consumer's account identifier  
C) NULL when accessed by a consumer  
D) An error because context functions are disabled in shares  

**Answer: B)**  
**Explanation:** When a consumer queries a secure view, CURRENT_ACCOUNT() returns the consumer's account identifier. This enables row-level security where the secure view filters data based on which consumer is querying. This is a key pattern for multi-tenant sharing from a single share object.

---

### Question 6
A provider revokes a consumer's access to a share. What happens to the consumer's database created from that share?

A) The database remains with a static snapshot of the last available data  
B) The database is automatically dropped from the consumer's account  
C) The database remains but all queries return errors — no data is accessible  
D) The database enters a 90-day retention period before being dropped  

**Answer: C)**  
**Explanation:** When access is revoked, the database object remains in the consumer's account but becomes completely inaccessible. Any queries against it fail immediately. The consumer would need to drop the dangling database object manually. No data snapshot is retained because there was never a physical copy.

---

### Question 7
Which objects can be included in a Snowflake share? (Choose the BEST answer)

A) Tables, secure views, secure UDFs, and stored procedures  
B) Tables, secure views, secure materialized views, and secure UDFs  
C) Tables, views, UDFs, stages, and file formats  
D) Only tables and secure views  

**Answer: B)**  
**Explanation:** Shares can include: tables, external tables, secure views, secure materialized views, and secure UDFs/UDTFs. Stored procedures CANNOT be shared. Stages, file formats, sequences, and pipes also cannot be directly shared. All views and UDFs in a share must be secure.

---

### Question 8
How does a consumer create a local database from a share?

A) `CREATE DATABASE mydb CLONE provider_acct.share_name`  
B) `CREATE DATABASE mydb FROM SHARE provider_acct.share_name`  
C) `IMPORT DATABASE FROM SHARE provider_acct.share_name AS mydb`  
D) `ALTER DATABASE mydb ADD SHARE provider_acct.share_name`  

**Answer: B)**  
**Explanation:** The correct syntax is `CREATE DATABASE <name> FROM SHARE <provider_account>.<share_name>`. This creates a read-only database in the consumer's account that references the provider's data. CLONE creates a physical copy (different concept). IMPORT and ADD SHARE are not valid syntax.

---

### Question 9
What is a Reader Account in the context of Snowflake data sharing?

A) An account type for consumers who only need SELECT access  
B) A managed account created by the provider for consumers without Snowflake accounts  
C) A limited account that can only read from the Marketplace  
D) An account with read-only access to the provider's entire account  

**Answer: B)**  
**Explanation:** Reader accounts (also called managed accounts) are created by providers to enable sharing with consumers who do not have their own Snowflake account. The provider manages and pays for the reader account's compute resources. Reader accounts can ONLY access data shared by the provider that created them.

---

### Question 10
A secure view in a share uses a WHERE clause filtering on a column. The consumer runs EXPLAIN on their query. What can they see about the filter?

A) The full WHERE clause and filter values  
B) Only that a filter exists, but not the column or values  
C) The query plan does not reveal secure view internals  
D) The same information as a regular view's EXPLAIN output  

**Answer: C)**  
**Explanation:** Secure views intentionally disable certain optimizer behaviors and hide internal details from EXPLAIN plans and query profiles. This prevents consumers from inferring filtered data through plan analysis, error messages, or timing attacks. This is a core security property of secure views, though it can impact performance.

---

### Question 11
What happens to Time Travel on data that has been shared with a consumer?

A) Consumer can use Time Travel on shared data using their own retention settings  
B) Consumer uses the provider's Time Travel retention settings  
C) Time Travel is not available on shared data for the consumer  
D) Time Travel works but only up to 1 day regardless of edition  

**Answer: C)**  
**Explanation:** Consumers cannot use Time Travel on shared data. Time Travel requires access to historical micro-partitions, which is managed by the provider's account. The consumer always sees the current state of the data as it exists in the provider's account. If the consumer needs historical access, the provider must share historical views or tables.

---

### Question 12
A provider wants to share data with 50 different consumer accounts but restrict each consumer to see only their own rows. What is the MOST efficient approach?

A) Create 50 separate shares, each with a dedicated view for that consumer  
B) Create one share with a secure view using CURRENT_ACCOUNT() to filter rows  
C) Create 50 reader accounts, each with custom views  
D) Use a data exchange with 50 members  

**Answer: B)**  
**Explanation:** The most efficient approach is a single share with a secure view that dynamically filters rows based on CURRENT_ACCOUNT(). This avoids managing 50 separate shares or views. A mapping table maps account identifiers to allowed data, and the secure view joins against it. All 50 consumers are added to the same share.

---

### Question 13
Which statement about shares and the objects within them is TRUE?

A) A table can belong to multiple shares simultaneously  
B) A table can only belong to one share at a time  
C) Shares can include objects from multiple databases  
D) A share can contain both secure and non-secure views  

**Answer: A)**  
**Explanation:** A single table (or any sharable object) can be granted to multiple different shares simultaneously. However, each share can only contain objects from a SINGLE database (not multiple). All views in a share must be secure — you cannot include non-secure views.

---

### Question 14
What privilege is required to create a share in Snowflake?

A) ACCOUNTADMIN role only  
B) CREATE SHARE privilege (grantable to any role)  
C) SYSADMIN role or higher  
D) MANAGE SHARES global privilege  

**Answer: B)**  
**Explanation:** The CREATE SHARE privilege can be granted to any role — it is not restricted to ACCOUNTADMIN. However, only ACCOUNTADMIN (or a role with IMPORT SHARE privilege) can create databases from incoming shares on the consumer side. By default, ACCOUNTADMIN has CREATE SHARE privilege.

---

### Question 15
A provider shares a table. Later, they add a new column to that table. What does the consumer see?

A) The consumer must re-create the database from share to see the new column  
B) The new column is automatically visible to the consumer  
C) The provider must update the share grant for the consumer to see it  
D) The consumer sees the new column only after their next query cache expires  

**Answer: B)**  
**Explanation:** Because sharing is zero-copy (no physical data movement), schema changes by the provider are immediately reflected for the consumer. Adding columns, modifying data, or other DDL changes are instantly visible. The consumer does not need to take any action to see updated schemas or data.

---

## Section B: Marketplace & Data Exchange (12 Questions)

### Question 16
What is the primary difference between the Snowflake Marketplace and a Private Data Exchange?

A) Marketplace is free; Data Exchange is paid  
B) Marketplace is publicly discoverable; Data Exchange is invitation-only  
C) Marketplace only supports structured data; Data Exchange supports all types  
D) Marketplace is cross-region; Data Exchange is single-region only  

**Answer: B)**  
**Explanation:** The Marketplace is a public catalog discoverable by all Snowflake customers. A Private Data Exchange is an invitation-only group where an administrator controls membership. Both support cross-region (via listings/auto-fulfillment), both support free and paid arrangements, and both support all data types.

---

### Question 17
A data provider publishes a free listing on the Snowflake Marketplace. A consumer in a different region wants to access it. What mechanism enables this?

A) The consumer must create an account in the provider's region  
B) Cross-region replication configured manually by the provider  
C) Auto-fulfillment — Snowflake automatically replicates to the consumer's region  
D) The consumer queries the data cross-region with higher latency  

**Answer: C)**  
**Explanation:** Auto-fulfillment is the mechanism that enables cross-region and cross-cloud Marketplace delivery. When enabled by the provider, Snowflake automatically replicates the shared data to the consumer's region. The provider pays for replication costs. This is transparent to the consumer.

---

### Question 18
In a Private Data Exchange, which role manages membership and governance?

A) The first provider who creates the exchange  
B) The Data Exchange Administrator  
C) Snowflake Support  
D) ACCOUNTADMIN of any member account  

**Answer: B)**  
**Explanation:** Each Private Data Exchange has a designated administrator who controls membership (inviting/removing members), sets governance policies, and manages the exchange. This role is separate from provider/consumer roles within the exchange. Members can be providers, consumers, or both.

---

### Question 19
A company wants to monetize their data on the Snowflake Marketplace. Which listing type should they use for custom, per-customer pricing?

A) Free Listing  
B) Standard Paid Listing  
C) Personalized Listing  
D) Private Listing  

**Answer: C)**  
**Explanation:** Personalized Listings allow the provider to set custom pricing, terms, and access on a per-consumer basis. Standard Paid Listings have uniform pricing for all consumers. Free Listings have no cost. Personalized Listings require the consumer to "Request" access rather than immediately "Get" the data.

---

### Question 20
Which Snowflake edition is required for a provider to publish PAID listings on the Marketplace?

A) Standard Edition  
B) Enterprise Edition  
C) Business Critical Edition  
D) Any edition can publish paid listings  

**Answer: C)**  
**Explanation:** Publishing paid listings on the Snowflake Marketplace requires Business Critical edition or higher for the provider account. Free listings can be published from any edition. This requirement exists because paid listings often involve cross-region fulfillment and advanced governance features.

---

### Question 21
A consumer "gets" a free Marketplace listing. What is created in their account?

A) A new database with a full copy of the provider's data  
B) A read-only database referencing the provider's shared data  
C) A set of tables imported into an existing database  
D) A link object that must be activated before use  

**Answer: B)**  
**Explanation:** Getting a Marketplace listing creates a read-only database in the consumer's account — identical to creating a database from a direct share. The data is zero-copy (no physical replication within the same region). The consumer uses their own warehouse to query. For cross-region, auto-fulfillment creates a local replica.

---

### Question 22
What types of data products can be published on the Snowflake Marketplace?

A) Only tables and views  
B) Tables, secure views, secure UDFs, entire databases, and Snowflake Native Apps  
C) Any Snowflake object including stored procedures and tasks  
D) Only pre-built dashboards and notebooks  

**Answer: B)**  
**Explanation:** Marketplace listings can include standard shared data (tables, secure views, secure UDFs packaged as a shared database) as well as Snowflake Native Apps (application packages). Stored procedures, tasks, pipes, stages, and other non-sharable objects cannot be directly listed. Native Apps can contain procedures internally.

---

### Question 23
How many Data Exchanges can a single Snowflake account participate in?

A) One as provider, one as consumer  
B) One total  
C) Multiple — there is no hard limit on membership  
D) Up to 5 exchanges per account  

**Answer: C)**  
**Explanation:** A single Snowflake account can participate in multiple Data Exchanges simultaneously without a fixed limit. Within each exchange, the account can serve as a provider, consumer, or both. This enables organizations to participate in multiple industry consortiums or partner groups.

---

### Question 24
A provider publishes a listing on the Marketplace with auto-fulfillment. Who pays for the cross-region data replication?

A) The consumer pays replication costs  
B) The provider pays replication costs  
C) Snowflake absorbs replication costs  
D) Costs are shared equally between provider and consumer  

**Answer: B)**  
**Explanation:** The provider pays for data replication and storage costs associated with auto-fulfillment to other regions. The consumer only pays for their compute (warehouse) when querying. This cost model incentivizes providers to only enable auto-fulfillment for regions where they expect consumer demand.

---

### Question 25
What is the difference between a "Get" and "Request" button on a Marketplace listing?

A) "Get" is for paid; "Request" is for free  
B) "Get" provides immediate access; "Request" requires provider approval  
C) "Get" is for same-region; "Request" is for cross-region  
D) "Get" is for tables; "Request" is for Native Apps  

**Answer: B)**  
**Explanation:** "Get" means the consumer can immediately access the data — typical for free or standard paid listings. "Request" means the consumer must ask for access and the provider must approve — used for personalized listings where custom terms, pricing, or eligibility verification is needed.

---

### Question 26
A data provider wants to track how many consumers have installed their Marketplace listing. Where can they find this?

A) In the SNOWFLAKE.ACCOUNT_USAGE schema  
B) In the Provider Studio within Snowsight  
C) By querying INFORMATION_SCHEMA in the shared database  
D) This information is not available to providers  

**Answer: B)**  
**Explanation:** The Provider Studio (accessible through Snowsight under Data Products → Provider Studio) shows providers analytics about their listings including: number of consumers, daily queries, which regions consumers are in, and usage trends. Providers cannot see individual query details but can see aggregate metrics.

---

### Question 27
In the context of the Marketplace, what is a "sample" or "trial" dataset?

A) A dataset with randomly generated fake data  
B) A free listing that provides a limited subset of the full paid dataset  
C) A temporary 30-day access to the full dataset  
D) A metadata-only preview with no actual query capability  

**Answer: B)**  
**Explanation:** Providers often publish a free sample listing alongside their paid full listing. The sample contains real but limited data (e.g., one month instead of five years, or aggregated instead of granular) so consumers can validate quality and compatibility before purchasing. It provides full query capability on the limited dataset.

---

## Section C: Replication & Cross-Region (13 Questions)

### Question 28
What is the key difference between a Replication Group and a Failover Group?

A) Replication Groups are for databases; Failover Groups are for accounts  
B) Replication Groups copy data; Failover Groups only copy metadata  
C) Replication Groups provide read-only replicas; Failover Groups allow promotion to primary  
D) Replication Groups are cross-cloud; Failover Groups are same-cloud only  

**Answer: C)**  
**Explanation:** Both replicate objects to secondary accounts. The key difference is that Failover Groups include the ability to PROMOTE the secondary to become the new primary (failover capability). Replication Groups create read-only replicas only. Both can work cross-cloud and include the same object types.

---

### Question 29
Which Snowflake edition is required for database replication?

A) Standard Edition  
B) Enterprise Edition  
C) Business Critical Edition  
D) Any edition supports replication  

**Answer: C)**  
**Explanation:** Database replication and failover require Business Critical edition or higher on BOTH the source and target accounts. This is a frequently tested fact on the exam. Standard and Enterprise editions do not support replication or failover groups.

---

### Question 30
A company replicates a database from Account A (primary) to Account B (secondary). Can users in Account B write to the replicated database?

A) Yes, with WRITE privilege granted by Account A  
B) Yes, but only for INSERT operations  
C) No, the replicated database is read-only until failover  
D) No, replication is for backup only and cannot be queried  

**Answer: C)**  
**Explanation:** A replicated database in the secondary account is read-only. Users can query it but cannot perform any DML (INSERT, UPDATE, DELETE) or DDL operations. The replica becomes writable ONLY if the secondary is promoted to primary through a failover operation.

---

### Question 31
Which objects can be replicated using replication/failover groups? (Choose the MOST complete answer)

A) Databases only  
B) Databases and shares  
C) Databases, shares, users, roles, warehouses, network policies, integrations  
D) Everything in the account including query history and billing  

**Answer: C)**  
**Explanation:** Replication/failover groups can include: DATABASES, SHARES, USERS, ROLES, WAREHOUSES, RESOURCE MONITORS, NETWORK POLICIES, INTEGRATIONS, and CONNECTIONS. Query history, billing data, session state, and some metadata are NOT replicated. Each object type is opt-in when configuring the group.

---

### Question 32
What is the replication lag in Snowflake database replication?

A) Always real-time (zero lag)  
B) Exactly 1 hour  
C) Depends on refresh schedule and data volume — typically minutes to hours  
D) Fixed at 24 hours  

**Answer: C)**  
**Explanation:** Replication lag depends on the configured refresh schedule (can be as frequent as every few minutes) and the volume of changes to replicate. Initial replication of a large database takes longer. Subsequent refreshes are incremental (only changed micro-partitions). There is always SOME lag — it is never truly real-time.

---

### Question 33
How is replication configured between two accounts?

A) Both accounts must be in the same Snowflake organization  
B) Accounts must be in the same cloud provider  
C) Accounts must be in the same region  
D) Any two Snowflake accounts can replicate regardless of organization  

**Answer: A)**  
**Explanation:** Replication (and failover) can only be configured between accounts within the SAME Snowflake organization. The accounts can be in different regions and different cloud providers (AWS, Azure, GCP), but they must belong to the same organization. This is a key exam point.

---

### Question 34
A global company has accounts in AWS US-East, Azure West Europe, and GCP Asia. They want all three to stay in sync. How many replication relationships are needed?

A) 1 (primary replicates to both)  
B) 2 (primary → secondary, primary → tertiary)  
C) 3 (each replicates to the other two)  
D) 6 (bidirectional between all pairs)  

**Answer: B)**  
**Explanation:** Replication follows a primary → secondary model. One account is designated as primary, and it replicates to one or more secondary accounts. You need one replication relationship per secondary. With one primary and two secondaries, that's 2 relationships. Secondaries do NOT replicate to each other.

---

### Question 35
What command promotes a secondary (failover) database to become the primary?

A) `ALTER DATABASE mydb PROMOTE`  
B) `ALTER DATABASE mydb PRIMARY`  
C) `ALTER FAILOVER GROUP mygroup PRIMARY`  
D) `SELECT SYSTEM$PRIMARY_FAILOVER('mygroup')`  

**Answer: C)**  
**Explanation:** Failover is performed at the group level, not the individual database level. The correct command is `ALTER FAILOVER GROUP <name> PRIMARY` executed in the secondary account. This promotes all objects in the group simultaneously. The old primary automatically becomes a secondary.

---

### Question 36
What is Client Redirect in the context of Snowflake replication?

A) A DNS-based mechanism to route client connections to the current primary account  
B) A proxy server that load-balances queries across replicas  
C) A connection parameter that switches between reader and writer endpoints  
D) A Snowflake feature that redirects failed queries to the secondary  

**Answer: A)**  
**Explanation:** Client Redirect (using Connection objects) allows applications to use a single connection URL that automatically points to whichever account is currently the primary. After a failover event, the connection URL redirects to the new primary without client-side configuration changes. This enables transparent failover for applications.

---

### Question 37
What data transfer costs are associated with replication?

A) No cost — replication is included in the Snowflake subscription  
B) Inter-region data transfer costs only when replicating across regions  
C) Per-byte costs for all replicated data regardless of region  
D) A flat monthly fee per replication group  

**Answer: B)**  
**Explanation:** Replication incurs: (1) data transfer costs when replicating across regions or clouds, and (2) storage costs for the replica in the target account. Within the same region (but different accounts), transfer costs are minimal or zero. Compute for the replication process is also charged. There is no flat fee — costs scale with data volume.

---

### Question 38
A company configures a failover group with DATABASES and ROLES but NOT WAREHOUSES. After failover, what happens?

A) Failover fails because all object types must be included  
B) Failover succeeds — users have roles but no warehouses, so they cannot query  
C) Failover succeeds — Snowflake auto-creates default warehouses  
D) Failover succeeds — warehouses from the secondary account's existing config are used  

**Answer: D)**  
**Explanation:** Failover group object types are independently configurable. If WAREHOUSES is not included, the secondary account uses whatever warehouses already exist there (which could be pre-provisioned for DR). If no warehouses exist in the secondary, users would need to create them. Snowflake does NOT auto-create warehouses during failover.

---

### Question 39
How does Snowflake handle replication of shared data (data shared TO the primary account from a third party)?

A) Shared data is automatically replicated to the secondary  
B) Shared data is NOT replicated — the secondary must establish its own share from the third party  
C) Only metadata about the share is replicated; data remains in the original provider  
D) Shared data is replicated but becomes read-write in the secondary  

**Answer: B)**  
**Explanation:** Data that is shared INTO an account (inbound shares/imported databases) is NOT replicated. The secondary account must independently establish sharing relationships with the same third-party providers. This is because the secondary does not own that data — it belongs to the external provider. Only data owned by the primary is replicated.

---

### Question 40
What is the maximum number of secondary accounts for a single replication or failover group?

A) 1  
B) 3  
C) No hard limit (within organization account limits)  
D) 10  

**Answer: C)**  
**Explanation:** There is no fixed maximum number of secondary accounts for a replication or failover group. You can replicate to as many secondary accounts as exist in your organization. Practical limits are driven by cost (each secondary incurs storage and transfer charges) and by the total number of accounts in the organization.

---

### Question 41
Which function can you use to monitor replication lag for a database?

A) `REPLICATION_USAGE_HISTORY()`  
B) `SNOWFLAKE.ACCOUNT_USAGE.REPLICATION_GROUP_REFRESH_HISTORY`  
C) `SYSTEM$GET_REPLICATION_STATUS()`  
D) `SHOW REPLICATION DATABASES`  

**Answer: B)**  
**Explanation:** The `REPLICATION_GROUP_REFRESH_HISTORY` view in `SNOWFLAKE.ACCOUNT_USAGE` provides details on replication refreshes including timing, bytes transferred, and status. `SHOW REPLICATION DATABASES` shows configuration but not detailed lag metrics. Multiple monitoring options exist, but the Account Usage view provides the most comprehensive history.

---

### Question 42
A database in a failover group has a refresh schedule of every 10 minutes. The primary fails. What is the maximum data loss (RPO)?

A) Zero — replication is synchronous  
B) Up to 10 minutes of data  
C) Depends on whether the last refresh completed before failure  
D) Up to 24 hours  

**Answer: C)**  
**Explanation:** RPO (Recovery Point Objective) depends on when the last successful refresh completed relative to the failure. If the refresh runs every 10 minutes and the last one completed 9 minutes before failure, RPO is approximately 9 minutes. If the last refresh failed or was still in progress, RPO could be longer (up to 10 min + duration of previous refresh cycle). Replication is asynchronous, so zero data loss is not guaranteed.

---

### Question 43
Cross-cloud replication is configured between AWS and Azure accounts. What formats does Snowflake use for data in transit?

A) Provider's cloud-native format (S3 format for AWS, Blob format for Azure)  
B) Snowflake's proprietary micro-partition format — cloud-agnostic  
C) Apache Parquet for cross-cloud compatibility  
D) CSV/JSON intermediate format  

**Answer: B)**  
**Explanation:** Snowflake uses its own proprietary micro-partition format for replication regardless of the underlying cloud. Data does not need to be converted between cloud-specific formats. This is part of Snowflake's cloud-agnostic architecture — the storage format is the same on AWS, Azure, and GCP.

---

### Question 44
Which statement about replication and Snowflake objects is TRUE?

A) A database can belong to multiple replication groups simultaneously  
B) A database can belong to only ONE replication or failover group  
C) Replication groups can include databases from different accounts  
D) You can replicate individual tables without replicating the full database  

**Answer: B)**  
**Explanation:** A database can belong to only ONE replication or failover group at a time. You cannot replicate the same database through multiple groups. Replication operates at the database level (not individual tables). All objects within the database are replicated together. The group is defined within a single primary account.

---

### Question 45
A company needs to share data cross-region but wants to minimize replication costs. The data is 5TB but the consumer only queries a specific 200GB subset. What should they do?

A) Replicate the full 5TB database and let the consumer filter  
B) Create a separate database with only the needed 200GB, then replicate or share that  
C) Use a secure view to filter, then share — cross-region sharing only replicates queried data  
D) Use Snowpipe to stream only the subset to the consumer's region  

**Answer: B)**  
**Explanation:** The most cost-effective approach is to isolate the needed subset into its own database and replicate only that. Replication copies the entire database — you cannot replicate a subset. Secure views filter at query time but don't reduce replication volume (the underlying tables are still fully replicated). Creating a purpose-built database for sharing is a common best practice.

---

## Bonus Questions — Mixed Topics

### Question 46
Which of the following is NOT possible with Snowflake Secure Data Sharing?

A) Sharing data across different cloud providers  
B) Sharing a stored procedure with a consumer  
C) Sharing a secure UDF with a consumer  
D) Sharing data with a consumer who has no Snowflake account (via reader account)  

**Answer: B)**  
**Explanation:** Stored procedures CANNOT be shared through Secure Data Sharing. Shareable objects are: tables, external tables, secure views, secure materialized views, and secure UDFs/UDTFs. Cross-cloud sharing is possible via listings/auto-fulfillment. Reader accounts enable sharing with non-Snowflake users. Secure UDFs are shareable.

---

### Question 47
What happens to a share when the provider account is suspended or terminated?

A) The share continues to work from cached data  
B) Consumer access is immediately revoked — shared databases become inaccessible  
C) The consumer keeps access for 45 days (data retention period)  
D) Ownership transfers to the consumer  

**Answer: B)**  
**Explanation:** If a provider account is suspended or dropped, all shares from that account become immediately inaccessible. Since sharing is zero-copy (no physical data in consumer's account), there is nothing to fall back on. Consumer databases from share return errors. There is no grace period or data retention for shared data on the consumer side.

---

### Question 48
A multinational bank uses Snowflake with Business Critical edition. They want to implement disaster recovery with <5 minute RPO across AWS regions. Is this achievable with Snowflake native features?

A) Yes — configure failover group with 5-minute refresh schedule  
B) Yes — use synchronous replication for zero RPO  
C) No — minimum RPO with Snowflake replication is approximately 1 hour  
D) Partially — <5 minute RPO is achievable but not guaranteed due to asynchronous nature  

**Answer: D)**  
**Explanation:** Snowflake replication is asynchronous with configurable refresh schedules. You CAN set a very frequent refresh (e.g., every few minutes), but actual RPO depends on data volume, network conditions, and whether the last refresh completed. Near-5-minute RPO is achievable for smaller datasets but is NOT guaranteed. Snowflake does not offer synchronous replication.

---

### Question 49
What is the relationship between a share object and the data it contains?

A) The share contains a physical copy of the data  
B) The share is a named object that holds grants/references to the underlying objects  
C) The share is an encrypted container of data files  
D) The share is a virtual database that wraps the original tables  

**Answer: B)**  
**Explanation:** A share is a Snowflake object that encapsulates grants (privileges) on database objects. It doesn't contain data — it references the underlying tables/views and specifies which accounts can access them. This is why sharing is "zero-copy" — the data stays in place and the share is just a permission envelope.

---

### Question 50
When using SHOW SHARES, what types of shares can you see?

A) Only shares you have created (outbound)  
B) Only shares available to you (inbound)  
C) Both OUTBOUND shares (you created) and INBOUND shares (shared to you)  
D) All shares in the Snowflake deployment  

**Answer: C)**  
**Explanation:** `SHOW SHARES` displays both OUTBOUND shares (created by your account, shared to others) and INBOUND shares (shared to your account by other providers). The output includes a KIND column that distinguishes them. You cannot see shares between other accounts that don't involve you.
