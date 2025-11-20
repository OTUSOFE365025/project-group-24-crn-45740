# ADD Iteration 1

## 1. Architectural Drivers

### 1.1 Functional Requirements

- Provide conversational interface for academic questions (R1, RS1)
- Integrate with LMS, Registration, and Calendar systems (R3)
- Notify students about deadlines (RS2)
- Allow lecturers to post announcements (RL2)
- Provide analytics and dashboards (RS3, RL3)
- Support SSO authentication (RS7)

---

### 1.2 Quality Attributes

- **Performance**: Query response under two seconds
- **Availability**: System uptime of ninety nine point five percent
- **Security**: Single sign on and role based access control
- **Scalability**: Support for more than five thousand concurrent users
- **Modifiability**: Pluggable architecture for new AI and NLP models
- **Usability**: Simple interface with conversational interaction
- 
---

### 1.3 System Constraints

- Must integrate with existing university systems
- Must use cloud based deployment
- Must follow security and privacy policies
- Must use AI for natural language processing (R5)

---

### 1.4 Architectural Concerns

- Protect sensitive student data
- Maintain fast response times
- Manage updates for AI models
- Prevent unauthorized access
- Ensure data is consistent across systems

---

## 2. Reference Architecture

**Chosen Architecture:**  
Microservices plus event driven plus cloud native

**Justification:**

- Independent services support scaling and independent development
- AI processing benefits from being isolated into its own microservice
- Integrations with university systems align well with an API gateway pattern
- Cloud based deployment supports high availability and reliability targets
- Event driven interactions support notifications, reminders, and updates

---

## 3. Top Level Modules

### Core Components

**Conversational Interface Service**  
Handles text and voice input and output.

**NLP and AI Processing Service**  
Uses AI models to determine intent and generate responses.

**User Profile and Interaction History Service**  
Stores personalization information and interaction logs.

**Academic Data Integration Service**  
Connects to LMS, registration, calendar, and related academic systems.

**Notification Engine**  
Sends reminders, announcements, and event driven alerts.

**Analytics Service**  
Generates dashboards and analytics for students, lecturers, and administrators.

**Authentication and Single Sign On Service**  
Handles user identity, tokens, and role verification.

**API Gateway**  
Unified entry point for web and mobile requests.

**Frontend User Interface**  
Web and mobile interfaces for all users.

---

## 1.6 Logical Architecture Elements

### Table 1. Logical Architecture Elements

| Component Name                        | Responsibility                                                        |
|--------------------------------------|-----------------------------------------------------------------------|
| Web Client                           | Browser interface for students, lecturers, and administrators         |
| Mobile Client                        | Mobile interface for students                                         |
| API Gateway                          | Routing, rate limiting, request authentication, and access control    |
| Conversation Service                 | Manages dialog flow, context, and orchestrates service calls          |
| NLP Service                          | Performs intent recognition and entity extraction                     |
| User Profile Service                 | Stores personalization settings and interaction history               |
| Academic Data Integration Service    | Connects to LMS, registration and calendar systems                    |
| Notification Service                 | Sends alerts, reminders, and announcements                            |
| Analytics Service                    | Provides dashboards and analytics insights                            |
| Authentication and SSO Service       | Integrates with SSO and enforces roles and permissions                |
| AIDAP Database                       | Stores persistent application data                                    |
| Event Bus                            | Supports event driven communication for background tasks              |
| University External Systems          | LMS, registration, calendar and email servers                         |






