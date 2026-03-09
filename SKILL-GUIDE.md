# Writing Skills for builder-skills

How every skill in this repo should be structured, written, and organized.

## Directory Structure

```
.claude/skills/
  <category>/                    # design, product, dev, tools
    <subcategory>/               # optional grouping (review, refine, adjust, etc.)
      <skill-name>/
        SKILL.md                 # required — the skill itself
        references/              # optional — deep-dive material
          topic-a.md
          topic-b.md
```

**Rules:**
- Every skill lives in its own folder with a `SKILL.md`
- Folder name = skill name (kebab-case)
- Reference files go in `references/` when a skill needs deep-dive material that would bloat the main file
- Categories group skills by domain. Subcategories group by function when a category gets large
- Don't nest deeper than `category/subcategory/skill-name/`

## SKILL.md Anatomy

### Frontmatter

```yaml
---
name: skill-name
description: What it does. Use when user says "trigger phrase", "another phrase", or [context]. Include both WHAT and WHEN.
# For command skills (user-invokable):
user-invokable: true
args:
  - name: area
    description: What the argument scopes (optional)
    required: false
# For framework skills: omit user-invokable and args
metadata:
  author: builder-skills
  version: "1.0.0"
---
```

**Description field — the most important part:**

The description is how Claude decides whether to load your skill. Structure it as `[What it does] + [When to use it] + [Key capabilities]`. Always include trigger phrases users might say.

```yaml
# Good — specific with triggers
description: Perform comprehensive audit of interface quality. Use when user says "audit this", "check accessibility", "review quality", or wants a detailed quality report.

# Bad — missing triggers
description: Helps with projects.

# Bad — no user context
description: Creates sophisticated multi-page documentation systems.
```

**Two types of skills:**

| Type | Purpose | Invokable? | Has references? |
|------|---------|-----------|-----------------|
| **Framework** | Knowledge base Claude draws on for decisions | No | Usually yes |
| **Command** | Action Claude performs when user runs `/skill-name` | Yes | Rarely |

### Body — How to Write It

**Write instructions, not explanations.** The reader is Claude, not a human learning theory. Tell it what to do.

| Instead of | Write |
|-----------|-------|
| "JTBD provides a systematic framework for understanding customer needs" | "When making product decisions, think in terms of the job someone is trying to get done" |
| "Accessibility is an important consideration in web design" | "Check contrast ratios. Verify ARIA labels. Test keyboard navigation." |
| "The five planes model suggests that strategy should come first" | "Start with strategy. Don't design the surface until structure is resolved." |

**Lead with action, follow with context.** If Claude needs background to do the work well, put it after the instruction, not before.

**Use tables for structured knowledge.** Rules, checklists, comparisons, and taxonomies are easier to scan and apply as tables than as prose.

**Use code blocks for formulas, templates, and patterns.** Anything Claude should reproduce or follow structurally.

**Be direct.** "Don't" is better than "it is generally advisable to avoid." "Always" is better than "in most cases, it is recommended to."

### Voice Checklist

Run every skill through these before committing:

- [ ] Could Claude act on this immediately, or does it need to "understand" something first?
- [ ] Is every paragraph either an instruction or context that supports an instruction?
- [ ] Are there sentences that describe the theory without connecting it to what Claude should do? Cut them.
- [ ] Does it read like a reference card or like a textbook chapter? It should read like a reference card.
- [ ] Would removing any section change what Claude actually does? If not, cut it.

## Reference Files

Reference files hold the depth. The SKILL.md should be self-contained enough to be useful alone — references add precision and detail for specific topics.

**Size guidance:** Keep SKILL.md under 5,000 words. Move detailed docs to `references/` and link to them. This ensures progressive disclosure — Claude loads the body only when relevant, and references only when needed.

### When to Use References

- The skill covers a broad framework with 3+ distinct subtopics
- Any subtopic needs more than ~30 lines to cover properly
- Claude needs lookup material it can consult selectively (not all at once)

### How to Write Them

Same voice rules as SKILL.md. References are still instructions, not textbook chapters.

Structure each reference file around what Claude needs to do with the information:

```markdown
# Topic Name

[1-2 sentence frame: when this reference applies]

## Section — Action-Oriented Heading

[Instructions, tables, patterns]

## Section — Another Action

[More instructions]
```

Link to references from SKILL.md at the bottom:

```markdown
→ Deep dives:
- [Topic A](references/topic-a.md) — Short description
- [Topic B](references/topic-b.md) — Short description
```

## Optional Frontmatter Fields

| Field | When to use |
|-------|------------|
| `license` | Open-source skills (`MIT`, `Apache-2.0`) |
| `compatibility` | Skills with environment requirements (e.g. specific CLI tools, network access) |
| `allowed-tools` | Restrict which tools the skill can use (e.g. `Bash(npx agent-browser:*)`) |
| `metadata` | Always. Include `author` and `version` at minimum |

## Naming Conventions

| Thing | Convention | Examples |
|-------|-----------|----------|
| Skill folder | kebab-case, short, verb or noun | `audit`, `jtbd`, `frontend-design` |
| Category folder | singular noun | `design`, `product`, `dev`, `tools` |
| Subcategory folder | action noun or gerund | `review`, `refine`, `adjust`, `hardening` |
| Reference files | kebab-case, topic-based | `core-concepts.md`, `delivering-value.md` |
| Frontmatter `name` | matches folder name exactly | `name: audit`, `name: jtbd` |

## Adding a New Skill

1. Pick the right category and subcategory (or create one if nothing fits)
2. Create `<category>/<subcategory>/<skill-name>/SKILL.md`
3. Write frontmatter with name and description
4. Write the body in instruction-first voice
5. Add reference files if the skill covers multiple deep topics
6. Update the README table and structure diagram
7. Test that the skill loads: run the slash command or verify Claude picks it up
