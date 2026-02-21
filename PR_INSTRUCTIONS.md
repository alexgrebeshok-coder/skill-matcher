# Инструкция: Создание PR в anthropics/skills

## Автоматический способ (если есть GitHub CLI)

```bash
# 1. Установите GitHub CLI если нет
# brew install gh  # macOS
# или см. https://cli.github.com/

# 2. Перейдите в клонированный репозиторий
cd /tmp/skills

# 3. Создайте форк и push
gh repo fork --remote=true
git push -u fork add-skill-matcher

# 4. Создайте PR
gh pr create --title "Add skill-matcher: intelligent skill recommendation and auto-activation" \
  --body-file /home/user/skill-matcher/PR_DESCRIPTION.md \
  --base main
```

---

## Ручной способ (через веб-интерфейс)

### Шаг 1: Форк репозитория

1. Откройте https://github.com/anthropics/skills
2. Нажмите кнопку **Fork** в правом верхнем углу
3. Выберите ваш аккаунт как место для форка

### Шаг 2: Применить патч к форку

```bash
# Клонируйте ВАШИ форк (замените YOUR_USERNAME)
git clone https://github.com/YOUR_USERNAME/skills.git ~/skills-fork
cd ~/skills-fork

# Создайте ветку
git checkout -b add-skill-matcher

# Примените патч
git am < /home/user/skill-matcher/skill-matcher-pr.patch

# Push в ваш форк
git push -u origin add-skill-matcher
```

### Шаг 3: Создать Pull Request

1. Откройте ваш форк на GitHub: `https://github.com/YOUR_USERNAME/skills`
2. Увидите баннер "Compare & pull request" — нажмите его
3. **Title**: `Add skill-matcher: intelligent skill recommendation and auto-activation`
4. **Description**: Скопируйте содержимое из файла `PR_DESCRIPTION.md`
5. Убедитесь что:
   - Base repository: `anthropics/skills`
   - Base: `main`
   - Head repository: `YOUR_USERNAME/skills`
   - Compare: `add-skill-matcher`
6. Нажмите **Create pull request**

---

## Альтернатива: Использовать готовый патч

Если не хотите клонировать весь репозиторий:

```bash
# На странице вашего форка на GitHub:
# 1. Перейдите в ветку main
# 2. Нажмите "Add file" → "Upload files"
# 3. Создайте структуру вручную или используйте GitHub web editor

# Или используйте GitHub API с gh CLI:
cd /tmp/skills
git remote add fork https://github.com/YOUR_USERNAME/skills.git
git push fork add-skill-matcher
```

Затем создайте PR через веб-интерфейс как описано выше.

---

## Проверка перед отправкой

Убедитесь что патч содержит:
- ✅ `skills/skill-matcher/SKILL.md`
- ✅ `skills/skill-matcher/LICENSE.txt`
- ✅ `skills/skill-matcher/references/skill-catalog.md`
- ✅ `skills/skill-matcher/references/external-sources.md`
- ✅ `skills/skill-matcher/scripts/scan_skills.py`
- ✅ Изменения в `.claude-plugin/marketplace.json`

Проверить:
```bash
cd /tmp/skills
git diff origin/main --name-only
```

---

## После создания PR

1. Следите за комментариями от мейнтейнеров
2. Будьте готовы внести изменения если попросят
3. PR может быть рассмотрен не сразу (у anthropics/skills 208 открытых/закрытых PR)

---

## Возможные вопросы от мейнтейнеров

**Q: Зачем нужен skill-matcher если Claude сам может выбирать скиллы?**
A: skill-matcher решает проблему discovery - пользователи не знают какие скиллы существуют. Это обучающий инструмент + диспетчер.

**Q: Почему билингвальные ключевые слова (EN/RU)?**
A: Демонстрирует, что скиллы могут работать с разными языками. RU keywords можно убрать если нужно.

**Q: Нужен ли scan_skills.py скрипт?**
A: Да, он позволяет skill-matcher программно находить установленные скиллы вместо hardcoded списка.

**Q: License совместим с Apache 2.0?**
A: Да, используется Apache 2.0 (как и другие скиллы в репозитории).
