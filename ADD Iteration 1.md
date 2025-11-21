## 1. Iteration Goal and Selected Drivers



The goal of iteration 1 is to make the initial architecture for the ai powered digital assistant platform (AIDAP) based on the project requirements. This iterations main focus is on the foundational, functional and quality drivers that create the system.



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

These define the main direction of the initial architecture.

---

## 2. Architectural Drivers

### 2 point 1 Functional Requirements (From AIDAP PDF)
- Conversational Q and A for students, lecturers and administrators  
- Integration with LMS, Registration, Academic Calendar, Email  
- Support for personalization and contextual responses  
- Role separation between student, lecturer and admin  
- Be able to fetch deadlines, announcements, schedules, analytics  

### 2 point 2 Quality Attributes
- **Performance** response timeunder 2 seconds on average
- **Availability** 99.5 percent uptime  
- **Security** SSO, RBAC, protected student data  
- **Scalability** Thousands of concurrent users  
- **Modifiability** Upgrade or swap AI models easily  
- **Usability** Simple conversational UI on web and mobile  

### 2 point 3 Constraints
-Must use the existing university systems (LMS, Calendar, Registration)
- Must use standard APIs (REST)
- Must support cloud deployment
- Must support logging, monitoring, and model configuration
---

## 3. Key Scenarios

### Scenario S1 – Student Checks Next Exam
1. Student logs in from SSO  
2. Asks “When is my next exam for SOFE 3650”  
3. AI interprets intent  
4. System queries Registration and LMS
5. Gathers date and returns the result  

### Scenario S2 – Lecturer Posts Course Announcement
1. Lecturer logs in  
2. Issues command to publish an announcement  
3. AI extracts course and its content
4. LMS adapter posts announcement  
5. Students receive the message  

### Scenario S3 – Administrator Requests Analytics
1. Admin signs in  
2. Asks “Show weekly usage statistics”  
3. Analytics subsystem retrieves logs  
4. System returns a summary  

---

## 4. Initial Architectural Solution

AIDAP is structured into three main layers.

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
