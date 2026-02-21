---
name: skill-matcher
description: Intelligent skill recommendation and activation system. Use when: (1) user asks what skills are available, (2) user needs help choosing the right skill for a task, (3) user wants to find and install new skills, (4) analyzing ANY prompt to determine which skills would help, (5) user says "подбери скилл", "какой скилл использовать", "recommend skills". This skill should activate automatically at session start to analyze user intent and suggest relevant skills.
---

# Skill Matcher

Automatically analyzes user prompts and recommends the most relevant skills from installed plugins and external catalogs.

## Workflow

### Phase 1: Analyze User Intent

Extract from user's message:
1. **Task type**: document creation, coding, design, communication, testing
2. **Keywords**: file formats (.pdf, .docx), tools (MCP, webapp), domains (brand, slides)
3. **Action verbs**: create, edit, analyze, test, build, design

### Phase 2: Match Skills

1. **Scan local skills** using `scripts/scan_skills.py`
2. **Match by category** (see Category Mapping below)
3. **Rank by relevance**: exact keyword match > category match > partial match
4. **Search external catalogs** if no local match (see [external-sources.md](references/external-sources.md))

### Phase 3: Recommend & Activate

Output format:
```
📋 **Recommended skills for your task:**

1. **[skill-name]** ⭐ (high relevance)
   → [why this skill matches your task]

2. **[skill-name]** (medium relevance)
   → [why this skill might help]

💡 Reply with skill number to activate, or "all" for all recommended.
```

After user confirms, activate the skill by mentioning it in context.

## Category Mapping

| Category | Keywords | Skills |
|----------|----------|--------|
| **Documents** | pdf, docx, word, excel, xlsx, powerpoint, pptx, slides, spreadsheet, document, форма, документ, презентация, таблица | pdf, docx, pptx, xlsx |
| **Development** | mcp, server, api, react, webapp, web app, artifact, component, код, сервер | mcp-builder, web-artifacts-builder |
| **Design** | art, design, canvas, theme, visual, frontend, ui, дизайн, тема, искусство | algorithmic-art, canvas-design, frontend-design, theme-factory |
| **Communication** | brand, guidelines, internal, memo, announcement, slack, gif, бренд, коммуникация | brand-guidelines, internal-comms, slack-gif-creator |
| **Testing** | test, qa, testing, webapp test, тест, тестирование | webapp-testing |
| **Meta** | skill, create skill, новый скилл | skill-creator |

## Quick Reference

### Document Tasks
- "Create a presentation" → **pptx**
- "Edit PDF form" → **pdf**
- "Write a Word document" → **docx**
- "Build a spreadsheet" → **xlsx**

### Development Tasks
- "Build MCP server" → **mcp-builder**
- "Create web artifact" → **web-artifacts-builder**
- "Test my webapp" → **webapp-testing**

### Design Tasks
- "Generate algorithmic art" → **algorithmic-art**
- "Design a theme" → **theme-factory**
- "Create UI mockup" → **frontend-design**

### Communication Tasks
- "Write internal announcement" → **internal-comms**
- "Apply brand guidelines" → **brand-guidelines**
- "Create Slack GIF" → **slack-gif-creator**

## Resources

- Full skill catalog: [skill-catalog.md](references/skill-catalog.md)
- External sources for new skills: [external-sources.md](references/external-sources.md)
- Scan local skills: `python scripts/scan_skills.py`

## When No Skill Matches

If user's task doesn't match any existing skill:

### Step 1: Search External Catalogs
Check [external-sources.md](references/external-sources.md) for skills that might help.

### Step 2: Propose Creating New Skill
If no external skill found, suggest creating a custom skill:

```
🔧 **Подходящий скилл не найден**

Для этой задачи нет готового скилла. Варианты:

1. **Создать новый скилл** с помощью skill-creator
   → Скилл будет сохранён и доступен для будущих задач

2. **Выполнить задачу без скилла**
   → Просто сделаю задачу, но без специализированных инструкций

Что предпочитаешь?
```

### When to Suggest skill-creator
- Task is repetitive (user will do it again)
- Task requires specific workflow/steps
- Task involves specialized domain knowledge
- User explicitly wants to automate similar tasks

### When NOT to Suggest skill-creator
- One-time simple task
- Task is too generic
- User just wants quick answer

## Auto-Activation Rules

This skill activates automatically when:
1. Session starts (analyze first user message)
2. User explicitly asks about skills
3. User's task doesn't have an obvious approach
4. Keywords from Category Mapping are detected

When activated silently (at session start), provide brief recommendation only if highly relevant skills exist. Don't interrupt if user's task is straightforward.
