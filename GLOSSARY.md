<h1 align="center">📘 Glossary</h1>

<p align="center">
  <strong>Key terms and definitions for the SnowPro Core Certification (COF-C03)</strong>
</p>

---

## Exam-Critical Terms

| Term | Why It Matters |
|------|---------------|
| **Multi-Cluster Shared Data Architecture** | Foundation of Snowflake — separates compute, storage, cloud services |
| **Micro-Partition** | 50-500MB compressed columnar files — basis for pruning and Time Travel |
| **Virtual Warehouse** | Named compute cluster — scaling, suspension, and sizing are heavily tested |
| **Result Cache** | 24-hour persisted query results — free, no warehouse needed |
| **Time Travel** | 0-90 day retention — Standard=1 day, Enterprise+=90 days |
| **Fail-Safe** | 7-day Snowflake-only recovery after Time Travel — non-configurable |
| **Zero-Copy Clone** | Metadata-only copy — no storage until changes — heavily tested |
| **Secure Data Sharing** | No data movement, real-time access — reader accounts for non-customers |
| **Snowpipe** | Serverless continuous loading — event-driven via cloud notifications |
| **VARIANT** | Semi-structured data type — stores JSON/Avro/Parquet natively |

---

## A

| Term | Definition |
|------|-----------|
| **Access Control** | System of granting and revoking privileges on objects to roles. Snowflake uses both DAC (Discretionary Access Control) and RBAC (Role-Based Access Control). |
| **Account** | A Snowflake deployment in a specific cloud region. Each account has a unique URL identifier (org-account format). |
| **AUTO_SUSPEND** | Warehouse parameter defining idle minutes before automatic suspension. Default: 600 seconds (10 minutes). |
| **AUTO_RESUME** | Warehouse parameter enabling automatic startup when a query is submitted. Default: TRUE. |

## B

| Term | Definition |
|------|-----------|
| **Billing** | Snowflake charges for compute (credits), storage (TB/month), and data transfer (cross-region/cloud). |
| **Bulk Loading** | Loading data using COPY INTO via a warehouse. Best for large batch loads. |

## C

| Term | Definition |
|------|-----------|
| **Caching** | Three-tier system: Result Cache (24h, free), Metadata Cache (cloud services), Warehouse Local Disk Cache (SSD). |
| **Clone** | Zero-copy metadata operation creating an independent copy. Supports tables, schemas, databases. No additional storage until modifications. |
| **Cloud Services Layer** | Always-on layer handling authentication, metadata, query parsing, optimization, and access control. Charges only if >10% of daily compute. |
| **Clustering** | Physical organization of data in micro-partitions. Automatic by default; manual CLUSTER BY for large tables (>1TB). |
| **COPY INTO** | Primary command for bulk data loading (into tables) and unloading (into stages). |
| **Credit** | Unit of compute billing. Cost varies by edition and cloud provider. 1 credit ≈ 1 X-Small warehouse per hour. |

## D

| Term | Definition |
|------|-----------|
| **Data Sharing** | Sharing data between accounts without copying. Provider creates SHARE object; consumer creates database FROM SHARE. |
| **Database Replication** | Cross-region/cross-cloud copy of databases for DR or data locality. Supports failover groups. |
| **Dynamic Data Masking** | Column-level security applying masking policies to hide sensitive data based on role context. |

## E

| Term | Definition |
|------|-----------|
| **Edition** | Snowflake service tier: Standard, Enterprise, Business Critical, Virtual Private Snowflake (VPS). Higher tiers add features. |
| **External Stage** | Named stage referencing cloud storage (S3, Azure Blob, GCS). Uses storage integration for authentication. |
| **External Table** | Read-only table referencing data files in external storage. Supports metadata columns. |

## F

| Term | Definition |
|------|-----------|
| **Fail-Safe** | 7-day recovery period after Time Travel expires. Snowflake-only recovery (contact support). Non-configurable. |
| **File Format** | Named object defining parse rules for data files (CSV, JSON, Avro, Parquet, ORC, XML). |
| **FLATTEN** | Table function that produces lateral view of VARIANT, OBJECT, or ARRAY data. Used with LATERAL keyword. |

## G

| Term | Definition |
|------|-----------|
| **GET** | Command to download files from an internal stage to a local directory. |

## I

| Term | Definition |
|------|-----------|
| **Information Schema** | SQL-standard metadata schema in every database. Views are real-time but limited to current database. |
| **Internal Stage** | Stage storing data within Snowflake. Types: User (@~), Table (@%table), Named (@stage_name). |

## L

| Term | Definition |
|------|-----------|
| **Listing** | Marketplace or private listing that makes data products available to consumers. |

## M

| Term | Definition |
|------|-----------|
| **Marketplace** | Snowflake's data marketplace for discovering and accessing third-party and first-party data products. |
| **Materialized View** | Pre-computed view maintained automatically by Snowflake. Enterprise+ feature. Charges for background maintenance. |
| **MERGE** | DML statement combining INSERT, UPDATE, and DELETE in a single atomic operation based on matching conditions. |
| **Micro-Partition** | Immutable, compressed columnar storage unit (50-500MB uncompressed). Contains metadata for pruning. |
| **Multi-Cluster Warehouse** | Enterprise+ feature allowing a warehouse to scale out to multiple clusters for concurrency. Min/max cluster settings. |

## N

| Term | Definition |
|------|-----------|
| **Network Policy** | Account or user-level rule allowing/blocking access based on IP addresses (allow list/block list). |

## O

| Term | Definition |
|------|-----------|
| **Object** | Any entity in Snowflake (database, schema, table, view, warehouse, etc.) that can have privileges granted on it. |

## P

| Term | Definition |
|------|-----------|
| **Pipe** | Object defining a Snowpipe auto-ingest configuration (stage + COPY INTO statement). |
| **Privilege** | Specific permission on an object (SELECT, INSERT, USAGE, OPERATE, etc.). Granted TO roles, not users. |
| **PUT** | Command to upload local files to an internal stage. Automatically compresses with gzip. |
| **Pruning** | Query optimization skipping micro-partitions that cannot contain matching data, based on partition metadata. |

## Q

| Term | Definition |
|------|-----------|
| **Query Profile** | Visual execution plan showing operators, statistics, and potential performance issues for a query. |

## R

| Term | Definition |
|------|-----------|
| **Reader Account** | Snowflake-managed account created by a provider for sharing data with non-Snowflake consumers. Provider pays compute. |
| **Resource Monitor** | Object tracking credit usage with configurable thresholds and actions (notify, suspend, suspend immediately). |
| **Result Cache** | 24-hour cache of query results at the cloud services layer. Returns identical results without warehouse. Invalidated by underlying data changes. |
| **Role** | Named object that holds privileges. Users are granted roles. Roles form hierarchy via GRANT ROLE. |
| **Row Access Policy** | Row-level security filtering rows based on runtime context (current_role, current_user). |

## S

| Term | Definition |
|------|-----------|
| **Scaling Policy** | Multi-cluster warehouse setting: Standard (favors starting clusters) or Economy (favors conserving credits). |
| **Schema** | Logical grouping of objects within a database. Supports managed access schemas for centralized privilege control. |
| **Secure View** | View with definition hidden from non-owners. Query optimizer may be restricted — impacts performance. |
| **Sequence** | Object generating unique, incrementing numbers. Used for surrogate keys. Not guaranteed gap-free. |
| **Share** | Object created by a data provider containing references to shared objects. Consumers create databases FROM SHARE. |
| **Snowpipe** | Serverless continuous ingestion. Triggered by cloud event notifications or REST API calls. Charges per-file overhead. |
| **Stage** | Location for data files (Internal: user/table/named; External: S3/Azure/GCS). Used for loading and unloading. |
| **Storage Integration** | Account-level object storing cloud credentials for external stages. Avoids embedding credentials in SQL. |
| **Stream** | Object tracking DML changes (inserts, updates, deletes) on a table. Enables CDC patterns. Types: Standard, Append-only. |
| **Stored Procedure** | User-defined procedure written in JavaScript, SQL, Python, Java, or Scala. Executes with caller's or owner's rights. |

## T

| Term | Definition |
|------|-----------|
| **Table Type** | Permanent (default, Time Travel + Fail-Safe), Transient (Time Travel, no Fail-Safe), Temporary (session-scoped, no Fail-Safe). |
| **Task** | Scheduled object executing SQL statements on a cron or interval. Supports task trees (DAGs). |
| **Time Travel** | Feature enabling access to historical data within retention period. Default 1 day (Standard), up to 90 days (Enterprise+). |

## U

| Term | Definition |
|------|-----------|
| **UNDROP** | Command to restore dropped tables, schemas, or databases within Time Travel retention period. |
| **UDF** | User-Defined Function returning scalar or tabular results. Supports SQL, JavaScript, Python, Java, Scala. |

## V

| Term | Definition |
|------|-----------|
| **VARIANT** | Semi-structured data type storing JSON, Avro, ORC, Parquet, or XML. Max 16MB compressed per value. |
| **View** | Named SELECT statement. Types: Standard, Secure, Materialized. Does not store data (except materialized). |
| **Virtual Warehouse** | Named compute cluster for query execution. Sizes: X-Small to 6X-Large (each doubles credits/nodes). |

## W

| Term | Definition |
|------|-----------|
| **Warehouse Cache** | SSD-based local disk cache on warehouse nodes. Lost on suspension. Caches raw data from remote storage. |

## Z

| Term | Definition |
|------|-----------|
| **Zero-Copy Clone** | CREATE ... CLONE command creating metadata-only copy. No storage cost until modifications. Supports tables, schemas, databases. |

---

## Quick Reference: Warehouse Sizes

| Size | Credits/Hour | Nodes |
|------|-------------|-------|
| X-Small | 1 | 1 |
| Small | 2 | 2 |
| Medium | 4 | 4 |
| Large | 8 | 8 |
| X-Large | 16 | 16 |
| 2X-Large | 32 | 32 |
| 3X-Large | 64 | 64 |
| 4X-Large | 128 | 128 |
| 5X-Large | 256 | 256 |
| 6X-Large | 512 | 512 |

## Quick Reference: Time Travel & Fail-Safe

| Edition | Max Time Travel | Fail-Safe | Total Protection |
|---------|----------------|-----------|-----------------|
| Standard | 1 day | 7 days | 8 days |
| Enterprise+ | 90 days | 7 days | 97 days |
| Transient/Temporary | 1 day (max) | 0 days | 1 day |

---

<p align="center">
  <a href="./README.md">🏠 Back to Main Study Guide</a>
</p>
