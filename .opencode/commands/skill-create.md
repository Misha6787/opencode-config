# Skill Create Command

Analyze git history to generate skills: $ARGUMENTS

## Your Task

1. **Analyze commits** - Pattern recognition from history
2. **Extract patterns** - Common practices and conventions
3. **Generate SKILL.md** - Structured skill documentation

## Analysis Process

### Step 1: Gather Commit Data

**Важно**: анализируй только коммиты текущей ветки (относительно базовой ветки), а не последние N коммитов всей истории — иначе в анализ попадут чужие ветки и смерженный код.

```bash
# Определи базовую ветку (origin/HEAD или main/master)
BASE_BRANCH=$(git symbolic-ref --short refs/remotes/origin/HEAD 2>/dev/null | sed 's|^origin/||')
BASE_BRANCH=${BASE_BRANCH:-main}

# Коммиты только текущей ветки
git log --oneline "$BASE_BRANCH"..HEAD

# Коммиты по типам файлов (только текущая ветка)
git log --name-only --pretty=format: "$BASE_BRANCH"..HEAD | sort | uniq -c | sort -rn

# Наиболее изменяемые файлы (только текущая ветка)
git log --pretty=format: --name-only "$BASE_BRANCH"..HEAD | sort | uniq -c | sort -rn | head -20
```

Если базовая ветка называется иначе (например, `master` или `develop`), подставь её вместо `main`.

### Step 2: Identify Patterns

**Commit Message Patterns**:
- Common prefixes (feat, fix, refactor)
- Naming conventions
- Co-author patterns

**Code Patterns**:
- File structure conventions
- Import organization
- Error handling approaches

**Review Patterns**:
- Common review feedback
- Recurring fix types
- Quality gates

### Step 3: Generate SKILL.md

```markdown
# [Skill Name]

## Overview
[What this skill teaches]

## Patterns

### Pattern 1: [Name]
- When to use
- Implementation
- Example

### Pattern 2: [Name]
- When to use
- Implementation
- Example

## Best Practices

1. [Practice 1]
2. [Practice 2]
3. [Practice 3]

## Common Mistakes

1. [Mistake 1] - How to avoid
2. [Mistake 2] - How to avoid

## Examples

### Good Example
```[language]
// Code example
```

### Anti-pattern
```[language]
// What not to do
```
```

## Output

Creates:
- `.opencode/skills/[name]/SKILL.md` - Skill documentation

---

**TIP**: Review generated skills with `/code-review` after creation.
