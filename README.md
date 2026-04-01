# Read Paper Skill for Claude Code

A Claude Code skill that reads arXiv papers and generates structured reading notes.

## Features

- Accepts arXiv ID or URL in multiple formats
- Automatically downloads paper PDF
- Generates structured notes following academic reading template

## Installation

```bash
# Clone this repository
git clone https://github.com/YOUR_USERNAME/claude-skill-read-paper.git

# Copy to Claude Code skills directory
cp -r claude-skill-read-paper/read-paper ~/.claude/skills/
```

Or manually:
```bash
mkdir -p ~/.claude/skills/read-paper
# Copy SKILL.md to ~/.claude/skills/read-paper/
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

## Note Template

The skill generates **two versions** of notes (Chinese + English) in the following structure:

- **Quick View**: Title, authors, arXiv link, year
- **Question**: Research question addressed
- **Task**: Specific task being solved
- **Challenge**: Technical challenges of previous methods
- **Insight**: Core high-level idea (not technical details)
- **Contribution**: Technical contributions with approach and advantages
- **Experiments**: Ablation studies and limitations

Both Chinese (中文) and English versions are generated automatically.

## Requirements

- Claude Code CLI
- Internet connection (for arXiv access)
- `curl` command available

## License

MIT
