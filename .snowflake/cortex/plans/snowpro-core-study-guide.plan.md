# Plan: SnowPro Core (COF-C03) Study Guide Repository

## Overview

Create a comprehensive certification study guide for the **SnowPro Core COF-C03** exam, mirroring the same detailed format as the existing SnowPro-GENAI project. The guide will be structured around the 5 official exam domains with weighted coverage.

## Exam Details (from official study guide)

| Field | Value |
|-------|-------|
| Exam Code | COF-C03 |
| Questions | ~100 |
| Duration | 115 minutes |
| Passing Score | 750 / 1000 |
| Study Guide Updated | July 8, 2026 |
| Prerequisite | 6 months Snowflake experience |

## Domain Weightings

| Domain | Weight | Approx Questions |
|--------|--------|-----------------|
| 1.0 Snowflake AI Data Cloud Features and Architecture | 31% | ~31 |
| 2.0 Account Management and Data Governance | 20% | ~20 |
| 3.0 Data Loading, Unloading, and Connectivity | 18% | ~18 |
| 4.0 Performance Optimization, Querying, and Transformation | 21% | ~21 |
| 5.0 Data Collaboration | 10% | ~10 |

## Repository Structure

```
snowPro-Core/
├── README.md                          # Rich README with badges, TOC, roadmap
├── GLOSSARY.md                        # 100+ terms
├── CONTRIBUTING.md                    # Contribution guidelines
├── LICENSE                            # MIT
├── .gitignore
├── assets/
│   └── snowpro-core-badge.png
├── 1_Snowflake_AI_Data_Cloud_Features_and_Architecture/
│   ├── README.md                      # Domain overview
│   ├── 1.1_Architecture_Overview.md
│   ├── 1.2_Interfaces_and_Tools.md
│   ├── 1.3_Object_Hierarchy_and_Types.md
│   ├── 1.4_Virtual_Warehouses.md
│   ├── 1.5_Storage_Concepts.md
│   ├── 1.6_AI_ML_and_App_Development.md
│   ├── Scenarios_Domain1.md
│   └── quiz/
│       └── Questions_Domain1.md       # 60+ questions
├── 2_Account_Management_and_Data_Governance/
│   ├── README.md
│   ├── 2.1_Security_Model_and_Principles.md
│   ├── 2.2_Data_Governance_Features.md
│   ├── 2.3_Monitoring_and_Cost_Management.md
│   ├── Scenarios_Domain2.md
│   └── quiz/
│       └── Questions_Domain2.md       # 50+ questions
├── 3_Data_Loading_Unloading_and_Connectivity/
│   ├── README.md
│   ├── 3.1_Data_Loading_and_Unloading.md
│   ├── 3.2_Automated_Data_Ingestion.md
│   ├── 3.3_Connectors_and_Integrations.md
│   ├── Scenarios_Domain3.md
│   └── quiz/
│       └── Questions_Domain3.md       # 50+ questions
├── 4_Performance_Optimization_Querying_and_Transformation/
│   ├── README.md
│   ├── 4.1_Evaluate_Query_Performance.md
│   ├── 4.2_Optimize_Query_Performance.md
│   ├── 4.3_Snowflake_Caching.md
│   ├── 4.4_Data_Transformation_Techniques.md
│   ├── Scenarios_Domain4.md
│   └── quiz/
│       └── Questions_Domain4.md       # 50+ questions
└── 5_Data_Collaboration/
    ├── README.md
    ├── 5.1_Data_Collaboration_and_Protection.md
    ├── 5.2_Data_Sharing_Capabilities.md
    ├── 5.3_Marketplace_and_Listings.md
    ├── Scenarios_Domain5.md
    └── quiz/
        └── Questions_Domain5.md       # 40+ questions
```

## Topic File Format (consistent across all files)

Each topic file follows this structure:
1. **Header** — exam domain, objective reference
2. **Key Terms table** — definitions for new vocabulary
3. **Concept explanation** — what it is, why it matters
4. **SQL examples** — runnable Snowflake SQL
5. **Architecture diagrams** — Mermaid syntax
6. **Comparison tables** — "X vs Y" for exam clarity
7. **Exam Tips / Gotchas** — traps and common wrong answers
8. **Navigation footer** — Previous / Home / Next links

## Content Sources

- Official SnowPro Core COF-C03 Exam Study Guide (attached PDF)
- SnowPro Exam Cheat Sheet (attached PDF)
- https://docs.snowflake.com/llms.txt for latest documentation
- Snowflake official documentation

## Approach

Files will be created sequentially domain by domain. Each domain will include:
- Detailed conceptual topic files with SQL examples
- Decision scenarios (real-world "when would you...?" questions)
- Practice quiz questions with answers and explanations
- Cross-references to other topics

The guide targets **250+ total practice questions** across all domains.
