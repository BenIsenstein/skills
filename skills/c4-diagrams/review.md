# C4 Diagram Review Checklist

Run this checklist against every diagram before considering it complete.

## Diagram-Level Checks

- [ ] Diagram has a title in the form: `{Type} diagram for {Scope}` (e.g. "Container diagram for Internet Banking System")
- [ ] Diagram has a key/legend explaining all notation (shapes, line styles, colours if used)
- [ ] Diagram operates at exactly ONE abstraction level — no mixing
- [ ] All acronyms/abbreviations are either universally known or explained in the legend

## Element Checks

For every element (box) in the diagram:

- [ ] Has a name
- [ ] Has its type explicitly stated (Person, Software System, Container, Component)
- [ ] Has a short description of its key responsibilities
- [ ] Container and Component elements specify technology (e.g. "Java/Spring Boot", "PostgreSQL 15")
- [ ] No element is labelled with generic terms like "business logic" or "utilities"

## Relationship Checks

For every line/arrow in the diagram:

- [ ] Is unidirectional (one arrow direction)
- [ ] Has a label describing intent or data flow
- [ ] Label reads naturally in the direction of the arrow
- [ ] Label is specific — not "Uses", "Calls", or "Depends on" without qualification
- [ ] Inter-container relationships specify technology/protocol (HTTP/REST, gRPC, AMQP, JDBC, etc.)

## Level-Specific Checks

### System Context (Level 1)
- [ ] The focal system is clearly centred/emphasised
- [ ] Only persons and external systems are shown (no internal containers)
- [ ] No technology details on relationships (high-level intent only)

### Container (Level 2)
- [ ] Every container in the focal system is shown
- [ ] No deployment details (no load balancers, clusters, replicas)
- [ ] External systems shown as single boxes (not decomposed)
- [ ] Technology is specified for each container

### Component (Level 3)
- [ ] Scope is a single container (not the whole system)
- [ ] Each component represents a logical grouping, not a JAR/DLL/package
- [ ] Other containers shown as external context (not decomposed)

### Deployment
- [ ] Scope is a single deployment environment (not mixed prod/staging)
- [ ] Deployment nodes are nested appropriately (cloud > region > VM > container runtime)
- [ ] Container instances are mapped to deployment nodes
- [ ] Infrastructure nodes (DNS, LB, firewall) are shown where relevant

## Common Mistakes

- Labelling a library, JAR, or DLL as a Container (it's not a runtime boundary)
- Showing a Docker container as the same thing as a C4 Container (different concepts)
- Creating Component diagrams that don't add value over the Container diagram
- Mixing deployment topology into a Container diagram
- Using bidirectional arrows (always pick a direction)
- Naming inconsistency: same element called different names across diagrams
