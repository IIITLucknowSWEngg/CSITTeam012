# Software Requirements Specification (SRS) for BookMyShow

## Version: 1.0  
Prepared by: CSITTeam012 (Team Octet)  
Date Created: November 30, 2024

---

## Table of Contents

1. **Introduction**  
   1.1 Purpose  
   1.2 Document Conventions  
   1.3 Intended Audience and Reading Suggestions  
   1.4 Product Scope  
   1.5 References  

2. **Overall Description**  
   2.1 Product Perspective  
   2.2 Product Functions  
   2.3 User Classes and Characteristics  
   2.4 Operating Environment  
   2.5 Design and Implementation Constraints  
   2.6 User Documentation  
   2.7 Assumptions and Dependencies  

3. **External Interface Requirements**  
   3.1 User Interfaces  
   3.2 Hardware Interfaces  
   3.3 Software Interfaces  
   3.4 Communications Interfaces  

4. **System Features**  
   4.1 User Registration and Login  
   4.2 Booking System  
   4.3 Payment Gateway Integration  
   4.4 Event Management  
   4.5 Advertisers and Sponsors Features  

5. **Other Nonfunctional Requirements**  
   5.1 Performance Requirements  
   5.2 Safety Requirements  
   5.3 Security Requirements  
   5.4 Software Quality Attributes  
   5.5 Business Rules  

6. **Other Requirements**

---

## 1. Introduction

### 1.1 Purpose  
The purpose of this Software Requirements Specification (SRS) is to outline the functional and non-functional requirements for the BookMyShow application. The goal of this application is to provide users with an online platform to browse, select, and book tickets for movies and events. This document serves as a comprehensive guide for developers, stakeholders, and project managers, ensuring that all expectations are clearly defined and understood.

### 1.2 Document Conventions  
- Functional requirements are identified using the "REQ-" prefix.  
- Priorities for each requirement are labeled as **High**, **Medium**, or **Low**.  

### 1.3 Intended Audience and Reading Suggestions  
- **Developers**: Focus on sections 3 and 4, which contain technical implementation details and system feature specifications.
- **Testers**: Refer to all sections, with particular attention to functional and performance requirements for system validation.
- **Project Managers**: Sections 1, 2, and 5 provide high-level project details, system scope, and non-functional requirements.

### 1.4 Product Scope  
The BookMyShow is a web-based application that allows users to:
- Browse movies and events.
- Select movies or events for booking.
- Choose available seats in a real-time seat map.
- Securely pay for tickets via integrated payment gateways.

The platform will also feature admin functionalities to manage movie schedules, events, and bookings.

### 1.5 References  
- IEEE SRS Template by Karl E. Wiegers, 1999.  
- MERN Stack Documentation (MongoDB, Express, ReactJS, Node.js)  
- MySQL Documentation

---

## 2. Overall Description

### 2.1 Product Perspective  
The BookMyShow is a standalone web application inspired by the BookMyShow platform. The system will operate using the **MERN Stack** (MongoDB, Express.js, ReactJS, Node.js) for the backend and frontend development. The application aims to replicate core functionalities such as ticket booking, event management, and payment integration.

### 2.2 Product Functions  
The system will support the following major functionalities:
1. **User Registration and Login**: Users must be able to register and log in with their credentials.
2. **Event Browsing**: Attendees can browse upcoming movies and events.
3. **Seat Booking**: Attendees can select seats in a real-time seat map for movie or event bookings.
4. **Payment Processing**: Secure online payment options for ticket purchases.
5. **Admin Panel**: Admin users can manage movie schedules, user queries, and ticket sales.

### 2.3 User Classes and Characteristics  
- **End Users**: Moviegoers and event attendees. These users will register, browse events, and book tickets.
- **Admin Users**: Administrators who manage events, ticket bookings, user information, and resolve queries.
**Advertisers and Sponsors**:Advertisers and sponsors use the platform to promote their events, products, or services. They require tools for ad campaign management, detailed analytics, and access to premium placements.  
- **Support Staff**: Users who handle customer support inquiries related to bookings and events.

### 2.4 Operating Environment  
- **Frontend**: The application will be developed using **ReactJS**, ensuring compatibility with modern browsers such as Google Chrome and Mozilla Firefox.
- **Backend**: The backend will be powered by **Node.js** and **Express.js** for handling API requests.
- **Database**: **PostgreSQL** will be used to store user data, event details, and ticket information.
- **Payment Gateway**: Integration with third-party APIs (such as Stripe or PayPal) for processing secure payments.

### 2.5 Design and Implementation Constraints  
- The system must use **PostgreSQL** as the relational database.
- The web application must comply with **HTTPS** for secure data transfer.
- The platform must comply with **GDPR** and other relevant data protection regulations for user privacy.

### 2.6 User Documentation  
User manuals, FAQs, and tutorial videos will be provided through the application’s help section to guide users in booking tickets, creating accounts, and troubleshooting common issues.

### 2.7 Assumptions and Dependencies  
- Users will have access to an internet-enabled device (PC, smartphone, tablet).
- **Payment Gateway** APIs (Stripe, PayPal, etc.) will be available and functional.
- The system assumes a stable internet connection for real-time booking and payments.

---

## 3. External Interface Requirements

### 3.1 User Interfaces  
The user interface will include:
- **Homepage**: A landing page displaying featured movies/events.
- **Registration/Login Page**: Forms for new user registration and login.
- **Event Listings**: A page with filtering and searching options for events and movies.
- **Seat Selection Page**: Real-time seat map for choosing available seats.
- **Checkout Page**: User's booking summary and payment options.

The UI will be designed to be responsive and user-friendly, ensuring a seamless experience across devices.

### 3.2 Hardware Interfaces  
The application will function on standard hardware including:
- PCs with internet access.
- Mobile devices (smartphones and tablets) with modern web browsers.

### 3.3 Software Interfaces  
The following software interfaces will be integrated:
- **Payment API**: Integration with third-party payment gateways (Stripe, PayPal, etc.).
- **Email/SMS API**: To send booking confirmations and notifications.
- **Admin Panel**: Web interface for event management and user control.

### 3.4 Communications Interfaces  
- **HTTPS**: All communication between the frontend and backend will be secure, using HTTPS to prevent unauthorized data interception.
- **WebSocket/Real-time API**: For real-time seat availability and booking updates.

---

## 4. System Features

### 4.1 User Registration and Login  
**Description and Priority**:  
User authentication is essential for allowing personalized experiences and secure booking. **Priority: High**.

**Functional Requirements**:  
- **REQ-1**: Users must register with their email address, name, and password.
- **REQ-2**: Users should be able to log in using registered credentials (email and password).
- **REQ-3**: Implement password recovery via email for users who forget their password.
- **REQ-4**: Users can update their profile information (name, email, password).

### 4.2 Booking System  
**Description and Priority**:  
Users should be able to browse events, select movies, and book tickets through a real-time seat map. **Priority: High**.

**Functional Requirements**:  
- **REQ-5**: Users can browse a list of upcoming movies and events.
- **REQ-6**: Users can select showtimes and view available seats.
- **REQ-7**: Users can book tickets and receive a confirmation via email/SMS.
- **REQ-8**: Admins can view and manage bookings for each event.

### 4.3 Payment Gateway Integration  
**Description and Priority**:  
Users must be able to pay for tickets securely via integrated third-party payment gateways. **Priority: High**.

**Functional Requirements**:  
- **REQ-9**: Users can pay for tickets using credit/debit cards, PayPal, or other payment methods.
- **REQ-10**: Payments must be processed securely, and sensitive data should be encrypted.
- **REQ-11**: Users will receive a payment receipt after successful payment.

### 4.4 Event Management  
**Description and Priority**:  
Admins should be able to create, modify, and delete events, as well as manage movie schedules and seat availability. **Priority: Medium**.

**Functional Requirements**:  
- **REQ-12**: Admins can create new events and set showtimes.
- **REQ-13**: Admins can edit event details, including date, time, and ticket pricing.
- **REQ-14**: Admins can manage seat availability for each showtime.

### 4.5 Advertisers and Sponsors Features

**Description and Priority**:  
The platform provides tools for advertisers and sponsors to create, manage, and analyze ad campaigns. These features are essential for driving revenue and offering promotional opportunities to sponsors. **Priority: Medium**.

**Functional Requirements**:  
- **REQ-14**: Advertisers can create ad campaigns using a self-service tool.  
- **REQ-15**: Ads can be targeted based on demographics (e.g., age, location, and interests).  
- **REQ-16**: Advertisers can monitor ad performance with detailed analytics (e.g., impressions, click-through rates, and ROI).  
- **REQ-17**: Premium ad placements, such as homepage banners or event-specific sponsorships, can be purchased by sponsors.  
- **REQ-18**: Integration of promotional materials (e.g., images, videos, and text) provided by sponsors.  

---


## 5. Other Nonfunctional Requirements

### 5.1 Performance Requirements  
- The system should be able to handle **100 concurrent users** without degradation in performance.

### 5.2 Safety Requirements  
- **REQ-15**: System backups will occur every 24 hours to ensure data is not lost.

### 5.3 Security Requirements  
- **REQ-16**: All user data (including passwords and payment details) will be encrypted using **AES-256** encryption.
- **REQ-17**: Implement **2-factor authentication (2FA)** for admin access.

### 5.4 Software Quality Attributes  
- **Scalability**: The system should scale to handle an increasing number of users and events.
- **Usability**: The platform must be easy to use with a responsive design for all devices.

### 5.5 Business Rules

- The platform must offer clear guidelines for ad content, prohibiting misleading or offensive materials.  
- Ads must comply with regional advertising standards and regulations.  
- Sponsors can bid for premium ad placements based on availability.  
- Campaign performance reports must be provided to sponsors in real time.  

---

# Compatibility Matrix for Browsers and OS

| Platform             | Supported Browsers                                     | Supported Operating Systems                     |
|----------------------|--------------------------------------------------------|------------------------------------------------|
| **Web (Desktop)**     | Chrome (version 100 and later), Firefox (version 100 and later), Safari (version 14 and later), Microsoft Edge (version 100 and later) | Windows, macOS, Linux                          |
| **Web (Mobile)**      | Chrome (version 100 and later), Safari (version 14 and later), Firefox (version 100 and later)  | Android, iOS                                   |
| **Mobile App (iOS)**  | N/A                                                    | iOS 13.0 or later                              |
| **Mobile App (Android)** | N/A                                                 | Android 5.0 or later                           |

---

## 6. Other Requirements

- The system must be deployed and tested on a staging server before production deployment.
- All code should be documented for maintainability.

---  
