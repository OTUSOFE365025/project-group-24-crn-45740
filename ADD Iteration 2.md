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

### 2.3 Sequence diagrams and service interfaces

The following sequence diagrams show how the logical architecture is used for key use cases.

- `diagrams/seq_student_query.puml`: student asking “When is my next exam”
- `diagrams/seq_lecturer_announcement.puml`: lecturer posting a course announcement

#### Table 2 – Service methods used in the sequence diagrams

| Service                                | Method name                                   | Description                                                               |
|----------------------------------------|-----------------------------------------------|---------------------------------------------------------------------------|
| Web Client                             | typeQuestion(text)                            | Captures the question that the user types                                |
| Web Client                             | submitAnnouncement(token, courseId, text)     | Sends an announcement form for a course                                  |
| API Gateway                            | postMessage(token, text)                      | HTTP endpoint for sending a new conversational message                   |
| API Gateway                            | postAnnouncement(token, courseId, text)       | HTTP endpoint for posting a new announcement                             |
| Auth and SSO Service                   | validateToken(token)                          | Validates SSO token and returns user identity and roles                  |
| Conversation Service                   | handleMessage(userId, text)                   | Main entry point for processing a conversational message                 |
| Conversation Service                   | handleAnnouncement(userId, courseId, text)    | Processes an announcement created by a lecturer                          |
| NLP Service                            | interpretQuery(text, context)                 | Performs intent detection and entity extraction                          |
| Academic Data Integration Service      | getNextExamSchedule(userId)                   | Reads upcoming exam information for the student from external systems    |
| Academic Data Integration Service      | saveAnnouncement(courseId, text)              | Persists the announcement for a course                                   |
| Notification Service                   | scheduleAnnouncementNotifications(courseId, text) | Schedules notifications to students subscribed to the course        |
| Web Client                             | showAnswer(responseText)                      | Displays the assistant answer to the student                              |
| Web Client                             | showSuccess()                                 | Shows a success message after the lecturer post                          |

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

### 2.5 Design decisions for Iterations 1 and 2

#### Table 4 – Main design decisions and justification

| Iteration | Decision                                                | Alternatives considered           | Justification                                                                                             |
|----------|----------------------------------------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| 1        | Use microservices with an API gateway                    | Single monolithic web application | Microservices allow independent scaling, clearer separation of concerns and easier integration with LMS  |
| 1        | Use AI based NLP for query interpretation                | Rule based keyword matching       | Natural language questions need flexible interpretation and learning from interaction history            |
| 1        | Store user profile and history in a dedicated service    | Embed all data in conversation    | Separates personalization concerns and supports future analytics and privacy rules                        |
| 2        | Add a message queue in front of the NLP service          | Direct synchronous calls only     | Smooths traffic spikes before exams and prevents timeouts while keeping response time within target      |
| 2        | Introduce a data synchronization service for LMS linkage | Each service calls LMS directly   | Centralizes retries, caching and conflict resolution and simplifies error handling                        |
| 2        | Use an event bus for notifications                       | Immediate direct sends only       | Events make deadline reminders and announcements reliable and decoupled from the main request path       |
