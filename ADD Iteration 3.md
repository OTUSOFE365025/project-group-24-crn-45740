# ADD Iteration 3 – Use Case 1: Ask a Question

This document applies the ADD process (Iteration 3) to **Use Case 1: Ask a Question** for the AI-Powered Digital Assistant Platform (AIDAP).

---

## 1. Confirm the Iteration Goal and Selected Drivers

### 1.1 Iteration Goal

Refine the architecture for **Use Case 1: Ask a question** so that a student can:

Use the AI system after having logged in through SSO to ask questions about deadlines, schedules, and grades and get an individualized response instantaneously while the system operates within the defined performance, availability, and security parameters.

Summary of Use Case 1:

- **Actor**: student  
- **Goal**: to ask the AI Assistant questions about deadlines and schedules as well as campus services  
- **Main flow**:  
  1. The student authenticates using SSO  
  2. The AI understands the question  
  3. The AI retrieves data from the calendar and/or LMS  
  4. The result is displayed in the chat  

### 1.2 Selected Quality Attribute Drivers

We focus on these quality attributes:

- **Performance**  
  - The assistant provides a response to a typical question asked by students within a span of **2 seconds** on an average basis on typical system load.

- **Availability and Reliability**  
  - The platform is able to continue operating even when an external source is slow or when a node crashes and should obtain at least 99.5 uptime.  

- **Security and Privacy**  
  - Users can only access their own information through SSO and authentication.  
  - No student information is disclosed to any other user.  

- **Personalization** (cross cutting)  
  - Responses are customized based on prior engagements and user preferences.  

---

## 2. Pick an Element to Improve

In this case, we focus on refining the **Conversational Core and Data Integration Path** based on the previous iterations - 1 and 2, and this will be the focus for Use Case 1.

This path is composed of:

- Web Browser Chat Client  
- API Gateway and Load Balancer  
- Authentication and SSO Adapter  
- Orchestrator of Conversations  
- NLP/Model Gateway  
- Personalization Service  
- Session and Context Store  
- Integration Facade Service (LMS, Registration, Calendar, Email)  
- AIDAP Database  
- Monitoring and Logging  

The reasons focus on this slice are:

- It defines the maximum allowable end-to-end latency (performance).  
- It would be the most impacted by external system failures (availability).  
- This is where SSO and authorization policies are applied (security).  
- It uses history and preferences (personalization).

---

## 3. Identify Design Concepts, Patterns, and Approaches

Design concepts used in Iteration 3:

- **Stateless microservices with shared session store**  
  - Conversation, NLP, personalization, and integration services are stateless.  
  - Recent context and session data are stored in a fast Session and Context Store.

- **API Gateway as single entry point**  
  - All external calls go through the gateway, which:  
    - Terminates HTTPS  
    - Validates SSO tokens  
    - Applies rate limits  
    - Routes requests to the right services  

- **Latency budgets and caching**  
  - Each hop (gateway, conversation, NLP, integration) has a rough time budget so total latency stays within 2 seconds.  
  - Integration Facade caches read-only data such as “next exam” or “today’s schedule” for a short time.

- **Bulkhead and circuit breaker patterns**  
  - External systems (LMS, Registration, Calendar) are accessed only via Integration Facade.  
  - Timeouts and circuit breakers will prevent slow or failing external systems from blocking the entire request pipeline.

- **Centralized authorization and auditing**  
  - Role and scope checks are done at the gateway and in Integration Facade.  
  - Access to the student-specific data is logged for auditing.

- **Configurable personalization depth**  
  - Personalization Service can use more or less history based of the request, trading off answer quality and latency.

---


## 4. Decompose the Selected Element

### 4.1 Components and Responsibilities

| Component                    | Responsibility                                                                                      |
|-----------------------------|-----------------------------------------------------------------------------------------------------|
| Chat Web Client             | Renders chat UI, sends messages to API Gateway, displays answers and error messages                |
| API Gateway                 | Terminates HTTPS, validates SSO tokens, enforces rate limits, routes chat requests                 |
| Auth and SSO Adapter        | Integrates with institutional SSO, validates tokens, exposes user identity and roles               |
| Conversation Orchestrator   | Coordinates the full chat turn: context lookup, NLP call, data queries, answer assembly            |
| NLP / Model Gateway         | Sends prompts and context to AI model, receives interpretation, extracts intents and entities      |
| Personalization Service     | Uses preferences and history summaries to adjust prompts and ranking of results                    |
| Session and Context Store   | Stores recent conversation turns and transient context keyed by sessionId and userId               |
| Integration Facade Service  | Provides stable APIs to query LMS, Registration, Calendar, and Email systems                       |
| AIDAP Database              | Stores long-term interaction history, user profiles, configuration, and analytics                  |
| Monitoring and Logging      | Collects metrics (latency, errors, throughput) and logs for debugging and auditing                 |

### 4.2 Allocation to Deployment Nodes

Deployment (reusing Iteration 2):

- **Student device** – Web browser running Chat Web Client.  
- **Web Frontend Host** – Serves the built React app and static assets.  
- **API Gateway and Load Balancer** – Hosts API Gateway and Auth / SSO Adapter, performs TLS termination and routing.  
- **Application Service Cluster** – Hosts Conversation Orchestrator, NLP / Model Gateway, Personalization Service, Integration Facade, Monitoring and Logging, and scales horizontally.  
- **Session and Context Store** – Shared in-memory data store (for example Redis) for session and context.  
- **AIDAP Database** – Relational or document database for persistent data.  
- **University Systems** – LMS, Registration, Calendar, and Email systems accessed through standard APIs.

---

## 5. Define Interfaces

### 5.1 Chat Web Client → API Gateway

Endpoint: `POST /api/chat/query`  

Request example:

    {
      "sessionId": "abc123",
      "userId": "s1234567",
      "message": "When is my next CSCI 3070U midterm?",
      "locale": "en-CA"
    }

Response example:

    {
      "answerText": "Your next CSCI 3070U midterm is on November 30 at 9:00 AM in UA 2220.",
      "sourceSystems": ["LMS", "Calendar"],
      "latencyMs": 850,
      "fromCache": false
    }

### 5.2 API Gateway → Conversation Orchestrator

Endpoint: `POST /conversation/handleQuery`  

Includes:

- userId and roles  
- validated token claims  
- sessionId  
- message text  
- locale  
- correlationId for logging  

### 5.3 Conversation Orchestrator → NLP / Model Gateway

Endpoint: `POST /nlp/interpret`  

Payload:

- message text  
- trimmed conversation history / context  
- latencyBudgetMs  

### 5.4 Conversation Orchestrator → Integration Facade Service

Example endpoints:

- `POST /integration/getNextExam`  
- `POST /integration/getUpcomingDeadlines`  
- `POST /integration/getTodaySchedule`  

Payload includes:

- userId and role  
- optional course identifiers  
- date ranges or filters  

### 5.5 Conversation Orchestrator / Personalization → Session and Context Store

- `GET /session/{sessionId}` – returns recent turns and flags.  
- `PUT /session/{sessionId}` – appends new turn and updates stored context.  

All internal interfaces are secured by internal authentication and reachable only within the cluster network.

---

## 6. Verify and Validate Against Drivers

### 6.1 Performance

**Scenario P1 – Normal query latency**

- Environment: 500–1000 concurrent users, all external systems healthy.  
- Stimulus: authenticated student asks “When is my next exam?”.  
- Response: answer returned to Chat Web Client within 2 seconds on average.

Design support:

- Stateless services with horizontal scaling in the Application Service Cluster.  
- Fast Session and Context Store for context lookups.  
- Short-lived caching of read-heavy data (for example next exam) in Integration Facade.  
- Latency budgets and timeouts configured in Conversation Orchestrator and Integration Facade.

### 6.2 Availability and Reliability

**Scenario A1 – LMS slowdown**

- Environment: LMS API is slow or temporarily unavailable.  
- Stimulus: student asks about upcoming exams that require LMS data.  
- Response:
  - Integration Facade uses cached data when possible.  
  - Circuit breakers open to stop repeated failing calls.  
  - AIDAP returns a cached or partial answer and indicates limited live data instead of hanging.

Design support:

- External dependencies are isolated behind Integration Facade.  
- Circuit breaker and retry policies are applied to external calls.  
- Independent scaling and health checks keep core services responsive.

### 6.3 Security and Privacy

**Scenario S1 – Unauthorized request**

- Environment: public internet.  
- Stimulus: request to `/api/chat/query` without a valid SSO token.  
- Response:
  - API Gateway rejects the request with an error code.  
  - No internal services are invoked.  
  - Event is logged as a security warning.

Design support:

- SSO token validation at API Gateway and Auth / SSO Adapter.  
- Internal services trust only validated tokens and check userId and roles before accessing data.  
- Access to LMS and registration data goes through Integration Facade with additional role checks.

---

## 7. Record Risks, Issues, and Trade-offs

### 7.1 Risks

- **R3.1 – Model latency spikes**  
  AI model inference may sometimes take longer than expected, causing responses to exceed the 2-second target.

- **R3.2 – External dependency failures**  
  Long outages or severe slowdowns in LMS or Calendar can result in stale or partial answers and reduced user trust.

- **R3.3 – Session consistency issues**  
  Bugs or race conditions in session read/write logic can break personalization or confuse the conversation context.

- **R3.4 – Misconfigured security rules**  
  Incorrect role or scope settings may either block legitimate access or accidentally expose data.

### 7.2 Non-Risks

- **NR3.1 – Chat UI rendering performance**  
  The chat UI is lightweight and unlikely to be the main performance bottleneck.

- **NR3.2 – SSO infrastructure stability**  
  The system reuses a mature institutional SSO system already used in production by other services.

- **NR3.3 – Internal network latency**  
  Internal service-to-service calls are fast compared with external API calls and are unlikely to dominate latency.

### 7.3 Sensitivity Points

- **SP3.1 – Cache TTL for academic data**  
  Short TTL improves freshness but increases load and latency.  
  Long TTL improves performance but risks stale exam dates and schedules.

- **SP3.2 – Maximum NLP inference time**  
  Higher values improve response richness but increase end-to-end latency.  
  Lower values protect latency but may cut off model responses.

- **SP3.3 – Session context size**  
  More context improves personalization and continuity but increases memory and access time.  
  Less context improves speed but may reduce answer quality.

- **SP3.4 – Autoscaling thresholds**  
  If too conservative, the system may not scale up quickly enough under spikes.  
  If too aggressive, the system may overprovision and waste resources.

### 7.4 Trade-offs

- **TP3.1 – Freshness vs performance**  
  Using only live data gives the freshest answers but increases latency and dependency on external systems.  
  Using cached data improves performance and resilience but may present slightly outdated information.

- **TP3.2 – Personalization depth vs latency**  
  Deeper personalization uses more history and computation, improving relevance but slowing responses.  
  Shallower personalization keeps responses fast but may feel generic.

- **TP3.3 – Security strictness vs usability**  
  Very strict security (short token lifetimes, more checks) improves protection but may require frequent logins and cause user frustration.  
  More relaxed settings improve convenience but increase risk.
