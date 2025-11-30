## ATAM Utility Tree 
![ATAM Utility Tree](utilitytree.png)

## ATAM Analysis

### Sensitivities:
- **S1: Sensitivity to failure-detection interval** - The system’s availability is highly sensitive to how frequently the load balancer and monitoring service check the health of each AppServer replica. If the heartbeat interval is too long, the system takes longer to notice failures, which directly increases downtime.
- **S2: Sensitivity to database replication lag** - The failover behavior depends on how quickly the replica database receives updates from the primary. High replication lag means that switching to the replica could result in stale or inconsistent data, which negatively affects perceived availability.
- **S3: Sensitivity to message queue durability settings** - If the queue is configured with low durability, messages could be lost when a server fails. This impacts availability because losing requests is functionally equivalent to the system being “down” for the user.
- **S4: Sensitivity to external API timeout thresholds** - Since AIDAP relies on LMS, Registration, and Calendar APIs, the timeout settings become a major sensitivity point. If timeouts are too long, the system appears unresponsive. If they are too short, the system may trigger unnecessary failovers.

### Tradeoffs:
- **T1: Availability (+) vs. Performance (–)** - Increasing the frequency of heartbeat allows failures to be detected faster, improving availability. However, this introduces extra network and processing overhead, which can reduce performance.
- **T2: Availability (+) vs. Consistency (–)** - Using asynchronous database replication keeps performance high and reduces system bottlenecks, but it can also introduce stale data during failover. This improves availability guarantees but slows down write operations.
- **T3: Availability (+) vs. Cost (–)** - Running multiple AppServer replicas and a database replica significantly increases uptime, but this adds extra cloud resource cost. A minimal deployment would cost less but wouldn’t achieve the same availability target.
- **T4: Availability (+) vs. Responsiveness (–)** - Longer external API timeout thresholds reduce the chance of unnecessary failover events, improving overall stability. However, long timeouts may make the system feel sluggish, especially if an external service is failing.

### Risks:
- **R1: The load balancer might act as a single point of failure** - If the load balancer itself is not replicated or highly available, then the entire system availability depends on that single component, which introduces a major risk.
- **R2: Failure-detection latency may be too high** - If the health-check interval or threshold is poorly configured, server failures may not be detected quickly enough, causing a longer downtime than acceptable.
- **R3: Replica database may not be fully synchronized during failover** - If the replica lags behind the primary, the system might recover quickly but with stale data. This could lead users to think the system is malfunctioning even though it’s “available.”
- **R4: External APIs may cause cascading failures** - If LMS/Registration/Calendar APIs hang without timing out properly, AIDAP might appear unresponsive even though internal components are fine.
- **R5: Worker processes may stall under heavy load** - If message-queue workers cannot keep up with peak demand, requests may queue indefinitely, leading users to experience freezes or timeouts.

### Non-risks:
- **N1: Stateless AppServer replicas** - Because the application servers are stateless, failover between replicas does not interrupt user sessions. This design choice already aligns well with high availability goals.
- **N2: Use of a durable message queue** - The presence of a message queue that supports durable storage ensures that in-flight messages are not lost when servers crash, reducing the risk of functional downtime.
- **N3: Separation of external services through the ExternalAPIConnector** - This abstraction isolates failures in external systems and prevents them from taking down the core application, making this a non-risk.
- **N4: Automated monitoring is already implemented** - Since heartbeat monitoring is provided by the load balancer and/or monitoring service, the system does not depend on manual detection of failures.
- **N5: Multiple AppServer replicas** - The system is no longer dependent on a single application instance, removing one of the largest availability risks present.

---
### Analysis Scenario

| **Analysis Scenario** | **P1.1** |
|-----------------------|----------|
| **Scenario** | A failure occurs in one of the AIDAP system components (AppServer, load balancer routing, or database), and the system resumes operation with little or no downtime (99.5% monthly uptime target). |

### Attributes Table

| **Attributes** | **Availability** |
|----------------|------------------|
| **Stimulus** | A server instance, the database primary, or an external dependency fails during normal operation. |
| **Environment** | Normal load of around 50–100 concurrent users; stable institutional network; AIDAP deployed using the 3-tier architecture with AppServer replicas and DB replica. |
| **Response** | The system detects the failure automatically, reroutes or fails over to a healthy replica, and restores full availability within a few seconds without losing user requests. |

### Architecture Decision Analysis

| **Architecture Decision** | **Sensitivity** | **Tradeoff** | **Risk** | **Nonrisk** |
|---------------------------|-----------------|--------------|----------|-------------|
| AD1 - Three-tier architecture and single DB | S2 | T2 | R2 | - |
| AD2 - Replicated AppServer tier behind a load balancer | S1 | T1 | R1 | N5 |
| AD3 - Database cluster (primary & replica) with asynchronous replication | S2, S4 | T2, T4 | R3 | N3 |
| AD4 - Fault-detection mechanism (heartbeat + monitoring service) | S1 | T1 | R4 | N4 |
| AD5 - ExternalAPIConnector abstraction for external system isolation | S4 | T4 | R5 | N3 |
| AD6 - Durable message queue for asynchronous tasks | S3 | T3 | R5 | N2 |

