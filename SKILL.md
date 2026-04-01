---
name: read-paper
description: Read and analyze arXiv papers. Given an arXiv link or ID, download the paper, generate structured reading notes in both Chinese and English with figures, and push to GitHub. (user)
---

# Read Paper Skill

Read arXiv papers and generate structured reading notes with figures, then push to GitHub.

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

### Step 3: Create Folder and Download PDF

1. Create the paper folder:
```bash
cd ~/projects/claude-skill-read-paper
mkdir -p "paper-YYYY-MM-DD-short-title"
```

2. Download the paper PDF directly to the folder:
```bash
curl -L -o "paper-YYYY-MM-DD-short-title/paper.pdf" "https://arxiv.org/pdf/ARXIV_ID.pdf"
```

### Step 4: Extract Figures from PDF

Extract images/figures from the PDF for use in notes:
```bash
cd "paper-YYYY-MM-DD-short-title"

# Create figures directory
mkdir -p figures

# Method 1: Extract embedded images (requires poppler-utils)
pdfimages -png ../paper.pdf figures/fig

# Method 2: If pdfimages not available, convert pages to images
# pdftoppm -png -r 150 paper.pdf figures/page
```

If `pdfimages` is not installed, inform the user:
```bash
# Install on Ubuntu/Debian:
sudo apt install poppler-utils

# Install on macOS:
brew install poppler
```

### Step 5: Read the PDF

Use the Read tool to read the PDF file. When reading:
1. Pay attention to all figures and their captions
2. Note which figures are most important for understanding the paper
3. Identify figure numbers (Fig. 1, Figure 2, etc.) and their content

### Step 6: Generate Notes with Figures

After reading the paper, generate TWO separate markdown files.

**IMPORTANT**: Include relevant figures in the notes to enhance understanding:
- Reference figures using relative paths: `![Figure X](figures/fig-XXX.png)`
- Add figures where they help explain concepts (architecture diagrams, result tables, ablation charts)
- Include figure captions in the notes

### Step 7: Save Files

Save the following files in the paper folder:
- `paper.pdf` - Original paper (already downloaded in Step 3)
- `notes_zh.md` - Chinese notes with figure references
- `notes_en.md` - English notes with figure references
- `figures/` - Directory containing extracted figures

### Step 8: Push to GitHub

```bash
cd ~/projects/claude-skill-read-paper
git add .
git commit -m "Add notes: Paper Title (arXiv:XXXX.XXXXX)"
git push
```

Return the GitHub folder URL to the user.

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

![Overview Figure](figures/fig-XXX.png)
*Figure X: Brief description of the main architecture or approach*

# Contribution

[List each technical contribution with:]
1. **[Contribution Name]**
   - **Approach**: [How is it done?]
   - **Technical Advantage**: [Why is this better?]

![Method Figure](figures/fig-XXX.png)
*Figure X: Diagram showing the method*

2. **[Contribution Name]**
   - **Approach**: [How is it done?]
   - **Technical Advantage**: [Why is this better?]

# Experiments

## Core Contribution Impact (Ablation Studies)
[What is the impact of each core contribution on performance?]

![Results Figure](figures/fig-XXX.png)
*Figure/Table X: Key experimental results*

## Limitation
[What are the failure cases? On what kind of data does it fail?]
```

---

## Output Requirements

1. **Two Separate Files**: Generate `notes_zh.md` (Chinese) and `notes_en.md` (English)
2. **Include Figures**: Add relevant figures from the paper to improve readability
3. **PDF Included**: Save original paper as `paper.pdf` in the folder
4. **Depth**: Be specific and technical, not generic summaries
5. **Insight vs Contribution**: Clearly distinguish high-level insight from concrete technical contributions
6. **Ablation Focus**: When discussing experiments, prioritize ablation studies

## Folder Structure

For paper `2301.07041` (Verifiable Fully Homomorphic Encryption, updated 2023-02-11):

```
~/projects/claude-skill-read-paper/
├── paper-2023-02-11-verifiable-fhe/
│   ├── paper.pdf           # Original paper
│   ├── notes_zh.md         # Chinese notes
│   ├── notes_en.md         # English notes
│   └── figures/            # Extracted figures
│       ├── fig-000.png
│       ├── fig-001.png
│       └── ...
├── README.md
└── SKILL.md
```

## Figure Selection Guidelines

When including figures in notes:
1. **Architecture/Overview diagrams** - Include in Insight or Contribution section
2. **Method illustrations** - Include in relevant Contribution subsection
3. **Results tables/charts** - Include in Experiments section
4. **Ablation study figures** - Include in Ablation Studies subsection
5. Skip decorative or less informative figures

## Final Output

After completing all steps, display:
1. Confirmation message with list of saved files
2. GitHub folder URL: `https://github.com/0xPabloxx/Paper-Skill/tree/main/paper-YYYY-MM-DD-short-title`
