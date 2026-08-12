# Domain 2: Account Management and Data Governance - Quiz Questions

![Domain 2](https://img.shields.io/badge/Domain_2-Account_Management_&_Data_Governance-blue)
![Questions](https://img.shields.io/badge/Questions-50-green)
![Weight](https://img.shields.io/badge/Exam_Weight-20%25-orange)

---

## 2.1 Access Control & RBAC

### Question 1
What is the default role assigned to every new user in Snowflake if no default role is explicitly set?

- A) SYSADMIN
- B) PUBLIC
- C) USERADMIN
- D) SECURITYADMIN

**Answer: B) PUBLIC**
**Explanation:** Every Snowflake user is automatically granted the PUBLIC role. If no default role is explicitly configured for a user, the PUBLIC role is used when they log in. All users in the account automatically have the PUBLIC role.

---

### Question 2
Which system-defined role has the privilege to create and manage users and roles?

- A) SYSADMIN
- B) ACCOUNTADMIN
- C) USERADMIN
- D) SECURITYADMIN

**Answer: C) USERADMIN**
**Explanation:** USERADMIN is specifically designed to create and manage users and roles. While SECURITYADMIN can also manage grants, and ACCOUNTADMIN inherits all privileges, USERADMIN is the dedicated role for user and role management.

---

### Question 3
In Snowflake's role hierarchy, which role sits at the top and inherits privileges from all other system-defined roles?

- A) SECURITYADMIN
- B) ORGADMIN
- C) ACCOUNTADMIN
- D) SYSADMIN

**Answer: C) ACCOUNTADMIN**
**Explanation:** ACCOUNTADMIN is the top-level role in the account and inherits privileges from both SYSADMIN and SECURITYADMIN (which in turn inherits from USERADMIN). ORGADMIN operates at the organization level, not within the account hierarchy.

---

### Question 4
A company needs to ensure that a custom role "ANALYST" can query tables in the SALES schema. Which statement correctly grants this access?

- A) GRANT SELECT ON ALL TABLES IN SCHEMA SALES TO ROLE ANALYST;
- B) GRANT USAGE ON DATABASE SALES TO ROLE ANALYST;
- C) GRANT SELECT ON SCHEMA SALES TO ROLE ANALYST;
- D) GRANT READ ON ALL TABLES IN SCHEMA SALES TO ROLE ANALYST;

**Answer: A) GRANT SELECT ON ALL TABLES IN SCHEMA SALES TO ROLE ANALYST;**
**Explanation:** To query tables, the SELECT privilege must be granted on the tables themselves. Note that USAGE on the database and schema is also required but option A directly answers the question about querying tables. There is no READ privilege in Snowflake for tables.

---

### Question 5
Which access control model does Snowflake primarily use?

- A) Mandatory Access Control (MAC)
- B) Attribute-Based Access Control (ABAC)
- C) Discretionary Access Control (DAC) combined with Role-Based Access Control (RBAC)
- D) Rule-Based Access Control

**Answer: C) Discretionary Access Control (DAC) combined with Role-Based Access Control (RBAC)**
**Explanation:** Snowflake uses a combination of DAC and RBAC. DAC means each object has an owner who can grant access to others. RBAC means privileges are assigned to roles, and roles are granted to users. This hybrid model provides flexible yet secure access management.

---

### Question 6
What happens to objects owned by a role when that role is dropped?

- A) The objects are automatically transferred to SYSADMIN
- B) The objects are deleted
- C) The role cannot be dropped if it owns objects
- D) The objects are transferred to ACCOUNTADMIN

**Answer: C) The role cannot be dropped if it owns objects**
**Explanation:** Snowflake prevents dropping a role that owns objects. You must first transfer ownership of all objects owned by that role to another role using GRANT OWNERSHIP, or drop the objects, before the role can be dropped.

---

### Question 7
A data engineer creates a view using the SYSADMIN role. Who is the owner of that view?

- A) The user who created it
- B) SYSADMIN
- C) ACCOUNTADMIN
- D) The database owner

**Answer: B) SYSADMIN**
**Explanation:** In Snowflake, object ownership is assigned to the role that was active when the object was created, not to the individual user. Since the data engineer was using SYSADMIN when creating the view, SYSADMIN owns the view.

---

### Question 8
Which privilege is required to create a database in Snowflake?

- A) CREATE DATABASE granted at the account level
- B) MANAGE GRANTS on the account
- C) USAGE on the account
- D) OWNERSHIP on the account

**Answer: A) CREATE DATABASE granted at the account level**
**Explanation:** The CREATE DATABASE privilege must be explicitly granted at the account level to a role. By default, SYSADMIN has this privilege. Other roles need it explicitly granted to create databases.

---

### Question 9
What is the purpose of FUTURE GRANTS in Snowflake?

- A) To grant privileges on objects that will be created in the future
- B) To schedule grants to take effect at a future date
- C) To grant temporary privileges that expire
- D) To preview what privileges will look like after a change

**Answer: A) To grant privileges on objects that will be created in the future**
**Explanation:** FUTURE GRANTS define privileges that are automatically applied to new objects created in a schema or database. This eliminates the need to manually grant privileges every time a new table, view, or other object is created.

---

### Question 10
Which statement about the SECURITYADMIN role is TRUE?

- A) It can create warehouses
- B) It inherits the USERADMIN role and can manage grants globally
- C) It can manage resource monitors
- D) It is the only role that can create network policies

**Answer: B) It inherits the USERADMIN role and can manage grants globally**
**Explanation:** SECURITYADMIN inherits USERADMIN (so it can manage users and roles) and additionally has the MANAGE GRANTS privilege, allowing it to grant or revoke privileges on any object in the account. It cannot create warehouses or manage resource monitors (those are SYSADMIN/ACCOUNTADMIN tasks).

---

### Question 11
A developer needs SELECT access on a table but should NOT be able to see the DDL definition. Which approach is most appropriate?

- A) Grant SELECT but revoke USAGE on the schema
- B) This is not possible in Snowflake
- C) Use a secure view to expose only the data
- D) Grant SELECT with the RESTRICT option

**Answer: C) Use a secure view to expose only the data**
**Explanation:** Secure views hide their DDL definition from users who don't own them. By granting SELECT on a secure view instead of the underlying table, users can query data without seeing the view definition or underlying table structure. There is no RESTRICT option for grants.

---

### Question 12
Your organization requires that only ACCOUNTADMIN can grant the ACCOUNTADMIN role to other users. Which Snowflake feature enforces this?

- A) This is the default behavior — only ACCOUNTADMIN can grant itself
- B) Network policies
- C) Access policies
- D) Organization-level controls

**Answer: A) This is the default behavior — only ACCOUNTADMIN can grant itself**
**Explanation:** By default, only users with the ACCOUNTADMIN role (or a role with MANAGE GRANTS) can grant the ACCOUNTADMIN role to other users. This is built-in security behavior that prevents privilege escalation without additional configuration.

---

### Question 13
What is the minimum privilege required for a role to clone a table?

- A) OWNERSHIP on the source table
- B) SELECT on the source table and CREATE TABLE on the destination schema
- C) CREATE TABLE on the destination schema only
- D) ALL PRIVILEGES on the source table

**Answer: B) SELECT on the source table and CREATE TABLE on the destination schema**
**Explanation:** To clone a table, a role needs SELECT on the source table (to read its data/metadata) and CREATE TABLE privilege on the target schema where the clone will be created. OWNERSHIP is not required for cloning.

---

### Question 14
In a scenario where Role_A is granted to Role_B, and Role_B is granted to a user, which statement is correct?

- A) The user has only Role_B privileges
- B) The user has both Role_A and Role_B privileges when using Role_B
- C) The user must explicitly switch to Role_A to use its privileges
- D) Role inheritance only works with system-defined roles

**Answer: B) The user has both Role_A and Role_B privileges when using Role_B**
**Explanation:** Snowflake's role hierarchy means that when a role is granted to another role, the parent role inherits all privileges of the child role. When the user activates Role_B, they automatically have all privileges from both Role_B and Role_A.

---

### Question 15
Which statement about managed access schemas is TRUE?

- A) Only the schema owner can grant privileges on objects within the schema
- B) Object owners can grant privileges on their objects to any role
- C) Only the schema owner or a role with MANAGE GRANTS can grant privileges on objects in the schema
- D) Managed access schemas prevent any grants from being made

**Answer: C) Only the schema owner or a role with MANAGE GRANTS can grant privileges on objects in the schema**
**Explanation:** In a managed access schema (created with WITH MANAGED ACCESS), the object owner loses the ability to grant privileges on their objects. Only the schema owner or a role with the MANAGE GRANTS privilege can manage grants, providing centralized access control.

---

### Question 16
A company wants to prevent users from seeing actual salary data. They want to show "***" instead for unauthorized roles. Which feature should they use?

- A) Row Access Policy
- B) Dynamic Data Masking Policy
- C) Secure View
- D) Column-level Security Tag

**Answer: B) Dynamic Data Masking Policy**
**Explanation:** Dynamic Data Masking policies conditionally mask column data at query time based on the role of the user executing the query. This allows authorized roles to see actual values while others see masked values like "***". Row Access Policies filter rows, not columns.

---

### Question 17
Which privilege allows a role to grant ownership of objects it owns to other roles?

- A) MANAGE GRANTS
- B) OWNERSHIP (implicitly includes this ability)
- C) GRANT OWNERSHIP
- D) TRANSFER OWNERSHIP

**Answer: B) OWNERSHIP (implicitly includes this ability)**
**Explanation:** The owner of an object can transfer ownership to another role. This is an implicit capability of the OWNERSHIP privilege. However, in managed access schemas, this ability is restricted to the schema owner or roles with MANAGE GRANTS.

---

### Question 18
What happens when you revoke a role from another role in the hierarchy?

- A) All users with the parent role lose the child role's privileges immediately
- B) Objects owned by the child role are transferred to the parent role
- C) The child role is dropped
- D) A 24-hour grace period allows data migration

**Answer: A) All users with the parent role lose the child role's privileges immediately**
**Explanation:** When a child role is revoked from a parent role, the privilege inheritance is broken immediately. Any user operating under the parent role will instantly lose access to privileges that were inherited through the child role.

---

## 2.2 Data Protection & Security

### Question 19
Which Snowflake edition is required to use Dynamic Data Masking?

- A) Standard
- B) Enterprise or higher
- C) Business Critical
- D) Virtual Private Snowflake (VPS)

**Answer: B) Enterprise or higher**
**Explanation:** Dynamic Data Masking is available in Enterprise edition and above. Standard edition does not support masking policies. Business Critical and VPS also support it since they are higher editions, but Enterprise is the minimum requirement.

---

### Question 20
What type of encryption does Snowflake use for data at rest?

- A) AES-128
- B) AES-256
- C) RSA-2048
- D) TLS 1.2

**Answer: B) AES-256**
**Explanation:** Snowflake encrypts all data at rest using AES-256 (Advanced Encryption Standard with 256-bit keys). This applies to all data files, temporary files, and result sets. TLS 1.2 is used for data in transit, not at rest.

---

### Question 21
A healthcare company requires that their encryption keys be managed by them, not Snowflake. Which feature and edition do they need?

- A) Customer-Managed Keys (Tri-Secret Secure) — Business Critical edition
- B) Customer-Managed Keys — Enterprise edition
- C) BYOK (Bring Your Own Key) — Standard edition
- D) External Key Management — VPS only

**Answer: A) Customer-Managed Keys (Tri-Secret Secure) — Business Critical edition**
**Explanation:** Tri-Secret Secure is available in Business Critical edition and above. It creates a composite master key from a Snowflake-maintained key and a customer-managed key (stored in the customer's cloud provider KMS). This ensures Snowflake alone cannot decrypt data.

---

### Question 22
Which statement about network policies in Snowflake is TRUE?

- A) They can only be applied at the account level
- B) They can be applied at the account level and individual user level
- C) They require Business Critical edition
- D) They only support IPv6 addresses

**Answer: B) They can be applied at the account level and individual user level**
**Explanation:** Network policies can be applied at two levels: the entire account (affecting all users) or to specific individual users. This allows organizations to enforce IP allowlists/blocklists broadly while making exceptions for specific users. They are available in all editions and support IPv4.

---

### Question 23
What is the purpose of a Row Access Policy in Snowflake?

- A) To encrypt specific rows of data
- B) To filter rows returned by queries based on the role or attributes of the querying user
- C) To prevent INSERT operations on specific rows
- D) To audit which rows are accessed by users

**Answer: B) To filter rows returned by queries based on the role or attributes of the querying user**
**Explanation:** Row Access Policies determine which rows a user can see when querying a table or view. The policy uses conditions (often based on the querying role via CURRENT_ROLE()) to filter rows dynamically at query time without modifying the underlying data.

---

### Question 24
A security team wants to track all login attempts, including failed ones, for the past 365 days. Which Snowflake feature should they use?

- A) INFORMATION_SCHEMA.LOGIN_HISTORY
- B) ACCOUNT_USAGE.LOGIN_HISTORY
- C) QUERY_HISTORY view
- D) ACCESS_HISTORY view

**Answer: B) ACCOUNT_USAGE.LOGIN_HISTORY**
**Explanation:** ACCOUNT_USAGE.LOGIN_HISTORY retains data for 365 days and includes both successful and failed login attempts. INFORMATION_SCHEMA.LOGIN_HISTORY only retains data for 7 days. QUERY_HISTORY tracks queries, not logins, and ACCESS_HISTORY tracks data access patterns.

---

### Question 25
Which Snowflake feature provides end-to-end encryption where data is encrypted before leaving the client?

- A) Client-side encryption with PUT command
- B) TLS 1.2
- C) Internal stage encryption
- D) SSL certificates

**Answer: A) Client-side encryption with PUT command**
**Explanation:** When using the PUT command to upload files, Snowflake encrypts data on the client side (using AES-256) before transmission. Combined with TLS for transit and AES-256 at rest, this provides end-to-end encryption. TLS alone only protects data in transit.

---

### Question 26
What is the key difference between ACCOUNT_USAGE and INFORMATION_SCHEMA views?

- A) ACCOUNT_USAGE is real-time; INFORMATION_SCHEMA has latency
- B) INFORMATION_SCHEMA is real-time with 7-day retention; ACCOUNT_USAGE has 45-min to 3-hour latency with up to 365-day retention
- C) ACCOUNT_USAGE is only available to ACCOUNTADMIN; INFORMATION_SCHEMA is available to all
- D) They contain identical data with different retention periods only

**Answer: B) INFORMATION_SCHEMA is real-time with 7-day retention; ACCOUNT_USAGE has 45-min to 3-hour latency with up to 365-day retention**
**Explanation:** INFORMATION_SCHEMA provides near real-time data but only retains 7 days (or less for some views). ACCOUNT_USAGE has a latency of 45 minutes to 3 hours but retains data for up to 365 days. ACCOUNT_USAGE also includes dropped objects, while INFORMATION_SCHEMA does not.

---

### Question 27
A data masking policy is applied to a column. If a user with an authorized role queries the column through a view, what happens?

- A) The masking policy is bypassed because the view owner has access
- B) The masking policy is always enforced based on the querying user's role
- C) The masking policy only applies to direct table queries
- D) The view must have its own separate masking policy

**Answer: B) The masking policy is always enforced based on the querying user's role**
**Explanation:** Masking policies are enforced at query execution time based on the active role of the user running the query, regardless of whether they access the column through a table or view. The policy follows the column, not the access path.

---

### Question 28
Which Snowflake feature allows organizations to classify sensitive data like PII or PHI across their account?

- A) Data Masking
- B) Object Tagging and Data Classification
- C) Information Schema
- D) Access History

**Answer: B) Object Tagging and Data Classification**
**Explanation:** Snowflake's Object Tagging allows you to tag columns and objects with classification metadata (e.g., PII, PHI, CONFIDENTIAL). The automated Data Classification feature can scan columns and suggest/apply sensitivity tags. These tags can then be used to drive masking policies.

---

### Question 29
What encryption is used for data in transit in Snowflake?

- A) AES-256
- B) TLS 1.2
- C) SSH tunneling
- D) IPSec VPN

**Answer: B) TLS 1.2**
**Explanation:** All data transmitted between clients and Snowflake, and between Snowflake components, is encrypted using TLS 1.2 (Transport Layer Security). AES-256 is used for data at rest, not in transit. Snowflake does not require VPN or SSH tunnels.

---

### Question 30
A company needs to ensure that users connecting from a specific office IP range can access Snowflake, while all other IPs are blocked. How should this be configured?

- A) Create a network policy with the office IP range in the allowed list only
- B) Create a network policy with all non-office IPs in the blocked list
- C) Configure a firewall rule in the cloud provider
- D) Use a VPN connection exclusively

**Answer: A) Create a network policy with the office IP range in the allowed list only**
**Explanation:** Network policies use an allowed list (ALLOWED_IP_LIST) approach. When you specify allowed IPs, all other IPs are implicitly blocked. You only need to add the office IP range to the allowed list. The blocked list (BLOCKED_IP_LIST) is used for exceptions within an allowed range.

---

### Question 31
Which edition of Snowflake supports Private Link connectivity?

- A) Standard
- B) Enterprise
- C) Business Critical or higher
- D) All editions

**Answer: C) Business Critical or higher**
**Explanation:** AWS PrivateLink, Azure Private Link, and Google Cloud Private Service Connect are features available only in Business Critical edition and above. These allow private connectivity from your VPC/VNet to Snowflake without traversing the public internet.

---

### Question 32
What does Snowflake's periodic rekeying of encrypted data accomplish?

- A) It re-encrypts data with new keys on a regular schedule to limit exposure from compromised keys
- B) It rotates user passwords
- C) It refreshes TLS certificates
- D) It regenerates access tokens

**Answer: A) It re-encrypts data with new keys on a regular schedule to limit exposure from compromised keys**
**Explanation:** Snowflake automatically rotates encryption keys and periodically re-encrypts data (rekeying). If a key were ever compromised, the exposure window is limited because data is regularly re-encrypted with new keys. Business Critical edition supports annual rekeying by default.

---

### Question 33
A security analyst needs to find out which tables a user queried in the last 90 days. Which view provides this information?

- A) ACCOUNT_USAGE.QUERY_HISTORY
- B) ACCOUNT_USAGE.ACCESS_HISTORY
- C) INFORMATION_SCHEMA.QUERY_HISTORY
- D) ACCOUNT_USAGE.LOGIN_HISTORY

**Answer: B) ACCOUNT_USAGE.ACCESS_HISTORY**
**Explanation:** ACCESS_HISTORY provides detailed information about what data objects (tables, views, columns) were accessed by queries, including read and write operations. While QUERY_HISTORY shows queries executed, ACCESS_HISTORY specifically tracks which base tables were accessed, making it ideal for data access auditing.

---

### Question 34
Which statement about external tokenization in Snowflake is correct?

- A) It requires Standard edition
- B) It uses external functions to tokenize and detokenize data with a third-party service
- C) It is a built-in feature requiring no external services
- D) It replaces the need for masking policies

**Answer: B) It uses external functions to tokenize and detokenize data with a third-party service**
**Explanation:** External tokenization in Snowflake leverages external functions that call third-party tokenization services (like Protegrity or Voltage). Data is sent to the external service for tokenization/detokenization. This requires Enterprise edition and external function configuration.

---

## 2.3 Resource Monitors & Account Management

### Question 35
What are the possible actions a resource monitor can take when a credit quota threshold is reached?

- A) Notify only, Notify and Suspend, Notify and Suspend Immediately
- B) Notify, Suspend, Terminate
- C) Alert, Pause, Kill
- D) Warn, Throttle, Stop

**Answer: A) Notify only, Notify and Suspend, Notify and Suspend Immediately**
**Explanation:** Resource monitors support three actions: Notify (sends an alert only), Notify & Suspend (sends alert and suspends after current queries finish), and Notify & Suspend Immediately (sends alert and kills running queries immediately). These can be set at different percentage thresholds.

---

### Question 36
At what level(s) can resource monitors be applied?

- A) Account level only
- B) Warehouse level only
- C) Account level and/or warehouse level
- D) Account, warehouse, and database levels

**Answer: C) Account level and/or warehouse level**
**Explanation:** Resource monitors can be set at the account level (monitoring total credit consumption across all warehouses) and/or at individual warehouse levels. They cannot be applied at the database or schema level. A warehouse can only have one resource monitor assigned.

---

### Question 37
Which role is required to create a resource monitor?

- A) SYSADMIN
- B) ACCOUNTADMIN
- C) SECURITYADMIN
- D) Any role with CREATE RESOURCE MONITOR privilege

**Answer: B) ACCOUNTADMIN**
**Explanation:** Only ACCOUNTADMIN can create resource monitors. Unlike many other objects, the CREATE RESOURCE MONITOR privilege cannot be granted to other roles. However, ACCOUNTADMIN can grant the MODIFY and MONITOR privileges on existing resource monitors to other roles.

---

### Question 38
A resource monitor is set to "Suspend Immediately" at 100% quota. What happens to queries currently executing when the threshold is hit?

- A) They are allowed to complete before the warehouse suspends
- B) They are immediately terminated/aborted
- C) They are paused and can be resumed later
- D) They are moved to a different warehouse

**Answer: B) They are immediately terminated/aborted**
**Explanation:** "Suspend Immediately" kills all currently running queries and statements on the warehouse without waiting for them to complete. This is the most aggressive action. In contrast, "Suspend" (without "Immediately") allows running queries to finish before suspending the warehouse.

---

### Question 39
Which statement about Snowflake editions is TRUE?

- A) Standard edition supports Multi-Factor Authentication (MFA)
- B) Only Enterprise edition supports Time Travel
- C) Standard edition supports up to 90 days of Time Travel
- D) Data Sharing requires Enterprise edition

**Answer: A) Standard edition supports Multi-Factor Authentication (MFA)**
**Explanation:** MFA is available in all Snowflake editions including Standard. Time Travel is available in all editions but Standard is limited to 0-1 days retention, while Enterprise and above support up to 90 days. Data Sharing is available in all editions.

---

### Question 40
An organization wants to monitor credit usage across all warehouses in their account. The monitor should send an alert at 75% and suspend all warehouses at 100%. How many resource monitors are needed?

- A) One resource monitor at the account level
- B) One resource monitor per warehouse
- C) Two resource monitors — one for alerting and one for suspension
- D) One resource monitor at the account level and one per warehouse

**Answer: A) One resource monitor at the account level**
**Explanation:** A single account-level resource monitor can track total credit consumption across all warehouses. It can have multiple thresholds — a notify action at 75% and a suspend action at 100% — within the same resource monitor. Separate monitors per warehouse are only needed for per-warehouse quotas.

---

### Question 41
What is the maximum Time Travel retention period available in Enterprise edition?

- A) 1 day
- B) 30 days
- C) 90 days
- D) 365 days

**Answer: C) 90 days**
**Explanation:** Enterprise edition (and higher) supports Time Travel retention of up to 90 days for permanent tables. Standard edition is limited to 0 or 1 day. Transient and temporary tables are limited to 0 or 1 day regardless of edition.

---

### Question 42
Which parameter controls how long Snowflake retains historical data for Time Travel?

- A) MAX_DATA_RETENTION_TIME_IN_DAYS
- B) DATA_RETENTION_TIME_IN_DAYS
- C) TIME_TRAVEL_RETENTION_DAYS
- D) HISTORY_RETENTION_PERIOD

**Answer: B) DATA_RETENTION_TIME_IN_DAYS**
**Explanation:** DATA_RETENTION_TIME_IN_DAYS is the parameter that controls Time Travel retention. It can be set at the account, database, schema, or table level. Values range from 0 to 1 for Standard edition and 0 to 90 for Enterprise and above.

---

### Question 43
A Snowflake account has been running for two weeks. The admin wants to analyze warehouse credit usage patterns over this entire period. Which approach is correct?

- A) Query INFORMATION_SCHEMA.WAREHOUSE_METERING_HISTORY
- B) Query ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY
- C) Check the resource monitor dashboard
- D) Query INFORMATION_SCHEMA.WAREHOUSE_LOAD_HISTORY

**Answer: B) ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY**
**Explanation:** ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY retains data for 365 days, making it suitable for analyzing two weeks of history. INFORMATION_SCHEMA.WAREHOUSE_METERING_HISTORY only retains data for 14 days (and may not cover the full period) and does not include older data reliably.

---

### Question 44
What is the Fail-safe period in Snowflake and can it be configured?

- A) 7 days, non-configurable, available in all editions
- B) 7 days, configurable up to 30 days in Enterprise
- C) 1 day, non-configurable
- D) 7 days, non-configurable, only for permanent tables

**Answer: D) 7 days, non-configurable, only for permanent tables**
**Explanation:** Fail-safe provides a non-configurable 7-day period after Time Travel expires for data recovery by Snowflake support. It applies only to permanent tables — transient and temporary tables do not have Fail-safe. It cannot be disabled or modified by users.

---

### Question 45
An account administrator wants to set up billing alerts before the monthly budget is exceeded. Which feature should they configure?

- A) Resource monitors at the account level
- B) Billing alerts in the cloud provider console
- C) Budget feature in Snowsight
- D) Credit usage notifications via email

**Answer: A) Resource monitors at the account level**
**Explanation:** Account-level resource monitors are the primary mechanism for tracking and alerting on credit consumption. They can be configured with notify-only thresholds at various percentages (e.g., 50%, 75%, 90%) to provide early warnings before the budget is fully consumed.

---

### Question 46
Which view would you query to find information about roles and their granted privileges in the last 30 days, including deleted roles?

- A) INFORMATION_SCHEMA.APPLICABLE_ROLES
- B) ACCOUNT_USAGE.GRANTS_TO_ROLES
- C) INFORMATION_SCHEMA.TABLE_PRIVILEGES
- D) SHOW GRANTS command

**Answer: B) ACCOUNT_USAGE.GRANTS_TO_ROLES**
**Explanation:** ACCOUNT_USAGE.GRANTS_TO_ROLES includes historical data for up to 365 days, including information about grants that involved roles which have since been dropped. INFORMATION_SCHEMA views only show current state and have 7-day retention. SHOW GRANTS only shows current grants.

---

### Question 47
A multi-cluster warehouse is configured with MIN_CLUSTER_COUNT=1 and MAX_CLUSTER_COUNT=3. At what point does Snowflake start an additional cluster?

- A) When CPU usage exceeds 80%
- B) When queries begin queueing due to insufficient resources
- C) When active queries exceed 8
- D) At a fixed time schedule

**Answer: B) When queries begin queueing due to insufficient resources**
**Explanation:** Snowflake's auto-scaling for multi-cluster warehouses starts additional clusters when queries begin queueing because the existing clusters cannot handle the concurrent load. The scaling policy (Standard or Economy) determines how aggressively new clusters are added.

---

### Question 48
Which Snowflake feature is ONLY available in Business Critical edition and above?

- A) Multi-Factor Authentication
- B) Data Masking
- C) Database failover and replication for disaster recovery
- D) Resource Monitors

**Answer: C) Database failover and replication for disaster recovery**
**Explanation:** Database failover and replication (business continuity features) require Business Critical edition or higher. MFA is available in all editions, Data Masking requires Enterprise, and Resource Monitors are available in all editions. Business Critical adds HIPAA/PCI-DSS compliance, Tri-Secret Secure, and failover/replication.

---

### Question 49
An administrator notices that the SNOWFLAKE database is visible in the account. What is the purpose of this database?

- A) It stores temporary query results
- B) It is a shared database containing ACCOUNT_USAGE, ORGANIZATION_USAGE, and other metadata schemas
- C) It is where user data is stored by default
- D) It contains backup copies of all databases

**Answer: B) It is a shared database containing ACCOUNT_USAGE, ORGANIZATION_USAGE, and other metadata schemas**
**Explanation:** The SNOWFLAKE database is a system-provided, read-only shared database that contains schemas like ACCOUNT_USAGE, ORGANIZATION_USAGE, DATA_SHARING_USAGE, and READER_ACCOUNT_USAGE. It provides administrative and monitoring views about account activity.

---

### Question 50
What is the relationship between a resource monitor's credit quota and the monitoring interval?

- A) The quota resets based on the configured frequency (daily, weekly, monthly, yearly, or never)
- B) The quota is always monthly
- C) The quota accumulates and never resets
- D) The quota resets daily at midnight

**Answer: A) The quota resets based on the configured frequency (daily, weekly, monthly, yearly, or never)**
**Explanation:** Resource monitors have a configurable frequency that determines when the credit quota resets. Options include Daily, Weekly, Monthly, Yearly, and Never (the quota is a one-time lifetime limit). The start date and frequency together determine the reset schedule.

---

### Question 51
A Snowflake administrator needs to audit all DDL changes (CREATE, ALTER, DROP) made in the last 60 days. Which view should they query?

- A) ACCOUNT_USAGE.QUERY_HISTORY with query_type filter
- B) INFORMATION_SCHEMA.QUERY_HISTORY
- C) ACCOUNT_USAGE.ACCESS_HISTORY
- D) ACCOUNT_USAGE.OBJECT_PRIVILEGES

**Answer: A) ACCOUNT_USAGE.QUERY_HISTORY with query_type filter**
**Explanation:** ACCOUNT_USAGE.QUERY_HISTORY retains data for 365 days and includes a QUERY_TYPE column that can be filtered for DDL operations like CREATE_TABLE, ALTER_TABLE, DROP_TABLE, etc. INFORMATION_SCHEMA.QUERY_HISTORY only retains 7 days, insufficient for 60 days of history.

---

### Question 52
Which scenario correctly describes privilege inheritance through the role hierarchy?

- A) SYSADMIN automatically inherits all custom roles' privileges
- B) If CUSTOM_ROLE is granted to SYSADMIN, then ACCOUNTADMIN also inherits CUSTOM_ROLE privileges
- C) Privilege inheritance only flows downward in the hierarchy
- D) ACCOUNTADMIN must be explicitly granted each custom role

**Answer: B) If CUSTOM_ROLE is granted to SYSADMIN, then ACCOUNTADMIN also inherits CUSTOM_ROLE privileges**
**Explanation:** Privilege inheritance flows upward through the role hierarchy. If CUSTOM_ROLE is granted to SYSADMIN, and SYSADMIN is granted to ACCOUNTADMIN (which is the default hierarchy), then ACCOUNTADMIN inherits CUSTOM_ROLE's privileges transitively. Snowflake recommends granting all custom roles to SYSADMIN for this reason.

---

### Question 53
A network policy has ALLOWED_IP_LIST = '192.168.1.0/24' and BLOCKED_IP_LIST = '192.168.1.100'. What happens when a user connects from 192.168.1.100?

- A) The connection is allowed because 192.168.1.100 is within the allowed range
- B) The connection is blocked because the blocked list takes precedence over the allowed list
- C) An error occurs due to conflicting rules
- D) The connection is allowed on the first attempt and blocked on subsequent attempts

**Answer: B) The connection is blocked because the blocked list takes precedence over the allowed list**
**Explanation:** In Snowflake network policies, the BLOCKED_IP_LIST is used to define exceptions within the ALLOWED_IP_LIST range. The blocked list takes precedence — any IP in both lists is blocked. This allows broad CIDR ranges in the allowed list with specific exclusions.

---

### Question 54
Which statement about transient tables in Snowflake is TRUE?

- A) They support up to 90 days of Time Travel in Enterprise edition
- B) They have a Fail-safe period of 7 days
- C) They support 0 or 1 day of Time Travel and have no Fail-safe period
- D) They are automatically dropped when the session ends

**Answer: C) They support 0 or 1 day of Time Travel and have no Fail-safe period**
**Explanation:** Transient tables are designed to reduce storage costs for data that doesn't need full protection. They support only 0 or 1 day of Time Travel (regardless of edition) and have no Fail-safe period. They persist until explicitly dropped (unlike temporary tables which are session-scoped).

---

### Question 55
What is the recommended best practice for ACCOUNTADMIN usage in Snowflake?

- A) Use it as the default role for daily operations
- B) Limit its use to administrative tasks, enable MFA, and assign it to at least two users
- C) Only one user should ever have ACCOUNTADMIN
- D) ACCOUNTADMIN should be disabled and replaced with custom roles

**Answer: B) Limit its use to administrative tasks, enable MFA, and assign it to at least two users**
**Explanation:** Snowflake recommends that ACCOUNTADMIN should not be used for daily operations (use lower-privilege roles instead), MFA should be enabled for all ACCOUNTADMIN users, and at least two users should have the role to prevent lockout scenarios. It should not be the default role for any user.

---
