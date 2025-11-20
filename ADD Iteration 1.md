# Iteration 1 – Architectural Drivers and Initial Design

## 1. Architectural Drivers

### 1.1 Functional Requirements
- Provide conversational interface for academic questions (R1, RS1)  
- Integrate with LMS, Registration, Calendars (R3)  
- Notify students about deadlines (RS2)  
- Allow lecturers to post announcements (RL2)  
- Provide analytics and dashboards (RS3, RL3)  
- Provide secure SSO authentication (RS7)

### 1.2 Quality Attributes
- **Performance:** Response time < 2 seconds (RS10)  
- **Availability:** 99.5 percent uptime (RS11)  
- **Security:** SSO, role-based access, institutional privacy compliance  
- **Scalability:** Support for 5000+ concurrent users (RA7)  
- **Modifiability:** Plug-in support for new AI models  
- **Usability:** Intuitive UI and conversational behaviour  

### 1.3 System Constraints
- Must integrate with existing university systems  
- Must use cloud-based deployment  
- Must comply with institutional security/privacy policies  
- Must use AI/NLP models for query interpretation (R5)

### 1.4 Architectural Concerns
- Protecting sensitive student data  
- Fast response times under load  
- Managing AI model updates  
- Ensuring authorization boundaries  
- Data consistency across multiple external systems  

---

## 2. Reference Architecture

**Chosen Architecture:**  
**Microservices + Event-Driven + Cloud-Native**

**Justification:**
- Independent scaling of services  
- AI processing isolated from user-facing components  
- Microservice APIs match LMS/Registration integration needs  
- Cloud-native deployment improves availability and resilience  
- Event-driven design supports notifications & reminders  

---

## 3. Top-Level Modules

### Core Components

1. **Conversational Interface Service**  
   Handles chat/voice input + output and conversation state  

2. **NLP / AI Processing Service**  
   Interprets intent, extracts entities, and generates responses  

3. **User Profile & Interaction History Service**  
   Stores personalization data  

4. **Academic Data Integration Service**  
   Connects to LMS, Registration, Calendar, Email systems  

5. **Notification Engine**  
   Sends reminders, deadlines, announcements  

6. **Analytics Service**  
   Dashboards and academic insights  

7. **Authentication / SSO Service**  
   OAuth + institutional SSO  

8. **API Gateway**  
   Entry point, request routing, rate limiting, auth enforcement  

9. **Frontend UI (Web + Mobile)**  
   Primary user-facing interface  

---

# 🚀 **INSERT DIAGRAM HERE**

## 1.5 Logical Architecture Diagram

Logical Architecture <img width="975" height="680" alt="image" src="https://github.com/user-attachments/assets/0cb03eeb-d6ba-49d7-9fb5-c0e2d88d5c33" />

---

## 1.6 Logical Architecture Elements Table

| Component Name                        | Responsibility                                                             |
|--------------------------------------|----------------------------------------------------------------------------|
| API Gateway                          | Routing, throttling, SSO token validation                                 |
| Conversation Service                 | Dialogue flow and orchestrating NLP + data services                       |
| NLP Service                          | AI model calls, NLP inference, intent classification                      |
| User Profile Service                 | Stores preferences and user history                                       |
| Academic Data Integration Service    | Connects to LMS, Registration, Calendar, Email systems                    |
| Notification Service                 | Sends announcements, reminders, deadlines                                 |
| Analytics Service                    | Generates dashboards and analytics                                        |
| Auth & SSO Service                   | Single sign-on integration + role validation                              |
| Event Bus                            | Asynchronous event handling and background tasks                          |
| AIDAP Database                       | Persistent storage for application data                                   |
| Mobile/Web Clients                   | User-facing UIs                                                           |
| University Systems                   | External LMS, Registration, Calendar, Email systems                       |

---
