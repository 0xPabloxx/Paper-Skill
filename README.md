# Read Paper Skill for Claude Code

A Claude Code skill that reads arXiv papers and generates structured reading notes in both Chinese and English, with figures extracted from the paper, then automatically pushes to GitHub.

## Features

- Accepts arXiv ID or URL in multiple formats
- Automatically downloads paper PDF and saves to repo
- **Extracts figures** from PDF and embeds in notes
- Generates structured notes in **both Chinese and English**
- Auto-saves to GitHub with organized folder structure

## Installation

```bash
# Clone this repository
git clone https://github.com/0xPabloxx/Paper-Skill.git

# Copy skill to Claude Code skills directory
mkdir -p ~/.claude/skills/read-paper
cp Paper-Skill/SKILL.md ~/.claude/skills/read-paper/
cp Paper-Skill/README.md ~/.claude/skills/read-paper/
```

### Optional: Install PDF tools for figure extraction

```bash
# Ubuntu/Debian
sudo apt install poppler-utils

# macOS
brew install poppler
```

## Usage

In Claude Code, use the `/read-paper` command:

```
/read-paper 2301.07041
```

Or with full URL:
```
/read-paper https://arxiv.org/abs/2301.07041
```

## Output Structure

After running the skill, files are saved to:

```
Paper-Skill/
├── paper-2023-02-11-verifiable-fhe/
│   ├── paper.pdf          # Original paper
│   ├── notes_zh.md        # Chinese notes (with figures)
│   ├── notes_en.md        # English notes (with figures)
│   └── figures/           # Extracted figures
│       ├── fig-000.png
│       ├── fig-001.png
│       └── ...
├── paper-2024-01-15-another-paper/
│   └── ...
├── README.md
└── SKILL.md
```

## Note Template

Each note includes:

- **Quick View**: Title, authors, arXiv link, year
- **Question**: Research question addressed
- **Task**: Specific task being solved
- **Challenge**: Technical challenges of previous methods
- **Insight**: Core high-level idea + overview figure
- **Contribution**: Technical contributions with method figures
- **Experiments**: Ablation studies with result figures/tables
- **Limitation**: Failure cases

## Figure Embedding

Notes automatically include relevant figures:
- Architecture diagrams in the Insight section
- Method illustrations in Contribution sections
- Result tables/charts in Experiments section
- Ablation study figures where applicable

## Requirements

- Claude Code CLI
- Internet connection (for arXiv access)
- Git configured with push access to your fork
- (Optional) `poppler-utils` for figure extraction

## License

MIT
