# Container Diagram

## Syntax

```
C4Container
    title Container diagram for {System Name}
```

### Elements

```
Person(alias, "Label", "Description")
System_Ext(alias, "Label", "Description")

Container(alias, "Label", "Technology", "Description")
Container_Ext(alias, "Label", "Technology", "Description")
ContainerDb(alias, "Label", "Technology", "Description")
ContainerDb_Ext(alias, "Label", "Technology", "Description")
ContainerQueue(alias, "Label", "Technology", "Description")
ContainerQueue_Ext(alias, "Label", "Technology", "Description")
```

### Boundaries

```
Container_Boundary(alias, "Label") {
    ...containers...
}
System_Boundary(alias, "Label") {
    ...containers...
}
```

### Relationships

```
Rel(from, to, "Label", "Technology/Protocol")
Rel_Back(from, to, "Label", "Technology/Protocol")
Rel_U(from, to, "Label", "Technology/Protocol")
Rel_D(from, to, "Label", "Technology/Protocol")
Rel_L(from, to, "Label", "Technology/Protocol")
Rel_R(from, to, "Label", "Technology/Protocol")
```

At container level, **inter-container relationships MUST include technology/protocol**.

---

## Rules for This Level

- Scope: one software system, zoomed in
- Every container specifies its technology
- External systems shown as single boxes (not decomposed)
- NO deployment details (no load balancers, clusters, replicas)
- Relationships between containers must show protocol (HTTP, gRPC, AMQP, JDBC, etc.)

---

## Example

```mermaid
C4Container
    title Container diagram for Ride Sharing Platform

    Person(rider, "Rider", "Requests rides via mobile app.")
    Person(driver, "Driver", "Accepts rides and reports location.")
    System_Ext(maps, "Mapping Service", "Provides routing and ETAs.")
    System_Ext(payment, "Payment Processor", "Handles transactions.")

    Container_Boundary(platform, "Ride Sharing Platform") {
        Container(mobile, "Mobile App", "React Native", "Rider and driver-facing UI for ride requests and tracking.")
        Container(api, "API Gateway", "Kong", "Routes requests, handles auth, rate limiting.")
        Container(matching, "Matching Service", "Go", "Matches riders with nearby available drivers.")
        Container(trips, "Trip Service", "Go", "Manages trip lifecycle from request to completion.")
        Container(payments, "Payment Service", "Node.js", "Orchestrates charges, refunds, and driver payouts.")
        ContainerDb(db, "Trip Database", "PostgreSQL", "Stores trips, users, payment records.")
        ContainerDb(cache, "Location Cache", "Redis", "Stores real-time driver locations for fast lookups.")
        ContainerQueue(events, "Event Bus", "Kafka", "Distributes domain events between services.")
    }

    Rel(rider, mobile, "Requests and tracks rides", "HTTPS")
    Rel(driver, mobile, "Accepts rides and shares location", "HTTPS")
    Rel(mobile, api, "Makes API calls to", "JSON/HTTPS")
    Rel(api, matching, "Forwards ride requests to", "gRPC")
    Rel(api, trips, "Forwards trip operations to", "gRPC")
    Rel(matching, cache, "Reads driver locations from", "Redis protocol")
    Rel(matching, events, "Publishes match events to", "Kafka protocol")
    Rel(trips, db, "Reads/writes trip data", "SQL/TCP")
    Rel(trips, events, "Publishes trip state changes to", "Kafka protocol")
    Rel(payments, db, "Reads/writes payment records", "SQL/TCP")
    Rel(payments, payment, "Charges riders via", "JSON/HTTPS")
    Rel(events, payments, "Delivers trip-completed events to", "Kafka protocol")
    Rel(matching, maps, "Calculates distance/ETA via", "JSON/HTTPS")
```

---

## Post-Generation Check

- Does every container specify its technology?
- Does every inter-container relationship include protocol/technology?
- Are there zero deployment details (load balancers, clusters, replicas)?
- Are external systems shown as single boxes (not decomposed)?
- Does every element have a name, type, and description?
