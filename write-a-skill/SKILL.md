---
name: write-a-skill
description: Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.
---

# Writing Skills

Use this skill when a user asks to create a new AI skill and wants high activation quality, spec compliance, and maintainable structure.

## Outcome

Produce a complete, usable skill folder with:
- Spec-valid `SKILL.md` frontmatter
- Clear trigger language for activation
- Progressive disclosure (small core, deeper references)
- Optional scripts/assets only when needed
- Validation and activation tests

## Process

1. **Gather requirements** - ask user about:
   - Task/domain covered by the skill
   - Top use cases (3-5 concrete tasks)
   - Inputs/outputs and constraints
   - Whether deterministic scripts are needed
   - Environment assumptions (network access, tools, runtime)

2. **Design activation behavior** - create:
   - Define what should trigger the skill (keywords, file types, user intents)
   - Define what should not trigger the skill (nearby but out-of-scope intents)
   - Draft a description that distinguishes this skill from similar ones

3. **Create the skill structure** - create:
   - Write a focused `SKILL.md`
   - Add `references/` for deeper details that are used for specific needs
   - Add `scripts/` only for deterministic repeated operations
   - Add `assets/` for templates or static resources

4. **Validate and test** - review and revise:
   - Validate frontmatter and naming rules
   - Run activation tests with positive and negative prompts
   - Refine description and examples based on misses

5. **Review with user** - present draft and ask:
   - Confirm coverage of intended use cases
   - Confirm trigger precision (loads when needed, not otherwise)
   - Confirm maintainability (core concise, details externalized)

## Hard Requirements (Specification)

Apply these rules exactly:

- `name` is required, 1-64 chars, lowercase letters/numbers/hyphens only
- `name` must not start/end with `-` and must not contain `--`
- Skill directory name must exactly match `name`
- `description` is required, 1-1024 chars, non-empty
- `SKILL.md` must contain YAML frontmatter followed by Markdown body

Optional supported fields:
- `license`
- `compatibility`
- `metadata`
- `allowed-tools` (experimental)

## Recommended Folder Layout

```text
skill-name/
├── SKILL.md              # Required: frontmatter + instructions
├── scripts/              # Optional: executable deterministic code that agents can run (Python, Bash, and JavaScript)
├── references/           # Optional: additional documentation that agents can read on demand
└── assets/               # Optional: templates/images/static data
```

## SKILL.md Template

```md
---
name: skill-name
description: Does ${specific_capability}. Use when ${specific_triggers_keywords_file_types_or_user_intents}.
# Optional:
# compatibility: Requires git and internet access.
# allowed-tools: Bash(git:*) Bash(jq:*) Read
---

# Skill Name

## When to use this skill
- Trigger 1
- Trigger 2
- Non-trigger 1

## Quick start
${minimal_working_example}

## Workflows
${step_by_step_instructions_for_common_tasks}

## Edge cases
${failure_modes_validation_checks_fallback_behavior}

## References
See [reference guide](references/REFERENCE.md) for deep details.
```

## Description Writing Rules

Treat `description` as the primary activation surface: systems typically match user requests to skills using `name` + `description`.

Write descriptions that are:
- Specific about capabilities (what it does)
- Explicit about triggers (when to use)
- Distinct from adjacent skills
- Rich with relevant keywords users naturally type

Good:
```text
Extract text and tables from PDF files, fill PDF forms, and merge documents. Use when users mention PDFs, document extraction, form filling, or combining PDF files.
```

Bad:
```text
Helps with documents.
```

## Progressive Disclosure Rules

- Keep `SKILL.md` focused on the most common workflows
- Move deep/rare details to `references/`
- Keep file references shallow and relative from skill root
- Prefer splitting long content before it becomes hard to scan

## When to Add Scripts

Add `scripts/` when operations are deterministic and repeated:
- Validation and formatting
- Parsing/transforms with predictable outputs
- Tasks requiring robust error handling

If scripts are added, include:
- Required runtime/dependencies
- Expected inputs/outputs
- Error behavior and failure messages
- Safety assumptions (sandbox, permissions, network)

## Validation and Activation Test Loop

1. **Spec validation**
   - Run: `skills-ref validate ./skill-name`
   - Fix all naming/frontmatter violations

2. **Activation tests**
   - Create at least 3 positive prompts that should load the skill
   - Create at least 3 negative prompts that should not load it
   - Refine `description` and "When to use" until behavior is precise

3. **Quality pass**
   - Ensure examples are concrete and realistic
   - Ensure terminology is consistent
   - Remove stale/time-sensitive statements

## Final Checklist

- [ ] `name` is spec-valid and matches directory
- [ ] `description` is specific and includes clear "Use when..." triggers
- [ ] Frontmatter is valid YAML with required fields
- [ ] `SKILL.md` contains quick start, workflows, and edge cases
- [ ] Progressive disclosure used (`references/` for deep details)
- [ ] Script usage is justified and documented (if present)
- [ ] Validation command passes
- [ ] Positive/negative activation tests completed