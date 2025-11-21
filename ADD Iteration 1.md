# Iteration 1 – Initial Architecture for AIDAP

## 1. Iteration Goal and Selected Drivers

The goal of Iteration 1 is to create the initial architecture for the AI Powered Digital Assistant Platform (AIDAP) based directly on the project requirements. This iteration focuses on the foundational functional and quality drivers that shape the system.

### Selected Drivers
- **R1** Conversational access for institutional data  
- **R3** Integration with LMS, Registration, Calendar  
- **R5** AI models for natural language interpretation  
- **R7** Cloud deployable and scalable system  
- **RS1** Student conversational queries  
- **RS7, RS8** SSO security and privacy  
- **RS10, RS11** Response time and availability  
- **RA5** Security compliance  
- **RM7** Privacy and risk management  

These define the primary direction of the initial architecture.

---

## 2. Architectural Drivers

### 2 point 1 Functional Requirements (From AIDAP PDF)
- Conversational Q and A for students, lecturers, administrators  
- Integration with LMS, Registration, Academic Calendar, Email  
- Support for personalization and contextual responses  
- Role separation (student, lecturer, admin)  
- Ability to fetch deadlines, announcements, schedules, analytics  

### 2 point 2 Quality Attributes
- **Performance** Under 2 seconds average response time  
- **Availability** 99.5 percent uptime  
- **Security** SSO, RBAC, protected student data  
- **Scalability** Thousands of concurrent users  
- **Modifiability** Swap or upgrade AI models easily  
- **Usability** Simple conversational UI on web and mobile  

### 2 point 3 Constraints
- Must use existing university systems (LMS, Registration, Calendar)
- Must use standard APIs (REST)
- Must support cloud deployment
- Must support monitoring, logging, and model configuration

---

## 3. Key Scenarios

### Scenario S1 – Student Checks Next Exam
1. Student logs in via SSO  
2. Asks “When is my next exam for SOFE 3650”  
3. AI interprets intent  
4. System queries LMS and Registration  
5. Gathers date and returns result  

### Scenario S2 – Lecturer Posts Course Announcement
1. Lecturer logs in  
2. Issues command to publish announcement  
3. AI extracts course and content  
4. LMS adapter posts announcement  
5. Students receive message  

### Scenario S3 – Administrator Requests Analytics
1. Admin signs in  
2. Asks “Show weekly usage statistics”  
3. Analytics subsystem retrieves logs  
4. System returns summary  

---

## 4. Initial Architectural Solution

AIDAP is structured into three major layers.

### 4 point 1 Layered Architecture Overview
- **Presentation Layer**: Web chat, mobile chat  
- **Application and AI Layer**: AI query processing, response generation, personalization  
- **Integration Layer**: Connectors for LMS, Registration, Calendar, Email, and Analytics  

### 4 point 2 Subsystems
- User Interaction Gateway  
- Authentication and Identity Service  
- AI Query Processing Service  
- Response Generation and Personalization Module  
- Integration Adapters (LMS, Registration, Calendar, Email)  
- Data Storage for history and preferences  
- Monitoring and Configuration  

---

## 5. Risks and Open Issues
- AI interpretation accuracy under load  
- Handling LMS or Registration outages  
- Meeting availability targets  
- Privacy and data retention compliance  

---

## 6. Plan for Iteration 2
- Add **Logical Architecture Diagram**  
- Add **Process / Runtime View Diagram**  
- Add **Deployment Diagram**  
- Evaluate architecture against drivers  
- Refine security, performance, and reliability decisions  

---

### **Logical Architecture Diagram**
<img width="975" height="680" alt="image" src="https://github.com/user-attachments/assets/809d4dbc-bb88-49a9-984d-89fa980c9bc3" />
