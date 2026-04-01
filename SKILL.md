---
name: read-paper
description: Read and analyze arXiv papers. Given an arXiv link or ID, download the paper, generate structured reading notes in both Chinese and English, and push to GitHub. (user)
---

# Read Paper Skill

Read arXiv papers and generate structured reading notes, then push to GitHub.

## Trigger

Use `/read-paper` followed by an arXiv link or ID.

## Input Formats

Accepts any of these formats:
- arXiv ID: `2301.07041`
- arXiv URL: `https://arxiv.org/abs/2301.07041`
- arXiv PDF URL: `https://arxiv.org/pdf/2301.07041.pdf`

## Workflow

When triggered with an arXiv paper:

### Step 1: Parse Input

Extract the arXiv ID from the input. Examples:
- `2301.07041` → `2301.07041`
- `https://arxiv.org/abs/2301.07041` → `2301.07041`
- `https://arxiv.org/pdf/2301.07041.pdf` → `2301.07041`

### Step 2: Fetch Paper Information

Use the arXiv API to get paper metadata (title, authors, last updated date):
```bash
curl -s "http://export.arxiv.org/api/query?id_list=ARXIV_ID"
```

Extract from the XML response:
- `<title>` - Paper title
- `<author>` - Authors
- `<updated>` - Last edited date (format: YYYY-MM-DD from the full timestamp)

### Step 3: Download PDF

Download the paper PDF for reading:
```bash
curl -L -o /tmp/paper_ARXIV_ID.pdf "https://arxiv.org/pdf/ARXIV_ID.pdf"
```

### Step 4: Read the PDF

Use the Read tool to read the downloaded PDF file at `/tmp/paper_ARXIV_ID.pdf`.

### Step 5: Generate Notes

After reading the full paper, generate TWO separate markdown files using the template below.

### Step 6: Save and Push to GitHub

1. Create a folder in the Paper-Skill repository:
   - Folder name format: `paper-YYYY-MM-DD-short-title` where:
     - `YYYY-MM-DD` is the paper's last updated date from arXiv
     - `short-title` is a URL-safe short version of the title (lowercase, hyphens, max 30 chars)
   - Example: `paper-2023-02-11-verifiable-fhe`

2. Save two markdown files in this folder:
   - `notes_zh.md` - Chinese version
   - `notes_en.md` - English version

3. Git commands to execute:
```bash
cd ~/projects/claude-skill-read-paper
mkdir -p "paper-YYYY-MM-DD-short-title"
# Write notes_zh.md and notes_en.md using the Write tool
git add .
git commit -m "Add notes: Paper Title (arXiv:XXXX.XXXXX)"
git push
```

4. Return the GitHub folder URL to the user.

---

## Note Template

Each markdown file should follow this structure:

```markdown
# Quick View

**Title**: [Paper title]
**Authors**: [Author list]
**arXiv**: [arXiv ID with link]
**Year**: [Publication year]

# Question

[What research question does this paper address?]

# Task

[What specific task is this paper trying to solve?]

# Challenge

[What technical challenges did previous methods face? Why is this problem hard?]

# Insight

[What is the core insight or key idea that solves the challenge? One sentence high-level thought, NOT specific technical contribution.]

# Contribution

[List each technical contribution with:]
1. **[Contribution Name]**
   - **Approach**: [How is it done?]
   - **Technical Advantage**: [Why is this better?]

2. **[Contribution Name]**
   - **Approach**: [How is it done?]
   - **Technical Advantage**: [Why is this better?]

# Experiments

## Core Contribution Impact (Ablation Studies)
[What is the impact of each core contribution on performance?]

## Limitation
[What are the failure cases? On what kind of data does it fail?]
```

---

## Output Requirements

1. **Two Separate Files**: Generate `notes_zh.md` (Chinese) and `notes_en.md` (English)
2. **Depth**: Be specific and technical, not generic summaries
3. **Insight vs Contribution**: Clearly distinguish high-level insight from concrete technical contributions
4. **Ablation Focus**: When discussing experiments, prioritize ablation studies that show the impact of each contribution

## File Naming Convention

- Folder: `paper-{updated_date}-{short_title}`
  - `updated_date`: From arXiv `<updated>` field, format `YYYY-MM-DD`
  - `short_title`: Lowercase, spaces replaced with hyphens, max 30 characters, URL-safe
- Files:
  - `notes_zh.md` - Chinese notes
  - `notes_en.md` - English notes

## Example

For paper `2301.07041` (Verifiable Fully Homomorphic Encryption, updated 2023-02-11):

```
~/projects/claude-skill-read-paper/
├── paper-2023-02-11-verifiable-fhe/
│   ├── notes_zh.md
│   └── notes_en.md
├── README.md
└── SKILL.md
```

## Final Output

After completing all steps, display:
1. Confirmation message
2. GitHub folder URL: `https://github.com/0xPabloxx/Paper-Skill/tree/main/paper-YYYY-MM-DD-short-title`
