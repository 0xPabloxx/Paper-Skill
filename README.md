# Read Paper Skill for Claude Code

A Claude Code skill that reads arXiv papers and generates structured reading notes in both Chinese and English, then automatically pushes to GitHub.

## Features

- Accepts arXiv ID or URL in multiple formats
- Automatically downloads paper PDF
- Generates structured notes in **both Chinese and English**
- Auto-saves to GitHub with organized folder structure
- Folder naming: `paper-{date}-{short-title}`

## Installation

```bash
# Clone this repository
git clone https://github.com/0xPabloxx/Paper-Skill.git

# Copy skill to Claude Code skills directory
mkdir -p ~/.claude/skills/read-paper
cp Paper-Skill/SKILL.md ~/.claude/skills/read-paper/
cp Paper-Skill/README.md ~/.claude/skills/read-paper/
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

After running the skill, notes are saved to:

```
Paper-Skill/
├── paper-2023-02-11-verifiable-fhe/
│   ├── notes_zh.md    # Chinese notes
│   └── notes_en.md    # English notes
├── paper-2024-01-15-another-paper/
│   ├── notes_zh.md
│   └── notes_en.md
├── README.md
└── SKILL.md
```

## Note Template

Each note includes:

- **Quick View**: Title, authors, arXiv link, year
- **Question**: Research question addressed
- **Task**: Specific task being solved
- **Challenge**: Technical challenges of previous methods
- **Insight**: Core high-level idea (not technical details)
- **Contribution**: Technical contributions with approach and advantages
- **Experiments**: Ablation studies and limitations

## Requirements

- Claude Code CLI
- Internet connection (for arXiv access)
- Git configured with push access to your fork

## License

MIT
