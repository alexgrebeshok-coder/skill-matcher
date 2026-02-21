# Add skill-matcher: Intelligent skill recommendation and auto-activation

## Summary

This PR adds **skill-matcher**, a meta-skill that automatically analyzes user prompts and recommends the most relevant skills from installed plugins and external catalogs.

## Motivation

As the number of available skills grows, users face challenges:
- **Discovery**: Not knowing which skill exists for their task
- **Selection**: Choosing between similar skills
- **Gaps**: Identifying when a new skill should be created

skill-matcher solves these by acting as an intelligent dispatcher that matches user intent to available skills.

## Key Features

### 🔍 Automatic Intent Analysis
- Extracts task type, keywords, and action verbs from user messages
- Classifies requests across 6 categories: Documents, Development, Design, Communication, Testing, Meta

### 💡 Smart Recommendations
- Keyword-based matching with relevance ranking
- Bilingual support (English/Russian keywords)
- Clear explanations of why each skill matches

### 🎯 Intelligent Filtering
- Avoids spam by not recommending for trivial tasks
- Detects when user just wants a quick answer vs. specialized workflow
- Only shows recommendations when there's high confidence

### 🌐 External Integration
- Searches GitHub and MCP Registry when no local match found
- Provides installation instructions for external skills
- Links to skill marketplaces and catalogs

### 🛠 Skill Creation Guidance
- Suggests creating custom skills for repetitive workflows
- Distinguishes one-time tasks from automation opportunities
- Integrates with existing skill-creator

## What's Included

```
skills/skill-matcher/
├── SKILL.md                    # Core matching logic and instructions
├── LICENSE.txt                 # Apache 2.0 license
├── references/
│   ├── skill-catalog.md       # Complete skill reference with keywords
│   └── external-sources.md    # External skill sources and installation
└── scripts/
    └── scan_skills.py         # Utility for discovering local skills
```

## How It Works

### Phase 1: Analyze Intent
Extracts task characteristics from user message:
- Task type (document creation, coding, design, etc.)
- Relevant keywords
- Action verbs

### Phase 2: Match Skills
1. Scans local skills using `scan_skills.py`
2. Matches by category and keywords
3. Ranks by relevance (exact > category > partial match)
4. Falls back to external catalog search if needed

### Phase 3: Recommend & Activate
Presents ranked recommendations with explanations, then activates user's choice.

## Integration with Existing Skills

skill-matcher complements the existing **skill-creator** skill:
- **skill-matcher**: Discovers and recommends existing skills
- **skill-creator**: Creates new custom skills when gaps are identified

Together they form a complete skill lifecycle: discovery → usage → creation.

## Category Mapping

| Category | Example Keywords | Covered Skills |
|----------|------------------|----------------|
| Documents | pdf, docx, pptx, xlsx, form, presentation, spreadsheet | pdf, docx, pptx, xlsx |
| Development | mcp, server, api, react, webapp, artifact, component | mcp-builder, web-artifacts-builder |
| Design | art, canvas, theme, ui, frontend, generative, colors | algorithmic-art, canvas-design, frontend-design, theme-factory |
| Communication | brand, guidelines, memo, slack, gif, announcement | brand-guidelines, internal-comms, slack-gif-creator |
| Testing | test, qa, webapp test | webapp-testing |
| Meta | skill, create skill | skill-creator |

## Example Usage

### User: "Create a presentation about our product"
```
Claude: 💡 For this task, the **pptx** skill is recommended. Activating it.
        [Begins creating presentation using pptx skill instructions]
```

### User: "Help me set up Kubernetes monitoring"
```
Claude: 🔧 No existing skill found for this task.
        Would you like to create a custom skill using skill-creator?
        It will be available for future monitoring tasks.
```

## Testing

Tested with:
- ✅ Document creation tasks (PDF, Word, PowerPoint, Excel)
- ✅ Development tasks (MCP servers, web artifacts)
- ✅ Design requests (art generation, UI design)
- ✅ Communication needs (announcements, branding)
- ✅ Trivial queries (correctly ignores)
- ✅ Bilingual keywords (English/Russian)

## Breaking Changes

None - this is a new addition that doesn't modify existing skills.

## Checklist

- [x] Skill follows Agent Skills specification
- [x] SKILL.md includes required frontmatter (name, description)
- [x] Clear, actionable instructions provided
- [x] Supporting files organized in subdirectories
- [x] LICENSE.txt included (Apache 2.0)
- [x] Added to marketplace.json (example-skills plugin)
- [x] Tested in Claude Code environment
- [x] Documentation is comprehensive

## Related Links

- [skill-matcher repository](https://github.com/alexgrebeshok-coder/skill-matcher)
- [Agent Skills specification](http://agentskills.io)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
