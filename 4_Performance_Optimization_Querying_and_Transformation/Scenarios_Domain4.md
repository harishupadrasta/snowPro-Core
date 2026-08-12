# Domain 4: Performance Optimization, Querying, and Transformation — Decision Scenarios

## Scenario 1: Query Optimization — Warehouse Sizing vs. Clustering

### Situation
A retail analytics team runs nightly reports on a 2TB `ORDERS` table filtered by `ORDER_DATE`. Queries take 45 minutes on an XL warehouse. The Query Profile shows 80% of partitions being scanned despite filtering on `ORDER_DATE`. No clustering key is defined; data was loaded in random order over 3 years.

### Decision Flow
1. Is the table large enough to benefit from clustering? → Yes (2TB, multi-terabyte)
2. Is the filter predicate consistent across queries? → Yes (`ORDER_DATE` is always used)
3. Does the Query Profile show poor partition pruning? → Yes (80% scanned)
4. Would scaling up the warehouse help? → No — scanning more partitions faster doesn't reduce the I/O volume

### Answer
**Define a clustering key on `ORDER_DATE`.**

After reclustering, queries will prune 90%+ of micro-partitions, reducing scan from ~1.6TB to ~200GB. The XL warehouse may even be downsized afterward.

### Why Not Others
| Alternative | Why Not |
|---|---|
| Scale up to 2XL/3XL | More compute doesn't fix bad pruning — you just read unneeded data faster and pay more |
| Materialized view | MV is for pre-computed aggregations, not for fixing scan patterns on detail queries |
| Multi-cluster warehouse | Solves concurrency, not single-query performance |
| Result caching | Reports change nightly; cache invalidates on DML |

### Exam Trap
"Scale up the warehouse" is the most common distractor when the real problem is partition pruning. The exam tests whether you can distinguish compute-bound (scale up) from I/O-bound (cluster/filter) problems. **Always check the Query Profile first** — if `Partitions Scanned` >> `Partitions Total * filter selectivity`, it's a pruning problem.

---

## Scenario 2: Diagnosing Query Spilling

### Situation
A data engineer notices a complex JOIN query on a LARGE warehouse takes 20 minutes. The Query Profile shows:
- `Bytes Spilled to Local Storage: 45GB`
- `Bytes Spilled to Remote Storage: 12GB`
- The most expensive operator is a Hash Join consuming 98% of processing time

### Decision Flow
1. Is there spilling? → Yes (both local and remote)
2. What's causing it? → Hash Join building a hash table larger than available memory
3. Options to reduce spilling:
   - Scale UP the warehouse (more memory per node)
   - Optimize the query (reduce data before JOIN)
   - Filter earlier in the query plan

### Answer
**First optimize the query by pushing filters before the JOIN. If spilling persists, scale up the warehouse size.**

Moving WHERE clauses and adding subqueries/CTEs that pre-filter reduces the hash table size. If the data volume genuinely requires more memory, scaling from LARGE to XL or 2XL doubles/quadruples memory.

### Why Not Others
| Alternative | Why Not |
|---|---|
| Scale OUT (multi-cluster) | Multi-cluster adds clusters for concurrency, not memory per query |
| Add clustering key | Helps pruning but doesn't reduce the in-memory hash table for the JOIN |
| Use a materialized view | Doesn't solve memory pressure for ad-hoc complex joins |
| Increase query timeout | Query still runs slowly; you just wait longer |

### Exam Trap
Spilling to **local** storage (SSD) is common and has moderate impact. Spilling to **remote** storage (S3/Azure Blob) is severe. The exam may ask "which type of spilling has the greatest performance impact?" — answer is always remote. Also: scaling OUT does NOT reduce spilling; only scaling UP adds memory per node.

---

## Scenario 3: Clustering Key Selection

### Situation
A `CUSTOMER_TRANSACTIONS` table (5TB) is queried in three patterns:
1. `WHERE region = 'EMEA' AND transaction_date > '2024-01-01'` (60% of queries)
2. `WHERE customer_id = 12345` (30% of queries)
3. `WHERE product_category = 'Electronics'` (10% of queries)

The table has 500M rows, `region` has 5 distinct values, `transaction_date` is a DATE, and `customer_id` has 100M distinct values.

### Decision Flow
1. Which columns appear most frequently in filters? → `region` + `transaction_date` (60%)
2. What is the cardinality? → `region` (5) is low; `transaction_date` (~1000 days) is medium; `customer_id` (100M) is very high
3. Ideal clustering: low-to-medium cardinality columns that appear in WHERE/JOIN
4. Can we combine? → Yes: `CLUSTER BY (region, transaction_date)`

### Answer
**`CLUSTER BY (region, transaction_date)`** — list lower-cardinality column first.

### Why Not Others
| Alternative | Why Not |
|---|---|
| `CLUSTER BY (customer_id)` | 100M distinct values = too high cardinality; clustering is ineffective when cardinality approaches row count |
| `CLUSTER BY (transaction_date, region)` | Works but suboptimal — putting the lower-cardinality column first gives better pruning on the most common query pattern |
| `CLUSTER BY (product_category)` | Only 10% of queries use this filter |
| No clustering | 5TB table with poor pruning wastes significant compute |

### Exam Trap
The exam loves testing cardinality rules. Remember: clustering works best with columns that have **low-to-medium** cardinality (hundreds to low millions of distinct values). Extremely high cardinality (approaching row count) makes clustering ineffective. Also: column ORDER in the clustering key matters — put the most-filtered, lowest-cardinality column first.

---

## Scenario 4: Streams vs. Scheduled Tasks for Change Tracking

### Situation
A company needs to propagate changes from a staging table (`STG_ORDERS`) to a production fact table (`FACT_ORDERS`). Changes arrive every 5 minutes via Snowpipe. Requirements:
- Process only new/changed rows (no full reload)
- Must not miss any records
- Processing should happen within 10 minutes of data landing

### Decision Flow
1. Need CDC (change data capture)? → Yes (only new/changed rows)
2. Need guaranteed delivery? → Yes (must not miss records)
3. Frequency? → Near-real-time (within 10 minutes)
4. Solution: Stream (captures changes) + Task (executes on schedule)

### Answer
**Create a Standard Stream on `STG_ORDERS` and a Task that runs every 5 minutes, consuming the stream with a MERGE statement.**

```sql
CREATE STREAM stg_orders_stream ON TABLE stg_orders;
CREATE TASK process_orders
  WAREHOUSE = compute_wh
  SCHEDULE = '5 MINUTE'
  WHEN SYSTEM$STREAM_HAS_DATA('stg_orders_stream')
AS
  MERGE INTO fact_orders t USING stg_orders_stream s ...;
```

### Why Not Others
| Alternative | Why Not |
|---|---|
| Scheduled task without stream (full scan) | Wastes compute scanning unchanged rows; doesn't identify changes |
| Append-only stream | Misses UPDATEs and DELETEs; only captures INSERTs |
| Dynamic Table | Good for declarative transforms, but less control over exactly-once MERGE semantics with complex business logic |
| Continuous data pipeline (Kafka) | Over-engineered for 5-minute latency; Snowflake-native solution simpler |

### Exam Trap
Key distinctions the exam tests:
- **Standard stream**: captures INSERT, UPDATE, DELETE (default)
- **Append-only stream**: captures INSERT only (use for staging/append tables)
- `SYSTEM$STREAM_HAS_DATA()` in the WHEN clause prevents the task from consuming credits when there's no new data
- A stream's offset advances **only when consumed in a DML transaction that commits successfully**

---

## Scenario 5: MERGE vs. Separate INSERT/UPDATE

### Situation
An ETL pipeline needs to:
- Insert new customer records from a staging table
- Update existing customer records where the email or phone changed
- The staging table has 1M rows; the target has 50M rows
- This runs every hour

### Decision Flow
1. Need both INSERT and UPDATE in one operation? → Yes
2. Is there a reliable match key? → Yes (`customer_id`)
3. Is the operation idempotent (safe to re-run)? → MERGE is naturally idempotent
4. Performance: one pass vs. two passes?

### Answer
**Use MERGE — single statement handles both INSERT (not matched) and UPDATE (matched + changed).**

```sql
MERGE INTO customers t
USING staging s ON t.customer_id = s.customer_id
WHEN MATCHED AND (t.email != s.email OR t.phone != s.phone)
  THEN UPDATE SET t.email = s.email, t.phone = s.phone, t.updated_at = CURRENT_TIMESTAMP()
WHEN NOT MATCHED
  THEN INSERT (customer_id, email, phone, created_at)
  VALUES (s.customer_id, s.email, s.phone, CURRENT_TIMESTAMP());
```

### Why Not Others
| Alternative | Why Not |
|---|---|
| Separate INSERT + UPDATE | Two full passes over the data; non-atomic (partial failure possible); not idempotent |
| DELETE + INSERT (full reload) | Wasteful for 1M changes against 50M rows; loses audit trail; resets Time Travel |
| INSERT OVERWRITE | Replaces entire table/partition — nuclear option for small change sets |
| Dynamic Table | Could work but MERGE gives explicit control over matching logic and conditional updates |

### Exam Trap
The exam tests MERGE specifics:
- MERGE is **not** atomic across multiple WHEN clauses by default in some edge cases — but in Snowflake, the entire MERGE is a single transaction
- A source row can match AT MOST ONE target row (1:1) — duplicates in source cause non-deterministic behavior
- You can have multiple WHEN MATCHED clauses with different conditions
- MERGE supports `WHEN NOT MATCHED BY SOURCE` (Snowflake-specific extension for deletes)

---

## Scenario 6: Time Travel Recovery — Dropped Table vs. Truncated Table

### Situation
A developer accidentally ran `TRUNCATE TABLE analytics.monthly_kpis` at 2:15 PM. The table had 3 years of aggregated KPI data. They notice at 2:45 PM. The table is on Enterprise Edition with 90-day Time Travel.

### Decision Flow
1. What happened? → TRUNCATE (not DROP)
2. Is the table still there? → Yes (TRUNCATE removes data but table persists)
3. Can we use UNDROP? → No (UNDROP is for DROP TABLE only)
4. Can we use Time Travel? → Yes — query data AT a timestamp before the TRUNCATE

### Answer
**Use Time Travel with `AT(TIMESTAMP => ...)` to recover the data via INSERT:**

```sql
INSERT INTO analytics.monthly_kpis
SELECT * FROM analytics.monthly_kpis
  AT(TIMESTAMP => '2024-01-15 14:10:00'::TIMESTAMP_LTZ);
```

### Why Not Others
| Alternative | Why Not |
|---|---|
| UNDROP TABLE | Only works for DROP TABLE, not TRUNCATE — table still exists, just empty |
| Clone from before truncate | `CREATE TABLE ... CLONE ... AT(TIMESTAMP)` works but creates a new table; you'd need to swap names |
| Restore from backup | Slower; Time Travel is faster and built-in |
| Fail-safe recovery | Fail-safe is for Snowflake support only (after Time Travel expires); not self-service |

### Exam Trap
Critical distinction: **DROP** → use `UNDROP`. **TRUNCATE/DELETE** → use Time Travel `AT` or `BEFORE`. The exam also tests:
- TRUNCATE resets the table's micro-partition history but Time Travel still works within retention
- `AT(STATEMENT => '<query_id>')` lets you go to the moment BEFORE a specific statement executed
- `BEFORE(STATEMENT => '<query_id>')` is equivalent to just before that statement
- Time Travel retention is configurable: 0-1 day (Standard), 0-90 days (Enterprise+)

---

## Scenario 7: FLATTEN Usage — Nested Arrays vs. Nested Objects

### Situation
A JSON events table contains:
```json
{
  "user_id": 101,
  "events": [
    {"type": "click", "page": "/home", "ts": "2024-01-15T10:00:00Z"},
    {"type": "purchase", "page": "/cart", "ts": "2024-01-15T10:05:00Z"}
  ],
  "metadata": {
    "browser": "Chrome",
    "os": "Windows",
    "plugins": ["ad_block", "dark_mode"]
  }
}
```
Need: One row per event with user_id, event type, page, and timestamp.

### Decision Flow
1. Target data is in an ARRAY of OBJECTs → FLATTEN the array
2. Each array element is an OBJECT → access keys with `:` or `.` notation after flatten
3. Need to preserve outer columns (`user_id`) → use LATERAL FLATTEN

### Answer
**Use LATERAL FLATTEN on the events array:**

```sql
SELECT
    raw:user_id::INT AS user_id,
    f.value:type::STRING AS event_type,
    f.value:page::STRING AS page,
    f.value:ts::TIMESTAMP AS event_ts
FROM events_table,
LATERAL FLATTEN(input => raw:events) f;
```

### Why Not Others
| Alternative | Why Not |
|---|---|
| FLATTEN without LATERAL | Works in Snowflake (LATERAL is implicit) but explicit LATERAL is best practice and clearer |
| Multiple FLATTEN for nested plugins | Not needed for the events array; only needed if you also want to unnest `metadata.plugins` |
| PARSE_JSON + string manipulation | Over-complicated; FLATTEN handles this natively |
| Recursive FLATTEN | Only needed for deeply nested/unknown-depth structures |

### Exam Trap
The exam tests FLATTEN output columns:
- `VALUE` — the element from the array/object
- `KEY` — the key (for objects) or index (for arrays)
- `INDEX` — numeric position in the array
- `PATH` — path to the element
- `THIS` — the element being flattened (useful for nested flattens)

Also: FLATTEN with `OUTER => TRUE` preserves rows even when the array is NULL or empty (like a LEFT JOIN). Default (`OUTER => FALSE`) drops those rows.

---

## Scenario 8: Materialized View vs. Regular View

### Situation
A dashboard queries a 500GB fact table with:
```sql
SELECT region, product_category, DATE_TRUNC('month', order_date) AS month,
       SUM(revenue) AS total_revenue, COUNT(*) AS order_count
FROM fact_orders
WHERE order_date >= DATEADD('year', -2, CURRENT_DATE())
GROUP BY 1, 2, 3;
```
This runs 200 times/day by 50 different users. The underlying table is updated once daily at midnight.

### Decision Flow
1. Is the query expensive? → Yes (500GB scan with aggregation)
2. Is it run frequently? → Yes (200x/day)
3. Does it need real-time data? → No (table updates once/day)
4. Does the query meet MV requirements? → Check: no UDFs, no LIMIT, simple aggregations, single table... Yes
5. Savings: 200 scans/day × 500GB = 100TB/day of I/O saved

### Answer
**Create a Materialized View.** The precomputed results are maintained automatically and serve queries instantly.

```sql
CREATE MATERIALIZED VIEW mv_revenue_summary AS
SELECT region, product_category, DATE_TRUNC('month', order_date) AS month,
       SUM(revenue) AS total_revenue, COUNT(*) AS order_count
FROM fact_orders
WHERE order_date >= DATEADD('year', -2, CURRENT_DATE())
GROUP BY 1, 2, 3;
```

### Why Not Others
| Alternative | Why Not |
|---|---|
| Regular view | No performance benefit — still scans 500GB every time |
| Table clone refreshed by task | Manual maintenance; not auto-refreshed on DML; stale until task runs |
| Result caching only | Works for identical queries from same role, but 50 users with slight variations won't all hit cache |
| Dynamic Table | Viable alternative, but MV is more efficient for simple aggregations on a single table with auto-maintenance |

### Exam Trap
Materialized View limitations the exam tests:
- **Cannot** use: JOINs, UDFs, HAVING, ORDER BY, LIMIT, window functions, subqueries
- **Can** use: WHERE, GROUP BY, aggregate functions (SUM, COUNT, MIN, MAX, AVG)
- MVs consume **storage** (for precomputed results) and **compute** (for background maintenance)
- MVs are **automatically refreshed** when base table changes — no manual maintenance needed
- If the MV becomes too stale (suspended), it falls back to querying the base table

---

## Scenario 9: Table Type Selection — Transient vs. Permanent vs. Temporary

### Situation
A data engineering team needs tables for three use cases:
1. Production fact tables with audit requirements (7-year retention compliance)
2. ETL staging tables that are rebuilt every pipeline run
3. Session-specific scratch tables for analyst ad-hoc exploration

### Decision Flow

| Use Case | Data Persistence | Time Travel Needed | Fail-safe Needed | Multi-session? |
|---|---|---|---|---|
| Production facts | Permanent | Yes (90 days) | Yes (7 days) | Yes |
| ETL staging | Until next run | Minimal (0-1 day) | No | Yes (cross-session) |
| Analyst scratch | Current session only | No | No | No |

### Answer
1. **Permanent table** for production facts — full Time Travel + Fail-safe
2. **Transient table** for ETL staging — no Fail-safe, reduces storage costs, persists across sessions
3. **Temporary table** for analyst scratch — exists only for session duration, no Fail-safe

### Why Not Others
| Alternative | Why Not |
|---|---|
| All permanent tables | Staging and scratch tables don't need Fail-safe; wastes storage (7 days of Fail-safe on throwaway data) |
| All temporary tables | ETL staging needs to persist across sessions; temporary tables vanish on disconnect |
| External tables | For data that lives outside Snowflake; not applicable for internal processing |
| Transient for production | Loses Fail-safe protection — unacceptable for compliance-required data |

### Exam Trap
Table type storage comparison:

| Type | Time Travel | Fail-safe | Persists Across Sessions |
|---|---|---|---|
| Permanent | 0-90 days | 7 days | Yes |
| Transient | 0-1 day | **None** | Yes |
| Temporary | 0-1 day | **None** | **No** |

The exam tests: "Which table type has NO Fail-safe period?" — Both Transient AND Temporary. "Which table type is visible only within the creating session?" — Temporary only. Transient tables persist until explicitly dropped.

---

## Scenario 10: Sequence vs. Identity Column vs. AUTOINCREMENT

### Situation
A team needs to generate unique integer IDs for a `CUSTOMERS` table. Requirements:
- IDs must be unique (but not necessarily gap-free)
- Multiple concurrent INSERT sessions must not block each other
- IDs should be roughly increasing over time
- Need the same ID generator across multiple related tables

### Decision Flow
1. Must IDs be shared across tables? → Yes (customer_id used in orders, addresses, etc.)
2. Must they be gap-free? → No
3. Concurrency requirement? → High
4. Gap-free not required → Sequence (not a gap-free counter)

### Answer
**Use a Sequence** — shared across tables, high-concurrency, gap-tolerant.

```sql
CREATE SEQUENCE customer_id_seq START = 1 INCREMENT = 1;

-- Used across multiple tables:
CREATE TABLE customers (id INT DEFAULT customer_id_seq.NEXTVAL, ...);
CREATE TABLE orders (customer_id INT, ...);  -- FK references
INSERT INTO customers (name) VALUES ('Alice');  -- id auto-assigned
```

### Why Not Others
| Alternative | Why Not |
|---|---|
| Identity/Autoincrement column | Tied to a SINGLE table — cannot share across multiple tables; also gaps occur anyway |
| UUID (UUID_STRING()) | Not integer; not roughly ordered; larger storage; harder to read/debug |
| MAX(id) + 1 | Race condition under concurrency; requires serialization; terrible performance |
| Row number from source | Non-unique if sources overlap; requires coordination |

### Exam Trap
Sequence vs. Identity — the exam tests these differences:
- **Sequence**: Standalone object, reusable across tables, explicit `.NEXTVAL` call
- **Identity/Autoincrement**: Column property, single table only, auto-populated
- Both can produce **gaps** (this is by design for performance — gaps occur on rollbacks, multi-node generation)
- `GENERATED ALWAYS AS IDENTITY` prevents manual override; `GENERATED BY DEFAULT` allows it
- Sequences guarantee **uniqueness**, NOT **contiguity** (no-gap guarantee)
