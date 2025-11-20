# ADD Iteration 2

## 1. Confirm the Iteration Goal and Selected Drivers

This iteration focuses on drivers that affect **security**, **performance**, **data-source reliability**, and **personalization**.

### 1.1 Performance & Scalability
- RS10  
- RS11  
- RA7  

### 1.2 Security & Privacy
- RS7  
- RS8  
- RA5  
- RM7  

### 1.3 Personalization & History
- R2  
- RS3  
- RS5  
- RS6  

### 1.4 Reliable Integration with University Systems
- R3  
- RD1  
- RD2  
- RD3  
- RD4  

---

## 2. Choose the Element to Refine

**Element Selected:** Conversational Core and Data Integration Subsystem

This refinement includes:
- Conversation Orchestrator  
- NLP / Model Gateway  
- Personalization Components  
- Session & Context Storage  

This subsystem was selected because it directly impacts **security**, **performance**, **personalization**, and **external integrations**.

---

## 3. Identify Design Concepts, Patterns, and Approaches

The following design concepts are applied in this iteration:

- Microservice decomposition of the conversational core  
- API Gateway as the system entry point  
- Stateless services with shared session/context store  
- Event-driven integration using queues and retries  
- Read-optimized query service for dashboards  
- Central monitoring and metrics for observability  

These concepts address availability, reliability, scalability, and personalization.

---

## 4. Decompose the Selected Element

The subsystem is decomposed into the following components:

1. **API Gateway Service**  
   Performs SSO token validation, routing, and rate-limiting  

2. **Conversation Orchestrator**  
   Manages the conversation flow and session retrieval  

3. **NLP / Model Gateway Service**  
   Handles AI model calls, versioning, prompt management, and timeout protection  

4. **Personalization Service**  
   Stores user preferences and history summaries  

5. **Integration Facade Service**  
   Connects to LMS, registration, calendar, and email systems, with retries and mapping  

6. **Session & Context Store**  
   Holds conversation context shared across user channels  

7. **Read-Optimized Dashboard Query Service**  
   Provides pre-computed analytics and fast dashboard access  

8. **Monitoring & Metrics Component**  
   Provides system-level metrics, logs, and traces  

---

## 5. Define Interfaces

### Key Interfaces

- **12-1: Client → API Gateway:**  
  Authentication, normalization, and forwarding  

- **12-2: API Gateway → Conversation Orchestrator:**  
  REST/gRPC internal calls  

- **12-3: Orchestrator ↔ Session & Context Store:**  
  Read/write session state  

- **12-4: Orchestrator → NLP / Model Gateway:**  
  Prompt requests, timeouts, circuit breakers  

- **12-5: Orchestrator ↔ Personalization Service:**  
  Fetch personalization data, submit history updates  

- **12-6: Orchestrator → Integration Facade:**  
  Real-time academic data access  

- **12-7: Integration Facade ↔ External Systems:**  
  REST/GraphQL, scheduled sync jobs, retry queues  

- **12-8: All Services → Monitoring System:**  
  Logs, metrics, traces  

---

## 6. Verify & Validate Against Drivers

### Performance & Scalability
- Stateless services  
- Session store  
- Read-optimized queries  
- Independent scaling  

### Security & Privacy
- SSO validation at gateway  
- Role-based access  
- Internal-only communication  
- Logging and auditing  

### Personalization
- Dedicated personalization service  
- Session continuity  
- History summaries  

### Integration Reliability
- Integration façade  
- Retry queues  
- Scheduled sync jobs  
- Circuit breakers  

All refined components support the targeted drivers.

---

## 7. Record Risks, Issues, and Trade-offs

### Remaining Architectural Risks

- Choosing message broker, session store, and monitoring stack  
- Balancing model latency with compute cost  
- Detailed RBAC rules for students, staff, faculty, and admins  
- History/log retention and privacy policies  
- Scaling during high-traffic periods (exams, registration weeks)  

### 2.4 Quality attributes addressed by the architecture

Table 3 links the main quality attributes from Iteration 1 to the concrete design decisions in Iteration 2.

| Quality attribute | Scenario                                                  | Architectural support                                                                          |
|-------------------|-----------------------------------------------------------|------------------------------------------------------------------------------------------------|
| Performance       | Answer user questions in under two seconds               | GPU capable NLP service behind a queue, caching in data services, stateless app instances     |
| Availability      | Remain available 99.5 percent of the time per month      | Multiple instances behind a load balanced API gateway and health checks in the cloud platform |
| Security          | Only authorized users see their own data                 | Institution SSO, token validation in the gateway, role based checks in each service           |
| Scalability       | Support at least 5000 concurrent users                   | Microservices with independent autoscaling groups and stateless conversation layer            |
| Modifiability     | Ability to switch AI model provider without major rework | NLP Service hides model provider behind a stable interface and configurable model version     |
| Usability         | Intuitive conversational interface                       | Dedicated Conversation Service plus Web and Mobile clients optimized for chat interactions    |
