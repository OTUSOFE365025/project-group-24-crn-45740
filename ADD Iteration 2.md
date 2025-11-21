# Iteration 2 – Refined Architecture and Views for AIDAP

## 1. Iteration Goal and Selected Drivers

The goal of Iteration 2 is to refine the AIDAP architecture and produce detailed architectural views. The focus of this iteration is on drivers related to:

- Performance and scalability (RS10, RA7)  
- Security and privacy (RS7, RS8, RA5, RM7)  
- Data source reliability (RD1 to RD3)  
- Personalization and analytics (RS3, R2, RL3, RA4)  

These represent the highest risk and highest architectural impact areas.

---

## 2. Updated Scenarios

### Scenario S4 – High Load During Exam Week
- Thousands of simultaneous student questions  
- System must maintain sub two second response time  
- Horizontal scaling required  

### Scenario S5 – Data Source Outage
- LMS or Registration service becomes unavailable  
- Adapter handles retry logic and partial degradation  
- Assistant returns partial but correct information  

### Scenario S6 – Personalized Student Dashboard
- Student asks for upcoming deadlines plus risk indicators  
- Requires history, analytics, and real time data  

---

## 3. Refined Architectural Views

### 3.1 Logical Architecture View

The platform is composed of the following logical components:

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

A typical runtime flow for student exam lookup:

1. Client sends query  
2. API gateway forwards request  
3. Identity service validates SSO token  
4. Orchestrator sends message to AI adapter  
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
- RBAC enforced at domain service layer  
- Data encrypted at rest  
- Strict logging and monitoring for incident detection  

---

## 3.4 Deployment Architecture View

Deployment is cloud based and supports horizontal scaling.

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
- AI intent accuracy for domain specific content  
- Performance under heavy exam period load  
- Privacy issues for conversation logs  

---

