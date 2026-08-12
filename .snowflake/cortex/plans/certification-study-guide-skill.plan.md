# Plan: Create Global Certification Study Guide Skill

## Context

After thoroughly analyzing the SnowPro-GENAI project (`C:\Users\HUpadrasta\Downloads\Research_Projects\SnowPro-GENAI`), I've extracted every pattern, convention, and formatting rule used across the project. The skill will codify these into a reusable standard that can be invoked for any future Snowflake certification study guide (SnowPro Core, Advanced Architect, Data Engineer, etc.).

### Key Findings from GENAI Project Analysis

The project follows a strict, repeatable structure:
- **Root files:** README.md, CONTRIBUTING.md, GLOSSARY.md, .gitignore, LICENSE, assets/
- **Domain folders:** Numbered `N_Domain_Name/` with README.md, topic files, Scenarios file, quiz/ subfolder
- **Topic files:** `N.M_Topic_Name.md` with `N.Ma_Deep_Dive.md` for advanced topics
- **Consistent sections in every topic file:** Exam context header, Key Terms box, Key Characteristics table, Mermaid diagrams, detailed content with tables, Exam Tips, Quick Reference SQL, References, Navigation footer
- **Quiz files:** Sectioned questions with answers and explanations, answer key table
- **Scenarios:** Situation + Decision Flow (code block) + Why-not reasoning + Exam traps

### Skill Location

```
C:\Users\HUpadrasta\.snowflake\cortex\skills\certification-study-guide\
├── SKILL.md                                    # Main skill definition
└── references/
    ├── readme-template.md                      # Root README template
    ├── topic-file-template.md                  # Per-topic file template
    ├── quiz-template.md                        # Quiz questions template
    ├── scenarios-template.md                   # Scenarios guide template
    ├── domain-readme-template.md               # Per-domain README template
    └── supporting-files-templates.md           # GLOSSARY, CONTRIBUTING, .gitignore
```

---

## Implementation Steps

### Step 1: Create SKILL.md

The main skill file at `~/.snowflake/cortex/skills/certification-study-guide/SKILL.md` will contain:

```markdown
---
name: certification-study-guide
description: >
  Create comprehensive Snowflake certification study guide repositories with
  consistent formatting, exam-mapped content, mermaid diagrams, practice quizzes,
  and scenario-based decision guides. Use when creating study materials for any
  SnowPro certification (Core, Gen AI, Advanced Architect, Data Engineer, etc.).
  Triggers: certification guide, study guide, exam prep, SnowPro study, cert bible,
  create certification documentation, exam study materials.
---
```

The body will include:
1. **Skill overview** (what it does, when to invoke)
2. **Project structure specification** (exact folder/file layout)
3. **File naming conventions** (numbering scheme, deep-dive suffix)
4. **README.md format rules** (badges, exam table, pie chart, TOC, roadmap, stats)
5. **Topic file format rules** (13-section structure with exact ordering)
6. **Quiz format rules** (sections, question format, answer keys)
7. **Scenarios format rules** (situation/flow/reasoning pattern)
8. **Domain README format rules** (badges, study approach, decision matrix, traps)
9. **Glossary format rules** (alphabetical tables, exam-critical section)
10. **Contributing format rules** (structure tree, guidelines tables)
11. **Cross-referencing rules** (navigation footers, relative links)
12. **Content quality rules** (exam-mapped, gotcha-aware, scenario-driven, self-contained)
13. **Workflow** (step-by-step process for building a new certification guide)

### Step 2: Create references/readme-template.md

A complete README.md template with `{{PLACEHOLDERS}}` for:
- Certification name, exam code, question count, duration, passing score
- Domain names, weights, and approximate question counts
- Badge image path and shield URLs
- TOC structure with links
- Study roadmap mermaid graph
- Content statistics
- Official resource links

### Step 3: Create references/topic-file-template.md

The canonical 13-section topic file structure:
1. H1 title with topic number
2. Exam domain/objective blockquote
3. Horizontal rule
4. Opening definition paragraph (bold key phrase)
5. Key Terms blockquote table
6. Key Characteristics table (exam-critical)
7. Mermaid architecture/flow diagram
8. Detailed content sections with tables
9. SQL/code examples
10. Exam Tips (numbered list of gotchas)
11. Quick Reference SQL code block
12. References (official doc links)
13. Navigation footer (Previous | Home | Next)

### Step 4: Create references/quiz-template.md

The quiz template specifying:
- Header with badge shields (question count, domain weight, sections)
- Covers line listing all topic files
- Sections (A-E, grouped by topic area, 10 questions each)
- Question format: numbered, situation-based, A-D options
- Answer format: bold Answer + Explanation paragraph
- Separator: `---` between questions
- Answer key table at bottom

### Step 5: Create references/scenarios-template.md

The scenarios template specifying:
- Title with domain reference
- Blockquote purpose statement
- 8-12 scenarios per domain
- Per-scenario format: Situation paragraph, Decision Flow (code block with arrows), "Why not X?" alternatives, Key facts/exam traps
- Quick Decision Matrix (ASCII table) at the end
- Navigation footer

### Step 6: Create references/domain-readme-template.md

Template for each domain's README.md:
- Centered H1 with emoji and domain name
- Badges: weight, approximate questions, topic file count
- Focus statement blockquote
- "What You Need to Know" section with numbered objectives
- Prerequisites section
- Study Approach mermaid flowchart
- Topics table (emoji, number, name, file link)
- Scenarios and Quiz table
- Quick Decision Matrix (ASCII art)
- Key Exam Traps section (Trap/Truth format)
- Relationship diagram (mermaid)
- Navigation footer

### Step 7: Create references/supporting-files-templates.md

Combined templates for:
- **GLOSSARY.md**: Badge header, usage note, alphabetical sections (A-Z), table format (Term | Definition), Exam-Critical Terms section, Quick Reference sections
- **CONTRIBUTING.md**: Badges, Getting Started (clone), Structure tree, What to Contribute table, What Not to Accept table, Guidelines table, PR format, Running Locally note
- **.gitignore**: Standard patterns for markdown study guide repos

---

## Verification

After creating the skill:
1. The skill should appear in the skill list (`/find-skills` should discover it)
2. Invoking `/certification-study-guide` or asking to "create a certification study guide" should load the full instruction set
3. The templates in `references/` should be loadable as context when building a new guide
4. Every formatting rule from the GENAI project should be captured without ambiguity

---

## Critical Files

- `C:\Users\HUpadrasta\.snowflake\cortex\skills\certification-study-guide\SKILL.md` - Main skill definition with all rules and workflow
- `C:\Users\HUpadrasta\.snowflake\cortex\skills\certification-study-guide\references\topic-file-template.md` - Most complex template; defines the 13-section structure
- `C:\Users\HUpadrasta\.snowflake\cortex\skills\certification-study-guide\references\readme-template.md` - Root README with all placeholder patterns
- `C:\Users\HUpadrasta\Downloads\Research_Projects\SnowPro-GENAI\1_Snowflake_for_Gen_AI_Overview\1.1_Snowflake_Cortex_Overview.md` - Reference exemplar for topic files
- `C:\Users\HUpadrasta\Downloads\Research_Projects\SnowPro-GENAI\README.md` - Reference exemplar for root README