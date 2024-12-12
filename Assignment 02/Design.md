# Software Design Description (SDD) for BookMyShow Competitor

## 1. Introduction

### 1.1 Purpose

This document provides a comprehensive software design description for the BookMyShow application, outlining its architecture and design to guide development and ensure all stakeholder requirements are met.

### 1.2 Scope

The SDD covers the design aspects of the BookMyShow application, including system architecture, component interactions, data management, and user interfaces. The application is built using a microservices architecture, with each service focused on specific functionalities.

### 1.3 Overview

BookMyShow facilitates movie and event ticket bookings by integrating with multiple theater chains, providing real-time seat availability, and supporting various payment methods. The system is designed to handle high user concurrency and ensure data consistency.

### 1.4 References

- IEEE Std 1016-2009: IEEE Standard for Information Technology—Systems Design—Software Design Descriptions.
- [Design BookMyShow – A System Design Interview Question](https://www.geeksforgeeks.org/design-bookmyshow-a-system-design-interview-question/)
- [Design a movie ticket booking system like BookMyShow](https://www.geeksforgeeks.org/design-movie-ticket-booking-system-like-bookmyshow/)

### 1.5 Definitions and Acronyms

- **SDD**: Software Design Description
- **API**: Application Programming Interface
- **UML**: Unified Modeling Language

---

## 2. System Overview

The BookMyShow system employs a microservices-based architecture. Each service is designed for scalability, fault tolerance, and efficient processing.

---

## 3. System Architecture

### 3.1 Architectural Design

The system comprises the following components:

- **Mobile Frontend (Flutter)**: Enables user interaction on mobile devices.
- **Web Frontend (React)**: Provides a responsive user interface for web users.
- **Backend Services**: API Gateway routes requests to microservices, ensuring modularity.

### 3.2 Microservices

- **Authentication Service**: Handles user login, signup, and session management.
- **Event Management Service**: Manages events, movies, showtimes, and venues.
- **Seat Management Service**: Updates and tracks seat availability.
- **Payment Service**: Processes payments securely.
- **Notification Service**: Sends booking confirmations and promotional notifications.

---

## 4. Data Design

### 4.1 MongoDB Schema (Authentication Service)

```json
{
  "_id": ObjectId,
  "name": String,
  "email": String,
  "password_hash": String,
  "phone_number": String,
  "preferences": {
    "genres": [String],
    "languages": [String],
    "notifications": Boolean
  },
  "created_at": Date,
  "last_login": Date
}
```
* password hashed using AES-256 Encryption

### 4.2 MySQL Schemas

#### **Movies Table (Event Management Service)**

```sql
CREATE TABLE Movies (
    movie_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    genre VARCHAR(100),
    language VARCHAR(50),
    release_date DATE,
    duration INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Venues Table (Event Management Service)**

```sql
CREATE TABLE Venues (
    venue_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    location VARCHAR(255),
    total_screens INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Shows Table (Event Management Service)**

```sql
CREATE TABLE Shows (
    show_id INT AUTO_INCREMENT PRIMARY KEY,
    movie_id INT NOT NULL,
    venue_id INT NOT NULL,
    show_date DATE NOT NULL,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    screen_number INT,
    available_seats INT,
    price DECIMAL(10, 2),
    pricing_tier ENUM('VIP', 'Premium', 'General') NOT NULL,
    seating_chart JSON,
    rating DECIMAL(3, 2) DEFAULT 0.00,
    reviews JSON, -- Stores a JSON array of reviews
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (movie_id) REFERENCES Movies(movie_id),
    FOREIGN KEY (venue_id) REFERENCES Venues(venue_id)
);
```
### **Reviews Table (Event Management Service)**

```sql
CREATE TABLE Reviews (
    review_id INT AUTO_INCREMENT PRIMARY KEY,
    show_id INT NOT NULL, -- Foreign key to the Shows table
    user_id INT,          -- Optional: Track which user wrote the review
    review_text TEXT NOT NULL, -- The actual review
    rating DECIMAL(3, 2), -- Optional: User-specific rating for the show
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (show_id) REFERENCES Shows(show_id)
);

```

#### **Bookings Table (Seat Management Service)**

```sql
CREATE TABLE Bookings (
    booking_id INT AUTO_INCREMENT PRIMARY KEY,
    show_id INT NOT NULL,
    user_id VARCHAR(24) NOT NULL,
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    seats_booked JSON,
    add_ons JSON,
    total_amount DECIMAL(10, 2),
    payment_status ENUM('Pending', 'Completed', 'Failed') DEFAULT 'Pending',
    booking_status ENUM('Active', 'Cancelled') DEFAULT 'Active',
    FOREIGN KEY (show_id) REFERENCES Shows(show_id)
);
```
* add-ons include parking-passes, food vouchers and merchandise.
### 4.3 RabbitMQ Queue (Notification Service)

- **Queue Name**: `notification_queue`
- **Message Format**:

```json
{
  "user_id": "ObjectId",
  "message": "Your booking is confirmed!",
  "type": "EMAIL|SMS",
  "timestamp": "2024-12-12T15:30:00Z"
}
```

---

## 5. Component Design

### 5.1 Authentication Service

- **Responsibilities**: User registration, authentication, profile management.
- **Database**: MongoDB.

### 5.2 Event Management Service

- **Responsibilities**: Managing movies, venues, and showtimes.
- **Database**: MySQL.

### 5.3 Seat Management Service

- **Responsibilities**: Tracking and updating seat availability.
- **Database**: MySQL.

### 5.4 Payment Service

- **Responsibilities**: Handling payment transactions and refunds.
- **External Dependencies**: Payment Gateway.

### 5.5 Notification Service

- **Responsibilities**: Sending user notifications via RabbitMQ.

---
## 6. Activity and Sequence Diagrams

### 6.1 Sequence Diagram: User Registration Process

![alt text](<Diagrams/User_Registration _Swimlane.svg>)

```plantuml
@startuml
actor User
participant Frontend
participant "API Gateway"
participant "Authentication Service"
participant "MongoDB"

User -> Frontend: Enter Name, Email, and Password
Frontend -> "API Gateway": Submit Registration Request
"API Gateway" -> "Authentication Service": Validate and Create User
"Authentication Service" -> "MongoDB": Store User Data
"MongoDB" --> "Authentication Service": Confirmation
"Authentication Service" --> "API Gateway": Registration Successful
"API Gateway" --> Frontend: Display Success Message
Frontend --> User: Registration Complete
@enduml
```
### 6.2 Sequence Diagram: Login Authentication Process

![alt text](<Diagrams/User_Login Swimlane Diagram.svg>)

```plantuml
@startuml
actor User
participant Frontend
participant "API Gateway"
participant "Authentication Service"

User -> Frontend: Enter Email and Password
Frontend -> "API Gateway": Submit Login Request
"API Gateway" -> "Authentication Service": Validate Credentials
"Authentication Service" --> "API Gateway": Return Authentication Token
"API Gateway" --> Frontend: Login Success
Frontend --> User: Redirect to Dashboard
@enduml
```

### 6.3 Activity Diagram: Attendee Booking Workflow

![alt text](<Diagrams/Activity Diagram.svg>)

```plantuml
@startuml
start
:Open BookMyShow Application;
:Search for Movies/Events;
if (Results Found?) then (Yes)
  :Select Movie/Event;
  :Choose Date and Time;
  :Select Seats from Seating Chart;
  :Proceed to Payment;
  if (Payment Successful?) then (Yes)
    :Booking Confirmed;
    :Generate E-Ticket;
  else (No)
    :Show Payment Failed Message;
  endif
else (No)
  :Show "No Results Found" Message;
endif
stop
@enduml
```

### 6.4 Sequence Diagram: Attendee Booking Process

![alt text](Diagrams/Swimlane_Diagram_attendee.svg)

```plantuml
@startuml
actor "Attendee"
participant Frontend
participant "API Gateway"
participant "Seat Management Service"
participant "Payment Service"
participant "Notification Service"

"Attendee" -> Frontend: Search for Movies
Frontend -> "API Gateway": Fetch Movie Data
"API Gateway" -> "Event Management Service": Get Movie Details
"Event Management Service" --> "API Gateway": Return Movie Data
"API Gateway" --> Frontend: Display Results
"Attendee"-> Frontend: Select Movie, Date, Time, Seats
Frontend -> "API Gateway": Reserve Seats
"API Gateway" -> "Seat Management Service": Update Seat Availability
"Seat Management Service" --> "API Gateway": Seats Reserved
"API Gateway" --> Frontend: Confirm Seat Selection
"Attendee" -> Frontend: Proceed to Payment
Frontend -> "API Gateway": Initiate Payment
"API Gateway" -> "Payment Service": Process Payment
"Payment Service" --> "API Gateway": Payment Success
"API Gateway" -> "Notification Service": Send Confirmation
"Notification Service" --> "Attendee": Booking Confirmed
@enduml
```



## 7. Human Interface Design

### 7.1 User Interface Modules

- **Search Module**: Allows users to find movies and events.
- **Booking Module**: Provides seat selection and ticket confirmation.
- **Profile Module**: Displays user information and booking history.

### 7.2 External Services Integration

- **Email/SMS Service**: For sending notifications.
- **Payment Gateway**: For secure transaction processing.

---

## 8. Scalability in System Design

## Objectives
The system is designed to handle large-scale user interactions and high traffic scenarios. Scalability ensures smooth user experiences during peak load conditions, such as ticket sales for popular events. Key components to achieve scalability include load balancing, caching, auto-scaling, database sharding, and asynchronous task processing.

---

### Components of Scalability

#### 1. **AWS Elastic Load Balancer (ELB)**
- **Purpose**: Distributes incoming traffic evenly across multiple application server instances.
- **Implementation**:
  - Use **Application Load Balancer (ALB)** for HTTP and HTTPS traffic.
  - Configure health checks to monitor the availability of server instances.
  - Route traffic based on path or host headers for services like API and static resources.

---

#### 2. **Redis Cache**
- **Purpose**: Reduces load on the database by caching frequently accessed data, ensuring faster response times.
- **Implementation**:
  - Deploy **Amazon ElastiCache for Redis** for a managed caching solution.
  - Cache frequently requested data, such as event details, seat availability, and user preferences.
  - Set appropriate TTL (Time-to-Live) values to keep cache updated with fresh data.

---

#### 3. **Auto Scaling**
- **Purpose**: Dynamically adjust the number of application server instances based on traffic.
- **Implementation**:
  - Use **AWS Auto Scaling Groups** to monitor traffic and adjust capacity.
  - Define scaling policies based on CPU utilization, request count, or other metrics.
  - Set minimum and maximum instance limits to manage costs and ensure availability.

---

#### 4. **Database Sharding**
- **Purpose**: Distribute data across multiple database servers to handle high read/write traffic.
- **Implementation**:
  - Implement horizontal sharding by splitting the database based on a key (e.g., event ID or user ID).
  - Use **Amazon RDS** or **Amazon Aurora** for managed relational database services with sharding support.
  - Monitor and balance shard usage to avoid uneven load distribution.

---

#### 5. **Task Queue for Asynchronous Processing**
- **Purpose**: Offload non-critical tasks to background workers to keep the application responsive.
- **Implementation**:
  - Use a message queue system like **AWS Simple Queue Service (SQS)** or **RabbitMQ**.
  - Offload tasks such as:
    - Sending email or SMS notifications.
    - Generating analytics reports.
    - Processing media files (e.g., image or video compression).
  - Deploy worker nodes to process queue messages asynchronously.

---

## 9. Requirements Traceability Matrix

| Requirement ID | Description                          | Design Component(s)      |
| -------------- | ------------------------------------ | ------------------------ |
| FR-4.1           | User Registration and Authentication | Authentication Service   |
| FR-4.4           | Movie and Event Listings             | Event Management Service |
| FR-4.2           | Seat Booking and Updates             | Seat Management Service  |
| FR-4.3           | Payment Processing                   | Payment Service          |
| FR-3.3        | Notifications                        | Notification Service     |

---

## 10. Appendices

### Appendix A: Glossary

- **MongoDB**: NoSQL database for flexible, document-oriented storage.
- **MySQL**: Relational database for structured data.
- **RabbitMQ**: Message broker for asynchronous communication.

### Appendix B: References

- IEEE Std 1016-2009: IEEE Standard for Information Technology—Systems Design—Software Design Descriptions.
- [Design BookMyShow – A System Design Interview Question](https://www.geeksforgeeks.org/design-bookmyshow-a-system-design-interview-question/)
- [Design a movie ticket booking system like BookMyShow](https://www.geeksforgeeks.org/design-movie-ticket-booking-system-like-bookmyshow/)

---

