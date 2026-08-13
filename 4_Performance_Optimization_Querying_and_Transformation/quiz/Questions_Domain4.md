# Domain 4: Performance Optimization, Querying, and Transformation — Practice Questions

## Section A: Query Profile & Performance (12 Questions)

### Question 1
A query on a 1TB table scans 95% of micro-partitions despite a WHERE clause filtering on `sale_date`. What is the MOST likely cause?

A) The warehouse is too small  
B) The table lacks a clustering key aligned with the filter column  
C) Result caching is disabled  
D) The query uses a non-deterministic function  

**Answer: B)**  
**Explanation:** When a query scans far more micro-partitions than the filter selectivity warrants, the data is not physically organized (clustered) by the filter column. Defining a clustering key on `sale_date` would allow natural partition pruning. Warehouse size affects processing speed, not pruning. Result caching and non-deterministic functions are unrelated to partition scan ratios.

---

### Question 2
In the Query Profile, which metric indicates that a query ran out of memory and used disk?

A) Bytes Sent Over Network  
B) Bytes Spilled to Local Storage  
C) Percentage Scanned from Cache  
D) Partitions Scanned  

**Answer: B)**  
**Explanation:** "Bytes Spilled to Local Storage" (and "Bytes Spilled to Remote Storage") indicates the query exceeded available memory and had to write intermediate results to disk. This is a key performance indicator suggesting the warehouse may need to be scaled up. Network bytes relate to result delivery, cache percentage shows data reuse, and partitions scanned relates to pruning efficiency.

---

### Question 3
A data engineer scales a warehouse from MEDIUM to LARGE. Which statement is TRUE about the impact on a single running query?

A) The query will use double the compute resources immediately  
B) The change applies only to the next query submitted  
C) Running queries are cancelled and restarted  
D) The warehouse cannot be resized while a query is running  

**Answer: B)**  
**Explanation:** Warehouse resizing in Snowflake takes effect for subsequent queries, not currently executing ones. Running queries continue with their originally allocated resources. Warehouses CAN be resized while queries are running — the resize queues for the next statement.

---

### Question 4
Which Query Profile operator typically consumes the most resources when joining a small table (1000 rows) to a large table (1 billion rows)?

A) TableScan on the small table  
B) Hash Join building the probe side from the large table  
C) Sort operator for ORDER BY  
D) The join should automatically use a broadcast join with minimal overhead  

**Answer: D)**  
**Explanation:** When one side of a join is very small, Snowflake's optimizer uses a broadcast join — replicating the small table to all processing nodes. This is highly efficient. The large table becomes the probe side (streamed through), and the small table is the build side (broadcast). No expensive sort or large hash table is needed.

---

### Question 5
A user runs the exact same SELECT query twice within 24 hours. The second execution returns in under 100ms. What mechanism is responsible?

A) Materialized view auto-refresh  
B) Result cache  
C) Warehouse local disk cache  
D) Metadata-based optimization  

**Answer: B)**  
**Explanation:** Snowflake's result cache stores query results for 24 hours. If the same query is re-executed by the same role, the underlying data hasn't changed, and the query is identical, results are returned from cache without using any warehouse compute. This explains sub-100ms response times. Warehouse cache speeds up but doesn't eliminate compute; metadata optimization only applies to simple operations like COUNT(*).

---

### Question 6
Which operation can be answered from metadata alone WITHOUT scanning any micro-partitions?

A) SELECT COUNT(*) FROM large_table  
B) SELECT COUNT(DISTINCT column) FROM large_table  
C) SELECT MIN(column) FROM large_table WHERE status = 'active'  
D) SELECT AVG(column) FROM large_table  

**Answer: A)**  
**Explanation:** Snowflake maintains row count metadata at the micro-partition level. A simple `COUNT(*)` with no filter can be answered by summing partition-level row counts — zero partitions scanned. COUNT(DISTINCT), filtered MIN, and AVG all require reading actual data from partitions.

---

### Question 7
A query joins three large tables and the Query Profile shows "Exploding Joins" with output rows far exceeding input rows. What is the MOST likely issue?

A) Missing WHERE clause  
B) Cartesian product due to missing or incorrect JOIN condition  
C) Using LEFT JOIN instead of INNER JOIN  
D) Tables are not clustered  

**Answer: B)**  
**Explanation:** "Exploding Joins" in the Query Profile indicates the join produced far more rows than expected — classic sign of a missing join condition (causing a Cartesian product) or a join condition that isn't selective enough. LEFT vs INNER affects NULL preservation but shouldn't explode row counts. Clustering affects pruning, not join correctness.

---

### Question 8
What is the PRIMARY purpose of Snowflake's query pruning?

A) To remove duplicate rows from results  
B) To skip micro-partitions that cannot contain qualifying rows  
C) To eliminate unused columns from the scan  
D) To cache frequently accessed data  

**Answer: B)**  
**Explanation:** Pruning uses metadata (min/max values, distinct counts) stored for each micro-partition to skip partitions that provably cannot contain rows matching the query's filter predicates. Column elimination is a separate optimization (columnar storage). Deduplication and caching are unrelated concepts.

---

### Question 9
A warehouse is configured with `MAX_CLUSTER_COUNT = 5` and `MIN_CLUSTER_COUNT = 1` using AUTO scaling policy. Under what condition does Snowflake add a cluster?

A) When a single query requires more memory  
B) When queries begin queueing due to insufficient resources  
C) When the warehouse has been running for over 1 hour  
D) When storage consumption exceeds a threshold  

**Answer: B)**  
**Explanation:** Multi-cluster warehouses in AUTO mode add clusters when queries start queueing — indicating concurrency demand exceeds current capacity. Scaling OUT (adding clusters) handles concurrent users; scaling UP (larger size) handles single-query memory/compute needs. Storage and runtime duration don't trigger scaling.

---

### Question 10
Which Query Profile indicator suggests that increasing warehouse size would MOST likely improve performance?

A) High Percentage Scanned from Cache  
B) Bytes Spilled to Remote Storage  
C) Low Partitions Scanned relative to Total  
D) High Rows Produced by TableScan  

**Answer: B)**  
**Explanation:** Spilling to remote storage (S3/Azure Blob) is the strongest signal that the warehouse needs more memory (scale UP). Local spilling is moderate concern; remote spilling is severe. High cache percentage means good reuse (no issue), low partitions scanned means good pruning (no issue), and high rows produced is just the nature of the data volume.

---

### Question 11
A query filtering on `WHERE status IN ('active', 'pending')` on a table clustered by `created_date` scans 100% of partitions. Why?

A) IN clauses bypass clustering  
B) The filter column doesn't match the clustering key  
C) The table needs to be re-clustered  
D) Status values are evenly distributed across all partitions  

**Answer: B)**  
**Explanation:** The table is clustered by `created_date`, but the filter is on `status`. Clustering only helps prune partitions when the filter column matches (or overlaps with) the clustering key. Since `status` values are spread across all date-ordered partitions, no pruning occurs. This is not about re-clustering — the clustering key itself is mismatched for this query pattern.

---

### Question 12
What is the MAXIMUM result cache retention period in Snowflake?

A) 1 hour  
B) 24 hours  
C) 7 days  
D) 31 days  

**Answer: B)**  
**Explanation:** Snowflake's persisted query result cache retains results for 24 hours. If the same query is run again within 24 hours (same SQL text, same role, unchanged underlying data), results are served from cache. After 24 hours, the cache entry expires regardless of whether data changed. This can be extended to 31 days if the result is accessed within each 24-hour window (rolling expiry), but the base retention is 24 hours.

---

## Section B: DML, Streams, and Tasks (13 Questions)

### Question 13
A stream on table `source_data` shows `STALE_AFTER` as yesterday's date. What does this mean?

A) The stream has been consumed and is empty  
B) The stream's unconsumed changes may be lost because the underlying data's Time Travel has expired  
C) The stream was created yesterday  
D) The source table was modified yesterday  

**Answer: B)**  
**Explanation:** A stale stream means the stream's offset points to data that has passed beyond the table's Time Travel retention period. The change records may no longer be accessible. This is critical — if a stream goes stale, the changes are unrecoverable from that stream. The stream must be recreated and an alternative recovery method (full reload) used.

---

### Question 14
Which type of stream captures INSERT, UPDATE, and DELETE operations?

A) Append-only stream  
B) Insert-only stream  
C) Standard stream  
D) Delta stream  

**Answer: C)**  
**Explanation:** Standard streams (the default type) capture all three DML types: INSERT, UPDATE, DELETE. They use `METADATA$ACTION` (INSERT/DELETE) and `METADATA$ISUPDATE` (TRUE for updates) columns. Append-only streams capture only INSERTs, making them suitable for append-only staging tables. "Insert-only" and "Delta" are not valid Snowflake stream types.

---

### Question 15
What happens to a task's schedule when it is created?

A) It begins executing immediately  
B) It is created in a suspended state and must be explicitly resumed  
C) It runs once immediately then follows the schedule  
D) It waits for the next aligned schedule interval  

**Answer: B)**  
**Explanation:** Tasks are ALWAYS created in a suspended state. You must execute `ALTER TASK ... RESUME` to activate them. This is a safety mechanism preventing accidental immediate execution. This is a commonly tested fact — tasks require explicit activation.

---

### Question 16
In a task tree (DAG), which statement is TRUE?

A) Child tasks can be resumed before the root task  
B) The root task must be resumed last, after all child tasks  
C) All tasks in the DAG must be resumed simultaneously  
D) Child tasks must be resumed first, then the root task is resumed last  

**Answer: D)**  
**Explanation:** In a task DAG, you must resume child tasks BEFORE resuming the root task. The root task triggers the DAG execution — if you resume it before children are ready, children won't execute. Conversely, when suspending, suspend the ROOT first, then children. Think: "resume bottom-up, suspend top-down."

---

### Question 17
A MERGE statement is executed with a source that contains duplicate keys matching the same target row. What happens?

A) Both source rows update the target (last writer wins)  
B) The statement fails with a non-deterministic error  
C) Only the first matching source row is applied  
D) Snowflake deduplicates automatically  

**Answer: B)**  
**Explanation:** If multiple source rows match the same target row, the MERGE behavior is non-deterministic and Snowflake raises an error. This is by design — MERGE requires a 1:1 (or many:1 source-to-target) relationship. You must deduplicate the source (using ROW_NUMBER or QUALIFY) before the MERGE.

---

### Question 18
What does `SYSTEM$STREAM_HAS_DATA('my_stream')` return when used in a task's WHEN clause?

A) The number of rows in the stream  
B) TRUE if the stream contains change records, FALSE otherwise  
C) The timestamp of the last change  
D) The stream's offset position  

**Answer: B)**  
**Explanation:** `SYSTEM$STREAM_HAS_DATA()` returns a Boolean — TRUE if there are unconsumed change records, FALSE if the stream is empty. When used in a task's WHEN clause, it prevents the task from starting (and consuming credits) when there's nothing to process. It does NOT return counts or timestamps.

---

### Question 19
Which statement about Snowflake stream offsets is TRUE?

A) The offset advances when the stream is queried with SELECT  
B) The offset advances when the stream's data is consumed in a committed DML transaction  
C) The offset advances automatically every 24 hours  
D) The offset must be manually advanced using ALTER STREAM  

**Answer: B)**  
**Explanation:** A stream's offset advances ONLY when its data is consumed within a DML statement (INSERT, MERGE, etc.) that successfully commits. Simply querying (SELECT) a stream does NOT advance the offset — this allows you to preview changes without consuming them. There is no automatic advancement or manual ALTER command for offsets.

---

### Question 20
A task with `SCHEDULE = 'USING CRON 0 2 * * * America/New_York'` runs at what time?

A) Every 2 minutes  
B) 2:00 AM New York time daily  
C) Every 2 hours  
D) On the 2nd of every month  

**Answer: B)**  
**Explanation:** The CRON expression `0 2 * * *` means: minute 0, hour 2, any day of month, any month, any day of week = 2:00 AM daily. The timezone `America/New_York` means this is Eastern Time. CRON format: minute, hour, day-of-month, month, day-of-week.

---

### Question 21
After a MERGE statement consuming a stream completes successfully, what is the state of the stream?

A) The stream is dropped automatically  
B) The stream is empty (offset advanced past consumed records)  
C) The stream retains all historical records  
D) The stream enters a "consumed" error state  

**Answer: B)**  
**Explanation:** After successful DML consumption, the stream's offset advances to the current table version. The consumed records are no longer visible in the stream — it appears empty until new changes occur. The stream object persists (not dropped); it simply has no pending change records.

---

### Question 22
What privilege is required to create a task?

A) CREATE TASK on the schema  
B) EXECUTE TASK on the account  
C) Both CREATE TASK on the schema AND EXECUTE TASK on the account  
D) OWNERSHIP of the warehouse  

**Answer: C)**  
**Explanation:** Creating a task requires `CREATE TASK` privilege on the schema. However, to actually RESUME (activate) the task, the role also needs `EXECUTE TASK` (account-level privilege) or `EXECUTE MANAGED TASK` (for serverless tasks). The exam often tests that both are needed for a task to actually run.

---

### Question 23
Which DML operation is NOT supported directly in Snowflake?

A) MERGE with multiple WHEN MATCHED clauses  
B) INSERT with VALUES for multiple rows  
C) UPDATE with JOIN syntax  
D) DELETE with a subquery in WHERE  

**Answer: C)**  
**Explanation:** Snowflake does NOT support `UPDATE ... FROM ... JOIN` syntax (common in SQL Server/PostgreSQL). Instead, you must use a subquery or MERGE. All other options are supported: MERGE supports multiple WHEN MATCHED clauses, INSERT supports multi-row VALUES, and DELETE supports subqueries in WHERE.

---

### Question 24
A serverless task is created without specifying a warehouse. How is compute provisioned?

A) It uses the account's default warehouse  
B) Snowflake automatically provisions and manages compute resources  
C) The task fails without a warehouse specification  
D) It runs on the smallest XS warehouse by default  

**Answer: B)**  
**Explanation:** Serverless tasks (no WAREHOUSE specified, uses `USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE` parameter) have Snowflake manage the compute. Snowflake provisions resources automatically, scaling based on workload. This eliminates warehouse management overhead but costs are billed differently (per-second Snowflake-managed compute).

---

### Question 25
What is the default behavior of a task when its predecessor task fails?

A) The dependent task runs anyway with NULL inputs  
B) The dependent task is skipped for that run  
C) The entire DAG is suspended  
D) The dependent task retries the predecessor  

**Answer: B)**  
**Explanation:** In a task DAG, if a predecessor fails, dependent (child) tasks are skipped for that execution cycle. The DAG is NOT permanently suspended — it will attempt the full tree again on the next scheduled run. Individual task failure doesn't cascade-suspend the DAG; it just skips the downstream branch for that run.

---

## Section C: Semi-Structured Data (12 Questions)

### Question 26
What data type should be used to store JSON documents in Snowflake?

A) JSON  
B) VARCHAR  
C) VARIANT  
D) OBJECT  

**Answer: C)**  
**Explanation:** VARIANT is Snowflake's semi-structured data type that stores JSON, Avro, ORC, Parquet, and XML data. While OBJECT and ARRAY are sub-types, VARIANT is the column type used for storage. There is no JSON data type in Snowflake. VARCHAR could store JSON as text but loses all semi-structured query capabilities.

---

### Question 27
Given a VARIANT column `data` containing `{"name": "Alice", "age": 30}`, which syntax correctly extracts the name?

A) `data['name']::STRING`  
B) `data:name::STRING`  
C) Both A and B are correct  
D) `GET(data, 'name')`  

**Answer: C)**  
**Explanation:** Both bracket notation (`data['name']`) and colon/dot notation (`data:name`) are valid for accessing VARIANT fields. The `::STRING` cast is needed to convert from VARIANT to a native type. `GET()` function also works but isn't listed with proper return type casting in option D.

---

### Question 28
What does LATERAL FLATTEN do that regular FLATTEN cannot?

A) LATERAL FLATTEN handles nested arrays  
B) LATERAL FLATTEN correlates with columns from the outer table row  
C) LATERAL FLATTEN is faster  
D) There is no difference; LATERAL is implicit in Snowflake  

**Answer: D)**  
**Explanation:** In Snowflake, FLATTEN implicitly performs a lateral join — LATERAL is syntactically optional but semantically always present. Both `FROM table, FLATTEN(...)` and `FROM table, LATERAL FLATTEN(...)` produce identical results. The LATERAL keyword is accepted for ANSI SQL clarity but doesn't change behavior.

---

### Question 29
A FLATTEN operation on a NULL array value returns zero rows. How do you preserve the parent row?

A) Use COALESCE on the array  
B) Use `OUTER => TRUE` in the FLATTEN function  
C) Use LEFT JOIN with FLATTEN  
D) Replace NULL arrays with empty arrays before FLATTEN  

**Answer: B)**  
**Explanation:** `FLATTEN(input => col, OUTER => TRUE)` preserves the outer row even when the input is NULL or an empty array — similar to a LEFT JOIN. Without OUTER => TRUE (default is FALSE), rows with NULL/empty arrays are dropped from the result. This is a frequently tested FLATTEN parameter.

---

### Question 30
Which function converts a relational result set into a JSON array?

A) TO_JSON()  
B) ARRAY_AGG()  
C) PARSE_JSON()  
D) OBJECT_CONSTRUCT()  

**Answer: B)**  
**Explanation:** `ARRAY_AGG()` aggregates multiple rows into an ARRAY (which renders as a JSON array). `OBJECT_CONSTRUCT()` creates a single JSON object from key-value pairs. `TO_JSON()` serializes a VARIANT to a JSON string. `PARSE_JSON()` does the reverse — parses a JSON string into VARIANT.

---

### Question 31
What is the maximum size of a single VARIANT value in Snowflake?

A) 8 MB  
B) 16 MB  
C) 64 MB  
D) Unlimited  

**Answer: B)**  
**Explanation:** A single VARIANT value (one cell/field) can be at most 16 MB compressed. This means a single JSON document stored in a VARIANT column cannot exceed 16 MB. For larger documents, they must be split across multiple rows or columns. This is a hard limit tested on the exam.

---

### Question 32
Which function extracts all keys from a JSON object stored in a VARIANT column?

A) GET_KEYS()  
B) OBJECT_KEYS()  
C) JSON_KEYS()  
D) VARIANT_KEYS()  

**Answer: B)**  
**Explanation:** `OBJECT_KEYS()` returns an array of all top-level keys in a VARIANT object. There is no `GET_KEYS()`, `JSON_KEYS()`, or `VARIANT_KEYS()` function in Snowflake. You can then FLATTEN the result of OBJECT_KEYS() to work with individual keys.

---

### Question 33
When querying a VARIANT column, what is the default type of an extracted value?

A) STRING  
B) VARIANT  
C) The original JSON type (number, string, boolean)  
D) NULL  

**Answer: B)**  
**Explanation:** Extracting a field from a VARIANT always returns VARIANT, regardless of the underlying JSON type. You must explicitly cast (`::STRING`, `::NUMBER`, `::BOOLEAN`) to convert to native Snowflake types. This is why comparisons without casting can produce unexpected results — VARIANT comparison semantics differ from native types.

---

### Question 34
How does Snowflake handle semi-structured data internally for performance?

A) Stores it as plain text with a JSON index  
B) Automatically flattens it into columnar storage when possible  
C) Compresses it using a special JSON codec  
D) Converts it to XML for internal representation  

**Answer: B)**  
**Explanation:** Snowflake uses "columnarization" — automatically detecting repeated structures in semi-structured data and storing frequently accessed paths in columnar format (like regular columns). This gives near-native performance for common access patterns without manual schema definition. Only truly unstructured or rarely-accessed paths remain in the generic VARIANT representation.

---

### Question 35
Given nested JSON: `{"orders": [{"id": 1, "items": [{"sku": "A"}, {"sku": "B"}]}]}`, how many FLATTEN operations are needed to get one row per SKU?

A) 1 — FLATTEN on items  
B) 2 — FLATTEN on orders, then FLATTEN on items  
C) 3 — FLATTEN on root, orders, then items  
D) 0 — direct path navigation is sufficient  

**Answer: B)**  
**Explanation:** Two FLATTENs are needed: first on `data:orders` to get one row per order, then on `value:items` to get one row per item within each order. The root object doesn't need flattening (it's a single object). Path navigation (`data:orders[0].items[0].sku`) only gets one specific element, not all rows.

---

### Question 36
What is the correct syntax to cast a VARIANT value to a Snowflake TIMESTAMP?

A) `data:event_time::TIMESTAMP`  
B) `CAST(data:event_time AS TIMESTAMP)`  
C) `TO_TIMESTAMP(data:event_time)`  
D) All of the above  

**Answer: D)**  
**Explanation:** All three syntaxes are valid for converting a VARIANT value to TIMESTAMP. The `::` cast notation, `CAST()` function, and `TO_TIMESTAMP()` conversion function all produce equivalent results. The exam may present any of these as the "correct" answer — recognizing all are valid is key.

---

### Question 37
Which file format does NOT require the VARIANT data type for loading into Snowflake?

A) JSON  
B) Parquet  
C) Avro  
D) CSV  

**Answer: D)**  
**Explanation:** CSV files are structured/tabular and load directly into typed columns (STRING, NUMBER, DATE, etc.) without needing VARIANT. JSON, Parquet, and Avro can be loaded into VARIANT columns to preserve their semi-structured nature, although Parquet and Avro can also be loaded into structured tables with schema-on-read or explicit column mapping.

---

## Section D: Time Travel & Data Recovery (13 Questions)

### Question 38
What is the maximum Time Travel retention period on Snowflake Enterprise Edition?

A) 1 day  
B) 7 days  
C) 90 days  
D) 365 days  

**Answer: C)**  
**Explanation:** Enterprise Edition (and higher) supports up to 90 days of Time Travel. Standard Edition is limited to 0 or 1 day. The parameter `DATA_RETENTION_TIME_IN_DAYS` controls this at the account, database, schema, or table level. 90 days is the maximum regardless of edition.

---

### Question 39
Which statement correctly queries a table as it existed 5 minutes ago?

A) `SELECT * FROM my_table TIME_TRAVEL(5 MINUTES)`  
B) `SELECT * FROM my_table AT(OFFSET => -300)`  
C) `SELECT * FROM my_table BEFORE(MINUTES => 5)`  
D) `SELECT * FROM my_table AT(TIMESTAMP => DATEADD('minute', -5, CURRENT_TIMESTAMP()))`  

**Answer: B)**  
**Explanation:** `AT(OFFSET => -300)` uses a negative offset in SECONDS (5 minutes = 300 seconds) relative to the current time. Option D is also valid syntax but the offset approach is more direct for "N minutes ago" scenarios. Option A and C use invalid syntax. Note: both B and D work, but B is the most precise match for the question's "5 minutes ago" phrasing using offset semantics.

---

### Question 40
A table is dropped and a new table with the same name is created. Can the original table be recovered?

A) No — the new table overwrites all history  
B) Yes — using UNDROP TABLE with the original table's ID  
C) Yes — by first dropping or renaming the new table, then running UNDROP TABLE  
D) Yes — using Time Travel AT(TIMESTAMP) on the new table  

**Answer: C)**  
**Explanation:** UNDROP TABLE restores the most recently dropped table with that name. If a new table with the same name exists, you must first rename or drop it, then UNDROP restores the original. You cannot have two tables with the same name — UNDROP would conflict. Time Travel on the NEW table has no history from the OLD table.

---

### Question 41
What is the Fail-safe period and who can access it?

A) 7 days; accessible by any ACCOUNTADMIN  
B) 7 days; accessible only by Snowflake Support  
C) 90 days; accessible by any ACCOUNTADMIN  
D) 7 days; accessible by the table owner  

**Answer: B)**  
**Explanation:** Fail-safe provides a 7-day window AFTER Time Travel expires, but it is accessible ONLY by Snowflake Support — not by customers directly. It's a last-resort disaster recovery mechanism, not a self-service feature. You must contact Snowflake Support and they may charge for recovery. Transient and temporary tables have NO Fail-safe.

---

### Question 42
A developer ran `DELETE FROM orders WHERE region = 'EMEA'` by mistake. The query ID is `01abc-def`. How do you recover the deleted rows?

A) `UNDROP TABLE orders`  
B) `INSERT INTO orders SELECT * FROM orders BEFORE(STATEMENT => '01abc-def')`  
C) `ALTER TABLE orders UNDO LAST STATEMENT`  
D) `ROLLBACK`  

**Answer: B)**  
**Explanation:** `BEFORE(STATEMENT => '<query_id>')` returns the table state immediately before that statement executed. You INSERT the difference back. UNDROP is for dropped tables, not deleted rows. There's no `UNDO LAST STATEMENT` command. ROLLBACK only works within an uncommitted transaction — once committed, only Time Travel can recover data.

---

### Question 43
Setting `DATA_RETENTION_TIME_IN_DAYS = 0` on a table has what effect?

A) The table is immediately dropped  
B) Time Travel is disabled; historical data is immediately purged  
C) The table becomes read-only  
D) Only the Fail-safe period is removed  

**Answer: B)**  
**Explanation:** Setting retention to 0 disables Time Travel for that table. No historical versions are retained, and you cannot query past states or recover from accidental changes. Fail-safe is separate (still applies to permanent tables). The table is not dropped or made read-only.

---

### Question 44
Which objects support Time Travel? (Select the BEST answer)

A) Tables, schemas, and databases only  
B) Tables, views, and stages  
C) All database objects  
D) Tables, schemas, databases, and their contained objects  

**Answer: D)**  
**Explanation:** Time Travel applies to tables (including their data), and by extension to schemas and databases (UNDROP SCHEMA/DATABASE). When you UNDROP a schema, all its tables are restored. Views, stages, and other objects don't have Time Travel independently — but they're restored when their parent container is undropped. The exam focuses on tables, schemas, and databases as the primary Time Travel targets.

---

### Question 45
A cloned table initially shares storage with the source. When does the clone consume additional storage?

A) Immediately upon creation  
B) When the clone is queried  
C) When data in either the source or clone is modified  
D) After 24 hours  

**Answer: C)**  
**Explanation:** Snowflake clones use zero-copy (metadata-only) — no additional storage at creation time. Storage diverges only when DML modifies data in either the source OR the clone, creating new micro-partitions that aren't shared. Reading (querying) doesn't cause divergence. This is a key cost optimization concept.

---

### Question 46
`AT` vs `BEFORE` in Time Travel — what is the difference?

A) AT includes changes made at that point; BEFORE excludes them  
B) AT is for timestamps only; BEFORE is for query IDs only  
C) They are identical and interchangeable  
D) AT goes forward in time; BEFORE goes backward  

**Answer: A)**  
**Explanation:** `AT(STATEMENT => 'id')` returns the state including the changes made by that statement. `BEFORE(STATEMENT => 'id')` returns the state just BEFORE that statement executed (excluding its changes). For recovery, BEFORE is typically what you want — the state before the mistake. Both work with timestamps and statement IDs.

---

### Question 47
What happens when Time Travel is queried for a point beyond the retention period?

A) Returns NULL for all rows  
B) Returns the oldest available snapshot  
C) Fails with an error  
D) Returns current data with a warning  

**Answer: C)**  
**Explanation:** Querying beyond the Time Travel retention period produces an error: "insufficient data retention period." Snowflake does NOT silently return stale or current data — it fails explicitly. This protects against accidentally using outdated data. The Fail-safe period that follows is not accessible via normal Time Travel queries.

---

### Question 48
A table with `DATA_RETENTION_TIME_IN_DAYS = 90` is dropped. For how long can it be recovered using UNDROP?

A) 90 days  
B) 97 days (90 + 7 Fail-safe)  
C) 1 day  
D) Until the Time Travel retention period expires  

**Answer: A)**  
**Explanation:** A dropped table can be undone with UNDROP TABLE within the Time Travel retention period (90 days in this case). After that window, the data enters Fail-safe (7 additional days, Snowflake Support only). UNDROP is a self-service operation available only during the Time Travel window.

---

### Question 49
Which statement about Time Travel storage costs is TRUE?

A) Time Travel storage is free for the first 30 days  
B) Time Travel storage is charged at the same rate as active storage  
C) Time Travel storage costs are included in compute credits  
D) Time Travel storage is only charged for Enterprise Edition  

**Answer: B)**  
**Explanation:** Time Travel storage (historical micro-partitions retained for recovery) is billed at the same rate as active table storage. There is no free tier. This is why setting appropriate retention periods matters for cost management — 90 days of retention on a frequently-modified table can significantly increase storage costs.

---

### Question 50
A database is dropped and then undropped. What happens to the tables inside it?

A) Only the database container is restored; tables must be individually undropped  
B) All tables and their data are restored to their state at drop time  
C) Tables are restored but streams and tasks are lost  
D) Only tables with Time Travel enabled are restored  

**Answer: B)**  
**Explanation:** UNDROP DATABASE restores the database AND all its contained objects (schemas, tables, views, etc.) to their state at the time of the drop. It's an atomic operation — you don't need to individually recover each table. Streams and tasks within the database are also restored.

---

## Bonus Questions (Mixed Topics)

### Question 51
Which function is used to generate a unique identifier suitable for surrogate keys without a sequence?

A) UUID_STRING()  
B) RANDOM()  
C) SEQ1()  
D) UNIQUE_ID()  

**Answer: A)**  
**Explanation:** `UUID_STRING()` generates a 128-bit universally unique identifier as a string. It's suitable for surrogate keys when integer sequences aren't required. `RANDOM()` generates random numbers (not guaranteed unique). `SEQ1()` and similar are sequence-related but used in table generation contexts. `UNIQUE_ID()` doesn't exist.

---

### Question 52
What is the key difference between `COPY INTO` and `INSERT INTO ... SELECT`?

A) COPY INTO supports file formats; INSERT INTO only works with existing tables  
B) COPY INTO tracks loaded files to prevent duplicates; INSERT INTO does not  
C) INSERT INTO is faster for bulk operations  
D) COPY INTO cannot be used with transformations  

**Answer: B)**  
**Explanation:** `COPY INTO` maintains load metadata (which files have been loaded) preventing duplicate loading of the same file. `INSERT INTO ... SELECT` has no such tracking. COPY INTO also supports file format specifications and stage references. However, COPY INTO does support transformations (column reordering, casting, etc.) in its SELECT clause — option D is incorrect.

---

### Question 53
A query uses `QUALIFY ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) = 1`. What does this accomplish?

A) Numbers all rows sequentially  
B) Returns only the most recent order per customer  
C) Removes duplicate customers  
D) Counts orders per customer  

**Answer: B)**  
**Explanation:** QUALIFY filters window function results (like HAVING for GROUP BY). `ROW_NUMBER() PARTITION BY customer_id ORDER BY order_date DESC` assigns 1 to the most recent order per customer. `= 1` keeps only that row. This is Snowflake's elegant deduplication pattern — more efficient than correlated subqueries.

---

### Question 54
Which caching layer in Snowflake persists even when the warehouse is suspended?

A) Warehouse local disk cache  
B) Result cache  
C) Metadata cache  
D) Both B and C  

**Answer: D)**  
**Explanation:** The result cache (Cloud Services layer) and metadata cache persist regardless of warehouse state — they're stored in the Cloud Services layer, not on warehouse nodes. The warehouse local disk cache (SSD) is lost when the warehouse suspends. This is why identical queries can return instantly even from a suspended warehouse — the result cache is checked before the warehouse is even resumed.

---

### Question 55
What is a Dynamic Table's primary advantage over a Stream + Task combination?

A) It's faster  
B) Declarative definition — you define WHAT, Snowflake manages WHEN and HOW to refresh  
C) It costs less  
D) It supports more complex transformations  

**Answer: B)**  
**Explanation:** Dynamic Tables are declarative — you write a SELECT defining the desired state, and Snowflake automatically determines when and how to incrementally refresh it (based on `TARGET_LAG`). Streams + Tasks are imperative — you must design the CDC logic, schedule, and error handling yourself. Dynamic Tables simplify pipeline development at the cost of some control.

---

## Bonus: Advanced Scenario Questions

### Question 1
A data engineer examines the Query Profile for a slow query and sees: "Bytes Spilled to Local Storage: 12GB" and "Bytes Spilled to Remote Storage: 45GB." The warehouse is MEDIUM. What is the BEST remediation?
- A) Add a clustering key to the target table
- B) Scale UP the warehouse to LARGE or XLARGE to increase memory
- C) Scale OUT by adding more clusters
- D) Rewrite the query to use a subquery instead of a CTE

**Answer: B) Scale UP the warehouse to LARGE or XLARGE to increase memory**
**Explanation:** Remote spilling indicates severe memory pressure — the query exhausted both memory and local SSD, spilling to remote cloud storage (S3/Blob). Scaling UP adds more memory per node. Scaling OUT (multi-cluster) helps concurrency, not single-query memory. Clustering helps pruning, not memory.
**Exam Trap:** Local spilling is moderate concern; REMOTE spilling is severe — always prioritize fixing remote spills by scaling up.
---

### Question 2
A stream on table `orders` has DATA_RETENTION_TIME_IN_DAYS = 1 on the source table. The stream hasn't been consumed in 36 hours. A task attempts to read from the stream. What happens?
- A) The task reads empty results because the stream auto-clears after 24 hours
- B) The stream is stale — the query fails with an error indicating the stream cannot access historical data
- C) The task reads only changes from the last 24 hours, losing earlier changes
- D) The stream remains valid because stream offsets are independent of Time Travel

**Answer: B) The stream is stale — the query fails with an error indicating the stream cannot access historical data**
**Explanation:** A stream's offset requires access to the table's historical data via Time Travel. If DATA_RETENTION_TIME_IN_DAYS = 1 and the stream hasn't been consumed in 36 hours, the offset points beyond the 1-day retention — the stream is stale and unusable. You must recreate the stream and perform a full reconciliation.
**Exam Trap:** Stream staleness is tied to the SOURCE TABLE's Time Travel retention, not the stream's creation date — always ensure retention exceeds your maximum consumption interval.
---

### Question 3
A MERGE statement targets `dim_customer` from staging table `stg_customer`. The staging table has two rows with customer_id = 'C001' — one from 9:00 AM and one from 9:05 AM. The MERGE matches on customer_id. What occurs?
- A) The 9:05 AM record wins (last write wins)
- B) The MERGE fails with a non-deterministic error
- C) Both rows are applied sequentially
- D) Only the 9:00 AM record is applied (first match wins)

**Answer: B) The MERGE fails with a non-deterministic error**
**Explanation:** When multiple source rows match the same target row, Snowflake cannot determine which update to apply and raises an error: "Join in MERGE produces nondeterministic results." Deduplicate the source first using QUALIFY ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY timestamp DESC) = 1.
**Exam Trap:** The fix is to deduplicate BEFORE the MERGE, not to add ORDER BY to the MERGE itself (which isn't supported).
---

### Question 4
A developer wants to recover a table to its state just BEFORE an accidental UPDATE (query ID: '01abc123'). They write: `CREATE TABLE recovered AS SELECT * FROM my_table AT(STATEMENT => '01abc123')`. Is this correct?
- A) Yes — AT includes the state after the statement, which is what they want
- B) No — they should use BEFORE(STATEMENT => '01abc123') to get the state before the UPDATE
- C) No — AT(STATEMENT) is not valid; only AT(TIMESTAMP) works
- D) Yes — but they should use CLONE instead of CREATE TABLE AS SELECT

**Answer: B) No — they should use BEFORE(STATEMENT => '01abc123') to get the state before the UPDATE**
**Explanation:** AT(STATEMENT => id) returns the table state AFTER that statement executed (including its changes). BEFORE(STATEMENT => id) returns the state immediately BEFORE the statement — which is what's needed for recovery. Using AT would give them the already-corrupted state.
**Exam Trap:** AT = inclusive (after the statement); BEFORE = exclusive (before the statement). For recovery from a bad DML, always use BEFORE.
---

### Question 5
A table contains JSON arrays nested two levels deep: `{"departments": [{"name": "Sales", "employees": [{"id": 1}, {"id": 2}]}]}`. A query uses a single FLATTEN on `data:departments`. How many rows does it produce per source row, and can employee IDs be accessed?
- A) One row per department; employee IDs require a second FLATTEN on value:employees
- B) One row per employee; single FLATTEN auto-expands nested arrays
- C) One row per source row; FLATTEN on an object key returns the object, not array elements
- D) An error — you must specify the path to the inner array directly

**Answer: A) One row per department; employee IDs require a second FLATTEN on value:employees**
**Explanation:** FLATTEN only expands one level of array/object. The first FLATTEN on `data:departments` produces one row per department. To get individual employees, chain a second FLATTEN: `FLATTEN(value:employees)`. Each FLATTEN expands exactly one array level.
**Exam Trap:** FLATTEN is NOT recursive — nested arrays always require multiple FLATTEN operations chained together.
---

### Question 6
A developer clones a production table: `CREATE TABLE dev_copy CLONE prod_table`. They then INSERT 1 million rows into dev_copy and DELETE 500K rows from prod_table. Which statement about storage is TRUE?
- A) dev_copy and prod_table share all original micro-partitions; new storage is consumed only for the 1M inserted rows in dev_copy and new partitions from the DELETE in prod_table
- B) The clone immediately doubles storage
- C) Only dev_copy consumes new storage; prod_table DELETE doesn't affect shared partitions
- D) Storage is shared until the first query against either table

**Answer: A) dev_copy and prod_table share all original micro-partitions; new storage is consumed only for the 1M inserted rows in dev_copy and new partitions from the DELETE in prod_table**
**Explanation:** Zero-copy cloning shares micro-partitions at creation. New storage is consumed only when DML creates new partitions in EITHER table. The INSERT into dev_copy creates new partitions (billed to dev_copy). The DELETE from prod_table creates new versions (billed to prod_table). Unchanged partitions remain shared.
**Exam Trap:** DML on EITHER the source OR clone triggers storage divergence — not just the clone.
---

### Question 7
A task DAG has: Root → (Task_A, Task_B) → Task_C (depends on both A and B). Task_A completes in 2 minutes. Task_B fails after 5 minutes. What happens to Task_C?
- A) Task_C runs after Task_A completes (doesn't wait for Task_B)
- B) Task_C is skipped for this execution because one of its predecessors failed
- C) Task_C waits for Task_B to retry and complete
- D) The entire DAG is suspended permanently

**Answer: B) Task_C is skipped for this execution because one of its predecessors failed**
**Explanation:** In a task DAG, a child task only executes if ALL its predecessors succeed. If any predecessor fails, the child is skipped for that run. The DAG is not permanently suspended — it will retry the full tree on the next scheduled execution.
**Exam Trap:** "ALL predecessors must succeed" — even if Task_A succeeded, Task_C still requires Task_B to also succeed.
---

### Question 8
A data architect needs a staging table that: (1) holds data temporarily during ETL, (2) minimizes storage costs, (3) doesn't need Fail-safe protection, and (4) requires 3 days of Time Travel. Which table type should they use?
- A) Transient table with DATA_RETENTION_TIME_IN_DAYS = 3
- B) Temporary table with DATA_RETENTION_TIME_IN_DAYS = 3
- C) Permanent table with DATA_RETENTION_TIME_IN_DAYS = 3
- D) Transient table — but it only supports 0 or 1 day of Time Travel

**Answer: D) Transient table — but it only supports 0 or 1 day of Time Travel**
**Explanation:** Transient tables have no Fail-safe (reducing storage cost) but only support 0 or 1 day of Time Travel (not 3 days). The requirement for 3 days of Time Travel cannot be met with a transient table. The architect must choose: permanent table (3 days TT + 7 days Fail-safe) or accept only 1 day TT with transient.
**Exam Trap:** Transient and temporary tables max out at 1 day Time Travel — only permanent tables support up to 90 days.
---

### Question 9
A Query Profile shows a TableScan operator with "Partitions Scanned: 50,000" and "Partitions Total: 50,000" (100% scan). The table has a clustering key on `region`. The query filters on `WHERE region = 'US-EAST' AND order_date > '2024-01-01'`. What explains the full scan?
- A) The clustering key is not effective because `region` has low cardinality — most partitions contain 'US-EAST' rows
- B) The `order_date` filter is overriding the clustering benefit
- C) Clustering keys don't help with equality filters
- D) The query cache is disabled, forcing a full scan

**Answer: A) The clustering key is not effective because `region` has low cardinality — most partitions contain 'US-EAST' rows**
**Explanation:** If `region` has few distinct values (e.g., 5 regions), each region's data is spread across many partitions, or many partitions contain mixed regions. With low cardinality, even a perfectly clustered table may have most partitions containing at least one 'US-EAST' row. A compound clustering key on (region, order_date) would improve pruning.
**Exam Trap:** Low-cardinality columns alone make poor clustering keys — combine with a high-cardinality column for effective pruning.
---

### Question 10
An analyst runs: `SELECT * FROM orders AT(OFFSET => -3600)`. The table had DATA_RETENTION_TIME_IN_DAYS reduced from 7 to 0 yesterday. Today is within the original 7-day window. Can the query succeed?
- A) Yes — existing historical data is retained until the original retention period expires
- B) No — setting retention to 0 immediately purges all historical data
- C) Yes, but only for the next 24 hours
- D) It depends on whether the data has been modified since the retention change

**Answer: B) No — setting retention to 0 immediately purges all historical data**
**Explanation:** When DATA_RETENTION_TIME_IN_DAYS is reduced (e.g., from 7 to 0), historical data beyond the new retention period is eligible for immediate purging. Snowflake may purge it at any time after the change. The query will likely fail with an insufficient retention error.
**Exam Trap:** Reducing retention is destructive — historical data can be purged immediately upon the change, not after the old period expires.
---

### Question 11
A stream captures changes to a table used in a MERGE. The MERGE reads the stream and inserts into a target. If the MERGE transaction fails and rolls back, what happens to the stream offset?
- A) The offset advances anyway — stream consumption is independent of transaction success
- B) The offset does NOT advance — it only advances on successful COMMIT
- C) The offset partially advances for rows that were processed before failure
- D) The stream becomes stale after a rollback

**Answer: B) The offset does NOT advance — it only advances on successful COMMIT**
**Explanation:** Stream offset advancement is transactional. If the consuming DML transaction fails or is rolled back, the stream offset remains at its previous position. The same changes will be available for the next consumption attempt. This ensures exactly-once processing semantics.
**Exam Trap:** Stream offsets are atomic with the transaction — no partial advancement, no advancement on failure.
---

### Question 12
A developer creates a clone of a table that has an active stream: `CREATE TABLE cloned CLONE source_table`. What happens to the stream?
- A) The stream is also cloned and tracks changes to the cloned table
- B) The stream remains on the source table only — it is NOT cloned
- C) The clone fails because tables with streams cannot be cloned
- D) A new stream is automatically created on the cloned table

**Answer: B) The stream remains on the source table only — it is NOT cloned**
**Explanation:** Streams are not included in clone operations. The original stream continues tracking the source table. The cloned table has no stream. If you need change tracking on the clone, create a new stream explicitly on the cloned table.
**Exam Trap:** Cloning copies data/structure but NOT streams, tasks, or pipes associated with the table.
---

### Question 13
A FLATTEN query uses `OUTER => TRUE` on a column that contains: row 1 = [1,2,3], row 2 = NULL, row 3 = []. How many total output rows are produced?
- A) 3 rows (from row 1 only — NULL and empty arrays produce nothing)
- B) 5 rows (3 from row 1, 1 from row 2 with NULLs, 1 from row 3 with NULLs)
- C) 4 rows (3 from row 1, 1 from row 2 with NULLs; empty array still produces nothing)
- D) 3 rows (from row 1; OUTER only affects NULL, not empty arrays)

**Answer: B) 5 rows (3 from row 1, 1 from row 2 with NULLs, 1 from row 3 with NULLs)**
**Explanation:** OUTER => TRUE preserves the parent row when the input is NULL OR an empty array. Row 1 produces 3 rows (one per element). Row 2 (NULL) produces 1 row with NULL flatten columns. Row 3 (empty []) produces 1 row with NULL flatten columns. Total: 5.
**Exam Trap:** OUTER => TRUE covers BOTH NULL inputs AND empty arrays — candidates often forget the empty array case.
---

### Question 14
A zero-copy clone is created from a permanent table. The clone inherits which properties?
- A) Data, structure, clustering keys, grants, and streams
- B) Data, structure, clustering keys, and table-level grants, but NOT streams or tasks
- C) Data and structure only — all other properties must be reconfigured
- D) Data, structure, clustering keys, stage files, and load history

**Answer: B) Data, structure, clustering keys, and table-level grants, but NOT streams or tasks**
**Explanation:** Cloning copies the table data (zero-copy), structure (columns, constraints), clustering keys, and grants on the table itself. It does NOT clone streams, tasks, pipes, or external objects referencing the table. Load history metadata is also NOT cloned.
**Exam Trap:** Grants ARE cloned (unlike streams/tasks) — this is frequently tested and often confused.
---

### Question 15
A task DAG has 5 tasks: Root → A → B → C → D (linear chain). The root task schedule is every 10 minutes. Task A takes 3 min, B takes 4 min, C takes 2 min, D takes 5 min. Total execution exceeds 10 minutes. What happens when the next scheduled run triggers while the current run is still executing?
- A) The new run queues and starts after the current run completes
- B) The new run is skipped entirely
- C) Both runs execute concurrently
- D) The current run is cancelled and the new run starts

**Answer: B) The new run is skipped entirely**
**Explanation:** If a task DAG is still executing when the next scheduled trigger fires, that scheduled execution is skipped. Tasks do not queue or run concurrently within the same DAG. The skipped execution is logged. Adjust the schedule interval to exceed maximum DAG execution time.
**Exam Trap:** Overlapping scheduled executions are SKIPPED, not queued — this can cause data processing gaps if the schedule is too aggressive.
---

### Question 16
A query uses `SELECT * FROM events BEFORE(STATEMENT => '01xyz')` where '01xyz' is a DDL statement (ALTER TABLE ADD COLUMN). Is this valid?
- A) Yes — BEFORE works with any statement ID including DDL
- B) No — BEFORE(STATEMENT) only works with DML statement IDs
- C) Yes, but it only returns data without the new column
- D) No — Time Travel doesn't support DDL statement references

**Answer: A) Yes — BEFORE works with any statement ID including DDL**
**Explanation:** Time Travel's AT/BEFORE clauses work with any statement ID — DML or DDL. BEFORE(STATEMENT => DDL_id) returns the table state before the DDL was applied (e.g., before a column was added or dropped). This is useful for seeing the schema and data before structural changes.
**Exam Trap:** Time Travel works with ALL statement types (DDL and DML) — not restricted to data-modifying statements only.
---

### Question 17
An append-only stream is created on a staging table. The ETL process performs: INSERT 100 rows, then DELETE 30 rows (cleanup), then INSERT 50 more rows. What does the stream contain?
- A) Net result: 120 rows (100 + 50 - 30) with INSERT actions
- B) 150 rows with INSERT actions only (100 + 50); DELETEs are ignored by append-only streams
- C) 100 rows from the first INSERT only
- D) 180 rows: 150 INSERTs and 30 DELETEs

**Answer: B) 150 rows with INSERT actions only (100 + 50); DELETEs are ignored by append-only streams**
**Explanation:** Append-only streams capture ONLY INSERT operations. The DELETE of 30 rows is completely invisible to the stream. Both INSERT operations (100 + 50 = 150 rows) appear as INSERT actions in the stream.
**Exam Trap:** Append-only streams see ALL inserts regardless of whether those rows are later deleted — they don't show "net" results.
---

### Question 18
A table is created as TRANSIENT. A developer attempts to clone it. What type is the clone?
- A) The clone is also TRANSIENT
- B) The clone is PERMANENT by default
- C) The clone fails — transient tables cannot be cloned
- D) The clone type depends on the CREATE TABLE CLONE syntax used

**Answer: A) The clone is also TRANSIENT**
**Explanation:** When cloning a table, the clone inherits the table type (transient, temporary, permanent) of the source by default. A clone of a transient table is transient. You CAN override this by explicitly creating a permanent clone of a transient table, but the default matches the source.
**Exam Trap:** The clone inherits the source table's type by default — a transient clone of a permanent table requires explicit TRANSIENT keyword.
---

### Question 19
A query joins table A (10M rows) with table B (10M rows) using an equality condition. The Query Profile shows the join operator produces 500M rows. There is no Cartesian product — the join condition exists. What is the issue?
- A) The join condition has a many-to-many relationship (fan-out/explosion)
- B) The tables need to be re-clustered
- C) A LEFT JOIN is preserving too many NULL rows
- D) The query needs a DISTINCT clause

**Answer: A) The join condition has a many-to-many relationship (fan-out/explosion)**
**Explanation:** A join key with duplicates on BOTH sides creates a Cartesian product for each matching group. If key 'X' appears 100 times in A and 50 times in B, that single key produces 5,000 rows. This multiplied across all keys explains 500M output from 10M+10M input. Fix by deduplicating one side or adding join conditions.
**Exam Trap:** Fan-out joins are not "Cartesian products" (they have a join condition) but produce similar row explosion — look for duplicate join keys on BOTH sides.
---

### Question 20
A developer writes a stored procedure that creates a temporary table, inserts processed data, and then SELECTs from it in a subsequent statement. Another session queries the same temporary table name. What does the other session see?
- A) The same data — temporary tables are session-scoped but visible to all sessions with the same role
- B) Nothing — temporary tables are session-scoped and invisible to other sessions
- C) An error — the table name conflicts
- D) Their own empty temporary table with the same name

**Answer: B) Nothing — temporary tables are session-scoped and invisible to other sessions**
**Explanation:** Temporary tables exist only within the session that created them and are automatically dropped when the session ends. Other sessions cannot see or access them, even with the same role. If another session queries that table name, they'll get "object does not exist" (unless a permanent table with that name exists).
**Exam Trap:** Temporary tables have session scope AND 0-1 day Time Travel AND no Fail-safe — they're truly ephemeral.

---

## Bonus: Advanced Scenario Questions

### Question 56
A data engineering team runs a MERGE statement that processes a staging table into a target. The MERGE fails with "Non-deterministic merge: multiple source rows matched the same target row." The staging table has 1M rows with customer_id as the join key. What must they do?

- A) Add a WHERE clause to the MERGE to filter duplicates
- B) Deduplicate the source BEFORE the MERGE (e.g., using QUALIFY ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY updated_at DESC) = 1)
- C) Use INSERT ... ON CONFLICT instead of MERGE
- D) Change the MERGE to use UPDATE only (no INSERT)

**Answer: B) Deduplicate the source BEFORE the MERGE (e.g., using QUALIFY ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY updated_at DESC) = 1)**

**Explanation:** MERGE requires that each target row matches at most ONE source row. Duplicate source keys matching the same target row creates non-determinism (which row "wins"?). The fix is to deduplicate the source first — ROW_NUMBER with QUALIFY is Snowflake's idiomatic approach. There is no ON CONFLICT syntax in Snowflake.

**Exam Trap:** MERGE with duplicate source rows matching the same target = ERROR. Always deduplicate the source before MERGE.

---

### Question 57
A stream on a table has STALE_AFTER = 2 days from now. The table has DATA_RETENTION_TIME_IN_DAYS = 3. If no one consumes the stream for 4 days, what happens?

- A) The stream automatically resets to the current table state
- B) The stream becomes STALE — unconsumed changes are permanently lost and the stream must be recreated
- C) Snowflake extends the retention automatically to preserve stream data
- D) The stream continues working with a warning

**Answer: B) The stream becomes STALE — unconsumed changes are permanently lost and the stream must be recreated**

**Explanation:** Stream staleness occurs when the stream's offset falls outside the table's Time Travel retention window. After 3 days of retention expire, the historical micro-partitions the stream needs are gone. The stream becomes stale and cannot be consumed — it must be recreated, and a full reload from another source is needed to catch up.

**Exam Trap:** Stream staleness = Time Travel retention expired for the stream's offset point. Consumed = offset advances. Unconsumed too long = stale.

---

### Question 58
A query joins TABLE_A (10B rows) with TABLE_B (100 rows) and the Query Profile shows 95% of execution time in a "Hash Join" operator with massive spilling. What optimization should be applied?

- A) Swap the join order to put the small table first
- B) No change needed — Snowflake's optimizer should automatically broadcast the small table (100 rows). Check if a filter on TABLE_B is missing or if TABLE_B is actually much larger than expected
- C) Add a clustering key to TABLE_A
- D) Increase the warehouse size

**Answer: B) No change needed — Snowflake's optimizer should automatically broadcast the small table (100 rows). Check if a filter on TABLE_B is missing or if TABLE_B is actually much larger than expected**

**Explanation:** With 100 rows, the optimizer should choose a broadcast join (no spilling). If massive spilling occurs, investigate whether TABLE_B is actually larger than expected (maybe the "100 rows" assumption is wrong), or if a missing filter is causing TABLE_B to expand. Snowflake's optimizer chooses join order regardless of SQL syntax.

**Exam Trap:** Snowflake's optimizer picks join strategy automatically. If performance is poor, verify your assumptions about data sizes rather than forcing join order.

---

### Question 59
A task is scheduled with `SCHEDULE = '5 MINUTE'`. The task's SQL takes 8 minutes to execute. What happens to the next scheduled run?

- A) The next run starts immediately after the current one finishes (at minute 8), then the schedule resets
- B) The next run is skipped entirely — the task runs sequentially, and the next execution begins at the next 5-minute boundary AFTER completion
- C) Two instances run concurrently
- D) The task is automatically suspended after timeout

**Answer: B) The next run is skipped entirely — the task runs sequentially, and the next execution begins at the next 5-minute boundary AFTER completion**

**Explanation:** Tasks do NOT run concurrently with themselves. If execution exceeds the schedule interval, the overlapping run is skipped. The task waits until the current execution completes, then the next run occurs at the next scheduled interval. This prevents resource contention and data race conditions.

**Exam Trap:** Tasks are single-threaded (no concurrent self-runs). If execution > interval, scheduled runs are skipped until the current one finishes.

---

### Question 60
A team uses a stream to track changes for incremental loading. They SELECT from the stream to preview changes but do NOT run any DML. The stream still shows the same records the next day. Why?

- A) The stream is broken and needs recreation
- B) SELECT does NOT advance the stream offset — only DML that CONSUMES the stream (INSERT INTO...SELECT FROM stream, MERGE, etc.) within a committed transaction advances it
- C) The stream data persists for 24 hours before auto-clearing
- D) A bug in stream behavior

**Answer: B) SELECT does NOT advance the stream offset — only DML that CONSUMES the stream (INSERT INTO...SELECT FROM stream, MERGE, etc.) within a committed transaction advances it**

**Explanation:** Stream offsets advance ONLY when stream data is consumed in a DML statement that commits. Simple SELECT queries are "read-only" against streams and never advance the offset. This design allows safe previewing without accidentally consuming change records.

**Exam Trap:** SELECT from stream = preview (non-consuming). DML consuming stream + COMMIT = offset advances. Transaction rollback = offset unchanged.

---

### Question 61
A table has 200,000 micro-partitions. The Query Profile shows "Partitions Scanned: 180,000" and "Partitions Total: 200,000" for a query filtering on `WHERE status = 'ACTIVE'`. The status column has 5 distinct values. Is adding a clustering key on `status` likely to help?

- A) Yes — clustering on status will dramatically reduce partitions scanned
- B) No — columns with very low cardinality (only 5 values) make poor clustering keys because partitions will still overlap significantly
- C) Only if combined with another column
- D) Clustering doesn't help with equality filters

**Answer: A) Yes — clustering on status will dramatically reduce partitions scanned**

**Explanation:** Low-cardinality columns can actually make EXCELLENT clustering keys because data can be perfectly segregated (all 'ACTIVE' rows in ~40,000 partitions, separate from 'INACTIVE' etc.). With only 5 values, clustering can achieve near-perfect pruning. High-cardinality columns (millions of unique values) are harder to cluster effectively.

**Exam Trap:** Low cardinality columns CAN be good clustering keys — they allow clean separation. The key metric is whether the filter matches the clustering order.

---

### Question 62
A secure view is used in a data sharing scenario. A consumer runs a query joining the shared secure view with their local table and applies a WHERE clause on a local column. The query is slow. What architectural limitation causes this?

- A) Cross-account joins are always slow
- B) Secure views disable query optimizer pushdown — the consumer's filter cannot be pushed into the secure view's underlying table scan, forcing more data to be processed before filtering
- C) The consumer's warehouse is undersized
- D) Network latency between provider and consumer storage

**Answer: B) Secure views disable query optimizer pushdown — the consumer's filter cannot be pushed into the secure view's underlying table scan, forcing more data to be processed before filtering**

**Explanation:** Secure views intentionally prevent predicate pushdown to avoid information leakage. The optimizer cannot push the consumer's WHERE clause through the secure view boundary. The secure view materializes its full result set first, THEN the consumer's filter is applied. This is a security-performance tradeoff by design.

**Exam Trap:** Secure views disable optimizer pushdown = security over performance. Regular views allow full optimization but can leak information.

---

### Question 63
A zero-copy clone of a 5TB table is created for testing. A developer runs `DELETE FROM clone_table WHERE region = 'EU'` which affects 30% of the data. Approximately how much ADDITIONAL storage does the clone now consume?

- A) 5TB (full copy was made during DELETE)
- B) ~1.5TB — only the micro-partitions affected by the DELETE are recreated (diverged storage)
- C) Zero — deletes don't consume storage
- D) 3.5TB — the remaining data is stored separately

**Answer: B) ~1.5TB — only the micro-partitions affected by the DELETE are recreated (diverged storage)**

**Explanation:** Zero-copy clones share micro-partitions with the source. When DML modifies data (DELETE recreates affected partitions), only the changed partitions diverge. The 30% of data deleted causes those specific micro-partitions to be recreated (new versions without EU rows). Unchanged partitions remain shared. So ~30% of 5TB ≈ 1.5TB additional storage.

**Exam Trap:** Clone storage diverges only for MODIFIED partitions. Unmodified data remains shared (zero additional cost).

---

### Question 64
A developer writes: `SELECT * FROM orders WHERE order_date BETWEEN '2025-01-01' AND '2025-12-31' AND YEAR(order_date) = 2025`. The table is clustered on order_date. The Query Profile shows 100% partition scan. What's the issue?

- A) BETWEEN doesn't work with clustering
- B) The YEAR() function wraps the clustering column, preventing pruning — the optimizer can't map YEAR(order_date) back to partition ranges
- C) The AND condition conflicts with BETWEEN
- D) String date comparisons bypass clustering

**Answer: B) The YEAR() function wraps the clustering column, preventing pruning — the optimizer can't map YEAR(order_date) back to partition ranges**

**Explanation:** While the BETWEEN clause CAN leverage pruning, the addition of YEAR(order_date) in an AND condition may cause the optimizer to choose a suboptimal plan. Actually in Snowflake, applying functions to a clustering column in WHERE can prevent pruning on that column. The BETWEEN alone would prune perfectly — the YEAR() function is redundant and may confuse the optimizer.

**Exam Trap:** Avoid wrapping clustering key columns in functions (YEAR(), MONTH(), etc.) in WHERE clauses — use direct range predicates for effective pruning.

---

### Question 65
A task DAG has: ROOT_TASK → TASK_A → TASK_C, and ROOT_TASK → TASK_B → TASK_C. TASK_A fails. What happens to TASK_C?

- A) TASK_C runs because TASK_B succeeded (any predecessor success triggers it)
- B) TASK_C is skipped because ALL predecessors must succeed for it to run
- C) TASK_C runs with partial input (only from TASK_B)
- D) It depends on the ALLOW_PARTIAL_DAG_EXECUTION setting

**Answer: B) TASK_C is skipped because ALL predecessors must succeed for it to run**

**Explanation:** In a Snowflake task DAG, a child task runs only when ALL of its predecessor tasks complete successfully. Since TASK_A failed and TASK_C depends on both TASK_A and TASK_B, TASK_C is skipped even though TASK_B succeeded. This ensures data consistency — TASK_C can rely on outputs from all parents.

**Exam Trap:** Task DAG rule: ALL predecessors must succeed. One failure = child is skipped for that run cycle.

---

### Question 66
A query references a table with 100,000 micro-partitions but the Query Profile shows "Partitions Scanned: 0" and returns results instantly. What happened?

- A) The result was served from the result cache (a prior identical query ran within 24 hours)
- B) The query was answered from metadata alone (e.g., SELECT COUNT(*))
- C) Either A or B — both can result in 0 partitions scanned
- D) The table is empty

**Answer: C) Either A or B — both can result in 0 partitions scanned**

**Explanation:** Zero partitions scanned occurs when: (1) the result cache serves a previously computed result, or (2) metadata-only queries like COUNT(*) without filters are answered from partition-level statistics. Both bypass actual data scanning. The Query Profile would show different indicators for each (result cache shows no execution plan at all).

**Exam Trap:** 0 partitions scanned = result cache OR metadata optimization. Check whether a warehouse was used to distinguish them.

---

### Question 67
A VARIANT column stores deeply nested JSON. A query with `data:level1:level2:level3:value::NUMBER` runs slowly despite the table being small (1000 rows). Why?

- A) Nested path traversal is O(n³) complexity
- B) Deep nesting prevents Snowflake's automatic columnarization — the path is stored in generic VARIANT format rather than columnar, requiring more I/O
- C) Each colon-separated level requires a separate join internally
- D) 1000 rows cannot cause slow queries

**Answer: B) Deep nesting prevents Snowflake's automatic columnarization — the path is stored in generic VARIANT format rather than columnar, requiring more I/O**

**Explanation:** Snowflake auto-columnarizes frequently accessed top-level paths in VARIANT data. Deeply nested paths may not be columnarized and require generic VARIANT traversal, which is slower. For performance-critical deeply nested access, consider flattening the data into explicit columns during loading.

**Exam Trap:** Snowflake auto-columnarizes VARIANT but only reliably for commonly-accessed top-level paths. Deep nesting degrades performance.

---

### Question 68
A table has DATA_RETENTION_TIME_IN_DAYS = 1 (Standard Edition). A user accidentally truncates the table at 10:00 AM. At 10:30 AM, they notice and want to recover. At 11:00 PM (13 hours later), another user also needs the data. Which user can recover?

- A) Both users can recover — the 1-day retention hasn't expired
- B) The 10:30 AM user can recover using Time Travel; the 11:00 PM user can also recover since it's within 24 hours
- C) Both can recover using UNDROP — TRUNCATE doesn't remove Time Travel data within the retention window
- D) Neither can recover — TRUNCATE bypasses Time Travel

**Answer: C) Both can recover using UNDROP — TRUNCATE doesn't remove Time Travel data within the retention window**

**Explanation:** TRUNCATE within the Time Travel retention period still retains history. Both users can recover using `INSERT INTO table SELECT * FROM table BEFORE(STATEMENT => '<truncate_query_id>')` or by cloning from a historical point. The 1-day retention means data is available for 24 hours after the truncate. Neither UNDROP nor TRUNCATE bypasses Time Travel.

**Exam Trap:** TRUNCATE preserves Time Travel history (within retention). DATA_RETENTION = 0 is what kills recoverability, not TRUNCATE.

---

### Question 69
A multi-cluster warehouse has MAX_CLUSTER_COUNT = 10 with Economy scaling policy. During a load test, only 2 clusters are active despite 50 queries queueing. What explains this behavior?

- A) Economy policy is broken and should be replaced with Standard
- B) Economy policy only starts new clusters when it estimates enough work exists to keep the cluster busy for ~6 minutes. Short-running queued queries may not meet this threshold
- C) Economy policy has a maximum of 2 clusters
- D) The warehouse is at its credit limit

**Answer: B) Economy policy only starts new clusters when it estimates enough work exists to keep the cluster busy for ~6 minutes. Short-running queued queries may not meet this threshold**

**Explanation:** Economy scaling policy is conservative — it won't start a new cluster unless it estimates at least 6 minutes of continuous work for that cluster. If the 50 queued queries would each complete in seconds, the system may determine that queueing is cheaper than spinning up more clusters. Standard policy would start clusters immediately when any queueing begins.

**Exam Trap:** Standard scaling = starts clusters sooner (responsive but expensive). Economy = waits until 6+ min of queued work justifies a new cluster.

---

### Question 70
A table's clustering key is (country, city). A query filters on `WHERE city = 'Paris'` without filtering on country. How effective is pruning?

- A) Perfectly effective — city is in the clustering key
- B) Partially effective — the first clustering column (country) determines primary partition organization, so filtering only on city (second column) is less effective but not useless
- C) Completely ineffective — you must always filter on the first column
- D) It depends on the warehouse size

**Answer: B) Partially effective — the first clustering column (country) determines primary partition organization, so filtering only on city (second column) is less effective but not useless**

**Explanation:** Multi-column clustering keys organize data by the first column primarily, then second column within each first-column partition. Filtering on only the second column provides some pruning (since 'Paris' exists in only a few countries' partitions) but significantly less than filtering on the first column. Think of it like a phone book sorted by last name then first name — searching only by first name requires scanning many sections.

**Exam Trap:** Clustering key column ORDER matters. Filtering on the first column = most pruning. Filtering on later columns = reduced (but not zero) benefit.

---

### Question 71
A developer creates an append-only stream on a staging table. The staging table undergoes INSERT, UPDATE, and DELETE operations. What does the append-only stream capture?

- A) All three operation types (INSERT, UPDATE, DELETE)
- B) Only INSERT operations — UPDATEs and DELETEs are invisible to append-only streams
- C) INSERTs and UPDATEs but not DELETEs
- D) The stream becomes stale because it cannot handle UPDATEs

**Answer: B) Only INSERT operations — UPDATEs and DELETEs are invisible to append-only streams**

**Explanation:** Append-only streams capture ONLY new row insertions. They ignore UPDATEs and DELETEs entirely. This makes them efficient for staging tables that are append-only (load-only) patterns. If the source table receives UPDATEs/DELETEs, a standard stream should be used instead.

**Exam Trap:** Append-only stream = INSERT only. Standard stream = INSERT + UPDATE + DELETE. Choose based on source table patterns.

---

### Question 72
A query uses `SELECT *, ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) as rn FROM orders QUALIFY rn = 1`. What is QUALIFY and why is it preferred over a subquery approach?

- A) QUALIFY is syntactic sugar for HAVING — no performance difference
- B) QUALIFY filters on window function results directly (like HAVING for GROUP BY), avoiding a subquery/CTE wrapper and potentially improving performance
- C) QUALIFY is required — you cannot filter on window functions any other way in Snowflake
- D) QUALIFY is deprecated in favor of WHERE

**Answer: B) QUALIFY filters on window function results directly (like HAVING for GROUP BY), avoiding a subquery/CTE wrapper and potentially improving performance**

**Explanation:** QUALIFY is Snowflake's extension that filters results of window functions directly in the same query block. Without QUALIFY, you'd need a subquery: `SELECT * FROM (SELECT *, ROW_NUMBER()... as rn FROM orders) WHERE rn = 1`. QUALIFY is more readable and may allow the optimizer to combine operations. It's unique to Snowflake (and a few other systems).

**Exam Trap:** QUALIFY = Snowflake's window function filter clause. It's unique and elegant — expect exam questions testing its purpose and syntax.

---

### Question 73
A data pipeline uses a standard stream on a source table. The source table has DATA_RETENTION_TIME_IN_DAYS = 1. The pipeline task runs every 2 hours normally but was disabled for maintenance lasting 30 hours. When the task resumes, what is the stream's state?

- A) The stream is healthy — it retained all changes during the 30-hour outage
- B) The stream is STALE — 30 hours exceeds the 1-day Time Travel retention, so the stream's offset points to data no longer available
- C) The stream has partial data (only the last 24 hours)
- D) The stream automatically resets to current and discards missed changes

**Answer: B) The stream is STALE — 30 hours exceeds the 1-day Time Travel retention, so the stream's offset points to data no longer available**

**Explanation:** Stream validity depends on the source table's Time Travel retention. With 1-day retention, historical micro-partitions are purged after 24 hours. After 30 hours without consumption, the stream's offset references purged data — making it stale. To avoid this: increase DATA_RETENTION_TIME_IN_DAYS or ensure the task consumes the stream more frequently than the retention period.

**Exam Trap:** Stream staleness = retention period < time between consumptions. Set retention > maximum expected processing gap.

---

### Question 74
A table has Time Travel set to 90 days. A user drops the table. 60 days later, they UNDROP it. How much Time Travel history does the recovered table have?

- A) 90 days from the original creation
- B) 30 days (90 - 60 days elapsed)
- C) 0 days — UNDROP resets the clock
- D) The table is recovered as it was at drop time, with its remaining Time Travel history (about 30 days of pre-drop history available)

**Answer: D) The table is recovered as it was at drop time, with its remaining Time Travel history (about 30 days of pre-drop history available)**

**Explanation:** UNDROP restores the table to its state at drop time. Time Travel data that was retained at drop time (and hasn't aged out of the 90-day window) is still available. Since 60 days passed, changes from before the drop that are older than 90 days from NOW are gone, but the last ~30 days before the drop remain queryable.

**Exam Trap:** UNDROP restores the table AND its Time Travel history — but only for changes still within the retention window from the current time.

---

### Question 75
A COPY INTO with VALIDATION_MODE = 'RETURN_ERRORS' returns zero errors for a staged file. The team then removes VALIDATION_MODE and runs the same COPY INTO. It reports "Copy executed with 0 files processed." What happened?

- A) VALIDATION_MODE consumed the file
- B) VALIDATION_MODE does not load data but REGISTERS the file in the load metadata — the file appears "already processed"
- C) The file was corrupted between validation and loading
- D) COPY INTO requires different syntax after validation

**Answer: B) VALIDATION_MODE does not load data but REGISTERS the file in the load metadata — the file appears "already processed"**

**Explanation:** This is a critical gotcha: VALIDATION_MODE validates without loading, but the file IS registered in Snowflake's 64-day load metadata. Subsequent COPY INTO commands see it as already loaded and skip it. Fix: use FORCE = TRUE on the actual load, or rename the file.

**Exam Trap:** VALIDATION_MODE = dry run but file IS marked as processed. Always use FORCE = TRUE for the subsequent real load.

---
