---
name: read-paper
description: Read and analyze arXiv papers. Given an arXiv link or ID, download the paper and generate structured reading notes. (user)
---

# Read Paper Skill

Read arXiv papers and generate structured reading notes.

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

Use the arXiv API to get paper metadata:
```bash
curl -s "http://export.arxiv.org/api/query?id_list=ARXIV_ID"
```

### Step 3: Download PDF

Download the paper PDF for reading:
```bash
curl -L -o /tmp/paper_ARXIV_ID.pdf "https://arxiv.org/pdf/ARXIV_ID.pdf"
```

### Step 4: Read the PDF

Use the Read tool to read the downloaded PDF file at `/tmp/paper_ARXIV_ID.pdf`.

### Step 5: Generate Notes

After reading the full paper, generate notes in the following template format:

---

## Note Template

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

1. **Language**: Generate TWO versions of notes - one in Chinese (中文) and one in English. Output both versions sequentially, with clear separators.
2. **Depth**: Be specific and technical, not generic summaries
3. **Insight vs Contribution**: Clearly distinguish high-level insight from concrete technical contributions
4. **Ablation Focus**: When discussing experiments, prioritize ablation studies that show the impact of each contribution

## Output Format

```
===== 中文笔记 =====

[Chinese version of the notes using the template above]

===== English Notes =====

[English version of the notes using the template above]
```

## Example Usage

User: `/read-paper 2301.07041`

Claude will:
1. Fetch paper info from arXiv API
2. Download the PDF
3. Read and analyze the paper
4. Generate structured notes in the template format
