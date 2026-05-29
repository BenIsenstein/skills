# c4-diagrams

An agent skill for producing software architecture diagrams using the [C4 model](https://c4model.com/) and [Mermaid](https://mermaid.js.org/) syntax. Designed for minimal context usage — typical execution loads ~150 lines (entry point + one example file), with additional references loaded only when needed.

Compatible with [opencode](https://opencode.ai), Claude Code, and any agent system that supports markdown-based skill files.

## What it does

When you mention "C4" in conversation, this skill guides the agent through:

1. Classifying which diagram level you need (context, container, component, dynamic, deployment, or code)
2. Loading only the syntax and rules for that specific diagram type
3. Generating a valid Mermaid C4 diagram inline
4. Self-validating the output against level-specific checks

## Installation

### opencode

Copy this folder into your skills directory:

```
.opencode/skills/c4-diagrams/
```

Or register the path in `opencode.json`:

```json
{
  "skills": {
    "paths": ["path/to/c4-diagrams"]
  }
}
```

### Claude Code

Place in your skills directory:

```
~/.claude/skills/c4-diagrams/
```

### Other agents

Point your agent at `SKILL.md` as the entry point. The skill is self-contained markdown with no external dependencies.

## File structure

```
SKILL.md              Entry point — routing, workflow, abstraction hierarchy
reference.md          C4 model definitions + Mermaid function index (on-demand)
review.md             Full validation checklist (on-demand, triggered by complexity)
examples/
  context.md          System Context diagram — syntax, example, post-check
  container.md        Container diagram — syntax, example, post-check
  component.md        Component diagram — syntax, example, post-check
  dynamic.md          Dynamic diagram — syntax, example, post-check
  deployment.md       Deployment diagram — syntax, example, post-check
  code.md             Code-level class diagram — syntax, example, post-check
```

## Diagram types supported

| Level | Mermaid keyword | Shows |
|-------|----------------|-------|
| 1 - Context | `C4Context` | Who uses the system and what it talks to |
| 2 - Container | `C4Container` | Applications and data stores inside the system |
| 3 - Component | `C4Component` | Logical modules inside one container |
| 4 - Code | `classDiagram` | Classes and interfaces inside one component |
| Dynamic | `C4Dynamic` | Runtime flow for a specific use case |
| Deployment | `C4Deployment` | How containers map to infrastructure |

## License

MIT
