# Sangam – System Overview

## 1. Introduction

Sangam is a collaboration and workflow management platform designed to help individuals, teams, and organizations manage their work efficiently.

The platform aims to simplify project management, task tracking, team collaboration, knowledge sharing, and workflow automation.

Sangam will support independent personal and organization account environments with clearly defined ownership, authentication, and access boundaries.

**Tagline:** One team. One connected workspace.

## 2. Problem Statement

Individuals and organizations often use multiple disconnected tools to manage their work.

This creates challenges such as:

- Fragmented communication.
- Difficulty tracking projects and tasks.
- Scattered documents and knowledge.
- Limited visibility into team activities.
- Repetitive manual workflows.
- Difficulty finding relevant organizational information.

Sangam aims to bring these activities together through a unified collaboration platform.

## 3. Product Vision

Build a scalable, secure, and extensible collaboration platform that supports individual users, small teams, and enterprise organizations.

The platform should evolve gradually, introducing advanced capabilities without unnecessarily complicating the initial implementation.

## 4. Target Users

### 4.1 Individual Users

Individuals who want to:

- Create and manage personal projects.
- Track tasks and deadlines.
- Organize notes and knowledge.
- Collaborate with other users.
- Manage their personal workspace independently.

### 4.2 Small Teams

Teams that want to:

- Collaborate on projects.
- Assign and track tasks.
- Share documents.
- Monitor project progress.
- Coordinate team activities.

### 4.3 Organizations

Organizations that want to:

- Create and manage organization accounts.
- Maintain employee records.
- Provision employee accounts.
- Organize employees into teams.
- Manage projects and tasks.
- Control access and permissions.
- Maintain organizational knowledge.
- Automate workflows.

## 5. Core Product Capabilities

The following capabilities represent the planned product scope.

| Module                          | Description                                              |
| ------------------------------- | -------------------------------------------------------- |
| Personal Account Management     | Personal registration, profiles, and account management  |
| Organization Account Management | Organization creation and administration                 |
| Authentication                  | Secure authentication and identity management            |
| Employee Management             | Employee records, provisioning, and lifecycle management |
| Team Management                 | Team creation and employee/team membership               |
| Project Management              | Project creation, collaboration, and tracking            |
| Task Management                 | Task assignment, deadlines, priorities, and status       |
| Collaboration                   | Comments, discussions, and activity history              |
| Guest Collaboration             | Controlled collaboration with external personal accounts |
| Document Management             | Document storage and knowledge organization              |
| AI Assistant                    | AI-powered assistance and information retrieval          |
| Workflow Automation             | Automation of repetitive business processes              |

These capabilities will be implemented incrementally.

## 6. Account and Ownership Architecture

Sangam will support two independent account domains.

### 6.1 Personal Account

A personal account belongs to an individual.

It supports:

- Personal profile management.
- Personal projects and tasks.
- Personal workspace management.
- Personal collaboration.

Personal accounts must remain independent of organization account administration.

### 6.2 Organization Account

An organization account represents an account environment managed by a company or organization.

It supports:

- Organization administration.
- Employee directory management.
- Employee account provisioning.
- Team management.
- Organization-owned projects and tasks.
- Organization-specific security policies.

Organizations can create employee records before employees register or activate their Sangam accounts.

### 6.3 Independent Account Lifecycle

Personal and organization accounts will have independent:

- Authentication.
- Ownership.
- Administration.
- Account activation and deactivation.
- Authorization policies.

No automatic linking or merging of personal and organization accounts will occur.

### 6.4 External Guest Collaboration

Organizations may invite external personal accounts to collaborate on specific organizational resources.

Guest collaboration will follow these principles:

- Explicit organization approval.
- Identity verification.
- Resource-specific access.
- Viewer permission by default.
- Explicit permission upgrades when required.
- Revocable access.
- Auditable activities.

Guest collaboration does not merge personal and organization accounts.

## 7. Employee and Account Lifecycle

Employee records and application accounts represent different concepts.

An organization can:

- Create employee records independently.
- Provision employee accounts before activation.
- Assign tasks to active employees before account activation.
- Add employees to teams and projects.
- Deactivate employment records independently of account activation status.

Only authenticated and authorized employees can access Sangam.

Historical tasks and activity records must be preserved when an employee becomes inactive.

## 8. Project Ownership and Transfer

Projects belong to the workspace or account environment in which they are created.

A personal project may be transferred into an organization-owned environment through an authorized transfer process.

The transfer process must:

1. Verify the requester's permissions.
2. Record the transfer request.
3. Obtain destination organization approval.
4. Preserve relevant project data and history.
5. Re-evaluate permissions and collaborators.
6. Record the operation in audit logs.

## 9. High-Level Technical Architecture

The initial implementation will follow a modular monolithic architecture.

Planned technology stack:

| Layer            | Technology                          |
| ---------------- | ----------------------------------- |
| Backend          | Java 21, Spring Boot                |
| Build Tool       | Maven                               |
| Frontend         | React, TypeScript                   |
| Database         | PostgreSQL                          |
| ORM              | Spring Data JPA / Hibernate         |
| Security         | Spring Security                     |
| AI Integration   | Spring AI                           |
| Vector Search    | PostgreSQL with pgvector            |
| Caching          | Redis                               |
| Messaging        | RabbitMQ                            |
| Testing          | JUnit, Mockito, Testcontainers      |
| Containerization | Docker                              |
| CI/CD            | GitHub Actions                      |
| Cloud            | Microsoft Azure                     |
| Monitoring       | Micrometer and Application Insights |

The technology stack is a planned direction, not a statement that all components have already been implemented.

## 10. Architectural Principles

The following principles will guide development:

1. **Separation of Concerns:** Each module should have a clearly defined responsibility.
2. **Modularity:** Modules should be independently understandable and maintainable.
3. **Security by Design:** Authentication, authorization, and data isolation must be considered from the beginning.
4. **Scalability:** Design should accommodate future growth without premature complexity.
5. **Maintainability:** Code should be readable, testable, and well structured.
6. **Data Integrity:** Relationships and constraints should protect the consistency of application data.
7. **Account Independence:** Personal and organization account domains must remain independent.
8. **Least Privilege:** Users should receive only the permissions required for their responsibilities.
9. **Documentation:** Important requirements and design decisions must be maintained in version control.
10. **Incremental Development:** Implement and test features in manageable stages.

## 11. Development Approach

Sangam will be developed incrementally.

Proposed phases:

| Phase | Focus                                        |
| ----- | -------------------------------------------- |
| 1     | Requirements and domain modelling            |
| 2     | Architecture and database design             |
| 3     | Project setup and development foundation     |
| 4     | Personal and organization account management |
| 5     | Authentication and authorization             |
| 6     | Employee and team management                 |
| 7     | Project and task management                  |
| 8     | Collaboration and document management        |
| 9     | AI-powered capabilities                      |
| 10    | Workflow automation                          |
| 11    | Deployment, monitoring, and optimization     |

The sequence may evolve as requirements become clearer.

## 12. Documentation and Decision Management

Documentation is part of the development lifecycle.

Important decisions will be recorded using Architecture Decision Records (ADRs).

Documentation will cover:

- Functional and non-functional requirements.
- System architecture.
- Domain model and database design.
- API contracts.
- Implementation details.
- Testing strategies.
- Deployment and operational procedures.

Documentation must be updated whenever an accepted design or implementation changes.

## 13. Current Project Status

**Status:** Initial project setup completed.

Completed:

- GitHub repository created.
- Initial project structure created.
- README.md and .gitignore committed.
- Initial architecture documentation files created.

In progress:

- Requirements clarification.
- Domain modelling.
- Architecture decisions.
- Database design.

Not yet implemented:

- Backend application.
- Frontend application.
- Database integration.
- Authentication and authorization.

# Architectural Style – Modular Monolith

Include:

- One Spring Boot deployment.
- Business modules organized by feature.
- Module ownership and communication rules.
- Shared MySQL database with module-owned tables.
- Future microservices extraction strategy.
