<h1 align="center">🔒 Domain 2: Account Management and Data Governance</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Domain_Weight-20%25-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topics-3-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Questions-50+-orange?style=for-the-badge"/>
</p>

---

## 📋 Domain Overview

This domain covers Snowflake's security model, access control mechanisms, and account administration. Expect questions about RBAC, privilege inheritance, data masking, encryption, network policies, and resource monitors.

## 🎯 Official Exam Objectives

- Manage Snowflake accounts and organizations
- Manage roles, privileges, and access control
- Define and apply data governance features

---

## 📚 Topics

| # | Topic | Exam Focus | Key Concepts |
|---|-------|-----------|--------------|
| 2.1 | [Access Control & RBAC](./2.1_Access_Control_and_RBAC.md) | Security model | Roles, privileges, hierarchy, DAC/RBAC |
| 2.2 | [Data Protection & Security](./2.2_Data_Protection_and_Security.md) | Data security | Encryption, masking, row access, network policies |
| 2.3 | [Resource Monitors & Account Mgmt](./2.3_Resource_Monitors_and_Account_Management.md) | Administration | Credit monitoring, parameters, billing |

---

## 🎯 Scenarios & Practice

| Resource | Description |
|----------|-------------|
| [📎 Scenario Decision Guides](./Scenarios_Domain2.md) | Governance and security decision scenarios |
| [✅ Practice Quiz](./quiz/Questions_Domain2.md) | 50+ exam-style questions with explanations |

---

## 💡 Key Exam Tips for Domain 2

- **RBAC hierarchy** — ACCOUNTADMIN > SECURITYADMIN > SYSADMIN > custom roles
- **Privileges flow UP** — child role privileges are inherited by parent roles
- **GRANT vs GRANT OPTION** — the WITH GRANT OPTION allows re-granting
- **System-defined roles** — know what each default role can and cannot do
- **Network policies** — can be set at account or user level; ALLOW list takes precedence
- **Resource monitors** — actions: Notify, Suspend, Suspend Immediately; can cover account or specific warehouses

---

<p align="center">
  <a href="../README.md">🏠 Back to Main Study Guide</a>
</p>
