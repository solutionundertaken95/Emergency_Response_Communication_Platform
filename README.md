# Emergency_Response_Communication_Platform
# Emergency Response Communication Platform

## 1. What We Built

The Emergency Response Communication Platform (ERCP) is a web application concept for coordinating emergency incidents among the public, administrators, and responders. It combines an Angular frontend with a Spring Boot microservices backend.

The frontend includes public, emergency, admin, and responder areas. Users can access incident reporting and tracking screens, while admin and responder routes are organized around role-based access. The backend is divided into services for incidents, victims, responders, users, locations, and logs, with an API gateway and Eureka service registry.

### Main components

- **Frontend:** Angular application with routes and screens for incident tracking, admin management, and responder workflows.
- **API Gateway:** Spring Cloud Gateway entry point for backend requests.
- **Service registry:** Eureka Server for service discovery.
- **Backend services:** Incident, Victim, Responder, User, Location, and Log services.
- **Data layer:** Spring Data JPA and PostgreSQL dependencies are used in the backend.

## 2. Architecture Diagram

```mermaid
flowchart LR
    Public[Public / Victim]
    Admin[Administrator]
    Responder[Emergency Responder]

    subgraph Frontend[Angular Frontend]
        UI[Web application]
        Guard[Role-aware route guard]
        UI --> Guard
    end

    Gateway[API Gateway]
    Registry[Eureka Service Registry]

    subgraph Services[Spring Boot Services]
        Incident[Incident Service]
        Victim[Victim Service]
        Response[Responder Service]
        User[User Service]
        Location[Location Service]
        Log[Log Service]
    end

    DB[(PostgreSQL)]

    Public --> UI
    Admin --> UI
    Responder --> UI

    Guard --> Gateway
    Gateway -. service discovery .-> Registry
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


The diagram shows the intended service layout. Configure database connections and gateway routes for each service in the deployment environment.

3. Result

The repository provides a working foundation for an emergency-response application:

Angular routes are organized for public, emergency-tracking, admin, and responder experiences.
Admin screens include incident and responder management routes.
The Incident Service exposes endpoints to create, retrieve, filter, update, and delete incidents, including status-related endpoints.
Eureka and the API Gateway provide the building blocks for service discovery and centralized backend access.
The incident dashboard currently includes sample incident, progress-stage, and communication-log data. Connecting these screens to live backend data and validating the complete end-to-end workflow are important next steps. No deployment or performance results are claimed here.

4. Challenges Faced and How We Addressed Them

Coordinating multiple backend services: Splitting the backend into domain-specific services can make service addresses and request routing harder to manage. We included Eureka for service discovery and an API Gateway as a centralized entry point.
Supporting different user roles: Public users, administrators, and responders need different screens and access paths. We organized the Angular routes by user area and added a route guard for admin and responder routes.
Representing an incident’s progress clearly: An emergency response involves multiple stages and updates. We structured the tracking UI around incident details, progress stages, and a communication log so these elements can be connected to live service data.
Integrating frontend and backend development: The frontend and Spring Boot services are separate applications with their own dependencies and configuration. Keeping them in distinct project directories makes their responsibilities clearer, while the gateway provides a defined point for frontend API requests.

5. Future Scopes

Connect incident reporting and tracking screens to live backend APIs, replacing sample data.
Implement end-to-end authentication and authorization, including consistent server-side role checks.
Add real-time incident and responder updates using WebSockets or server-sent events.
Integrate maps and location services to display incidents and responder locations.
Add notifications for incident creation, responder assignment, and status changes.
Improve resilience with service health monitoring, retries, and tested circuit-breaker behavior.
Add automated integration and end-to-end tests across the frontend, gateway, and services.
Provide deployment configuration, centralized environment settings, and operational documentation.
