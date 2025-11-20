# SOFE3650F25-Project
SOFE3650F25 Project Repository Template  

# Business Drivers  

## Project Overview  
The AIDAP, which stands for AI-Powered Digital Assistant Platform, is a system that provides a talking interface to assist students, teachers, and administrators in managing and accessing academic and campus information with ease. Furthermore, it also collaborates with the respective university's system, such as the LMS, registration portal, calendar, and email services. Thus, through NLU, the system enables users to pose queries, check timetables, receive alerts, and manage academic records all in a quick and efficient manner.  

## Business Goals  
1. Improve Student Experience: Let students access academic information, events, and deadlines quickly using natural language
2. Increase Efficiency for Lecturers: Provide tools for lecturers to post announcements, materials, and track analytics through a single assistant
3. Centralize Communication: Unify all university communication platforms into one AI-driven system
4. Enhance Data-Driven Decision Making: Allow administrators and faculty members to view engagement trends and performance analytics
5. Reduce Operation Overhead: Automate repetitive tasks like reminders, schedule lookups, and data synchronization

## Primary Stakeholders  
Students(S): End users who interact with the AI assistant to get information and reminders.  
Lecturer(L): Users who post announcements, materials, and view analytics.  
Administrator(A): Users who manage data policies and integrations.  
System Maintainers(M): People responsible for deployment, updates, and maintenance.  
Data Source Systems(D): External university systems that provide information to the assistant.  

## Architectural Drivers
### Functional Drivers:  
- Conversational AI for query handling (R1, R5, RS1)
- Integration with external systems (R3, RD1, RD2)
- Real-time notifications and dashboard features (RS2, RS3, RL2)  

### Quality Attribute Drivers:  
- Performance: Responses within 2 seconds (RS10)
- Availability: 99.5% uptime with failover (RS11, RA6)
- Security: Enforced SSO and data privacy compliance (RS7, RA5)
- Scalability: Support 5,000 concurrent users (RA7)
- Modifiability: Easy integration of new AI services (RM5)  

### Business Constraints:
- Must comply with institutional data protection policies (R8, RA5)
- Must use existing university infrastructure and APIs (RD2)
- Cloud-native deployment with continuous updates (R7, RM1)

  ## Architecture Design Deliverables (ADD – Exercise 8)

The ADD iterations and diagrams are located here:

- Iteration 1: `docs/add_iterations.md` section 1 (Architectural Drivers and Top Level Architecture)
- Iteration 2: `docs/add_iterations.md` section 2 (Refinement, Quality Scenarios, Sequence and Deployment Views)
- Logical architecture diagram: `diagrams/logical_architecture.puml`
- Sequence diagrams and method table: `docs/add_iterations.md` section 2.3 and `diagrams/seq_student_query.puml`, `diagrams/seq_lecturer_announcement.puml`
- Deployment diagram: `diagrams/deployment_architecture.puml`

Open `docs/add_iterations.md` to read Iterations 1 and 2 in order.


## Summary  
AIDAP makes it easier for universities to run their operations by providing a single platform through which to access academic information. For students, this means receiving personalized help; for teachers, it is a matter of having efficient communication tools; and for administrators, it is a matter of being able to manage the entire process from one central location. These business and architectural drivers not only outline the foundation for all subsequent ADD iterations and design decisions but also help sustain them.
