# Skill Matcher

Intelligent skill recommendation and auto-activation system for Claude Code.

## What it does

- **Analyzes your prompts** and automatically recommends the most relevant skills
- **Auto-activates at session start** to suggest skills based on your first message
- **Searches external catalogs** (GitHub, MCP Registry) if no local skill matches
- **Proposes creating new skills** when no existing skill fits your task

## Installation

### Option 1: Clone and copy manually
```bash
git clone https://github.com/alexgrebeshok-coder/skill-matcher.git
cp -r skill-matcher ~/.claude/plugins/marketplaces/anthropic-agent-skills/skills/
```

### Option 2: Add as Claude Code plugin marketplace
```bash
claude plugin marketplace add alexgrebeshok-coder/skill-matcher
```

## Auto-activation Setup

To enable automatic skill recommendations at every session start, add to `~/.claude/CLAUDE.md`:

```markdown
# Auto Skill Matching

При начале каждой сессии автоматически активируй skill-matcher для анализа первого сообщения пользователя.

## Доступные скиллы по категориям

**Документы:** pdf, docx, pptx, xlsx
**Разработка:** mcp-builder, web-artifacts-builder, webapp-testing
**Дизайн:** algorithmic-art, canvas-design, frontend-design, theme-factory
**Коммуникации:** brand-guidelines, internal-comms, slack-gif-creator
**Мета:** skill-creator
```

## Usage Examples

| Your prompt | Recommended skill |
|-------------|-------------------|
| "Create a presentation" | pptx |
| "Build an MCP server" | mcp-builder |
| "Fill out PDF form" | pdf |
| "Design a UI mockup" | frontend-design |
| "What skills are available?" | Shows full catalog |

## Structure

```
skill-matcher/
├── SKILL.md                    # Main skill instructions
├── references/
│   ├── skill-catalog.md        # Full catalog of known skills
│   └── external-sources.md     # External sources for finding new skills
└── scripts/
    └── scan_skills.py          # Script to scan local installed skills
```

## When No Skill Matches

If your task doesn't match any existing skill, skill-matcher will:
1. Search external catalogs for potentially useful skills
2. Offer to create a new custom skill using `skill-creator`

## License

MIT
