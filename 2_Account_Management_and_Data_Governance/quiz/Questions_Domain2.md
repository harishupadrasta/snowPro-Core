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

## Bonus: Advanced Scenario Questions

### Question 56
A company sets a resource monitor to "Suspend" at 90% and "Suspend Immediately" at 100%. At 91% usage, a critical 2-hour data pipeline is running. What happens to the pipeline?

- A) The pipeline is killed immediately at 91%
- B) The pipeline is allowed to complete because "Suspend" (not "Immediately") lets running queries finish before suspending
- C) The pipeline is paused and can be resumed later
- D) Nothing happens until 100% is reached

**Answer: B) The pipeline is allowed to complete because "Suspend" (not "Immediately") lets running queries finish before suspending**

**Explanation:** "Suspend" allows currently executing queries to complete before suspending the warehouse — no new queries are accepted but running ones finish. "Suspend Immediately" kills all running queries without waiting. At 91% (the "Suspend" threshold), the pipeline continues running. Only at 100% ("Suspend Immediately") would it be terminated mid-execution.

**Exam Trap:** The exam tests the difference between Suspend (graceful — running queries finish) and Suspend Immediately (aggressive — kills running queries).

---

### Question 57
An admin creates a network policy with ALLOWED_IP_LIST = '10.0.0.0/8' and assigns it to the account. They also create a user-level policy for user "JOE" with ALLOWED_IP_LIST = '192.168.1.0/24'. Joe connects from 192.168.1.50. What happens?

- A) Joe connects successfully because his user-level policy allows the IP
- B) Joe is BLOCKED because the account-level policy does not include 192.168.1.0/24, and account-level takes precedence
- C) Both policies are evaluated and the most permissive wins
- D) An error occurs because you cannot have both account and user-level policies

**Answer: B) Joe is BLOCKED because the account-level policy does not include 192.168.1.0/24, and account-level takes precedence**

**Explanation:** When both account-level and user-level network policies exist, the MOST RESTRICTIVE combination applies. The connection must satisfy BOTH policies. Since 192.168.1.50 is not within the account-level 10.0.0.0/8 (it is, actually — 192.168.x.x IS within 10.0.0.0/8... Wait, no — 192.168.x.x is NOT in 10.0.0.0/8). The account-level blocks anything outside 10.0.0.0/8, so Joe is blocked regardless of his user-level policy.

**Exam Trap:** Account-level network policy blocks override user-level allows. The user must pass BOTH checks.

---

### Question 58
A developer creates a table using the ANALYST role. Later, the Security team wants to apply a masking policy to a column. The SECURITYADMIN attempts to apply the policy but gets a privilege error. What is the issue?

- A) Only ACCOUNTADMIN can apply masking policies
- B) SECURITYADMIN needs APPLY MASKING POLICY privilege on the account or OWNERSHIP of the table
- C) Masking policies can only be applied at table creation time
- D) The ANALYST role must grant permission first

**Answer: B) SECURITYADMIN needs APPLY MASKING POLICY privilege on the account or OWNERSHIP of the table**

**Explanation:** Applying a masking policy requires either OWNERSHIP on the target table/column, or the APPLY MASKING POLICY account-level privilege. SECURITYADMIN doesn't inherently have this — it must be explicitly granted. This is a common oversight since SECURITYADMIN handles security but doesn't automatically have all security-related privileges.

**Exam Trap:** SECURITYADMIN manages grants but doesn't automatically have APPLY MASKING POLICY — it must be explicitly granted.

---

### Question 59
A company's custom role hierarchy has ANALYST_ROLE granted to SYSADMIN. A new data engineer creates objects using ENGINEER_ROLE but forgets to grant it to SYSADMIN. What problem does this create?

- A) The objects are inaccessible to ACCOUNTADMIN
- B) ACCOUNTADMIN can access the objects but SYSADMIN cannot, breaking the recommended role hierarchy and making object management difficult
- C) The objects are automatically dropped after 24 hours
- D) No problem — all custom roles are automatically granted to SYSADMIN

**Answer: B) ACCOUNTADMIN can access the objects but SYSADMIN cannot, breaking the recommended role hierarchy and making object management difficult**

**Explanation:** Snowflake recommends ALL custom roles be granted to SYSADMIN so that ACCOUNTADMIN (which inherits SYSADMIN) has full visibility. If ENGINEER_ROLE isn't in the hierarchy, SYSADMIN cannot manage those objects without explicitly switching roles. ACCOUNTADMIN can still access them via the MANAGE GRANTS privilege, but the broken hierarchy creates administrative complexity.

**Exam Trap:** Always grant custom roles to SYSADMIN — this is a best practice the exam tests frequently.

---

### Question 60
A table has DATA_RETENTION_TIME_IN_DAYS = 90. An admin changes it to 0. What happens to the existing 90 days of historical data?

- A) The historical data is retained for the original 90 days then purged
- B) All historical Time Travel data is immediately purged — past versions are no longer accessible
- C) The change only affects new modifications going forward
- D) The system prevents reducing retention to 0 if historical data exists

**Answer: B) All historical Time Travel data is immediately purged — past versions are no longer accessible**

**Explanation:** Setting DATA_RETENTION_TIME_IN_DAYS = 0 immediately purges all existing Time Travel history for that table. Historical micro-partition versions are marked for deletion. You cannot query past states or recover accidentally deleted data. This is irreversible and the exam specifically tests this behavior.

**Exam Trap:** Setting retention to 0 IMMEDIATELY purges history — it doesn't grandfather existing data. This is a destructive operation.

---

### Question 61
A reader account is created by Provider A for Consumer X. Consumer X tries to access a Marketplace listing from Provider B through the reader account. What happens?

- A) The access works because reader accounts can access any public listing
- B) The access is DENIED — reader accounts can ONLY access data shared by the provider that created them
- C) The access works if Provider B approves it
- D) Consumer X can access it but must pay Provider A for the compute

**Answer: B) The access is DENIED — reader accounts can ONLY access data shared by the provider that created them**

**Explanation:** Reader accounts (managed accounts) are restricted to accessing data shared by the single provider that created them. They cannot access other shares, Marketplace listings, or any other data. The provider also pays for all compute costs in the reader account. This is a frequently tested constraint.

**Exam Trap:** Reader accounts are locked to ONE provider's data. All compute costs are billed to the provider, not the reader account.

---

### Question 62
A Snowflake account has MFA enabled for all users. An ACCOUNTADMIN user loses their MFA device. They contact support, who resets their MFA. During the reset window, the user logs in without MFA. The security team is auditing logins. Which view shows this login without MFA?

- A) ACCOUNT_USAGE.LOGIN_HISTORY — it records whether MFA was used for each login
- B) INFORMATION_SCHEMA.LOGIN_HISTORY with an MFA filter
- C) ACCOUNT_USAGE.SESSIONS view
- D) This information is not tracked by Snowflake

**Answer: A) ACCOUNT_USAGE.LOGIN_HISTORY — it records whether MFA was used for each login**

**Explanation:** ACCOUNT_USAGE.LOGIN_HISTORY includes details about each authentication event including whether second-factor (MFA) was used. It retains data for 365 days. INFORMATION_SCHEMA.LOGIN_HISTORY only retains 7 days. The SECOND_AUTHENTICATION_FACTOR column shows if MFA was applied for each login.

**Exam Trap:** ACCOUNT_USAGE.LOGIN_HISTORY = 365 days, includes MFA status. INFORMATION_SCHEMA = only 7 days.

---

### Question 63
A managed access schema contains tables owned by various roles. A table owner (ANALYST_ROLE) tries to grant SELECT on their table to another role. What happens?

- A) The grant succeeds — owners can always grant access to their objects
- B) The grant FAILS — in managed access schemas, only the schema owner or a role with MANAGE GRANTS can grant privileges
- C) The grant succeeds but requires approval from ACCOUNTADMIN
- D) The grant succeeds but is logged as a security event

**Answer: B) The grant FAILS — in managed access schemas, only the schema owner or a role with MANAGE GRANTS can grant privileges**

**Explanation:** Managed access schemas (WITH MANAGED ACCESS) centralize grant control. Object owners CANNOT grant access to their own objects. Only the schema owner or roles with MANAGE GRANTS privilege can manage privileges within the schema. This provides tighter security governance.

**Exam Trap:** Managed access = owners lose grant ability. Know the difference between regular schemas (owners can grant) and managed access schemas.

---

### Question 64
A company's Snowflake bill shows unexpected Fail-safe storage charges. They have many ETL staging tables that are modified hourly. The tables are permanent. How can they reduce Fail-safe costs?

- A) Set DATA_RETENTION_TIME_IN_DAYS = 0 on staging tables
- B) Convert staging tables to TRANSIENT tables, which have no Fail-safe period
- C) Drop and recreate tables daily
- D) Compress the data more aggressively

**Answer: B) Convert staging tables to TRANSIENT tables, which have no Fail-safe period**

**Explanation:** Transient tables have NO Fail-safe period (and max 1-day Time Travel). For ETL staging data that is frequently modified and doesn't need disaster recovery protection, transient tables eliminate the 7-day Fail-safe storage overhead. Fail-safe is non-configurable for permanent tables — you cannot disable it.

**Exam Trap:** Fail-safe is NON-CONFIGURABLE and applies ONLY to permanent tables. Transient/temporary tables have NO Fail-safe.

---

### Question 65
A row access policy uses `CURRENT_ROLE()` to filter rows. A user has multiple roles but their primary (current) role is ANALYST. They switch to MANAGER role mid-session. How does the row access policy behave?

- A) The policy uses ANALYST (the role at session start) for the entire session
- B) The policy dynamically evaluates based on the CURRENT active role — switching to MANAGER immediately changes visible rows
- C) The policy uses ALL roles the user possesses
- D) Role switching requires the policy to be re-attached

**Answer: B) The policy dynamically evaluates based on the CURRENT active role — switching to MANAGER immediately changes visible rows**

**Explanation:** Row access policies using CURRENT_ROLE() evaluate at query execution time, not session start. When the user switches their active role (USE ROLE MANAGER), subsequent queries are evaluated with the MANAGER role, potentially showing different rows. This is dynamic and immediate.

**Exam Trap:** Policies evaluate at QUERY TIME with the CURRENT role, not at session creation time.

---

### Question 66
A company wants to track which specific columns were read by each query for GDPR compliance. Which ACCOUNT_USAGE view provides column-level access tracking?

- A) QUERY_HISTORY
- B) ACCESS_HISTORY
- C) COLUMNS view
- D) LOGIN_HISTORY

**Answer: B) ACCESS_HISTORY**

**Explanation:** ACCOUNT_USAGE.ACCESS_HISTORY provides column-level data access tracking — it records which base tables AND specific columns were accessed (read/written) by each query. This is critical for GDPR Article 30 (records of processing) compliance. QUERY_HISTORY shows the SQL but not which specific columns were accessed.

**Exam Trap:** ACCESS_HISTORY = column-level tracking (what data was accessed). QUERY_HISTORY = statement-level (what SQL was run).

---

### Question 67
An admin creates a resource monitor with credit_quota = 100 and frequency = MONTHLY. On the 15th of the month, usage is at 95 credits. The admin increases the quota to 200. What happens to the current month's usage tracking?

- A) Usage resets to 0 with the new quota
- B) Current usage (95) is retained — the account now has 105 credits remaining this month
- C) The change takes effect only next month
- D) The monitor must be dropped and recreated

**Answer: B) Current usage (95) is retained — the account now has 105 credits remaining this month**

**Explanation:** Modifying a resource monitor's quota takes effect immediately without resetting the usage counter. The 95 credits already consumed still count. With the new 200-credit quota, there are 105 credits remaining. This allows mid-cycle adjustments without losing tracking. The quota resets at the next frequency boundary (month start).

**Exam Trap:** Changing quota doesn't reset usage — it's a live adjustment to the ceiling, not a counter reset.

---

### Question 68
A data masking policy is defined as: if CURRENT_ROLE() IN ('ADMIN', 'ANALYST') return val, else return '***'. A view is created by ADMIN that selects the masked column. When PUBLIC queries the view, what do they see?

- A) The actual values because the view was created by ADMIN
- B) '***' because the masking policy evaluates based on the QUERYING user's role, not the view owner
- C) An error because PUBLIC cannot query masked columns
- D) NULL values

**Answer: B) '***' because the masking policy evaluates based on the QUERYING user's role, not the view owner**

**Explanation:** Masking policies are always evaluated at query execution time using the CURRENT_ROLE() of the person RUNNING the query, not the view/object owner. PUBLIC role is not in the allowed list ('ADMIN', 'ANALYST'), so they see masked values. This prevents privilege escalation through views.

**Exam Trap:** Masking policies evaluate with the QUERYING user's role, never the object owner's role. Views don't bypass masking.

---

### Question 69
An organization needs to prevent their ACCOUNTADMIN from accidentally running expensive queries on production data during daily work. What is the best practice?

- A) Remove USAGE privilege on all warehouses from ACCOUNTADMIN
- B) Users with ACCOUNTADMIN should set a LOWER default role (like SYSADMIN or a custom role) for daily work, using ACCOUNTADMIN only for administrative tasks
- C) Create a network policy blocking ACCOUNTADMIN access
- D) Set ACCOUNTADMIN's default warehouse to X-Small

**Answer: B) Users with ACCOUNTADMIN should set a LOWER default role (like SYSADMIN or a custom role) for daily work, using ACCOUNTADMIN only for administrative tasks**

**Explanation:** Snowflake best practice is to NEVER use ACCOUNTADMIN as a default daily role. Users should have a lower-privilege default role for routine work and switch to ACCOUNTADMIN only for administrative tasks. This follows the principle of least privilege and prevents accidental operations with the highest-privilege role.

**Exam Trap:** ACCOUNTADMIN best practices: don't use as default role, enable MFA, assign to 2+ users, use only for admin tasks.

---

### Question 70
A security team wants to classify all columns containing email addresses across the entire account. Which Snowflake feature automatically detects and tags sensitive data?

- A) Object Tagging with manual TAG assignment
- B) Data Classification — Snowflake's automatic classification system that suggests sensitivity categories
- C) Masking Policy auto-discovery
- D) Information Schema column scanning

**Answer: B) Data Classification — Snowflake's automatic classification system that suggests sensitivity categories**

**Explanation:** Snowflake's Data Classification feature (Enterprise+) uses system-defined classifiers to automatically analyze column data and suggest sensitivity tags (SEMANTIC_CATEGORY and PRIVACY_CATEGORY). It can identify emails, phone numbers, SSNs, and other PII patterns. Object Tagging is the mechanism to apply tags, but Classification automates the discovery.

**Exam Trap:** Data Classification = automatic detection. Object Tagging = the mechanism to apply/manage tags. They work together.

---

### Question 71
A warehouse has a resource monitor set to "Notify and Suspend" at 100%. The warehouse is used for Snowpipe loading. What happens to Snowpipe when the warehouse is suspended?

- A) Snowpipe stops loading and files queue up
- B) Snowpipe is NOT affected — it uses Snowflake-managed serverless compute, not customer warehouses
- C) Snowpipe continues using the suspended warehouse in a special mode
- D) Files are rejected and must be re-staged

**Answer: B) Snowpipe is NOT affected — it uses Snowflake-managed serverless compute, not customer warehouses**

**Explanation:** Snowpipe uses Snowflake's serverless compute model, completely independent of customer virtual warehouses. Resource monitors that suspend warehouses have NO impact on Snowpipe loading. Snowpipe costs appear as separate line items, not as warehouse credit consumption.

**Exam Trap:** Snowpipe = serverless (Snowflake-managed compute). Resource monitors on warehouses don't affect Snowpipe.

---

### Question 72
An admin sets a database-level DATA_RETENTION_TIME_IN_DAYS = 30. A table within that database has DATA_RETENTION_TIME_IN_DAYS = 1. What is the effective retention for that table?

- A) 30 days — database setting overrides table setting
- B) 1 day — the table-level setting takes precedence (most specific wins)
- C) 31 days — they are additive
- D) Whichever is higher (30 days) to ensure maximum protection

**Answer: B) 1 day — the table-level setting takes precedence (most specific wins)**

**Explanation:** Snowflake follows the principle of most-specific-wins for DATA_RETENTION_TIME_IN_DAYS. Object-level settings override container-level settings. The hierarchy is: Account → Database → Schema → Table, with each lower level able to override the parent. The table's explicit 1-day setting overrides the database's 30-day default.

**Exam Trap:** Retention settings follow most-specific-wins hierarchy. Table setting > Schema > Database > Account.

---

### Question 73
A company creates a custom role called DATA_LOADER and grants it CREATE TABLE, INSERT, and USAGE on a schema. They also grant EXECUTE TASK at the account level. A task created by DATA_LOADER fails with "insufficient privileges." What's missing?

- A) The DATA_LOADER role needs OWNERSHIP of the warehouse
- B) The DATA_LOADER role needs USAGE on the warehouse specified in the task definition
- C) Only SYSADMIN can run tasks
- D) The task needs to be created by ACCOUNTADMIN

**Answer: B) The DATA_LOADER role needs USAGE on the warehouse specified in the task definition**

**Explanation:** To run a task, the role needs: CREATE TASK on schema, EXECUTE TASK at account level, AND USAGE on the warehouse the task uses. Missing USAGE on the warehouse means the task cannot acquire compute resources. This is a common oversight — EXECUTE TASK alone is insufficient without warehouse access.

**Exam Trap:** Tasks need THREE things to run: CREATE TASK (schema), EXECUTE TASK (account), and USAGE (warehouse).

---

### Question 74
A HIPAA-regulated healthcare company discovers that their account is running on Enterprise edition. They store PHI (Protected Health Information) in Snowflake. Their compliance officer is concerned. What is the correct assessment?

- A) Enterprise edition is compliant for HIPAA as long as data is encrypted
- B) They must upgrade to Business Critical edition — Snowflake's BAA (Business Associate Agreement) and HIPAA support require Business Critical or higher
- C) Any edition is HIPAA compliant because all editions use AES-256 encryption
- D) They need Virtual Private Snowflake for HIPAA

**Answer: B) They must upgrade to Business Critical edition — Snowflake's BAA (Business Associate Agreement) and HIPAA support require Business Critical or higher**

**Explanation:** HIPAA compliance in Snowflake requires Business Critical edition. This is because HIPAA requires a Business Associate Agreement (BAA), which Snowflake only offers for Business Critical and VPS editions. Enterprise edition, despite having encryption, does not include the compliance certifications or BAA needed for HIPAA.

**Exam Trap:** HIPAA/PCI-DSS/HITRUST compliance = Business Critical minimum. Encryption alone doesn't equal compliance.

---

### Question 75
An admin accidentally grants ACCOUNTADMIN to a service account (used for ETL pipelines). The service account has key-pair authentication but no MFA. What security risks does this create, and what's the immediate fix?

- A) No risk — service accounts don't need MFA
- B) Critical risk: the service account has full account control with weaker authentication. Immediately REVOKE ACCOUNTADMIN from the service account and use a custom role with minimum required privileges
- C) The grant will automatically be revoked after 24 hours
- D) Enable MFA on the service account to resolve the issue

**Answer: B) Critical risk: the service account has full account control with weaker authentication. Immediately REVOKE ACCOUNTADMIN from the service account and use a custom role with minimum required privileges**

**Explanation:** Service accounts with ACCOUNTADMIN can perform any operation including dropping databases, modifying network policies, or granting privileges. Without MFA (which isn't practical for service accounts), this is a serious risk. Best practice: service accounts should use the minimum-privilege custom role needed for their specific tasks.

**Exam Trap:** Never assign ACCOUNTADMIN to service accounts. Use purpose-specific custom roles with minimum required privileges.

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

## Bonus: Advanced Scenario Questions

### Question 1
A custom role ANALYST is granted to SYSADMIN. A custom role DATA_ENGINEER is also granted to SYSADMIN. A user has only the DATA_ENGINEER role directly granted to them. Can this user access objects owned by the ANALYST role?

- A) Yes, because both roles are under SYSADMIN
- B) No, because privilege inheritance flows upward — DATA_ENGINEER does not inherit ANALYST's privileges
- C) Yes, if they switch to the SYSADMIN role
- D) Only if secondary roles are enabled

**Answer: B) No, because privilege inheritance flows upward — DATA_ENGINEER does not inherit ANALYST's privileges**
**Explanation:** Privilege inheritance flows UP the hierarchy, not sideways. SYSADMIN inherits both ANALYST and DATA_ENGINEER privileges, but DATA_ENGINEER does not inherit ANALYST's privileges (they are siblings, not parent-child). The user would need ANALYST or SYSADMIN explicitly granted to access ANALYST-owned objects.
**Exam Trap:** Sibling roles under the same parent do NOT inherit from each other — inheritance only flows upward.
---

### Question 2
A table has both a masking policy on the `salary` column and a row access policy on the table. A user queries the table. In what order are these policies evaluated?

- A) Masking policy first, then row access policy
- B) Row access policy first (filters rows), then masking policy (masks columns on remaining rows)
- C) Both are evaluated simultaneously
- D) Only one policy can be active on a table at a time

**Answer: B) Row access policy first (filters rows), then masking policy (masks columns on remaining rows)**
**Explanation:** Snowflake evaluates row access policies before masking policies. First, rows that the user is not authorized to see are filtered out. Then, on the remaining visible rows, masking policies are applied to mask column values as appropriate. This is the documented evaluation order.
**Exam Trap:** Row access policies filter FIRST, masking applies SECOND — not the reverse or simultaneously.
---

### Question 3
A network policy at the account level allows IPs in the range 10.0.0.0/8. A separate network policy on a specific user allows only 192.168.1.0/24. The user connects from 10.0.5.50. What happens?

- A) Connection succeeds because the account-level policy allows it
- B) Connection is blocked because the user-level policy takes precedence and 10.0.5.50 is not in 192.168.1.0/24
- C) Both policies must allow the connection, so it fails
- D) An error occurs due to conflicting policies

**Answer: B) Connection is blocked because the user-level policy takes precedence and 10.0.5.50 is not in 192.168.1.0/24**
**Explanation:** When both an account-level and user-level network policy exist, the user-level policy takes precedence for that specific user. The user-level policy's ALLOWED_IP_LIST is 192.168.1.0/24, and 10.0.5.50 is not in that range, so the connection is denied — regardless of what the account-level policy allows.
**Exam Trap:** User-level network policies OVERRIDE (not supplement) account-level policies for that user.
---

### Question 4
A resource monitor is set with "Suspend" action at 90% and "Suspend Immediately" at 100%. The warehouse hits 90% while executing a critical 3-hour ETL job. What happens to the ETL job?

- A) The ETL job is immediately terminated at 90%
- B) The ETL job continues to completion because "Suspend" allows running queries to finish; then the warehouse suspends
- C) The ETL job is paused and can be resumed when the quota resets
- D) The warehouse suspends after 5 minutes regardless of running queries

**Answer: B) The ETL job continues to completion because "Suspend" allows running queries to finish; then the warehouse suspends**
**Explanation:** The "Suspend" action (without "Immediately") allows all currently running queries and statements to complete before suspending the warehouse. Only "Suspend Immediately" kills running queries. However, note that the job continuing could push consumption past 100%, triggering the "Suspend Immediately" action which WOULD kill it.
**Exam Trap:** "Suspend" lets running queries finish; "Suspend Immediately" kills them — but the running query may push past the next threshold.
---

### Question 5
A data analyst needs to check how many queries were run on a specific warehouse in the last 30 days. They query INFORMATION_SCHEMA.QUERY_HISTORY but get incomplete results. Why?

- A) INFORMATION_SCHEMA.QUERY_HISTORY only retains data for 7 days
- B) The analyst doesn't have the required privileges
- C) INFORMATION_SCHEMA doesn't track warehouse-specific metrics
- D) The query history view is limited to 10,000 records

**Answer: A) INFORMATION_SCHEMA.QUERY_HISTORY only retains data for 7 days**
**Explanation:** INFORMATION_SCHEMA views have a maximum retention of 7 days (some views retain even less). For 30-day history, the analyst must use ACCOUNT_USAGE.QUERY_HISTORY, which retains data for 365 days. ACCOUNT_USAGE has 45-minute to 3-hour latency but much longer retention.
**Exam Trap:** INFORMATION_SCHEMA = 7 days max, real-time. ACCOUNT_USAGE = 365 days, with latency. Choose based on time range needed.
---

### Question 6
A DBA runs: `GRANT SELECT ON FUTURE TABLES IN SCHEMA sales.public TO ROLE analyst;`. Then a new table `sales.public.orders` is created. The analyst role can query it. Later, the DBA runs: `REVOKE SELECT ON FUTURE TABLES IN SCHEMA sales.public FROM ROLE analyst;`. Can the analyst still query `sales.public.orders`?

- A) No, revoking future grants removes access from all previously auto-granted tables
- B) Yes, revoking future grants only affects tables created AFTER the revoke — existing grants remain
- C) It depends on whether the table was created by the same role
- D) The analyst loses access after a 24-hour grace period

**Answer: B) Yes, revoking future grants only affects tables created AFTER the revoke — existing grants remain**
**Explanation:** Revoking a future grant only stops new objects from automatically receiving that grant. It does NOT retroactively remove grants that were already applied to existing objects. To remove access from tables already granted, you must explicitly revoke SELECT on those specific tables or use REVOKE SELECT ON ALL TABLES IN SCHEMA.
**Exam Trap:** REVOKE on FUTURE grants is forward-looking only — it never claws back already-applied grants.
---

### Question 7
In a managed access schema, a user creates a table using the DATA_ENGINEER role. The DATA_ENGINEER role is the object owner. Can DATA_ENGINEER grant SELECT on that table to the ANALYST role?

- A) Yes, object owners can always grant privileges on their objects
- B) No, in managed access schemas only the schema owner or MANAGE GRANTS holder can grant privileges
- C) Yes, but only if DATA_ENGINEER also owns the schema
- D) Only ACCOUNTADMIN can grant privileges in managed access schemas

**Answer: B) No, in managed access schemas only the schema owner or MANAGE GRANTS holder can grant privileges**
**Explanation:** Managed access schemas (WITH MANAGED ACCESS) centralize access control by removing the object owner's ability to grant privileges. Only the schema owner or a role with MANAGE GRANTS can grant/revoke privileges on objects in that schema. This prevents object creators from bypassing governance.
**Exam Trap:** Managed access schemas strip object owners of grant ability — this is the entire point of the feature.
---

### Question 8
A resource monitor has a monthly quota of 1000 credits. On March 15, the warehouse has consumed 950 credits. The "Notify" action is set at 90% and "Suspend Immediately" at 100%. What notifications have been sent so far?

- A) No notifications because 950 is below 100%
- B) A notification was sent when consumption crossed 900 credits (90%)
- C) Notifications are sent at both 90% (900 credits) and at 95% (950 credits)
- D) Notifications are only sent at the Suspend threshold

**Answer: B) A notification was sent when consumption crossed 900 credits (90%)**
**Explanation:** The "Notify" action at 90% sends an alert when consumption crosses 90% of the quota (900 credits). Since 950 credits exceeds this threshold, the notification has already been sent. The "Suspend Immediately" action will trigger at 1000 credits (100%). Snowflake sends the notification once per threshold crossing, not continuously.
**Exam Trap:** Notifications fire once when a threshold is CROSSED — they don't repeat or fire at intermediate values.
---

### Question 9
A security team queries ACCOUNT_USAGE.LOGIN_HISTORY and INFORMATION_SCHEMA.LOGIN_HISTORY for the same timeframe (last 2 hours). They notice ACCOUNT_USAGE is missing recent login events that appear in INFORMATION_SCHEMA. Why?

- A) ACCOUNT_USAGE doesn't track login events
- B) ACCOUNT_USAGE has a latency of up to 2 hours before data appears, while INFORMATION_SCHEMA is real-time
- C) The security team doesn't have permission to view ACCOUNT_USAGE
- D) INFORMATION_SCHEMA includes fabricated test entries

**Answer: B) ACCOUNT_USAGE has a latency of up to 2 hours before data appears, while INFORMATION_SCHEMA is real-time**
**Explanation:** ACCOUNT_USAGE views have a data latency of 45 minutes to 3 hours. Very recent events may not yet appear. INFORMATION_SCHEMA provides near real-time data (no significant latency). For the most recent events, INFORMATION_SCHEMA is more current; for historical analysis beyond 7 days, only ACCOUNT_USAGE has the data.
**Exam Trap:** ACCOUNT_USAGE latency means recent events are MISSING — use INFORMATION_SCHEMA for real-time, ACCOUNT_USAGE for historical.
---

### Question 10
A company grants FUTURE grants on tables in SCHEMA A to ROLE X. They also set regular grants on existing tables in SCHEMA A to ROLE X. A new table is created, but ROLE X cannot query it. What is the most likely cause?

- A) Future grants don't work with managed access schemas
- B) The future grant was set at the database level, not the schema level, and a schema-level future grant is overriding it with different permissions
- C) The table was created in a different schema
- D) Future grants require Enterprise edition

**Answer: B) The future grant was set at the database level, not the schema level, and a schema-level future grant is overriding it with different permissions**
**Explanation:** When future grants exist at both the database level and schema level for the same object type, the schema-level future grants take precedence and override the database-level ones. If the schema-level future grant doesn't include SELECT for ROLE X but the database-level one does, ROLE X won't get access. This precedence rule is a common source of confusion.
**Exam Trap:** Schema-level future grants OVERRIDE database-level future grants — they don't combine or add up.
---

### Question 11
A user with ACCOUNTADMIN role creates a custom role MARKETING_ADMIN and grants it directly to themselves — but does NOT grant it to SYSADMIN. What governance problem does this create?

- A) No problem — ACCOUNTADMIN can always access everything
- B) Objects created by MARKETING_ADMIN are invisible to SYSADMIN, breaking the recommended role hierarchy
- C) MARKETING_ADMIN automatically inherits ACCOUNTADMIN privileges
- D) The role will be auto-deleted after 30 days of inactivity

**Answer: B) Objects created by MARKETING_ADMIN are invisible to SYSADMIN, breaking the recommended role hierarchy**
**Explanation:** Snowflake recommends all custom roles be granted to SYSADMIN so that SYSADMIN (and by inheritance, ACCOUNTADMIN) can manage all objects. If MARKETING_ADMIN is not in the hierarchy under SYSADMIN, objects it owns are orphaned from the standard management chain. ACCOUNTADMIN can still access them (has MANAGE GRANTS), but it breaks the intended governance model.
**Exam Trap:** Custom roles not granted to SYSADMIN create "orphan" branches — Snowflake best practice is to always connect them.
---

### Question 12
An organization has MFA enabled for all users. They want to enforce that ACCOUNTADMIN can ONLY be used with MFA authentication — no exceptions. Which Snowflake edition is required for this enforcement?

- A) Standard — MFA is available in all editions
- B) Enterprise — required for policy-based MFA enforcement
- C) Business Critical — required for security policies on roles
- D) All editions support MFA, and MFA enforcement on specific roles is available in all editions

**Answer: D) All editions support MFA, and MFA enforcement on specific roles is available in all editions**
**Explanation:** MFA is available in all Snowflake editions (Standard and above). Snowflake allows enforcing MFA requirements on users (via user properties). The ability to require MFA for specific users (including those with ACCOUNTADMIN) does not require a specific higher edition. Authentication policies for MFA enforcement are available starting from Standard.
**Exam Trap:** MFA is ALL editions — don't confuse it with masking policies (Enterprise) or Tri-Secret Secure (Business Critical).
---

### Question 13
A masking policy returns the original value for the ANALYST role and '***' for all other roles. A user activates the ANALYST role and creates a view that selects from the masked table. A PUBLIC role user queries the view. What does the PUBLIC user see?

- A) Original values because the view was created by ANALYST
- B) '***' because masking policies evaluate based on the QUERYING user's role, not the view creator's role
- C) An error because PUBLIC cannot query views on masked tables
- D) NULL values

**Answer: B) '***' because masking policies evaluate based on the QUERYING user's role, not the view creator's role**
**Explanation:** Masking policies are enforced at query execution time based on the active role of the user running the query. The view creator's role is irrelevant — what matters is who is executing the final query. Since PUBLIC is not ANALYST, the masking policy returns '***' regardless of the access path (direct table or through a view).
**Exam Trap:** Masking evaluates the QUERYING role, not the view/object owner — you cannot bypass masking by creating a view.
---

### Question 14
A DBA sets up a resource monitor with "Suspend" at 80% and "Suspend Immediately" at 100%. A multi-cluster warehouse (3 active clusters) hits the 80% threshold. What happens?

- A) Only one cluster suspends
- B) All clusters are allowed to finish running queries, then the entire warehouse suspends
- C) The warehouse scales down to MIN_CLUSTER_COUNT
- D) Only new queries are blocked; existing clusters continue indefinitely

**Answer: B) All clusters are allowed to finish running queries, then the entire warehouse suspends**
**Explanation:** "Suspend" applies to the entire warehouse, not individual clusters. When the threshold is hit, all running queries across all clusters are allowed to finish, then the entire warehouse (all clusters) suspends. No new queries are accepted. Resource monitors do not interact with multi-cluster scaling — they suspend the whole warehouse.
**Exam Trap:** Resource monitors suspend the ENTIRE warehouse (all clusters) — they don't interact with scaling policies.
---

### Question 15
A company needs to see which columns of which tables were accessed in the last 180 days for GDPR compliance. They query INFORMATION_SCHEMA.ACCESS_HISTORY. What result do they get?

- A) Complete 180-day access history
- B) No results — ACCESS_HISTORY doesn't exist in INFORMATION_SCHEMA; it only exists in ACCOUNT_USAGE
- C) Only 7 days of data
- D) An error because ACCESS_HISTORY requires Business Critical edition

**Answer: B) No results — ACCESS_HISTORY doesn't exist in INFORMATION_SCHEMA; it only exists in ACCOUNT_USAGE**
**Explanation:** ACCESS_HISTORY is only available in ACCOUNT_USAGE schema (with 365-day retention), NOT in INFORMATION_SCHEMA. INFORMATION_SCHEMA has LOGIN_HISTORY and QUERY_HISTORY among others, but not ACCESS_HISTORY. For 180-day column-level access tracking, you must use ACCOUNT_USAGE.ACCESS_HISTORY.
**Exam Trap:** Not all views exist in both INFORMATION_SCHEMA and ACCOUNT_USAGE — ACCESS_HISTORY is ACCOUNT_USAGE only.
---

### Question 16
Three custom roles exist: ROLE_A, ROLE_B, and ROLE_C. ROLE_A is granted to ROLE_B. ROLE_B is granted to ROLE_C. ROLE_C is granted to SYSADMIN. A user has ROLE_B as their active role. Which privileges does this user have?

- A) Only ROLE_B privileges
- B) ROLE_A and ROLE_B privileges (ROLE_B inherits from ROLE_A)
- C) ROLE_A, ROLE_B, and ROLE_C privileges
- D) ROLE_B and ROLE_C privileges

**Answer: B) ROLE_A and ROLE_B privileges (ROLE_B inherits from ROLE_A)**
**Explanation:** Privilege inheritance flows UPWARD. ROLE_B inherits ROLE_A's privileges (because ROLE_A is granted to ROLE_B). However, ROLE_B does NOT inherit ROLE_C's privileges — ROLE_C is above ROLE_B in the hierarchy. The user with active ROLE_B gets ROLE_A + ROLE_B privileges. To get ROLE_C privileges, they'd need ROLE_C directly.
**Exam Trap:** Inheritance flows UP only — a child role's user gets privileges of roles BELOW it, not above.
---

### Question 17
A row access policy on `employees` table allows the HR role to see all rows and restricts others to see only their own department's data. The ACCOUNTADMIN role queries the table. What rows does ACCOUNTADMIN see?

- A) All rows because ACCOUNTADMIN bypasses all policies
- B) Only their department's rows because row access policies apply to ALL roles including ACCOUNTADMIN
- C) An error because policies cannot restrict ACCOUNTADMIN
- D) All rows because ACCOUNTADMIN inherits HR role

**Answer: B) Only their department's rows because row access policies apply to ALL roles including ACCOUNTADMIN**
**Explanation:** Row access policies and masking policies are enforced for ALL roles, including ACCOUNTADMIN. There is no built-in bypass for system-defined roles. If the policy only grants full access to the HR role and restricts others by department, ACCOUNTADMIN sees limited data — unless ACCOUNTADMIN is explicitly included in the policy condition or inherits HR.
**Exam Trap:** ACCOUNTADMIN does NOT automatically bypass masking or row access policies — it must be explicitly allowed in the policy logic.
---

### Question 18
A company needs SSO (Single Sign-On) with SAML 2.0 for their Snowflake account. Which edition is required?

- A) Enterprise or higher
- B) Business Critical or higher
- C) All editions support SAML SSO
- D) Only VPS supports SAML SSO

**Answer: C) All editions support SAML SSO**
**Explanation:** Federated authentication via SAML 2.0 (SSO) is available in all Snowflake editions, including Standard. Snowflake supports integration with identity providers like Okta, Azure AD, ADFS, and others regardless of edition. MFA is also available in all editions. These are core security features not restricted by edition.
**Exam Trap:** SSO/SAML and MFA are ALL editions. Don't confuse with edition-specific features like Tri-Secret Secure (Business Critical).
---

### Question 19
A managed access schema contains 50 tables. A new data engineer is hired and needs SELECT on all current AND future tables. The schema is owned by the DBA_ROLE. The engineer uses ENGINEER_ROLE. What is the correct approach?

- A) The engineer grants themselves SELECT since they create the tables
- B) DBA_ROLE (or MANAGE GRANTS holder) must grant SELECT on all existing tables AND set up future grants
- C) ENGINEER_ROLE can grant itself future access in managed schemas
- D) Only ACCOUNTADMIN can manage access in managed schemas

**Answer: B) DBA_ROLE (or MANAGE GRANTS holder) must grant SELECT on all existing tables AND set up future grants**
**Explanation:** In managed access schemas, only the schema owner or a role with MANAGE GRANTS can grant privileges. The engineer cannot self-grant. Two actions are needed: GRANT SELECT ON ALL TABLES for existing objects, and GRANT SELECT ON FUTURE TABLES for objects not yet created. Both must come from the schema owner or MANAGE GRANTS holder.
**Exam Trap:** Managed access requires TWO actions — existing grants AND future grants — neither of which the object creator can perform.
---

### Question 20
An ACCOUNTADMIN configures a resource monitor with Notify at 70%, Suspend at 90%, and Suspend Immediately at 100%. The frequency is "Monthly" starting on the 1st. On the 15th, the warehouse has consumed 92% of the quota. What is the current state?

- A) Warehouse is suspended immediately because it passed 90%
- B) Warehouse is suspended (running queries were allowed to finish first), and a notification was sent at 70%
- C) Only a notification was sent at 70%; the warehouse is still running
- D) Two notifications were sent (at 70% and 90%) and the warehouse is running

**Answer: B) Warehouse is suspended (running queries were allowed to finish first), and a notification was sent at 70%**
**Explanation:** At 70%, the Notify action sent an alert. At 90%, the Suspend action triggered — all running queries were allowed to complete, then the warehouse suspended. New queries are rejected. The 100% Suspend Immediately threshold wasn't reached. Two separate events occurred: notification at 70% and suspension at 90%.
**Exam Trap:** Multiple thresholds trigger independently as consumption crosses each — earlier actions don't prevent later ones from firing.
---
