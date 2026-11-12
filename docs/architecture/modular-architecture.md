# Sangam – Modular Architecture

**Document Type:** Architecture Documentation  
**Architecture Style:** Modular Monolith  
**Backend:** Java 21 + Spring Boot  
**Database:** MySQL 8.0  
**Status:** Adopted for V1

---

## 1. Purpose

This document describes the internal modular architecture of the Sangam backend application.

It defines:

- Business module organization.
- Package structure.
- Module responsibilities.
- Module communication rules.
- Database ownership.
- Dependency boundaries.
- Guidelines for introducing new modules.

The objective is to keep the application maintainable, testable and ready for potential future microservices extraction.

## 2. Architectural Overview

Sangam follows a Modular Monolith Architecture.

All business modules initially run inside one Spring Boot application and are deployed as a single application.

Although modules share the same runtime and database server, each module has clearly defined business responsibilities and internal boundaries.

### High-level structure

```text
                 React + TypeScript
                         |
                         v
                Sangam Spring Boot
                         |
       +-----------------+-----------------+
       |                 |                 |
   Identity          Organization       Employee
       |                 |                 |
       +-----------------+-----------------+
                         |
                  Team / Project
                         |
                       Task
                         |
                         v
                   MySQL Database
```

The diagram represents business capabilities, not unrestricted dependencies between modules.

## 3. Why Modular Monolith?

Microservices introduce independent deployment, network communication, distributed transactions, service discovery and additional operational requirements.

These capabilities are not currently necessary for Sangam V1.

A modular monolith provides:

- Clear business boundaries.
- Simpler application deployment.
- Easier debugging.
- Easier transaction management.
- Lower initial infrastructure complexity.
- A structured path toward future microservices.

The architecture will be reviewed when real scalability or independent deployment requirements justify service extraction.

## 4. Backend Package Structure

Base package:

```text
com.sangam.api
```

Proposed structure:

```text
com.sangam.api
│
├── SangamApiApplication.java
│
├── identity
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
│
├── organization
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
│
├── employee
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
│
├── team
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
│
├── project
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
│
├── task
│   ├── api
│   ├── application
│   ├── domain
│   └── infrastructure
│
└── shared
    ├── config
    ├── exception
    └── response
```

This is the target structure. Packages and classes should be created when their functionality is implemented, rather than generating empty classes in advance.

## 5. Package Responsibilities

Each business module follows four architectural layers.

### 5.1 API Layer

Package: `api`

Responsibilities:

- Expose REST endpoints.
- Accept HTTP requests.
- Validate request structures.
- Return response DTOs.
- Delegate operations to the application layer.

Typical classes:

```text
TaskController.java
CreateTaskRequest.java
TaskResponse.java
```

Controllers should not contain business logic or directly access database repositories.

### 5.2 Application Layer

Package: `application`

Responsibilities:

- Implement application use cases.
- Coordinate domain operations.
- Manage transaction boundaries.
- Coordinate communication with other modules.

Typical classes:

```text
TaskApplicationService.java
```

### 5.3 Domain Layer

Package: `domain`

Responsibilities:

- Represent business concepts.
- Define business rules.
- Define domain interfaces and contracts.
- Protect business invariants.

Typical classes:

```text
Task.java
TaskRepository.java
```

The domain layer should not depend on REST controllers, JPA repositories or other infrastructure implementations.

### 5.4 Infrastructure Layer

Package: `infrastructure`

Responsibilities:

- Implement persistence.
- Integrate with databases.
- Implement technical adapters.
- Provide external system integrations.

Typical classes:

```text
JpaTaskRepository.java
TaskJpaEntity.java
```

Infrastructure implementations should remain internal to their owning module.

## 6. Business Module Responsibilities

| Module       | Responsibility                                    |
| ------------ | ------------------------------------------------- |
| Identity     | Authentication, registration and account identity |
| Organization | Organization lifecycle and organization details   |
| Employee     | Employee records and employment information       |
| Team         | Team creation and team membership                 |
| Project      | Project creation and project membership           |
| Task         | Task creation, assignment and task lifecycle      |

Each module owns its business rules and persistence responsibilities.

## 7. Module Communication Rules

Module boundaries must be respected even though modules run in the same application.

### Rule 1: No direct access to internal repositories

The Task module must not directly access the Employee module's JPA repository.

Incorrect:

```java
@Autowired
private EmployeeJpaRepository employeeRepository;
```

Correct approach:

Expose a controlled Employee module contract:

```java
public interface EmployeeLookup {

    boolean existsById(Long employeeId);
}
```

The Task module uses this contract rather than depending on Employee's persistence implementation.

### Rule 2: No direct modification of another module's entities

A module must not directly modify another module's database entities.

Business changes must go through the owning module's application interface.

### Rule 3: Explicit communication contracts

Modules communicate through:

- Public application interfaces.
- Application-level events when appropriate.

Internal implementation classes should not be exposed.

### Rule 4: Avoid circular dependencies

For example:

```text
Task -> Employee
Employee -> Task
```

This creates circular coupling.

Dependencies should follow clearly defined business ownership and use case boundaries.

### Rule 5: Shared package restrictions

The shared package is reserved for genuinely cross-cutting functionality.

Examples:

- Common configuration.
- Global exception handling.
- Standard response structures.

Business-specific logic must remain in its owning module.

## 8. Database Ownership

Sangam initially uses one MySQL database:

```text
sangam_db
```

All modules share the same database server and database schema initially.

However, each module owns its tables and persistence operations.

For example:

| Module       | Illustrative tables           |
| ------------ | ----------------------------- |
| Identity     | accounts, credentials         |
| Organization | organizations                 |
| Employee     | employees                     |
| Team         | teams, team_memberships       |
| Project      | projects, project_memberships |
| Task         | tasks, task_assignments       |

These are illustrative ownership boundaries; the final database schema will be established during domain modeling.

Other modules must not bypass ownership rules by directly manipulating another module's persistence implementation.

## 9. Adding a New Module

When introducing a new business capability:

1. Identify its business responsibility.
2. Define which module owns the capability.
3. Define its domain concepts and rules.
4. Create its package structure.
5. Define the required application use cases.
6. Implement persistence inside the owning module.
7. Define explicit contracts for cross-module interactions.
8. Write unit and integration tests.
9. Update architecture documentation.

Avoid creating modules solely to organize technical classes. Modules should represent meaningful business capabilities.

## 10. Testing Strategy

Each module should support:

- Unit tests for domain rules.
- Application service tests.
- Repository integration tests.
- API tests for HTTP behavior.
- Cross-module integration tests where required.

Module boundaries should eventually be checked automatically using architecture tests.

## 11. Future Microservices Evolution

A module may be extracted into an independent microservice when there is a concrete reason.

Potential candidates for future evaluation:

- Identity Service.
- Notification Service.
- AI Service.
- Task Service.

Extraction must consider:

- Independent data ownership.
- API and event contracts.
- Network failures.
- Retry and idempotency behavior.
- Distributed transaction requirements.
- Observability and deployment.
- Security between services.

Microservices are a future architectural option, not a mandatory V1 implementation requirement.

---

**Related Documents**

- `system-overview.md`
- `domain-model.md`
- `ADR-001-modular-monolith.md`
