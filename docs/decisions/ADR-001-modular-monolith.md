# ADR-001: Adopt a Modular Monolith Architecture

- **Status:** Accepted
- **Date:** 2026-11-12
- **Project:** Sangam
- **Decision owner:** Sangam development team

## 1. Context

Sangam is a collaboration and workflow management platform intended to support individuals and organizations.

Its initial business capabilities include:

- Identity and authentication
- Organization management
- Employee management
- Team management
- Project management
- Task management

The platform may expand in the future to include notifications, workflow automation, AI assistance and external collaboration.

We need an architecture that supports maintainability, testability, clear business boundaries and future scalability.

Two primary architectural approaches were considered:

1. Monolithic application with limited internal separation.
2. Microservices with independently deployed services and distributed communication.

Starting with microservices would introduce additional infrastructure and operational complexity, including network communication, distributed transactions, service discovery, observability and independent deployment pipelines.

These concerns are not currently justified by Sangam's V1 requirements.

## 2. Decision

We will adopt a **Modular Monolith Architecture** for Sangam V1.

Sangam will initially run as one Spring Boot application, organized into independent business modules.

Each module will:

- Own its business logic.
- Own its domain entities and persistence responsibilities.
- Expose a controlled public interface to other modules.
- Keep its internal implementation hidden from other modules.
- Avoid directly accessing another module's repositories or internal entities.

The application will be deployed as one unit.

## 3. Initial business modules

The initial modules are:

| Module | Responsibility |
|---|---|
| Identity | Authentication, registration and account identity |
| Organization | Organization lifecycle and organization details |
| Employee | Employee records and employment information |
| Team | Team management and team membership |
| Project | Project creation and project membership |
| Task | Task creation, assignment and lifecycle |

Cross-cutting infrastructure such as configuration, error handling and observability will be maintained separately.

## 4. Architectural rules

### 4.1 Module ownership

Each module owns its business rules and persistence implementation.

For example, the Task module must not directly access the Employee module's JPA repository.

### 4.2 Module communication

Modules should communicate through explicitly exposed interfaces or application-level events.

Other modules must not depend on internal implementation classes.

### 4.3 Database ownership

Initially, all modules will use the same MySQL database, `sangam_db`.

However, each module must own its tables and persistence responsibilities.

Sharing the same database does not mean unrestricted access to every module's tables.

### 4.4 Transaction management

Transactions should remain within clearly defined business operations.

Cross-module workflows should be designed carefully to avoid unnecessary coupling.

### 4.5 Deployment

All modules will initially be packaged and deployed as one Spring Boot application.

### 4.6 Future microservices migration

A module may be extracted into an independent service when justified by concrete requirements such as:

- Independent scaling requirements.
- Independent deployment needs.
- Different technology or operational requirements.
- Clear organizational ownership.
- Significant differences in workload or availability requirements.

Extraction must include an explicit review of data ownership, communication contracts, consistency and failure handling.

## 5. Alternatives considered

### Alternative A: Traditional monolith

A single application with loosely separated or shared business logic.

**Rejected because:** It can lead to tight coupling and make future maintenance difficult.

### Alternative B: Microservices from day one

Independent services with separate deployment and data ownership.

**Rejected for V1 because:** The operational and distributed-system complexity is not justified by current requirements.

### Alternative C: Modular monolith

One deployable application with explicit business-module boundaries.

**Accepted because:** It offers a balance between maintainability, development simplicity and future architectural flexibility.

## 6. Consequences

### Positive consequences

- Simpler local development and deployment.
- Easier debugging and testing.
- Clear separation of business capabilities.
- Easier transaction management.
- Lower initial infrastructure cost.
- A structured path toward future microservices.

### Trade-offs

- Modules share the application's runtime.
- The application is initially deployed as one unit.
- Independent scaling of individual modules is not available.
- Module boundaries must be actively enforced through code reviews and automated tests.

## 7. Future review

This decision will be reviewed when Sangam's actual workload, deployment requirements, team structure or operational needs justify extracting one or more modules into independent services.

**Final decision:** Sangam V1 will use a Modular Monolith Architecture.