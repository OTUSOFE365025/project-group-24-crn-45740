# Iteration 2 – Refined Architecture and Views for AIDAP

## 1. Iteration Goal and Selected Drivers

The main goal for this iteration is to focus on drivers that shape security, performance, data reliability, and personalization:

- Performance and scalability (RS10, RA7)  
- Security and privacy (RS7, RS8, RA5, RM7)  
- Data source reliability (RD1 to RD3)  
- Personalization and analytics (RS3, R2, RL3, RA4)  

The element we selected to refine is the Conversational Core and Data Integration Subsystem.

---

## 2. Updated Scenarios

### Scenario S4 – High Load During Exam Week
- Thousands of simultaneous student questions  
- The system must maintain a sub-two-second response time  
- We also need to make sure it supports Horizontal scaling

### Scenario S5 – Data Source Outage
- The LMS or Registration service should become unavailable  
- The adapter should handle retry logic and partial degradation  
- The assistant should return partial but correct information  

### Scenario S6 – Personalized Student Dashboard
- The student will ask for upcoming deadlines and will also ask for risk indicators  
- This will require history, analytics, and real-time data  

---

## 3. Refined Architectural Views

### 3.1 Logical Architecture View

Our platform will be composed of the following logical components:

- Web and Mobile Clients  
- API Gateway  
- Authentication and Identity Service  
- Conversation Orchestrator  
- AI Model Adapter  
- Domain Services  
  - Student Service  
  - Lecturer Service  
  - Administrator Service  
  - Analytics Service  
- Integration Adapters  
  - LMS Adapter  
  - Registration Adapter  
  - Calendar Adapter  
  - Email Adapter  
- Database and Cache  
- Logging and Monitoring Services  

 
## **Logical Architecture Diagram**  
<img width="975" height="680" alt="image" src="https://github.com/user-attachments/assets/49621f10-98d3-4aba-b2d2-b68962c5e321" />


---

## 3.2 Process / Runtime View

This is an example that can be a typical runtime flow for student exam lookup:

1. Client sends query  
2. The API gateway forwards the request  
3. Identity service validates SSO token  
4. Orchestrator sends a message to the AI adapter  
5. AI detects intent + entities  
6. Domain service fetches required data  
7. Response generator formats output  

## **Lecturer Sequence Diagrams**
<img width="1709" height="917" alt="image" src="https://github.com/user-attachments/assets/a9176f55-71ba-4325-a9ab-72d1e8d64d1e" />

## **Student Sequence Diagram**
<img width="1416" height="567" alt="image" src="https://github.com/user-attachments/assets/a21665d5-2d54-4207-b348-23b827af6b70" />


---

## 3.3 Security View

Security decisions include:

- TLS for all external communication  
- SSO is mandatory for authentication  
- RBAC enforced at the domain service layer  
- Data encrypted at rest  
- Strict logging and monitoring for incident detection  

---

## 3.4 Deployment Architecture View

Deployment is actually cloud-based and will support horizontal scaling.

### Components:
- Load Balancer  
- API Gateway  
- Multiple AIDAP Application Servers  
- Database cluster  
- Cache layer  
- External systems: SSO, LMS, Registration, Calendar, Email  

 
## **Deployment Architecture Diagram**  
<img width="1903" height="1226" alt="image" src="https://github.com/user-attachments/assets/96b268df-1073-4153-96fd-b4687a7ef62b" />


---

## 4. Evaluation Against Drivers

### Performance and Scalability
- Horizontal autoscaling  
- Cached responses  
- Dedicated AI model adapter  

### Security and Privacy
- SSO integration  
- RBAC enforcement  
- Encrypted data storage  

### Data Reliability
- Retry policies  
- Partial data fallback  
- Health checks for adapters  

### Personalization and Analytics
- Historic conversation storage  
- LMS activity and calendar data aggregation  

---

## 5. Remaining Risks
- AI intent accuracy for domain-specific content  
- Performance under heavy exam period load  
- Privacy issues for conversation logs  

---
