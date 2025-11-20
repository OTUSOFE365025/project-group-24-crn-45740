Step 1 - Confirm the iteration Goal and & Selected Drivers
This iteration focuses on drivers that affect the security, performance, data-source reliability, and personalization. The selected drivers are:

  1. Performance & Scalability
    RS10
    RS11
    RA7
  2. Security & Privacy
    RS7
    RS8
    RA5
    RM7
  3. Personalization & History
    R2
    RS3
    RS5
    RS6
  4. Reliable Integration with University Systems
    R3
    RD1
    RD2
    RD3
    RD4

Step 2 - Choose to Element to Refine
Element Selected: Conversational Core and Data Integration Subsystem.

This Includes:
  - Conversation Orchestrator
  - NLP/Model Gateway
  - Personalization components
  - Session & contect storage

This element is refined because it directly impacts security, performance, personalization, and all external integrations.

Step 3 - Identify Design Concepts, Patterns, and Approaches
For this iteration, the following design concepts will be applied:
  - Microservice decomposition of the conversational core
  - API Gateway as the system entry point
  - Stateless services with a shared session/context store
  - Event-driven integration using queues and retries
  - Read-optimized query service for dashboards
  - Central monitoring/metrics for observability

These concepts address performance, availability, integration, reliability, and personalization.

Step 4 - Decompose the Selected Element
The subsystem is decomposed into the following elements:
  1. API Gateway Service
     Performs SSO token validation, routing, and rate limiting
  2. Converstion Orchestrator
     Manages dialogue flow, session retrieval, and routing to model and skill services.
  3. NLP / Model Gateway Service
     Handles AI model calls, versioning, and prompt management timeouts.
  4. Personalization Service
     Stores user preferences, history summaries, and returns personalization signals.
  5. Integration Facade Service
     Connects to LMS. registration, calendar, and email systems. Handles retries and data mapping.
  6. Session & Context Store
     Holds stateful conversation and user context, shared across channels.
  7. Read-Optimized Dashboard Query Service
     Pre-computes and serves fast dashboard views.
  8. Monitoring & Metrics Component
     Provides System-level and service-level observability.

Step 5 - Define Interfaces
Key interfaces include:
  12-1: Client -> API Gateway
    Auth, normalization, and request forwarding.
  12-2: API Gateway -> Conversation Orchestrator
    Internal REST/gRPC calls.
  12-3: Orchestrator <--> Session & Context Store
    Read/write session state.
  12-4: Orchestrator -> NLP/Model Gateway
    Prompt requests with timeouts and circuit breakers.
  12-5: Orchestrator <--> Personalization Service
    Fetch personalization data and submit history updates.
  12-6: Orchestrator -> Integration Facade
    Real-time data access.
  12-7: Integration Facade <--> External Systems
    REST/GraphSQL, scheduled sync, and retry queues.
  12-8: All services -> Monitoring System
    Logs, metrics, traces.

Step 6 — Verify & Validate Against Drivers

- Performance & scalability addressed by:
  Microservices, stateless services, session store, read-optimized querying, and independent scaling.

- Security & privacy addressed by:
  API Gateway SSO/token validation, internal-only service communication, role-based authorization, logging.

- Personalization addressed by:
  Dedicated Personalization service, session context continuity, and history summaries.

- Integration reliability is addressed by:
  Integration façade, message queues, scheduled syncs, retries, circuit breakers, isolation of external failures.

All selected design choices directly address the targeted drivers.

Step 7 — Record Risks, Issues, and Trade-offs
Remaining issues to be solved in future iterations:

- Selecting specific technologies for the message broker, session store, and monitoring.
- How to balance latency and cost for model calls.
- Detailed role-based access rules for faculty, staff, students, and admins.
- Long-term data retention/deletion policies for logs and user history.
- Capacity planning for peak loads (exam times, registration periods).
     
