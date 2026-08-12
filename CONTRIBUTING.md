<h1 align="center">🤝 Contributing</h1>

<p align="center">
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white"/>
</p>

---

## Getting Started

```bash
git clone https://github.com/yourusername/snowPro-Core.git
cd snowPro-Core
git checkout -b feature/your-contribution
```

## 📁 Structure

```
snowPro-Core/
├── README.md                          # 🎓 Main study guide
├── GLOSSARY.md                        # 📘 All key terms
├── CONTRIBUTING.md                    # 🤝 This file
├── 1_Snowflake_AI_Data_Cloud_.../     # 🏗️ Domain 1 (31%)
│   ├── README.md
│   ├── 1.1_*.md ... 1.6_*.md
│   ├── Scenarios_Domain1.md
│   └── quiz/Questions_Domain1.md
├── 2_Account_Management_.../          # 🔒 Domain 2 (20%)
├── 3_Data_Loading_.../                # 📦 Domain 3 (18%)
├── 4_Performance_Optimization_.../    # ⚡ Domain 4 (21%)
└── 5_Data_Collaboration/              # 🌐 Domain 5 (10%)
```

## 📝 What to Contribute

| Type | Description |
|------|-------------|
| 🐛 Fix errors | Correct inaccurate information or broken links |
| 📝 Add content | New exam tips, SQL examples, or explanations |
| ❓ Add questions | New quiz questions with detailed explanations |
| 📊 Add diagrams | Mermaid diagrams illustrating concepts |
| 🔗 Add references | Links to official Snowflake documentation |

## 🚫 Not Accepted

| Type | Why |
|------|-----|
| Brain dumps | Violates certification NDA |
| Exact exam questions | Against Snowflake certification policy |
| Promotional content | Keep it educational |
| Non-Snowflake topics | Out of scope for this guide |

## 📏 Guidelines

| Rule | Detail |
|------|--------|
| **Accuracy** | All content must be verifiable against official Snowflake docs |
| **Formatting** | Follow the 13-section topic file structure |
| **Navigation** | Include Previous / Home / Next footer on all topic files |
| **SQL examples** | Must be syntactically correct and include comments |
| **Diagrams** | Use mermaid syntax, max 15 nodes per diagram |
| **Language** | Write for exam preparation, not general learning |

## Pull Request Format

**Good:**
```
Add clustering key best practices to topic 4.1

- Added table comparing automatic vs manual clustering
- Included SQL examples for CLUSTER BY
- Added exam tip about clustering depth function
```

**Poor:**
```
Updated file
```

## Running Locally

No build step. Pure markdown. Open in any markdown viewer or VS Code with a markdown preview extension.

---

<p align="center">
  <em>Thank you for helping make this study guide better for everyone! 🎓</em>
</p>
