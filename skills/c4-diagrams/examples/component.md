# Component Diagram

## Syntax

```
C4Component
    title Component diagram for {System Name} - {Container Name}
```

### Elements

```
Container(alias, "Label", "Technology", "Description")
Container_Ext(alias, "Label", "Technology", "Description")
ContainerDb(alias, "Label", "Technology", "Description")
System_Ext(alias, "Label", "Description")

Component(alias, "Label", "Technology", "Description")
Component_Ext(alias, "Label", "Technology", "Description")
ComponentDb(alias, "Label", "Technology", "Description")
ComponentQueue(alias, "Label", "Technology", "Description")
```

### Boundaries

```
Container_Boundary(alias, "Label") {
    ...components...
}
```

### Relationships

```
Rel(from, to, "Label", "Technology/Protocol")
Rel(from, to, "Label")
```

Within a container, relationships between components don't need protocol (same process). Relationships TO external containers still need protocol.

---

## Rules for This Level

- Scope: ONE container, zoomed in
- Shows logical groupings of functionality behind interfaces
- Components are NOT separately deployable — the container is the deployment unit
- Other containers shown as external context (single boxes, not decomposed)
- Only create this diagram if it adds value beyond the Container diagram
- Each component should represent a cohesive responsibility, not a package/namespace

---

## Example

```mermaid
C4Component
    title Component diagram for Ride Sharing Platform - Matching Service

    Container(api, "API Gateway", "Kong", "Routes requests to services.")
    ContainerDb(cache, "Location Cache", "Redis", "Real-time driver locations.")
    ContainerQueue(events, "Event Bus", "Kafka", "Domain event distribution.")
    System_Ext(maps, "Mapping Service", "Provides routing and ETAs.")

    Container_Boundary(matching, "Matching Service") {
        Component(handler, "Request Handler", "Go net/http", "Accepts ride match requests from API gateway.")
        Component(finder, "Driver Finder", "Go module", "Queries nearby available drivers within radius.")
        Component(scorer, "Match Scorer", "Go module", "Ranks candidate drivers by distance, rating, and ETA.")
        Component(dispatcher, "Dispatch Controller", "Go module", "Assigns best driver and publishes match event.")
        Component(geo, "Geo Client", "Go module", "Wraps calls to mapping service for distance/ETA.")
    }

    Rel(api, handler, "Sends match requests to", "gRPC")
    Rel(handler, finder, "Passes ride location to")
    Rel(finder, cache, "Queries drivers within radius", "Redis protocol")
    Rel(finder, scorer, "Passes candidate list to")
    Rel(scorer, geo, "Requests ETA calculations from")
    Rel(geo, maps, "Calculates distance and ETA via", "JSON/HTTPS")
    Rel(scorer, dispatcher, "Passes ranked candidates to")
    Rel(dispatcher, events, "Publishes driver-matched event to", "Kafka protocol")
```

---

## Post-Generation Check

- Is the diagram scoped to exactly one container?
- Does every component represent a cohesive responsibility (not a package/namespace)?
- Are external containers shown as single boxes (not decomposed)?
- Do relationships to external containers include protocol/technology?
- Does every element have a name, type, and description?
