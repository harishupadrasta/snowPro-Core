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
