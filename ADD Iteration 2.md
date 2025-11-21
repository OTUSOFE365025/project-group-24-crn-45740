# Iteration 2 – Conversational Core and Data Integration Refinement

## 1. Iteration Goal and Selected Drivers

The main goal for this iteration is to focus on drivers that shape security, performance, data reliability, and personalization.

### Performance and Scalability
- RS10
- RS11
- RA7

### Security and Privacy
- RS7
- RS8
- RA5
- RM7

### Personalization and History
- R2
- RS3
- RS5
- RS6

### Reliable Integration with University Systems
- R3
- RD1
- RD2
- RD3
- RD4

---

## 2. Element Selected for Refinement

The element we selected to refine is the Conversational Core and Data Integration Subsystem.

This section will cover:
- Conversation Orchestrator
- NLP / Model Gateway
- Personalization Components
- Session & Context Storage

We picked this subsystem because it falls between security, performance, personalization, and external integrations.

---

## 3. Design Concepts Used in This Iteration

Now we will go over the design concepts we are using in this iteration:

- Breaking up the conversational core into microservices
- Using an API Gateway as the front door
- Running stateless services that lean on a shared session/context store
- Handling integrations with event queues and retries
- Speeding up dashboards with a read-optimized query service
- Keeping tabs on everything with central monitoring and metrics

Each of these concepts will help with availability, reliability, scalability, and of course personalization.

---

## 4. Breakdown of Subsystem Components

Now, breaking down the subsystem itself, here's what we end up with:

### API Gateway Service
- Checks SSO tokens, routes traffic, and rate-limits requests

### Conversation Orchestrator
- Controls the conversation flow and pulls session info

### NLP / Model Gateway Service
- Deals with AI model calls, keeps track of versions, manages prompts, and protects against timeouts

### Personalization Service
- Stores what users like and their conversation history

### Integration Facade Service
- Connects to LMS, registration, calendar, and email systems, handling retries and data mapping

### Session & Context Store
- Keeps conversation context available across channels

### Read-Optimized Dashboard Query Service
- Delivers quick analytics and dashboard access through pre-computed data

### Monitoring & Metrics Component
- Tracks system metrics, logs, and traces

---

## 5. Key Interfaces

Here are the key interfaces:

- Client to API Gateway: Handles authentication, normalizes requests, and forwards them
- API Gateway to Conversation Orchestrator: Internal calls over REST/gRPC
- Orchestrator and Session Store: Reads and writes session state
- Orchestrator to NLP / Model Gateway: Sends prompts, manages timeouts, applies circuit breakers
- Orchestrator and Personalization Service: Fetches user data, submits history updates
- Orchestrator to Integration Facade: Gets real-time academic data
- Integration Facade to External Systems: Uses REST/GraphQL, schedules sync jobs, manages retries
- All Services to the Monitoring System: Ship logs, metrics, and trace data

---

## 6. Driver Support Summary

### Performance & Scalability
- Stateless services  
- Central session store  
- Read-optimized queries  
- Services can scale independently  

### Security & Privacy
- SSO validation at the gateway  
- Role-based access control  
- Internal-only communications  
- Logging and auditing  

### Personalization
- Dedicated personalization service  
- Session continuity  
- Storing history summaries  

### Integration Reliability
- Integration facade  
- Retry queues  
- Scheduled sync jobs  
- Circuit breakers  

All of our components have been refined to support the refinements of each of the drivers.

---

## 7. Remaining Risks

Risks and trade-offs still on the table:

- Picking the right message broker, session store, and monitoring stack
- Balancing model latency against compute costs
- Nailing down detailed RBAC rules for different user groups
- Deciding on history/log retention and privacy policies
- Ensuring that everything scales smoothly during peak times, like exams or registration

---

## 8. Sequence Diagrams & Interfaces

Now here are our sequence diagrams and service interfaces. This will show how the architecture supports the real-world use cases.


