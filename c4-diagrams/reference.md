# C4 Model Reference

## Abstractions

### Person

A user of the system: an actor, role, persona, or named individual.

### Software System

The highest-level abstraction. Something that delivers value to its users. Typically what a single team owns/builds. May map to one source repo.

Examples: internet banking system, email system, mainframe banking system.

### Container

A runtime boundary — an application or data store that must be running for the system to work.

**IS:** a separately deployable/runnable process or runtime.
**IS NOT:** a Docker container, a JAR/DLL, a library, a module, a namespace, a package.

Examples: server-side web app (Spring Boot, Rails), single-page app (React, Angular), mobile app, database (PostgreSQL, Redis), serverless function (Lambda).

Note: A web app with a significant JavaScript SPA = two containers (server + client). External data services you control (S3 bucket, RDS schema) = containers, not external systems.

### Component

A grouping of related functionality encapsulated behind a well-defined interface, within a single container. **Not separately deployable** — the container is the deployment unit. All components in a container share the same process space.

Examples: authentication module, notification service facade, order processing engine.

### Code

Classes, interfaces, functions, database tables. The building blocks of the programming language. Lowest level.

---

## Diagram Levels

### Level 1: System Context

| Field | Value |
|-------|-------|
| Scope | A single software system |
| Shows | Your system centred, surrounded by its users and the other systems it interacts with |
| Primary elements | The software system in scope |
| Supporting elements | Persons and external software systems directly connected |
| Audience | Everyone — technical and non-technical |
| Recommended? | **YES** — always |

Detail is deliberately low. No technologies, no protocols. Big picture only.

### Level 2: Container

| Field | Value |
|-------|-------|
| Scope | A single software system (zoomed in) |
| Shows | High-level architecture shape, responsibility distribution, major technology choices, inter-container communication |
| Primary elements | Containers within the system in scope |
| Supporting elements | Persons and external software systems directly connected to containers |
| Audience | Technical people inside and outside the team |
| Recommended? | **YES** — always |

Says nothing about deployment (clustering, load balancers, replication). Use deployment diagrams for that.

### Level 3: Component

| Field | Value |
|-------|-------|
| Scope | A single container (zoomed in) |
| Shows | Components inside the container, their responsibilities, technology/implementation details |
| Primary elements | Components within the container |
| Supporting elements | Other containers, persons, and external systems connected to components |
| Audience | Architects and developers |
| Recommended? | **Maybe** — only if it adds value. Consider auto-generating. |

### Level 4: Code

| Field | Value |
|-------|-------|
| Scope | A single component (zoomed in) |
| Shows | How a component is implemented (class diagrams, ER diagrams) |
| Primary elements | Code elements (classes, interfaces, functions, tables) |
| Audience | Architects and developers |
| Recommended? | **Rarely** — only for the most critical/complex components. Ideally auto-generated. |

---

## Supplementary Diagrams

### System Landscape

A system context diagram without a focal system. Shows how all systems in an enterprise/department relate. Useful for large organisations.

### Dynamic

Shows how elements collaborate at runtime for a specific use case/feature. Numbered interactions indicate ordering. Can operate at any abstraction level (systems, containers, or components).

### Deployment

Shows how container instances map to infrastructure (physical, virtual, containerised). Uses deployment nodes (nested) and infrastructure nodes (DNS, LBs, firewalls). Create one per environment (prod, staging, dev).

---

## Mermaid Function Index

### Elements

| Function | Parameters | Used In |
|----------|-----------|---------|
| `Person` / `Person_Ext` | `(alias, label, ?descr)` | Context, Dynamic |
| `System` / `System_Ext` | `(alias, label, ?descr)` | Context, Dynamic |
| `SystemDb` / `SystemDb_Ext` | `(alias, label, ?descr)` | Context |
| `SystemQueue` / `SystemQueue_Ext` | `(alias, label, ?descr)` | Context |
| `Container` / `Container_Ext` | `(alias, label, ?techn, ?descr)` | Container, Component, Dynamic, Deployment |
| `ContainerDb` / `ContainerDb_Ext` | `(alias, label, ?techn, ?descr)` | Container, Component, Dynamic, Deployment |
| `ContainerQueue` / `ContainerQueue_Ext` | `(alias, label, ?techn, ?descr)` | Container, Deployment |
| `Component` / `Component_Ext` | `(alias, label, ?techn, ?descr)` | Component, Dynamic |
| `ComponentDb` / `ComponentDb_Ext` | `(alias, label, ?techn, ?descr)` | Component |
| `ComponentQueue` / `ComponentQueue_Ext` | `(alias, label, ?techn, ?descr)` | Component |
| `Deployment_Node` / `Node` | `(alias, label, ?type, ?descr)` | Deployment |

### Boundaries

| Function | Parameters |
|----------|-----------|
| `Enterprise_Boundary` | `(alias, label) { ... }` |
| `System_Boundary` | `(alias, label) { ... }` |
| `Container_Boundary` | `(alias, label) { ... }` |
| `Boundary` | `(alias, label, ?type) { ... }` |

### Relationships

| Function | Direction |
|----------|-----------|
| `Rel(from, to, label, ?techn)` | Default |
| `BiRel(from, to, label, ?techn)` | Bidirectional |
| `Rel_U` / `Rel_D` / `Rel_L` / `Rel_R` | Hint layout direction |
| `Rel_Back(from, to, label, ?techn)` | Reverse |

---

## Styling (Optional)

```
UpdateElementStyle(alias, $bgColor="...", $fontColor="...", $borderColor="...", $shadowing="true/false")
UpdateRelStyle(from, to, $textColor="...", $lineColor="...", $offsetX="...", $offsetY="...")
UpdateLayoutConfig($c4ShapeInRow="4", $c4BoundaryInRow="2")
```

---

## Key Notes

- `?` prefix in signatures means optional parameter
- Named parameters use `$` prefix: `$bgColor="red"`
- `_Ext` suffix = external element (rendered with different styling)
- `Db` suffix = database shape; `Queue` suffix = queue shape
- In Dynamic diagrams, `Rel` statement order determines sequence numbering
- `sprite`, `tags`, `link` parameters are accepted but not yet rendered by Mermaid

