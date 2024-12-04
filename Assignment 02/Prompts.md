# AI Usage Documentation

This document outlines how AI systems were utilized during the development of the **Book My Show Clone** project, detailing the prompts used to gather ideas, resolve challenges, and streamline various aspects of software development.

---

## Purpose of Using AI

1. **Feature Inspiration and Brainstorming**: Gathering advanced features for the event booking platform.
2. **Software Engineering Documentation**: Assisting with the creation of essential documents like SRS (Software Requirements Specification), URD (User Requirements Document), and stakeholder analysis.
3. **Complex Functional Implementations**: Implementing functionalities such as secure user authentication, real-time seat selection, and payment gateway integration.
4. **Code Optimization and Debugging**: Offering solutions for code optimization and bug fixing.
5. **Project Structuring and Task Prioritization**: Providing suggestions on project structure and task management.
6. **Diagram Design**: Creating UML diagrams for platform architecture and use cases.

---

## Prompts Used

### 1. Project Overview
- **Prompt**: "Suggest advanced features for an event booking platform similar to Book My Show."
- **AI Response**:
  - Personalized recommendations.
  - Real-time seat selection.
  - E-ticket downloads with QR codes.
  - Admin panel for analytics and management.

### 2. Stakeholders Document
- **Prompt**: "List potential stakeholders for an event booking platform and their roles."
- **AI Response**:
  - **Primary Stakeholders**: End users (event attendees), Event organizers.
  - **Secondary Stakeholders**: Payment gateway providers, Platform admins.
  - **Tertiary Stakeholders**: Advertisers, Technology partners.

### 3. User Requirements Document (URD)
- **Prompt**: "What user requirements should be included in a URD for an event booking platform?"
- **AI Response**:
  - Users must register/login securely.
  - Users can browse events with advanced filters (location, price, etc.).
  - Support for multiple payment gateways.
  - Easy access to e-tickets and booking history.

### 4. Software Requirements Specification (SRS)
- **Prompt**: "Provide a template and example for an SRS document tailored for a web-based ticket booking system."
- **AI Response**:
  - **Functional Requirements**: User authentication, Payment processing.
  - **Non-Functional Requirements**: Scalability, multi-language support.
  - **Interfaces**: Web and mobile compatibility.

### 5. UML Diagrams
#### 5.1. Error Case Diagram
- **Prompt**: "Show an example for creating a error case diagram using PlantUML for event Booking App such as BookMyShow."
- **AI Response**:
  - Diagram Syntax:
    ```text
    @startuml

    actor "User" as User
    actor "Theater Manager" as Manager

    rectangle "BookMyShow Platform" as System {
        usecase "Booking Error" as UC_BookingError
        usecase "Payment Failure" as UC_PaymentFailure
        usecase "Incorrect Seat Allocation" as UC_SeatAllocationError
        usecase "Show Cancellation" as UC_ShowCancellation
        usecase "Server Downtime" as UC_ServerDowntime
        usecase "Data Sync Issues" as UC_DataSyncIssues
        usecase "Account Login Failure" as UC_LoginFailure
        usecase "Invalid Discount Code" as UC_InvalidDiscountCode
    }

    User --> UC_BookingError : Encounters
    User --> UC_PaymentFailure : Encounters
    User --> UC_SeatAllocationError : Affected by
    User --> UC_LoginFailure : Affected by
    User --> UC_InvalidDiscountCode : Encounters

    Manager --> UC_ShowCancellation : Notifies
    Manager --> UC_DataSyncIssues : Encounters

    UC_BookingError ..> UC_PaymentFailure : "May result from"
    UC_BookingError ..> UC_SeatAllocationError : "Leads to"
    UC_ServerDowntime ..> UC_BookingError : "Causes"
    UC_ServerDowntime ..> UC_LoginFailure : "Causes"
    UC_DataSyncIssues ..> UC_SeatAllocationError : "Results in"

    @enduml
    ```

#### 5.2. Component Diagram
- **Prompt**: "Generate a component diagram showing the core modules of the event booking platform."
- **AI Response**:
  - Components:
    - **Frontend**: React-based UI for users.
    - **Backend**: Node.js API for managing bookings.
    - **Database**: MongoDB for event and user data storage.
    - **Payment Gateway**: Integration module for multiple providers.

#### 5.3. Abuse Case Diagram
- **Prompt**: "Give an example for abuse case diagram for potential security risks on an event booking platform such as BookMyShow."
- **AI Response**:
  - Security Concerns:
    - Unauthorized access attempts.
    - Fraudulent payment processing.
    - Data breaches through insecure APIs.

### 6. Documentation Assistance
- **Prompt**: "How do I structure a software engineering project folder in GitHub?"
- **AI Response**:
  - **Root Directory**:
    - `README.md` (Project overview).
    - `docs/` (Documentation files like SRS, URD, etc.).
    - `src/` (Source code).
    - `tests/` (Unit and integration tests).

---

## Impact of AI

- **Time Saved**: AI significantly reduced the time needed for brainstorming, writing documentation, and designing diagrams.
- **Clarity**: The suggestions improved the structure and coverage of requirements and implementation plans.
- **Technical Support**: AI provided solutions for complex functionalities like real-time seat updates and secure payments.

---
