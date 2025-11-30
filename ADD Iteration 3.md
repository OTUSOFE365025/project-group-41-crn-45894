## Iteration 3: Addressing Quality Attribute Scenario Drivers

### Step 2 - Establish Iteration Goal by Selecting Drivers

For this iteration, the architect focuses on three high-priority quality attribute scenarios, all of which were identified as high-importance architectural drivers in Iteration 1. **Primary Drivers for Iteration 3:**

1. **QA-2 – Availability:** The system should stay available 99.5% of the time each month. If a server goes down, the assistant should automatically recover with little or no downtime.
2. **QA-1 – Performance:** When a student provides a natural-language question during normal use, the assistant should reply within 2 seconds on average under normal load.
3. **QA-3 – Scalability:** The assistant should be able to handle up to 5,000 concurrent users without slowing down or losing responses.

The goal of this iteration is to make sure that the physical deployment architecture of AIDAP supports high availability, stable performance, and elastic scalability. By addressing these drivers, the iteration focuses on making the system’s resilience stronger and making sure that AIDAP can continue serving requests despite failures, traffic spikes, or cloud-level disruptions.

### Step 3 - Choose One or More Elements of the System to Refine

For these scenarios, the elements that will be refined are the physical nodes that were identified during the first iteration:

* Application Server
* Database Server
* External Systems (LMS, Registration, Calendar)
* Monitoring and Logging Infrastructure

### Step 4 - Choose One or More Design Concepts that Satisfy the Selected Drivers

The design concepts used in this iteration are the following:

| **Design Decisions and Location** | **Rationale and Assumptions** |
|----------------------------------|-------------------------------|
| Introduce the active redundancy tactic by replicating the application server and database server | By replicating these critical nodes, the system can continue operating even if one instance fails. This supports the availability requirement (QA-2) by ensuring that AIDAP can automatically recover with minimal downtime. Replication also increases throughput and contributes to scalability (QA-3) when handling up to 5,000 users. |
| Introduce an element from the message queue technology family for asynchronous processing | Conversational requests and external-system synchronization events are placed in a message queue and processed by worker components. This separates the Application Server from sudden traffic spikes, thus helping maintain the 2-second response requirement (QA-1). Queues also prevent work from being lost during node failures, improving availability (QA-2) and supporting scalability (QA-3). |
| Introduce caching for frequently accessed institutional and conversational data | Caching reduces repeated database access for common queries, therefore decreasing latency and supporting the performance goal of responding within 2 seconds (QA-1). This reduced database load also improves system stability and supports scalability (QA-3) by allowing many concurrent users. |
| Introduce monitoring and heartbeat-based fault detection for critical components | Monitoring and heartbeat tactics allow the system to detect server failures or slow-responding external APIs. Fast detection supports automatic recovery and 99.5% uptime (QA-2) and enables the system to proactively trigger failover or scale-out events to maintain good performance. |
| Introduce concurrency to support parallel handling of requests and synchronization tasks | Processing events, conversations, and external-system updates in parallel increases throughput and reduces wait times, directly supporting performance (QA-1) and enabling the system to sustain high concurrency (QA-3). |
