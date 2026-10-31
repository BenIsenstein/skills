---
name: develop-my-skills
description: Use when the user wants to create, edit, review, or maintain a skill in their personal skills repo at /home/ben/Work/repos/skills. Covers the repo layout, required SKILL.md frontmatter, and the workflow for shaping a new skill.
---

# Develop My Skills

Ben's personal, GitHub-backed agent skills repo is cloned locally at:

```
/home/ben/Work/repos/skills
```

Always work in that clone. Do not create skills in an unrelated project unless the user explicitly asks for a project-local skill.

## Repo layout

```
/home/ben/Work/repos/skills
├── README.md
├── LICENSE
└── skills/
    └── <skill-name>/
        └── SKILL.md
```

A skill is a directory inside `skills/`. The directory name is the skill name and must match the `name` field in its frontmatter. Skills may add supporting files (`reference.md`, `examples/`, scripts, assets) next to `SKILL.md`.

## Before writing: grill the idea

Do not start drafting from the first sentence of the request. Invoke the `grilling` skill (`/grilling`) and interview the user until you have a shared understanding of:

- What the skill should make an agent do differently
- The exact situations that should trigger it (and the near-misses that should not)
- What belongs in the skill body versus a supporting file
- Any examples or hard-won rules the user wants encoded

Do not write the skill until the user confirms you have reached that shared understanding.

## Minimum viable skill

Create `skills/<skill-name>/SKILL.md` with only the required frontmatter:

```markdown
---
name: <skill-name>
description: <when to use this skill, and when not to>
---

# <Title>

<Instructions, workflow, examples, or references.>
```

Rules:

- `name` must exactly match the directory name.
- `description` is the trigger surface. Describe the situations and user phrasing that should activate the skill; include exclusions when they matter. Keep it concrete and searchable.
- Prefer a short, imperative body that tells the agent what to do, not a description of the skill.
- Move long reference material, examples, or checklists into sibling files and link them from `SKILL.md`.
- Keep a skill focused on one job. Split unrelated guidance into separate skills.

## Workflow

1. Confirm which skill is being created or changed and why.
2. Run `/grilling` to settle scope, triggers, and content.
3. Create or edit `skills/<skill-name>/SKILL.md`.
4. Add supporting files only if they earn their place.
5. Re-read the frontmatter: does the directory name match `name`, and does `description` cover the real trigger phrases?
6. Show the user the diff and let them review.

## Installing into an agent

This repo is the source of truth. To make a skill available to a local agent, symlink the skill directory into the agent's skills folder rather than copying it, so edits in the repo take effect immediately. For example:

```bash
ln -s /home/ben/Work/repos/skills/skills/<skill-name> ~/.agents/skills/<skill-name>
```

Ask before creating or replacing symlinks outside the repo.
