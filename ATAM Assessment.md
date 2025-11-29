# ATAM Assessment for AIDAP

## 1. Context and Goals

AIDAP is an AI powered digital assistant platform that helps students, lecturers, and administrators interact with the university systems (LMS, Registration, Calendar, Email) using a conversational interface and dashboards.

The goal of this ATAM assessment is to check how well the architecture satisfies the main quality attributes:

- Performance  
- Availability and reliability  
- Security and privacy  
- Modifiability  

We focus on the **Ask a question** path as it was clarified in ADD Iteration 3 and reuse the architecture and quality attributes defined in Iterations 1 and 2.

---

## 2. Utility Tree

### 2.1 Root

**Overall utility**  
Students and staff receive secure, reliable, and responsive access to institutional data through AIDAP.

### 2.2 Performance

- **P1 – Normal query latency**  
  - **Stimulus:** A student asks a question about schedules or deadlines during normal load.  
  - **Environment:** 500–1000 concurrent users, all external systems healthy.  
  - **Response:** AIDAP replies in ≤ 2 seconds on average.  
  - **Importance:** High  
  - **Risk:** Medium  

- **P2 – Peak exam load**  
  - **Stimulus:** Many students ask questions during the week before exams.  
  - **Environment:** Up to 5000 concurrent users, bursts of traffic.  
  - **Response:** System stays responsive, most queries are answered in less than or equal to 3 seconds with degradation if needed.  
  - **Importance:** High  
  - **Risk:** High  

### 2.3 Availability and Reliability

- **A1 – External system failure**  
  - **Stimulus:** LMS becomes slow or unavailable.  
  - **Environment:** Normal or peak usage.  
  - **Response:** AIDAP degrades, returning cached or incomplete answers and clear messages instead of timing out.  
  - **Importance:** High  
  - **Risk:** High  

- **A2 – Node or container failure**  
  - **Stimulus:** One application instance in the cluster crashes.  
  - **Environment:** Normal load.  
  - **Response:** Load balancer routes traffic to other instances, with only a brief disruption.  
  - **Importance:** Medium  
  - **Risk:** Medium  

### 2.4 Security and Privacy

- **S1 – Unauthorized access attempt**  
  - **Stimulus:** A request hits `/api/chat/query` without a valid SSO token.  
  - **Environment:** Internet facing API gateway.  
  - **Response:** Request is rejected, no data is returned and event is logged.  
  - **Importance:** High  
  - **Risk:** Low  

- **S2 – Cross user data isolation**  
  - **Stimulus:** Internal bug or misconfiguration that could show one student’s data to another.  
  - **Environment:** Normal operation.  
  - **Response:** Architecture and access control prevent this from happening; any suspicious behavior can be detected in logs.  
  - **Importance:** Very High  
  - **Risk:** Medium to High  

### 2.5 Modifiability

- **M1 – Add a new data source**  
  - **Stimulus:** University adds a new analytics or advising system that AIDAP must integrate.  
  - **Environment:** System is live.  
  - **Response:** A new adapter is added behind the Integration Facade without big changes to core services.  
  - **Importance:** Medium  
  - **Risk:** Medium  

- **M2 – Swap AI model provider**  
  - **Stimulus:** The AI model provider changes (for example a new API or vendor).  
  - **Environment:** System is live.  
  - **Response:** Only the NLP or Model Gateway implementation needs to change, with minimal impact on other services.  
  - **Importance:** Medium  
  - **Risk:** Medium  

---

## 3. ATAM Scenario and Risk Assessment Table

The table bellow classifies key scenarios as **risks**, **non risks**, **sensitivity points**, or **trade-off points**.

| ID  | Quality Attribute | Scenario Summary                                      | Type             | Impact if not satisfied                                               | Mitigation / Design Response                                          |
|-----|-------------------|------------------------------------------------------|------------------|------------------------------------------------------------------------|------------------------------------------------------------------------|
| P1  | Performance       | Normal student query answered within 2 seconds       | Non risk         | Mild user frustration if slightly slower                               | Stateless services, in memory session store, light caching            |
| P2  | Performance       | Peak exam load with many concurrent users            | Risk             | High latency, timeouts, negative perception of AIDAP                   | Auto scaling, horizontal scaling of services, capacity planning       |
| A1  | Availability      | LMS or Calendar APIs become slow or unavailable      | Risk             | Incomplete answers, timeouts, increased help desk tickets              | Integration Facade, circuit breakers, cached answers, clear messaging |
| A2  | Availability      | Application node or container crash                  | Sensitivity point| Short outage or errors if failover is slow                             | Health checks, rolling deployments, multiple instances per service    |
| S1  | Security          | Unauthorized call to chat API                        | Non risk         | Data exposure if auth is bypassed                                      | API Gateway SSO validation, token checks in internal services         |
| S2  | Security          | Cross user data leak                                 | Risk             | Severe privacy breach, loss of trust, policy violations                | Strong tenant isolation, user scoping, defensive coding, audits       |
| M1  | Modifiability     | Adding a new external system                         | Sensitivity point| Large refactor if coupling is high                                     | Stable Integration Facade interfaces, adapter pattern                 |
| M2  | Modifiability     | Swapping AI model provider                           | Trade-off point  | Downtime, inconsistent behavior during migration                       | NLP or Model Gateway abstraction, configuration driven model choice   |
| C1  | Cross cutting     | Cache TTL for exams and deadlines                    | Sensitivity point| Too short hurts performance; too long returns stale information        | Per endpoint cache configuration, monitoring of cache hit rates       |
| C2  | Cross cutting     | Depth of personalization vs response time            | Trade-off point  | Shallow personalization hurts relevance; deep personalization adds lag | Configurable personalization level, tuning based on metrics           |

---

## 4. Risks, Non Risks, Sensitivity Points, Trade-off Points

### 4.1 Risks

- **R1 – Performance at peak load (P2)**  
  During exam periods, if auto scaling and capacity planning are not enough, response times and error rates can get higher.

- **R2 – External dependency failures (A1)**  
  AIDAP depends on LMS, Registration, and Calendar APIs that might be slow or fail, which affects the quality and freshness of the answers.

- **R3 – Data privacy issues (S2)**  
  Any bug that mixes sessions or misuses user IDs can cause cross-user data exposure, which is a risk.

### 4.2 Non-risks

- **NR1 – Normal query latency (P1)**  
  Under expected loads, the combination of stateless services, in memory session store, and light caching should keep latency within targets.

- **NR2 – Basic authentication enforcement (S1)**  
  By centralizing SSO and token validation in the API Gateway and using institutional infrastructure again, the risk of accepting unauthenticated requests is little.

### 4.3 Sensitivity Points

- **SP1 – Cache configuration for academic data (C1)**  
  Changing cache TTL for exam dates and deadlines has a huge effect on both data freshness and performance.

- **SP2 – Cluster size and scaling policy (P2, A2)**  
  The number of instances per service and auto scaling thresholds directly affect performance and resilience under load.

- **SP3 – Coupling in Integration Facade (M1)**  
  If Integration Facade APIs are too tightly coupled to specific external schemas, adding new systems will be costly.

### 4.4 Trade-off Points

- **TP1 – Freshness vs performance (C1)**  
  More aggressive caching improves latency and reduces external API calls but increases the risk of showing slightly outdated information.

- **TP2 – Personalization depth vs latency (C2)**  
  Using more context and historical data improves answer quality but increases processing time and data access overhead.

- **TP3 – Strict security vs convenience (S1, S2)**  
  Short token lifetimes and strict scopes improve security but may require students to re-authenticate more often.

---

## 5. Summary of ATAM Findings

- The current architecture supports the main business drivers by providing a microservice-based and scalable conversational platform with central security and integration points.  
- The **highest risks** are concentrated around peak performance, failures or slowdowns in external university systems, and strict data privacy requirements.  
- **Key sensitivity points** are cache configuration, sizing and scaling of the service cluster, and the design of the Integration Facade.  
- **Major trade-offs** involve balancing performance against data freshness and personalization depth, and balancing tight security controls against ease of use for students and staff.

These ATAM results should be revisited if new requirements are introduced (for example additional user roles or new institutional systems) or if the deployment environment changes.
