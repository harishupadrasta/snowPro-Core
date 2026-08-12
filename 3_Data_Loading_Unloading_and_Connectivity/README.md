<h1 align="center">📦 Domain 3: Data Loading, Unloading, and Connectivity</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Domain_Weight-18%25-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topics-3-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Questions-50+-orange?style=for-the-badge"/>
</p>

---

## 📋 Domain Overview

This domain tests your knowledge of how data gets into and out of Snowflake. Covers staging, bulk loading with COPY INTO, continuous loading with Snowpipe, file formats, and data unloading methods.

## 🎯 Official Exam Objectives

- Define concepts and best practices related to data loading
- Define concepts and best practices related to data unloading
- Outline available tools and interfaces for connecting to Snowflake

---

## 📚 Topics

| # | Topic | Exam Focus | Key Concepts |
|---|-------|-----------|--------------|
| 3.1 | [Stages & File Formats](./3.1_Stages_and_File_Formats.md) | Data staging | Internal/external stages, PUT/GET, format options |
| 3.2 | [Data Loading Methods](./3.2_Data_Loading_Methods.md) | Ingestion | COPY INTO, Snowpipe, bulk vs continuous, validation |
| 3.3 | [Data Unloading & Connectivity](./3.3_Data_Unloading_and_Connectivity.md) | Data export | Unloading, connectors, drivers, partner tools |

---

## 🎯 Scenarios & Practice

| Resource | Description |
|----------|-------------|
| [📎 Scenario Decision Guides](./Scenarios_Domain3.md) | Loading strategy decision scenarios |
| [✅ Practice Quiz](./quiz/Questions_Domain3.md) | 50+ exam-style questions with explanations |

---

## 💡 Key Exam Tips for Domain 3

- **Stage types** — User (@~), Table (@%tablename), Named (@stagename), External (cloud storage)
- **PUT/GET** — PUT uploads to internal stage (auto-gzip); GET downloads from internal stage
- **COPY INTO** — bulk loading; know all options (ON_ERROR, VALIDATION_MODE, PURGE, etc.)
- **Snowpipe** — serverless, event-driven; auto-ingest via cloud notifications; per-file overhead billing
- **File formats** — CSV, JSON, Avro, Parquet, ORC, XML; know which support which options
- **VALIDATION_MODE** — RETURN_ERRORS, RETURN_N_ROWS, RETURN_ALL_ERRORS — does NOT load data

---

<p align="center">
  <a href="../README.md">🏠 Back to Main Study Guide</a>
</p>
