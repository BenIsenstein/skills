# Deployment Diagram

## Syntax

```
C4Deployment
    title Deployment diagram for {System Name} - {Environment}
```

### Elements

```
Deployment_Node(alias, "Label", "Type/Technology", "Description") {
    ...nested nodes or containers...
}
Node(alias, "Label", "Type", "Description") { ... }

Container(alias, "Label", "Technology", "Description")
ContainerDb(alias, "Label", "Technology", "Description")
ContainerQueue(alias, "Label", "Technology", "Description")
```

### Nesting

Deployment nodes nest inside each other to represent infrastructure hierarchy:

```
Deployment_Node(cloud, "AWS", "us-east-1") {
    Deployment_Node(cluster, "ECS Cluster", "Fargate") {
        Deployment_Node(task, "api-task x3", "Docker") {
            Container(api, "API Server", "Node.js", "Handles requests.")
        }
    }
}
```

### Relationships

```
Rel(from, to, "Label", "Technology/Protocol")
Rel_U(from, to, "Label", "Technology")
Rel_D(from, to, "Label", "Technology")
Rel_L(from, to, "Label", "Technology")
Rel_R(from, to, "Label", "Technology")
```

---

## Rules for This Level

- Scope: ONE deployment environment (production, staging, dev — not mixed)
- Shows how container **instances** map to infrastructure
- Deployment nodes can be nested (cloud > region > cluster > host > runtime)
- Include infrastructure nodes where relevant (DNS, load balancers, firewalls)
- Show replication (e.g. "api-service x3") in the node label
- Container instances inside deployment nodes are the same containers from the Container diagram
- This is orthogonal to the C4 zoom hierarchy — it answers "where does it run" not "what is it"

---

## Example

```mermaid
C4Deployment
    title Deployment diagram for Ride Sharing Platform - Production

    Deployment_Node(phone, "Driver/Rider Phone", "iOS / Android") {
        Container(mobile, "Mobile App", "React Native", "Rider and driver UI.")
    }

    Deployment_Node(aws, "AWS", "us-east-1") {

        Deployment_Node(alb, "Application Load Balancer", "ALB") {
            Container(gateway, "API Gateway", "Kong on ECS", "Routes, auth, rate limits.")
        }

        Deployment_Node(ecs, "ECS Cluster", "Fargate") {
            Deployment_Node(matchTask, "matching-service x4", "Docker") {
                Container(matching, "Matching Service", "Go", "Driver-rider matching.")
            }
            Deployment_Node(tripTask, "trip-service x3", "Docker") {
                Container(trips, "Trip Service", "Go", "Trip lifecycle management.")
            }
            Deployment_Node(payTask, "payment-service x2", "Docker") {
                Container(payments, "Payment Service", "Node.js", "Payment orchestration.")
            }
        }

        Deployment_Node(rds, "RDS", "db.r6g.xlarge, Multi-AZ") {
            ContainerDb(db, "Trip Database", "PostgreSQL 15", "Trips, users, payments.")
        }

        Deployment_Node(elasticache, "ElastiCache", "cache.r6g.large, cluster mode") {
            ContainerDb(cache, "Location Cache", "Redis 7", "Real-time driver positions.")
        }

        Deployment_Node(msk, "Amazon MSK", "kafka.m5.large x3") {
            ContainerQueue(events, "Event Bus", "Kafka 3.5", "Domain event streaming.")
        }
    }

    Rel(mobile, gateway, "Makes API calls to", "JSON/HTTPS")
    Rel(gateway, matching, "Routes match requests to", "gRPC")
    Rel(gateway, trips, "Routes trip operations to", "gRPC")
    Rel(matching, cache, "Queries driver locations", "Redis protocol")
    Rel(matching, events, "Publishes match events", "Kafka protocol")
    Rel(trips, db, "Reads/writes trip data", "SQL/TCP")
    Rel(trips, events, "Publishes trip events", "Kafka protocol")
    Rel(events, payments, "Delivers payment triggers", "Kafka protocol")
    Rel(payments, db, "Reads/writes payment records", "SQL/TCP")
    Rel_R(db, db, "Multi-AZ replication")
```

---

## Post-Generation Check

- Is the diagram scoped to exactly one deployment environment?
- Does every container instance live inside a deployment node?
- Is replication/scaling expressed in deployment node labels?
- Are deployment nodes nested to reflect the real infrastructure hierarchy?
- Does every element have a name, type, and description?
