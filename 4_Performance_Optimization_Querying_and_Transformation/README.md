<h1 align="center">⚡ Domain 4: Performance Optimization, Querying, and Transformation</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Domain_Weight-21%25-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topics-4-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Questions-50+-orange?style=for-the-badge"/>
</p>

---

## 📋 Domain Overview

This domain tests your ability to optimize queries, transform data, and leverage Snowflake's unique features like Time Travel, semi-structured data handling, streams, and tasks.

## 🎯 Official Exam Objectives

- Explain query optimization techniques and best practices
- Outline virtual warehouse optimization techniques
- Use SQL commands to load and transform data

---

## 📚 Topics

| # | Topic | Exam Focus | Key Concepts |
|---|-------|-----------|--------------|
| 4.1 | [Query Optimization & Profiling](./4.1_Query_Optimization_and_Profiling.md) | Performance | Query profile, spilling, pruning, clustering |
| 4.2 | [DML, DDL & Data Transformation](./4.2_DML_DDL_and_Data_Transformation.md) | Data manipulation | MERGE, streams, tasks, sequences, cloning |
| 4.3 | [Semi-Structured Data](./4.3_Semi_Structured_Data.md) | VARIANT handling | JSON, FLATTEN, lateral joins, dot notation |
| 4.4 | [Time Travel & Fail-Safe](./4.4_Time_Travel_and_Fail_Safe.md) | Data protection | Retention, UNDROP, AT/BEFORE, storage costs |

---

## 🎯 Scenarios & Practice

| Resource | Description |
|----------|-------------|
| [📎 Scenario Decision Guides](./Scenarios_Domain4.md) | Performance and transformation decision scenarios |
| [✅ Practice Quiz](./quiz/Questions_Domain4.md) | 50+ exam-style questions with explanations |

---

## 💡 Key Exam Tips for Domain 4

- **Query Profile** — know the operators (TableScan, Filter, Join, Sort, Aggregate) and what spilling means
- **Clustering** — automatic for most tables; manual CLUSTER BY for large tables (>1TB); know clustering depth
- **Streams** — CDC mechanism; offset advances on DML consumption; standard vs append-only
- **Tasks** — cron or interval scheduling; task trees (DAGs); serverless or warehouse-based
- **FLATTEN** — used with LATERAL for semi-structured data; know path syntax (data:key::type)
- **Time Travel** — AT (specific point), BEFORE (just before a statement); UNDROP for dropped objects

---

<p align="center">
  <a href="../README.md">🏠 Back to Main Study Guide</a>
</p>
