<h1 align="center">🌐 Domain 5: Data Collaboration</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Domain_Weight-10%25-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topics-3-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Questions-40+-orange?style=for-the-badge"/>
</p>

---

## 📋 Domain Overview

This domain covers Snowflake's data sharing, marketplace, and replication capabilities. While it has the lowest weight (10%), it tests unique Snowflake features that differentiate it from other platforms.

## 🎯 Official Exam Objectives

- Define secure data sharing and collaboration features
- Define Snowflake Marketplace
- Outline account types and how to leverage replication

---

## 📚 Topics

| # | Topic | Exam Focus | Key Concepts |
|---|-------|-----------|--------------|
| 5.1 | [Secure Data Sharing](./5.1_Secure_Data_Sharing.md) | Sharing basics | Shares, reader accounts, permissions |
| 5.2 | [Data Marketplace & Exchange](./5.2_Data_Marketplace_and_Exchange.md) | Marketplace | Listings, data exchange, access |
| 5.3 | [Replication & Collaboration](./5.3_Replication_and_Data_Collaboration.md) | Cross-region | Database replication, failover groups |

---

## 🎯 Scenarios & Practice

| Resource | Description |
|----------|-------------|
| [📎 Scenario Decision Guides](./Scenarios_Domain5.md) | Sharing and collaboration decision scenarios |
| [✅ Practice Quiz](./quiz/Questions_Domain5.md) | 40+ exam-style questions with explanations |

---

## 💡 Key Exam Tips for Domain 5

- **Secure Sharing** — no data copying, real-time access, provider controls access
- **Reader Accounts** — created BY the provider FOR non-Snowflake customers; provider pays compute
- **Shared objects** — tables, secure views, secure UDFs can be shared; non-secure views CANNOT
- **Marketplace** — free and paid listings; personalized vs standard listings
- **Replication** — database and account replication; supports failover for Business Critical+
- **No data movement** — key differentiator; sharing is metadata-only with zero copy

---

<p align="center">
  <a href="../README.md">🏠 Back to Main Study Guide</a>
</p>
