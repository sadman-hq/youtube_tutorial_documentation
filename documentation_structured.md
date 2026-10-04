# Software Documentation — Structured Guide

> Software documentation is written text or illustration that explains **how software operates or how to use it**. It supports usability, communication, development, testing, maintenance, and knowledge preservation.

**Core idea:** *Good documentation communicates the right information to the right stakeholder at the right level of detail, throughout the software lifecycle.*

---

## Table of Contents

**Part I — Foundations**
1. [Introduction](#1-introduction)
2. [Types of Documentation at a Glance](#2-types-of-documentation-at-a-glance)

**Part II — Requirement Specification**
3. [Requirement Documents Overview](#3-requirement-documents-overview)
4. [BRS — Business Requirement Specification](#4-brs--business-requirement-specification)
5. [FRS — Functional Requirement Specification](#5-frs--functional-requirement-specification)
6. [SRS — Software Requirement Specification](#6-srs--software-requirement-specification)
7. [Functional vs Non-Functional Requirements](#7-functional-vs-non-functional-requirements)
8. [Use Cases](#8-use-cases)
9. [BRS vs FRS vs SRS](#9-brs-vs-frs-vs-srs)

**Part III — Design, Technical, User, Marketing**
10. [Architecture / Design Documentation](#10-architecture--design-documentation)
11. [Technical Documentation](#11-technical-documentation)
12. [User-End Documentation](#12-user-end-documentation)
13. [Marketing Documentation](#13-marketing-documentation)

**Part IV — Practice and Management**
14. [Documentation Across the SDLC](#14-documentation-across-the-sdlc)
15. [Stakeholders and Their Needs](#15-stakeholders-and-their-needs)
16. [Characteristics of Good Documentation](#16-characteristics-of-good-documentation)
17. [Requirement Traceability](#17-requirement-traceability)
18. [Quality Problems](#18-quality-problems)
19. [Maintenance and Version Control](#19-maintenance-and-version-control)

**Part V — Worked Example and Review**
20. [Worked Example: Online Course Registration](#20-worked-example-online-course-registration)
21. [Quick Revision](#21-quick-revision)
22. [Final Summary](#22-final-summary)

---

# PART I — FOUNDATIONS

## 1. Introduction

Documentation acts as a **communication bridge** among stakeholders: customers/clients, business analysts, project managers, architects, developers, testers/QA, system administrators, end users, marketing teams, and maintenance teams.

A well-documented system lets people understand **what it should do, how it is designed, how it is implemented, and how it is used** without relying on verbal explanations or the original developers.

### 1.1 Why documentation matters

| # | Benefit | Explanation |
|---|---|---|
| 1 | Preserves knowledge | Knowledge doesn't live only in individual developers' heads |
| 2 | Improves communication | Teams share a common understanding |
| 3 | Supports development | Requirements, architecture, APIs, dependencies, decisions are clear |
| 4 | Supports testing | Requirements/specs define correct behavior |
| 5 | Supports maintenance | Future developers can change the system more safely |
| 6 | Supports users | Tutorials, instructions, screenshots, examples |
| 7 | Provides organizational knowledge | Transfers knowledge when team members leave |

### 1.2 Different roles need different documents

| Role | Documentation primarily needed |
|---|---|
| Client | Business requirements |
| Business Analyst | Requirements and business rules |
| Developer | Technical and architecture documentation |
| Software Architect | Architecture/design documentation |
| Tester | Functional requirements and SRS |
| End User | User documentation |
| Marketing Team | Marketing documentation |
| Maintenance Developer | Technical + architecture documentation |

There is **no single document containing everything**; a project needs several types.

## 2. Types of Documentation at a Glance

Five major categories, covering the lifecycle from *business problem → requirements → design → implementation → use and promotion*:

| # | Category | Main question | Primary audience | Purpose |
|---|---|---|---|---|
| 1 | **Requirement Specification** (BRS / FRS / SRS) | Why / what must it do? | Stakeholders, developers, testers | Define needs and requirements |
| 2 | **Architecture / Design** | How is the system organized? | Architects, developers | Describe system structure |
| 3 | **Technical** | How is it built/maintained? | Developers, technical teams | Explain implementation |
| 4 | **User-End** | How do I use it? | End users | Help users operate software |
| 5 | **Marketing** | Why should I choose it? | Potential customers | Promote the product |

```
                  SOFTWARE DOCUMENTATION
                           │
        ┌──────────────────┼──────────────────┐
     BUSINESS          TECHNICAL              USER
      "WHY?"            "HOW?"            "HOW DO I?"
        │
  Requirements
   ┌────┼────┐
  BRS  FRS  SRS
```

---

# PART II — REQUIREMENT SPECIFICATION

## 3. Requirement Documents Overview

Requirement documentation describes **what the customer and stakeholders expect** from the software, at different levels of abstraction.

```
Business Needs → BRS → Functional Expectations → FRS
   → Complete Software Requirements → SRS
   → Architecture & Design → Implementation → Testing & Deployment
```

> In real organizations the boundaries between BRS, FRS and SRS vary; some combine them, others keep them separate.

## 4. BRS — Business Requirement Specification

| | |
|---|---|
| **Definition** | Formal document describing business needs and goals provided by the customer/stakeholders |
| **Prepared by** | Business analysts, from stakeholder specifications |
| **When** | Early in the product lifecycle |
| **Answers** | **Why does the organization need this software?** |

### 4.1 Purpose

- Identify business problems
- Define business objectives
- Capture stakeholder expectations
- Establish business scope
- Describe expected business outcomes
- Give a high-level understanding of the product

### 4.2 Typical contents

| Section | Description | Example |
|---|---|---|
| Business background | The organization and problem to address | — |
| Business problem | The existing problem | Customers must visit a branch to submit loan applications → long processing times |
| Business objectives | What the organization wants to achieve | Reduce processing time; allow online applications; reduce paperwork |
| Stakeholders | People/organizations affected | Customers, bank employees, managers, administrators |
| Business scope | What the project will and won't cover | — |
| Business constraints | Limits on the project | Budget, regulations, policies, time |
| Expected benefits | Anticipated gains | Lower costs, better satisfaction, faster processing, more revenue |

### 4.3 Example

> The university wants an online course registration system that lets students register without visiting administrative offices, reducing manual workload and improving efficiency.

It does **not** mention database tables, APIs, languages, or algorithms — only the business problem and goal.

## 5. FRS — Functional Requirement Specification

| | |
|---|---|
| **Definition** | Describes the functions the software/product must perform |
| **Also covers** | Sequence of operations required to develop the product; component behavior during user interaction |
| **Collaboration** | Close work between developers and testers |
| **Answers** | **What functions should the system perform?** |

### 5.1 Functional requirements

Examples:

- The system shall allow users to register.
- The system shall allow students to search for courses.
- The system shall allow students to enroll in courses.
- The system shall calculate the student's total credit load.
- The system shall generate a registration confirmation.

A good requirement is **clear, specific, testable, unambiguous, consistent**.

### 5.2 Typical FRS structure (example: Student Registration)

| Element | Content |
|---|---|
| Feature name | Student Registration |
| Description | What the feature does |
| Preconditions | Student must be logged in |
| Main flow | 1. Student selects a course → 2. System checks availability → 3. Checks prerequisites → 4. Verifies credit limit → 5. Registers the student → 6. Displays confirmation |
| Alternative flow | If the course is full, show a "Course Full" message |
| Postconditions | State after successful completion |

## 6. SRS — Software Requirement Specification

| | |
|---|---|
| **Definition** | Comprehensive document describing the requirements of the software system |
| **Prepared by** | System analysts |
| **Includes** | Functional and non-functional requirements, plus use cases |
| **Role** | Basis of agreement between stakeholders; foundation guiding the project team |
| **Answers** | **What exactly must the system provide, and what constraints must it satisfy?** |

## 7. Functional vs Non-Functional Requirements

| | Functional | Non-Functional |
|---|---|---|
| Specifies | **What** the system does | **How well** it performs / constraints it operates under |
| Examples | Registration, login, password reset, enrollment, payment, reports, email notification | Performance, security, reliability, availability, scalability, usability, maintainability, portability, compatibility |
| Sample | *The system shall allow students to enroll in an available course.* | *The system shall return course-search results within 2 seconds for 95% of requests under the specified normal workload.* |

**Make requirements measurable:**

| ✗ Vague | ✓ Measurable and testable |
|---|---|
| "The system should be fast." | "Results within 2 seconds for 95% of requests under normal workload." |

## 8. Use Cases

A **use case** describes how an actor interacts with a system to accomplish a goal.

**Example — Course Registration** (Actor: Student; Goal: register for a course)

```
Student → Login → Course Registration System
        → Search Course → Course List
        → Select Course → Eligibility Check
        → Eligible → Registration Confirmation
```

A use case normally identifies: **actor, goal, preconditions, main flow, alternative flows, exceptions, postconditions.**

## 9. BRS vs FRS vs SRS

| Aspect | BRS | FRS | SRS |
|---|---|---|---|
| Main focus | Business needs | Functions | Complete software requirements |
| Main question | Why? | What functions? | What must the system satisfy? |
| Abstraction | High | Medium | Detailed |
| Audience | Business stakeholders | Developers / testers / analysts | Stakeholders + technical teams |
| Covers | Business goals | System behavior/functions | Functional + non-functional requirements |
| Example | Reduce registration workload | Allow students to register | Support 5,000 concurrent users |

**Memory aid:** BRS → **WHY** · FRS → **WHAT FUNCTIONS** · SRS → **WHAT + CONSTRAINTS**

---

# PART III — DESIGN, TECHNICAL, USER, MARKETING

## 10. Architecture / Design Documentation

Provides a **comprehensive architectural view** of the system. Examples: ERD, UML diagrams, DFD.
**Answers:** *How is the system organized and how do its major components interact?*

**Helps readers understand:** major components, component relationships, data flow, system boundaries, external systems, database structure, deployment structure, communication mechanisms.

### 10.1 ERD — Entity Relationship Diagram

Represents the structure of data: **entities, attributes, relationships, primary keys, foreign keys, cardinality.**

```
STUDENT                      COURSE
-----------                  -----------
Student_ID                   Course_ID
Name                         Course_Name
Email                        Credit

Many-to-many via an associative entity:
STUDENT 1 ───── N ENROLLMENT N ───── 1 COURSE
```

### 10.2 UML — Unified Modeling Language

| Type | Diagrams |
|---|---|
| **Structural** | Class, Component, Deployment, Package |
| **Behavioral** | Use Case, Sequence, Activity, State Machine |

### 10.3 DFD — Data Flow Diagram

Shows how data moves through a system. Contains **external entities, processes, data stores, data flows.**

```
Student → (Registration Request) → [Registration Process]
        → [Student Database] → (Registration Confirmation) → Student
```

Best when the concern is **data movement and processing**.

## 11. Technical Documentation

Covers technical aspects of a project (or part of it): libraries, dependencies, APIs, and the codebase itself.
**Answers:** *How is the software built, configured, integrated, and maintained?*

### 11.1 Contents

| Area | Details | Example |
|---|---|---|
| **Technology stack** | Technologies used | React (frontend), FastAPI (backend), PostgreSQL (DB), JWT (auth), Docker (deployment) |
| **Dependencies** | External libraries and **versions** (behavior changes between versions) | Python 3.12, FastAPI, SQLAlchemy, Pydantic, PostgreSQL |
| **API documentation** | How components communicate | See below |
| **Code documentation** | Comments, docstrings, function/class/module docs, READMEs | See below |
| **Configuration** | Env variables, config files, DB, auth, deployment, network, third-party settings | — |

### 11.2 API documentation

Document: endpoint, HTTP method, authentication, request parameters, request body, response format, status codes, error responses, example requests.

```http
POST /api/users/login

Request:
{ "email": "user@example.com", "password": "password" }

Response:
{ "token": "...", "expires_in": 3600 }
```

### 11.3 Code documentation — explain *why*, not *what*

```python
# ✗ Poor: restates the obvious
# increment x
x = x + 1

# ✓ Better: explains intent
# Retry count is increased after a failed API request
# so the caller can enforce the maximum retry policy.
retry_count += 1
```

### 11.4 Configuration safety

> Passwords, API keys, and access tokens must **never** be committed into public documentation or source code.

## 12. User-End Documentation

Designed for **end users**; may include snapshots, diagrams, tutorials, steps, and do's and don'ts. Unlike technical documentation, it should avoid unnecessary implementation details.

### 12.1 Types

| Type | Description |
|---|---|
| User manual | Complete guide to using the product |
| Quick start guide | Short guide to begin quickly |
| Tutorials | Step-by-step task instructions |
| FAQs | Frequently asked questions and answers |
| Troubleshooting guide | Solutions to common problems |
| Help documentation | Searchable, feature-level documentation |

### 12.2 Example — Online banking: *How to Transfer Money*

1. Log in to your account
2. Select **Transfer Money**
3. Enter the recipient's account number
4. Enter the transfer amount
5. Review the transaction details
6. Confirm the transaction
7. Enter the required authentication code
8. Save the transaction confirmation

| ✓ Do | ✗ Don't |
|---|---|
| Verify the recipient's account number | Share your password |
| Check the amount before confirming | Share OTP/authentication codes |
| Keep authentication information private | Use public devices for sensitive transactions |

## 13. Marketing Documentation

Designed to **promote a product and encourage potential customers to use or buy it**. It should consider **human psychology, current trends, clear product features, and logical comparison with alternatives.**

| Technical documentation | Marketing documentation |
|---|---|
| Helps people understand and build/use the system correctly | Helps convince potential customers the product is valuable |

**Components:** product brochures, product webpages, feature comparison pages, presentations, promotional videos, case studies, product announcements, feature summaries, sales materials.

**Example** — *AI-Powered Productivity Assistant:* "Organize tasks, summarize documents, generate reports, and automate repetitive workflows from one platform." Highlights: AI automation, easy interface, faster workflow, secure data handling, integration with existing tools. The focus is **value and benefits**, not neural network architecture.

---

# PART IV — PRACTICE AND MANAGEMENT

## 14. Documentation Across the SDLC

Documentation is not only created after coding; it supports nearly every stage.

| SDLC stage | Documentation produced |
|---|---|
| Business analysis | BRS |
| Requirements analysis | FRS / SRS |
| System architecture | Architecture documentation |
| Implementation | Technical documentation |
| Testing | (uses SRS/FRS; supports user documentation) |
| Deployment | User documentation |
| Maintenance | Documentation updates |

Marketing documentation develops alongside product development and release.

**Documentation as a communication chain:**

```
Client → Business Analyst → Requirements → Architect → Architecture
       → Developer → Implementation → Tester → Validation → End User → Usage
```

Documentation lets information move between stages without every person talking directly to every other.

## 15. Stakeholders and Their Needs

| Stakeholder | Interested in | Most relevant documents |
|---|---|---|
| **Business stakeholder** | Goals, cost, benefits, scope, outcomes | BRS |
| **Developer** | Requirements, architecture, APIs, libraries, database, source code, configuration | SRS, architecture, technical |
| **Tester** | Functional requirements, expected behavior, acceptance criteria, edge cases, non-functional requirements | SRS / FRS |
| **End user** | Performing tasks, using features, troubleshooting, do's and don'ts | User documentation |
| **Customer / prospect** | Features, benefits, product value, competitive advantages | Marketing documentation |

## 16. Characteristics of Good Documentation

| Characteristic | Meaning |
|---|---|
| **Clear** | Understood without unnecessary interpretation |
| **Correct** | Accurately represents the software |
| **Complete** | No important information missing |
| **Consistent** | Terminology, formatting, diagrams, descriptions agree |
| **Unambiguous** | Only one reasonable interpretation |
| **Verifiable** | Requirements are testable where possible |
| **Maintainable** | Easy to update as the system changes |
| **Accessible** | Right stakeholders can find and use it |
| **Traceable** | Requirements trace through design, implementation, testing |

## 17. Requirement Traceability

Each requirement should connect through the lifecycle:

```
Business Requirement → Functional Requirement → System Requirement
  → Design Component → Implementation → Test Case → Test Result
```

| Requirement ID | Requirement | Design | Test Case |
|---|---|---|---|
| FR-001 | User shall log in | Authentication module | TC-001 |
| FR-002 | User shall reset password | Password service | TC-002 |
| FR-003 | User shall register for a course | Registration module | TC-003 |

This shows whether every important requirement was implemented **and** tested.

## 18. Quality Problems

| Problem | Description |
|---|---|
| Outdated | Software changed; documentation wasn't updated |
| Incomplete | Important information missing |
| Ambiguous | Different people interpret the same requirement differently |
| Excessive | Unnecessary content makes it hard to maintain |
| Inconsistent | Conflicting terminology or requirements across documents |
| No ownership | Nobody is responsible for maintaining it |

## 19. Maintenance and Version Control

### 19.1 Maintenance

Documentation should evolve with the software. Example change chain:

```
API changed → Technical docs updated → User workflow changed
  → User docs updated → Requirement/design impact reviewed
```

**Maintain:** version numbers, revision history, document owners, last-updated date, change descriptions, approval status.

### 19.2 Version control (e.g., Git)

```
docs/
├── requirements/
│   ├── BRS.md
│   ├── FRS.md
│   └── SRS.md
├── architecture/
│   ├── system-architecture.md
│   ├── erd.png
│   └── uml/
├── technical/
│   ├── api.md
│   ├── setup.md
│   └── deployment.md
└── user/
    ├── user-guide.md
    └── troubleshooting.md
```

---

# PART V — WORKED EXAMPLE AND REVIEW

## 20. Worked Example: Online Course Registration

| Document | Content |
|---|---|
| **BRS** | *Business goal:* reduce manual course-registration workload and allow online registration |
| **FRS** | System should: (1) let students log in, (2) display available courses, (3) let students select courses, (4) check prerequisites, (5) check seat availability, (6) check credit limits, (7) confirm registration |
| **SRS — functional** | Student authentication, course search, course registration, prerequisite validation, credit validation, registration confirmation |
| **SRS — non-functional** | Support expected registration workload; securely handle credentials; be available during registration periods; maintain data consistency in transactions |
| **Architecture** | Web App → Backend API → Business Logic → Database; plus ERD, class diagrams, sequence diagrams, DFD, deployment architecture |
| **Technical** | Frontend/backend frameworks, database, API endpoints, authentication, environment variables, deployment, dependencies, dev setup |
| **User** | *How to register for a course* — screenshots, step-by-step instructions, common errors, do's and don'ts, FAQs |
| **Marketing** | "Register for courses from anywhere, reduce administrative delays, and receive instant registration confirmation." |

## 21. Quick Revision

| Term | Remember it as |
|---|---|
| Software documentation | Information explaining software or its use |
| BRS | Business needs and goals |
| FRS | Software functions |
| SRS | Complete software requirements |
| Functional requirement | What the system does |
| Non-functional requirement | How well / under what constraints it operates |
| Architecture documentation | System structure |
| ERD | Data/entity relationships |
| UML | Software/system modeling |
| DFD | Data movement |
| Technical documentation | Implementation and technical details |
| User documentation | Instructions for users |
| Marketing documentation | Product promotion |

## 22. Final Summary

Software documentation underpins **usability, communication, development, testing, maintenance, and knowledge preservation**. It falls into five categories: **requirement specification, architecture/design, technical, user-end, and marketing**.

Within requirements, **BRS** captures business needs, **FRS** the functions the software must perform, and **SRS** the broader functional and non-functional specification that serves as the project's common foundation. Architecture documentation (ERDs, UML, DFDs) explains structure; technical documentation covers libraries, dependencies, APIs, and code; user documentation helps people operate the product; marketing documentation communicates value.

| Document | One-line meaning |
|---|---|
| **BRS** | **Why** — are we building it? |
| **FRS** | **What functions** — should it provide? |
| **SRS** | **Complete requirements** — what must it satisfy? |
| **Architecture** | **How it is structured** |
| **Technical** | **How it is built** |
| **User** | **How to use it** |
| **Marketing** | **Why to choose it** |
