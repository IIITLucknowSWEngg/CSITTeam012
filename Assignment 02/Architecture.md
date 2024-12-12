# Architecture Documentation

This document contains all the diagrams and their respective PlantUML code for the project.

---

## 1. System Context Diagram

### Diagram
![System Context Diagram](/Assignment%2002/Diagrams/Context.png)  

### Code
```plantuml
@startuml

' External Actors
actor "Attendee" as Attendee
actor "Admin" as Admin
actor "Event Organizer" as EventOrganizer

' External Systems
rectangle "PCI-DSS Compliant Payment Gateway" as PaymentGateway
rectangle "Email/SMS Notification Service" as NotificationService
database "Database Server" as Database

' System Boundary: BookMyShow
package "BookMyShow Application" {

    ' Subsystems
    rectangle "User Registration \nand Authentication" as Registration
    rectangle "Event Browsing \nand Discovery" as EventBrowsing
    rectangle "Seat Booking \nand Availability" as SeatBooking
    rectangle "Payment Processing" as PaymentProcessing
    rectangle "Admin Panel" as AdminPanel
    rectangle "Notifications \nand Alerts" as Notifications
    rectangle "Event Management" as EventManagement
    rectangle "User Profile Management" as UserProfile
}

' Relationships between actors and system components
Attendee --> Registration : Sign Up/Login
Attendee --> EventBrowsing : Browse Events
Attendee --> SeatBooking : Book Tickets
Attendee --> PaymentProcessing : Pay for Tickets
Attendee --> Notifications : Receive Digital Tickets/Updates
Attendee --> UserProfile : Manage Profile

Admin --> AdminPanel : Manage Users, Events, and Bookings
Admin --> EventManagement : Approve/Modify/Delete Events
Admin --> Notifications : Send Promotional Notifications

EventOrganizer --> EventManagement : Create/Update Event Details

' External System Interactions
EventManagement --> Database : Store Event Information
SeatBooking --> Database : Update Seat Availability
Registration --> Database : Manage User Accounts
UserProfile --> Database : Retrieve/Update User Data

PaymentProcessing --> PaymentGateway : Secure Payment Processing
Notifications --> NotificationService : Send Emails/SMS for Digital Tickets

@enduml
```
---

## 2. Container Diagram

### Diagram
![Container Diagram](/Assignment%2002/Diagrams/Container.png)  

### Code
```plantuml
@startuml

skinparam component {
    BackgroundColor<<frontend>> LightSkyBlue
    BackgroundColor<<backend>> LightSteelBlue
    BackgroundColor<<microservice>> LightGoldenrodYellow
    BackgroundColor<<database>> LightGreen
    BackgroundColor<<queue>> Moccasin
    BackgroundColor<<external>> Lavender
}

' External Users
actor "Attendee" as Attendee
actor "Admin" as Admin
actor "Event Organizer" as EventOrganizer

' System Boundary: BookMyShow
package "BookMyShow System" {

    ' Web Application
    rectangle "Web Application" as WebApp <<backend>> {
        component "Frontend (React)" as WebFrontend <<frontend>>
        component "Backend (Node.js + Express)" as WebBackend <<backend>>
    }

    ' Mobile Application
    rectangle "Mobile App" as MobileApp <<backend>> {
        component "Mobile Frontend (Flutter)" as MobileFrontend <<frontend>>
        component "Mobile Backend (API Gateway)" as MobileBackend <<backend>>
    }

    ' Microservices
    package "Microservices" <<microservice>> {
        component "Authentication Service" as AuthService <<microservice>>
        component "Event Management Service" as EventService <<microservice>>
        component "Seat Management Service" as SeatService <<microservice>>
        component "Payment Service" as PaymentService <<microservice>>
        component "Notification Service" as NotificationService <<microservice>>
    }

    ' Data Storage
    database "Relational Database (MySQL)" as RelationalDB <<database>>
    database "NoSQL Database (MongoDB)" as NoSQLDB <<database>>
    queue "Message Queue (RabbitMQ)" as MessageQueue <<queue>>
}

' External Services
package "External Services" <<external>> {
    rectangle "Payment Gateway" as PaymentGateway <<external>>
    rectangle "Email/SMS Service" as EmailService <<external>>
}

' Relationships and Data Flow
Attendee --> WebFrontend : "Access Website"
Attendee --> MobileFrontend : "Use Mobile App"
WebFrontend --> WebBackend : "API Calls"
MobileFrontend --> MobileBackend : "API Calls"

WebBackend --> AuthService : "Login/Signup"
MobileBackend --> AuthService : "Login/Signup"

WebBackend --> EventService : "Browse Events"
MobileBackend --> EventService : "Browse Events"

WebBackend --> SeatService : "Book/Cancel Tickets"
MobileBackend --> SeatService : "Book/Cancel Tickets"

WebBackend --> PaymentService : "Process Payment"
MobileBackend --> PaymentService : "Process Payment"

PaymentService --> PaymentGateway : "Secure Transactions"

NotificationService --> EmailService : "Send Notifications"
WebBackend --> NotificationService : "Notify Users"
MobileBackend --> NotificationService : "Notify Users"

EventService --> RelationalDB : "Store Event Data"
SeatService --> RelationalDB : "Update Seat Availability"
AuthService --> NoSQLDB : "Manage User Sessions"
NotificationService --> MessageQueue : "Queue Notifications"

Admin --> WebFrontend : "Manage Platform"
EventOrganizer --> WebFrontend : "Submit Events"

@enduml
```
---

## 3. Component Diagram

### Diagram
![Component Diagram](/Assignment%2002/Diagrams/Component.png)  

### Code
```plantuml
@startuml

skinparam component {
    BackgroundColor<<frontend>> LightSkyBlue
    BackgroundColor<<backend>> LightSteelBlue
    BackgroundColor<<service>> LightGoldenrodYellow
    BackgroundColor<<database>> LightGreen
    BackgroundColor<<external>> Lavender
    BackgroundColor<<queue>> Moccasin
}

' External Users
actor "Attendee" as User
actor "Admin" as Admin
actor "Event Organizer" as Organizer

' Web Application Components
package "Web Application" {
    component "Web Frontend" as WebFrontend <<frontend>> {
        [Home Page]
        [Event Search]
        [User Dashboard]
        [Ticket Booking]
    }
    component "Web Backend" as WebBackend <<backend>> {
        [API Controller]
        [Authentication Module]
        [Event Management Module]
        [Payment Processing Module]
        [Notification Module]
    }
}

' Mobile Application Components
package "Mobile Application" {
    component "Mobile Frontend" as MobileFrontend <<frontend>> {
        [Home Screen]
        [Event List]
        [Booking Confirmation]
        [User Profile]
    }
    component "Mobile Backend" as MobileBackend <<backend>> {
        [API Gateway]
        [Authentication Handler]
        [Event Handler]
        [Payment Handler]
        [Notification Sender]
    }
}

' Microservices
package "Microservices" <<service>> {
    component "Authentication Service" as AuthService <<service>> {
        [User Registration]
        [Session Management]
        [Password Recovery]
    }
    component "Event Management Service" as EventService <<service>> {
        [Event Creation]
        [Event Listing]
        [Event Search]
    }
    component "Seat Management Service" as SeatService <<service>> {
        [Seat Allocation]
        [Seat Availability]
        [Seat Cancellation]
    }
    component "Payment Service" as PaymentService <<service>> {
        [Payment Processing]
        [Transaction Logs]
        [Refunds]
    }
    component "Notification Service" as NotificationService <<service>> {
        [Email Notifications]
        [SMS Alerts]
        [Push Notifications]
    }
}

' Databases
database "Relational Database (MySQL)" as RelationalDB <<database>> {
    [User Data]
    [Event Data]
    [Booking Data]
}
database "NoSQL Database (MongoDB)" as NoSQLDB <<database>> {
    [Session Data]
}
queue "Message Queue (RabbitMQ)" as MessageQueue <<queue>> {
    [Notification Jobs]
}

' External Systems
package "External Services" <<external>> {
    component "Payment Gateway" as PaymentGateway <<external>> {
        [Transaction API]
    }
    component "Email/SMS Service" as EmailService <<external>> {
        [Send Emails]
        [Send SMS]
    }
}

' Relationships between components
User --> WebFrontend : "Access Website"
User --> MobileFrontend : "Use Mobile App"
Admin --> WebFrontend : "Manage Events/Users"
Organizer --> WebFrontend : "Add Events"

WebFrontend --> WebBackend : "REST API Calls"
MobileFrontend --> MobileBackend : "API Requests"

WebBackend --> AuthService : "Authenticate User"
MobileBackend --> AuthService : "Authenticate User"

WebBackend --> EventService : "Event Management"
MobileBackend --> EventService : "Event Management"

WebBackend --> SeatService : "Seat Operations"
MobileBackend --> SeatService : "Seat Operations"

WebBackend --> PaymentService : "Initiate Payments"
MobileBackend --> PaymentService : "Initiate Payments"

WebBackend --> NotificationService : "Trigger Notifications"
MobileBackend --> NotificationService : "Trigger Notifications"

PaymentService --> PaymentGateway : "Process Payments"
NotificationService --> EmailService : "Send Notifications"

AuthService --> RelationalDB : "Store User Info"
EventService --> RelationalDB : "Event Details"
SeatService --> RelationalDB : "Booking Info"
AuthService --> NoSQLDB : "Manage Sessions"
NotificationService --> MessageQueue : "Queue Notifications"

@enduml
```
---

## 4. Deployment Diagram

### Diagram
![Deployment Diagram](/Assignment%2002/Diagrams/Deployment.png)  

### Code
```plantuml
@startuml

skinparam component {
    BackgroundColor<<server>> LightSkyBlue
    BackgroundColor<<db>> LightGreen
    BackgroundColor<<service>> LightGoldenrodYellow
    BackgroundColor<<client>> Lavender
    BackgroundColor<<external>> Moccasin
}

' External Actors
actor "Attendee" as Attendee
actor "Admin" as Admin
actor "Event Organizer" as Organizer

' Client Devices
node "Client Devices" <<client>> {
    component "Web Frontend" as WebBrowser
    component "Mobile Frontend" as MobileApp
}

' Load Balancer
node "Load Balancer" <<server>> {
    component "AWS Elastic Load Balancer" as ELB
}

' Application Servers
node "Application Server Cluster" <<server>> {
    component "Web Backend (Node.js with Express)" as WebBackend
    component "Mobile Backend (Node.js with Express)" as MobileBackend
    node "Microservices Server" <<server>> {
        component "Authentication Service" as AuthService
        component "Event Service" as EventService
        component "Seat Management Service" as SeatService
        component "Payment Service" as PaymentService
        component "Notification Service" as NotificationService
    }
}

' Caching Layer
node "Caching Layer" <<server>> {
    component "Redis Cache" as Redis
}

' Asynchronous Processing
node "Asynchronous Processing" <<server>> {
    component "Task Queue (AWS SQS)" as TaskQueue
    component "Message Queue (RabbitMQ)" as RabbitMQ
}

' Databases
node "Database Cluster" <<db>> {
    database "Relational DB (MySQL with Read Replicas)" as RelationalDB <<db>>
    database "NoSQL DB (MongoDB with Sharding)" as NoSQLDB <<db>>
}

' External Services
node "External Services" <<external>> {
    component "Payment Gateway" as PaymentGateway
    component "Email/SMS Service" as EmailService
}

' Cloud Platform
cloud "Cloud Infrastructure (AWS, GCP, Azure)" as Cloud {
    RelationalDB -down-> NoSQLDB : "Auto-scaling & Sharding"
    ELB -down-> WebBackend : "Horizontal Scaling"
    ELB -down-> MobileBackend : "Horizontal Scaling"
}

' Relationships
Attendee --> WebBrowser : "Access Website"
Attendee --> MobileApp : "Use Mobile App"
Admin --> WebBrowser : "Manage Events/Users"
Organizer --> WebBrowser : "Create Events"

WebBrowser --> ELB : "API Requests"
MobileApp --> ELB : "API Requests"

ELB --> WebBackend : "Route to Web Backend"
ELB --> MobileBackend : "Route to Mobile Backend"

WebBackend --> Redis : "Fetch Cached Data"
WebBackend --> AuthService : "Authenticate User"
WebBackend --> EventService : "Manage Events"
WebBackend --> SeatService : "Manage Seat Operations"
WebBackend --> PaymentService : "Initiate Payment"
WebBackend --> NotificationService : "Trigger Notifications"

MobileBackend --> Redis : "Fetch Cached Data"
MobileBackend --> AuthService : "Authenticate User"
MobileBackend --> EventService : "Manage Events"
MobileBackend --> SeatService : "Manage Seat Operations"
MobileBackend --> PaymentService : "Initiate Payment"
MobileBackend --> NotificationService : "Trigger Notifications"

AuthService --> RelationalDB : "User Data Queries"
EventService --> RelationalDB : "Event Data Queries"
SeatService --> RelationalDB : "Booking Data Queries"
PaymentService --> PaymentGateway : "Process Payments"
NotificationService --> EmailService : "Send Notifications"

TaskQueue --> NotificationService : "Background Notifications"
TaskQueue --> PaymentService : "Process Payments Asynchronously"
TaskQueue --> SeatService : "Background Seat Updates"

RabbitMQ --> NotificationService : "Queue Notifications"

@enduml
```
---





