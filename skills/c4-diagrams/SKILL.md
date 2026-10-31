---
name: c4-diagrams
description: Use when the user explicitly mentions C4 diagrams, C4 model, or C4 architecture. Produces Mermaid C4 diagrams (context, container, component, dynamic, deployment, code). Do NOT use for generic architecture diagrams, flowcharts, or sequence diagrams that don't follow the C4 model.
---

# C4 Diagram Skill

## Abstraction Hierarchy

```
Person ──uses──► Software System
                    └── Container        (runtime boundary: app or data store)
                          └── Component  (logical grouping behind an interface; NOT separately deployable)
                                └── Code (classes, interfaces, functions, tables)
```

**You don't need all 4 levels.** Context + Container are sufficient for most teams. Only go deeper when it adds value.

## Workflow

### Step 1: Classify the Diagram Level

| Signal | Diagram Type | File to Load |
|--------|-------------|--------------|
| "who uses the system", "big picture", "stakeholders" | System Context | `examples/context.md` |
| "architecture", "services", "what runs", "tech stack" | Container | `examples/container.md` |
| "inside this service", "modules", "internal structure" | Component | `examples/component.md` |
| "class design", "interfaces", "data model", "UML" | Code | `examples/code.md` |
| "how does X work", "sequence", "flow", "at runtime" | Dynamic | `examples/dynamic.md` |
| "infrastructure", "deployment", "where does it run" | Deployment | `examples/deployment.md` |

**Classification rule:** Infer the level if the user's intent is clear. If ambiguous, ask.

### Step 2: Get context and generate

Load the example file. Gather more context from the user's direction. Generate inline as a Mermaid code block.

### Step 3: Post-Generation Check

Run the `## Post-Generation Check` questions at the bottom of the example file. If any answer is **no** or **unsure**, OR the diagram has 10+ elements, multiple boundaries, or mixed external systems — load `review.md` and run the full checklist. Fix violations before presenting.

### Step 4: Present and Offer Next Steps

Present the diagram. Ask if/where to save.

## Reference

- `reference.md` contains C4 model definitions and the Mermaid function index. Load only if you need to verify abstraction classification or explain C4 concepts to the user.
