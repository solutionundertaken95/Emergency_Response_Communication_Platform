# Emergency Response Communication Platform (ERCP)

[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Angular](https://img.shields.io/badge/Angular-16%2B-red.svg)](https://angular.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Database-blue.svg)](https://www.postgresql.org/)
[![Microservices](https://img.shields.io/badge/Architecture-Microservices-orange.svg)](https://microservices.io/)

The **Emergency Response Communication Platform (ERCP)** is a full-stack, microservice-based web application designed to streamline real-time emergency incident reporting, coordination, and tracking among victims, administrators, and first responders.

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Repository Structure](#repository-structure)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [Challenges & Solutions](#challenges--solutions)
- [Future Scope & Roadmap](#future-scope--roadmap)

---

## Overview

During emergency situations, rapid and structured communication is critical. ERCP acts as a centralized coordination hub:

- **Public & Victims:** Quickly report incidents and track response progress in real time.
- **Responders:** View assigned incidents, update progress stages, and communicate status back to dispatch.
- **Administrators:** Oversee platform operations, manage incidents, assign emergency responders, and audit activity logs.

---

## Key Features

- **Role-Based Workflows:** Tailored user interfaces for the public, emergency victims, administrative officers, and field responders.
- **Role-Aware Security:** Angular route guards prevent unauthorized access to restricted admin and responder management views.
- **Centralized Routing & Discovery:** Spring Cloud Gateway and Eureka Service Registry manage dynamic routing and microservice load balancing.
- **Domain-Driven Microservices:** Decoupled backend architecture split into 6 core microservices for high scalability and modularity.
- **Incident Lifecycle Tracking:** Frontend state management designed around incident creation, progress stages, and communication audit logs.

---

## Project Structure

```text
├── Frontend-ERCP/
│   └── emergency-response-platform/    # Angular Web Application
│
└── Backend-ERCP/
    ├── Eureka-Server/                   # Service Discovery Server (Netflix Eureka)
    ├── Api-Gateway-Server/              # Spring Cloud API Gateway Entry Point
    ├── Incident-Service/                # Incident Reporting & Status Management
    ├── Responder-Service/               # Field Responder Directory & Dispatching
    ├── User-Service/                    # User Authentication & Role Management
    ├── Location-Service/                # Geographic Coordinates & Mapping Service
    ├── Victim-Service/                  # Victim Profile & Assistance Needs
    └── Log-Service/                     # System Auditing & Communication Logs
```


## System Architecture

```mermaid
flowchart LR
    Public[Public / Victim]
    Admin[Administrator]
    Responder[Emergency Responder]

    subgraph Frontend [Angular Frontend Application]
        UI[Web UI Dashboard]
        Guard[Role-Aware Route Guard]
        UI --> Guard
    end

    Gateway[API Gateway Server]
    Registry[Eureka Service Registry]

    subgraph Services [Spring Boot Microservices Cluster]
        Incident[Incident Service]
        Victim[Victim Service]
        Response[Responder Service]
        User[User Service]
        Location[Location Service]
        Log[Log Service]
    end

    DB[(PostgreSQL Databases)]

    Public --> UI
    Admin --> UI
    Responder --> UI

    Guard --> Gateway
    Gateway -. Service Discovery .-> Registry

    Gateway --> Incident
    Gateway --> Victim
    Gateway --> Response
    Gateway --> User
    Gateway --> Location
    Gateway --> Log

    Incident --> DB
    Victim --> DB
    Response --> DB
    User --> DB
    Location --> DB
    Log --> DB
````

## Flow of Data

A user submits an incident report via the Angular Frontend.

The request hits the Spring Cloud API Gateway on port 8080.

The Gateway queries Eureka Server to locate an active instance of Incident Service.

Incident Service processes and persists the data into PostgreSQL.

Secondary microservices (Location, Victim, Log) synchronize related metrics asynchronously or via inter-service requests.

Administrators and responders view updated incident records on their respective role-guarded dashboards.

## Tech Stack

| **Layer**             | **Technology**                        |
| --------------------- | ------------------------------------- |
| **Frontend**          | Angular, TypeScript, RxJS, HTML5/CSS3 |
| **API Gateway**       | Spring Cloud Gateway                  |
| **Service Discovery** | Spring Cloud Netflix Eureka           |
| **Backend Framework** | Java 17 / Spring Boot 3.x             |
| **Data Access**       | Spring Data JPA / Hibernate           |
| **Database**          | PostgreSQL                            |

## Challenges & Solutions

| **Challenge**                                                                                                                 | **Solution**                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Multi-Service Coordination**<br><br>Managing dynamic service IPs and API endpoint complexity across multiple microservices. | Integrated **Netflix Eureka** for automated service registration and discovery, along with **Spring Cloud Gateway** as a single entry point for all frontend requests. |
| **Granular Access Control**<br><br>Differentiating public users from dispatchers, admins, and field personnel.                | Implemented Angular **Role-Aware Route Guards** to restrict navigation routes based on authenticated user roles.                                                       |
| **Clear Incident Progression UI**<br><br>Displaying fast-moving incident lifecycles without overwhelming the user.            | Designed dedicated UI panels for **Incident Details**, **Progress Stages**, and **Communication Logs**, ready for reactive data streams.                               |
| **Full-Stack Separation**<br><br>Decoupling client UI dependencies from backend microservices.                                | Organized the project into separate root directories (`Frontend-ERCP` and `Backend-ERCP`), enforcing a clear separation of concerns.                                   |

## Future Scope & Roadmap

The following enhancements are planned to improve scalability, security, real-time responsiveness, and overall system reliability.

- [ ] **Live Data Integration:** Replace current frontend mock/sample dashboard data with live REST API backend streams.
- [ ] **Real-Time Communication:** Implement WebSockets / Server-Sent Events (SSE) for instant incident status updates and dispatch notifications.
- [ ] **GIS & Map Integration:** Integrate Leaflet or Google Maps API within the **Location Service** for live GPS tracking of incidents and emergency responders.
- [ ] **End-to-End Security:** Implement OAuth2 / JWT authorization with centralized server-side role checks.
- [ ] **Resilience & Fault Tolerance:** Add Resilience4j circuit breakers, rate limiting, and centralized health monitoring.
- [ ] **Automated Testing & CI/CD:** Build automated unit, integration, and E2E test suites with Docker-based deployment configurations.
