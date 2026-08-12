# Domain 5: Data Collaboration — Decision Scenarios

## Scenario 1: Direct Sharing vs Reader Accounts

### Situation
Your company needs to share sales analytics data with a small consulting firm that does NOT have a Snowflake account. The consulting firm has 3 analysts who need query access to aggregated dashboards. Budget is limited on the consumer side.

### Decision Flow
1. Does the consumer have a Snowflake account? → **No**
2. Is the consumer willing to create one? → **No (cost concern)**
3. Do you need to control the consumer's compute costs? → **Yes**
4. Is this a temporary or ongoing relationship? → **Ongoing but limited scope**

### Answer
**Use a Reader Account (managed account)** created by the provider.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Direct Share | Requires consumer to have their own Snowflake account |
| Marketplace Listing | Overkill for single-consumer, private relationship |
| Data Exchange | Designed for group/multi-party collaboration, not 1:1 |
| Export/ETL | Loses zero-copy benefit, creates data staleness |

### Exam Trap
- Reader accounts are created and **managed by the provider** — the provider pays for compute
- Reader accounts can ONLY consume data from the provider that created them — they cannot share data with others or access the marketplace
- A reader account is NOT the same as a "free trial" account — it has significant limitations (no Time Travel beyond 1 day on standard, limited features)
- Reader accounts use **managed warehouses** provisioned by the provider

---

## Scenario 2: Marketplace Listing Type Decision

### Situation
A weather data company wants to monetize their real-time forecast dataset. They want broad distribution to any Snowflake customer, with both free trial access (limited historical data) and a paid tier (full real-time feed). They want to reach customers across multiple clouds and regions.

### Decision Flow
1. Is this for broad public distribution? → **Yes**
2. Do you need monetization/pricing? → **Yes**
3. Do you need free + paid tiers? → **Yes**
4. Cross-region required? → **Yes (auto-fulfillment needed)**

### Answer
**Snowflake Marketplace with a Paid Listing** (Standard or Personalized) with auto-fulfillment enabled for cross-region delivery.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Direct Share | No monetization, requires knowing each consumer, single-region only |
| Free Listing Only | Doesn't support paid tier for full dataset |
| Private Data Exchange | Limited audience, not public discovery |
| Reader Account | Cannot scale to broad distribution; provider bears all compute costs |

### Exam Trap
- Marketplace listings can be **free**, **paid**, or **personalized** (custom pricing per consumer)
- Cross-region/cross-cloud marketplace delivery uses **auto-fulfillment** via replication — the provider enables this
- A listing with "Get" means free; "Request" means personalized/paid requiring provider approval
- The **provider** account must be in a Business Critical or higher edition for paid listings
- Data products on Marketplace can include: tables, views, secure UDFs — NOT stored procedures

---

## Scenario 3: Data Exchange vs Marketplace

### Situation
A healthcare consortium of 12 hospitals wants to share de-identified patient outcome data among themselves. The data is sensitive (even de-identified), should NOT be publicly discoverable, and only verified consortium members should access it. New members need an approval process.

### Decision Flow
1. Is this for a closed, known group? → **Yes**
2. Should data be publicly discoverable? → **No**
3. Is there a membership/approval process? → **Yes**
4. Are there multiple providers within the group? → **Yes (each hospital shares)**

### Answer
**Private Data Exchange** — a closed, invitation-only group where multiple members can be both providers and consumers.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Marketplace (public) | Data should NOT be publicly discoverable |
| Direct Shares (point-to-point) | 12 × 11 = 132 share relationships needed; unmanageable |
| Personalized Marketplace Listing | Still publicly visible in catalog even if access is restricted |
| Replication | Replication is infrastructure-level, not a collaboration framework |

### Exam Trap
- Data Exchanges are managed by an **administrator** who invites members and sets policies
- Members of a Data Exchange can be **providers**, **consumers**, or **both**
- Data Exchanges are NOT public — they do not appear in the Snowflake Marketplace catalog
- A single account can participate in **multiple** Data Exchanges simultaneously
- Data Exchange is being superseded by **Listings** functionality — know both for the exam

---

## Scenario 4: Replication vs Sharing for Cross-Region

### Situation
A global retail company has a Snowflake account in AWS US-East-1. Their European analytics team (in AWS EU-West-1) needs access to product catalog data with <5 second query latency. The European team runs complex queries 14 hours/day. Data updates hourly in the US account.

### Decision Flow
1. Are provider and consumer in the same region? → **No**
2. Is low query latency critical? → **Yes (<5s)**
3. How frequently does the consumer query? → **Heavy (14 hrs/day)**
4. Is the consumer in the same organization? → **Yes (same company)**
5. Is near-real-time freshness needed? → **Yes (hourly updates)**

### Answer
**Database Replication** to a Snowflake account in EU-West-1, with replication scheduled to refresh at least hourly.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Direct Share (cross-region) | Cross-region shares via listing have higher latency; consumer queries traverse regions |
| Marketplace auto-fulfillment | Designed for provider→many consumers; overkill for internal same-org |
| Data export/import | Creates lag, operational complexity, no zero-copy benefit |
| Single account, EU warehouse | Cannot attach a warehouse in a different region to a US-based account |

### Exam Trap
- **Database replication** copies data physically to the target region — consumer queries run locally with full performance
- **Sharing** (even cross-region via listings) means queries execute against the provider's region — latency depends on distance
- Replication has **storage costs** in the target region + **data transfer costs** — sharing does not duplicate storage
- Replication refresh frequency is configurable but NOT real-time (seconds of lag are possible)
- For the exam: Replication = performance + cost; Sharing = freshness + no duplicate storage

---

## Scenario 5: Failover Group Configuration

### Situation
A financial services company must maintain <1 hour RTO (Recovery Time Objective) for their critical trading database. They have accounts in AWS US-East-1 (primary) and AWS US-West-2 (secondary). They need account-level failover including databases, warehouses, users/roles, shares, and network policies.

### Decision Flow
1. Is this disaster recovery (not just data replication)? → **Yes**
2. Do you need account-level object failover (roles, warehouses, etc.)? → **Yes**
3. What is the RTO requirement? → **<1 hour**
4. Does it need to include shares and integrations? → **Yes**

### Answer
**Failover Group** with replication of: DATABASES, USERS, ROLES, WAREHOUSES, NETWORK POLICIES, SHARES, and INTEGRATIONS. Configure with a replication schedule meeting RPO requirements and test failover procedures.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Database Replication alone | Only replicates data — not users, roles, warehouses, shares |
| Replication Group (no failover) | Replicates objects but does NOT support failover/promotion |
| Client-side redirect only | Connection redirect without underlying data = broken queries |
| Multi-cluster warehouse | Scales within a region; does not provide cross-region DR |

### Exam Trap
- **Replication Group**: replicates objects to secondary → read-only, NO failover capability
- **Failover Group**: replicates objects AND allows promotion of secondary to primary
- Failover groups can include: DATABASES, SHARES, USERS, ROLES, WAREHOUSES, RESOURCE MONITORS, NETWORK POLICIES, INTEGRATIONS, CONNECTIONS
- A database can belong to ONLY ONE replication or failover group
- **Client Redirect** allows connection URLs to automatically point to the promoted secondary
- Failover requires **Business Critical edition** or higher
- The secondary account must be in the **same organization** but can be different region/cloud

---

## Scenario 6: Secure View Requirements for Sharing

### Situation
You want to share customer order data with a partner, but you need to: (1) hide the underlying table structure, (2) prevent the consumer from seeing your query logic in the view definition, (3) filter rows so the partner only sees their own customers, and (4) ensure the Snowflake query optimizer cannot expose data through query plan analysis.

### Decision Flow
1. Need to hide base table structure? → **Yes → Use a view**
2. Need to hide view definition from consumer? → **Yes → Must be SECURE view**
3. Need row-level filtering? → **Yes → WHERE clause using CURRENT_ACCOUNT() or share context**
4. Need optimizer protection? → **Yes → SECURE view bypasses optimizer optimizations that could leak data**
5. Sharing to external account? → **Yes → SECURE view is MANDATORY**

### Answer
**Secure View** (CREATE SECURE VIEW) — this is the ONLY option when sharing views. Snowflake REQUIRES that any view shared via Secure Data Sharing be a secure view.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Regular View | CANNOT be shared — Snowflake rejects non-secure views in shares |
| Secure UDF | Can supplement but cannot replace the need for a secure view for tabular access |
| Table directly | Exposes all rows; cannot filter by consumer; reveals schema |
| Materialized View | Can be shared if secure, but not required for this use case and has refresh cost |

### Exam Trap
- **All views in a share MUST be secure views** — this is enforced by Snowflake, not optional
- Secure views **disable optimizer optimizations** (like predicate pushdown) that could leak data through timing or error messages
- `CURRENT_ACCOUNT()` in a secure view returns the **consumer's** account when queried by the consumer
- Secure views hide the view definition (SHOW VIEWS shows definition as blank to non-owners)
- Performance may be LOWER with secure views due to disabled optimizations — this is the tradeoff
- You CAN share secure materialized views, secure UDFs, and secure UDTFs in addition to secure views

---

## Scenario 7: Provider vs Consumer Responsibilities

### Situation
Company A (provider) shares a "Sales Analytics" database with Company B (consumer) via a direct share. Both sides need clarity on: who pays for storage, who pays for compute, who controls access within the consumer account, and who manages data freshness.

### Decision Flow

**Provider (Company A) is responsible for:**
1. Creating and managing the share object
2. Granting privileges on objects to the share
3. Adding consumer accounts to the share
4. Paying for data STORAGE (data lives in provider's account)
5. Maintaining data freshness/updates
6. Revoking access when needed

**Consumer (Company B) is responsible for:**
1. Creating a DATABASE FROM SHARE in their account
2. Granting roles/users access to the shared database within their account
3. Paying for all COMPUTE (warehouse costs for querying shared data)
4. Creating their own warehouses to query the data
5. Managing their own access control on the imported database

### Answer
**Provider pays storage; Consumer pays compute.** The consumer has full control over who in their organization can access the shared data and what warehouse size to use.

### Why Not Others
| Misconception | Reality |
|---------------|---------|
| "Provider pays for consumer queries" | NO — consumer always pays for their own compute |
| "Consumer can modify shared data" | NO — shared data is READ-ONLY to the consumer |
| "Provider can see who queries the data" | NO — provider cannot see consumer's query history |
| "Consumer needs to refresh data" | NO — data is always live/current (zero-copy) |
| "Provider manages consumer's roles" | NO — consumer manages their own internal access |

### Exam Trap
- The consumer creates the database: `CREATE DATABASE db_name FROM SHARE provider_account.share_name`
- Shared data is **always read-only** from the consumer side — no INSERT, UPDATE, DELETE
- There is **no data copying** — zero-copy means the consumer reads the provider's physical files
- The provider has **no visibility** into consumer usage (queries, users, frequency)
- If the provider revokes access, the consumer's database FROM SHARE becomes immediately inaccessible
- A consumer CAN create views/tables on top of shared data in a different database (not the shared DB itself)

---

## Scenario 8: Cross-Cloud Sharing Strategy

### Situation
Your organization runs on Azure West Europe but needs to share live inventory data with a partner whose Snowflake account is on GCP US-Central1. The data must be near-real-time (≤15 min lag acceptable) and the partner should access it as if it were a regular database in their account.

### Decision Flow
1. Are both parties on the same cloud? → **No (Azure vs GCP)**
2. Are both in the same region? → **No (EU vs US)**
3. Is this a direct share relationship? → **Yes (known partner)**
4. What freshness is needed? → **≤15 minutes**
5. Should it appear as a native database to consumer? → **Yes**

### Answer
**Cross-cloud auto-fulfillment via a Listing** — the provider creates a listing, enables auto-fulfillment to the consumer's region/cloud, and Snowflake automatically replicates the data. The consumer "gets" the listing and sees it as a standard shared database.

### Why Not Others
| Option | Why Not |
|--------|---------|
| Direct Share (same region only) | Direct shares only work within the same region AND cloud |
| Manual replication + share | Works but requires the provider to manage a target account in GCP manually |
| Data export/import | Doesn't meet ≤15 min freshness; operational burden |
| Snowpipe/ETL to partner | Requires infrastructure on both sides; not zero-copy |

### Exam Trap
- **Direct shares work ONLY within the same cloud region** — this is the #1 tested constraint
- Cross-region (same cloud) and cross-cloud sharing requires **Listings** with auto-fulfillment OR manual replication
- Auto-fulfillment uses replication under the hood — the **provider** pays replication/transfer costs
- The consumer experience is identical regardless of cross-cloud mechanics — they just "get" a listing
- For cross-cloud: Provider must be on **Business Critical** or higher edition
- Snowflake supports cross-cloud sharing across AWS, Azure, and GCP — all combinations work
- The replication lag for auto-fulfillment depends on data volume and configured refresh frequency

---

## Quick Reference: Decision Matrix

| Scenario | Same Region | Cross-Region | Cross-Cloud | Many Consumers | Monetization |
|----------|:-----------:|:------------:|:-----------:|:--------------:|:------------:|
| Direct Share | Yes | No | No | Limited | No |
| Reader Account | Yes | No | No | Limited | No |
| Listing (Free) | Yes | Yes* | Yes* | Yes | No |
| Listing (Paid) | Yes | Yes* | Yes* | Yes | Yes |
| Data Exchange | Yes | Yes* | Yes* | Group | No |
| Replication | N/A | Yes | Yes | N/A | N/A |
| Failover Group | N/A | Yes | Yes | N/A | N/A |

*Via auto-fulfillment (replication under the hood)
