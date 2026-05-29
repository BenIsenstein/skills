# Dynamic Diagram

## Syntax

```
C4Dynamic
    title Dynamic diagram for {System Name} - {Use Case}
```

### Elements

Use any elements from Context, Container, or Component levels — pick the abstraction that best illustrates the interaction:

```
Person(alias, "Label", "Description")
Container(alias, "Label", "Technology", "Description")
ContainerDb(alias, "Label", "Technology", "Description")
Component(alias, "Label", "Technology", "Description")
System_Ext(alias, "Label", "Description")
```

### Boundaries

```
Container_Boundary(alias, "Label") { ... }
System_Boundary(alias, "Label") { ... }
```

### Relationships

```
Rel(from, to, "Label", "Technology/Protocol")
```

**Sequence is determined by statement order.** Mermaid numbers interactions automatically based on the order `Rel` statements appear. Do NOT manually number them in labels.

### Styling (optional, for label positioning)

```
UpdateRelStyle(from, to, $textColor="...", $offsetX="...", $offsetY="...")
```

---

## Rules for This Level

- Scope: ONE specific feature, use case, or interaction pattern
- Shows how elements collaborate **at runtime** with ordered steps
- Can mix abstraction levels if needed (but prefer consistency)
- Statement order = sequence numbering (first Rel = step 1)
- Keep it focused — one flow per diagram, not the entire system
- Use sparingly: only for complex or non-obvious interactions

---

## Example

```mermaid
C4Dynamic
    title Dynamic diagram for Ride Sharing Platform - Ride Request Flow

    Person(rider, "Rider", "Requests a ride.")
    Container(mobile, "Mobile App", "React Native", "Rider-facing UI.")
    Container(api, "API Gateway", "Kong", "Routes and authenticates.")

    Container_Boundary(services, "Backend Services") {
        Container(trips, "Trip Service", "Go", "Manages trip lifecycle.")
        Container(matching, "Matching Service", "Go", "Finds and assigns drivers.")
    }

    ContainerDb(db, "Trip Database", "PostgreSQL", "Stores trip records.")
    ContainerDb(cache, "Location Cache", "Redis", "Real-time driver positions.")
    System_Ext(maps, "Mapping Service", "Routing and ETA.")

    Rel(rider, mobile, "Enters pickup and destination")
    Rel(mobile, api, "POST /rides with locations", "JSON/HTTPS")
    Rel(api, trips, "Creates new trip record", "gRPC")
    Rel(trips, db, "Persists trip with status=searching", "SQL/TCP")
    Rel(trips, matching, "Requests driver match for trip", "gRPC")
    Rel(matching, cache, "Queries drivers within 5km radius", "Redis protocol")
    Rel(matching, maps, "Calculates ETA for 8 candidates", "JSON/HTTPS")
    Rel(matching, trips, "Returns best match: driver_id=D42", "gRPC")
    Rel(trips, db, "Updates trip status=matched, driver=D42", "SQL/TCP")
    Rel(api, mobile, "Returns trip confirmation with ETA", "JSON/HTTPS")
```

---

## Post-Generation Check

- Does `Rel` statement order match the logical execution sequence?
- Is the diagram scoped to one specific use case or feature?
- Does each step describe a specific action (not vague "uses" or "calls")?
- Are inter-container/inter-system relationships labelled with protocol?
- Does every element have a name, type, and description?
