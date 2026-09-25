# ⚡ BidStream — Real-Time Auction & Event Platform

> A production-oriented real-time auction backend built with **Java 17**, **Spring Boot 3**, **PostgreSQL**, **Redis**, **Spring Security**, **JWT**, **WebSocket**, **Docker**, **JUnit**, **Mockito**, and **OpenAPI**.

---

## ✨ Overview

**BidStream** is a real-time auction backend where users can participate in live auctions, place bids, and receive instant updates through WebSocket connections.

The project focuses on backend engineering challenges beyond basic CRUD applications, including:

* Real-time communication
* Secure REST APIs
* Concurrent bid processing
* Data consistency
* Role-based authorization
* Redis-based caching
* Automated auction winner determination
* API documentation
* Automated testing
* Containerized development

The system is designed with a layered architecture to keep business logic, persistence, security, and real-time communication clearly separated.

---

# 🎯 Project Goals

BidStream was developed to gain practical experience with backend and real-time software engineering concepts, including:

* Building secure **REST APIs**
* Implementing real-time communication using **WebSockets**
* Designing services for concurrent user interactions
* Maintaining data consistency during simultaneous bidding
* Implementing authentication and authorization
* Using Redis to improve frequently accessed data
* Building testable service-layer components
* Containerizing backend services using Docker
* Documenting APIs using OpenAPI
* Structuring backend applications for maintainability

---

# ✨ Core Features

## 🔴 Real-Time Auctions

BidStream supports live auction workflows where users can:

* View active auctions
* View auction details
* Place bids
* Receive real-time bid updates
* Track auction state
* View winning information after auction completion

WebSocket communication enables clients to receive auction updates without repeatedly polling the REST API.

---

## 💰 Bidding

The bidding workflow focuses on correctness during concurrent access.

### Supported Operations

* Place Bid
* Validate Bid Amount
* Check Auction Status
* Update Highest Bid
* Broadcast Bid Updates
* Determine Auction Winner

Bid validation is performed on the backend so that invalid bids and inconsistent auction states are rejected centrally.

---

## 🏆 Auction Winner Determination

When an auction reaches its completion state, the backend determines the winning bid based on the configured auction rules.

The workflow ensures that the final auction state is persisted consistently before the winner is returned to clients.

---

# 🔐 Authentication & Authorization

BidStream uses **Spring Security** and **JWT** for API security.

### Authentication

* User Registration
* Login
* JWT-based Authentication
* Secure Password Handling
* Protected API Endpoints

### Authorization

The application uses role-based access control with:

* `USER`
* `ADMIN`

Administrative operations are restricted to authorized users while regular users can access user-scoped auction functionality.

---

# 🔒 WebSocket Security

WebSocket communication is integrated with the application's authentication model.

Authenticated users can establish secure WebSocket sessions and receive updates for authorized auction interactions.

This allows the real-time layer to work together with the application's existing REST security model.

---

# 👤 User-Scoped Resources

BidStream supports user-specific functionality such as:

* Favorites
* Ratings
* User-specific auction interactions

Access to these resources is controlled through authenticated user context.

---

# ⚡ Redis Caching

**Redis** is used to cache frequently accessed auction and product data.

### Caching Goals

* Reduce repeated database reads
* Improve response times for frequently accessed data
* Reduce unnecessary database load
* Provide faster access to active auction information

Redis is used as a caching layer while PostgreSQL remains the primary persistent data store.

---

# 🗄️ Data & Persistence

## PostgreSQL

PostgreSQL is used as the primary relational database for storing:

* Users
* Products
* Auctions
* Bids
* Favorites
* Ratings
* Auction-related state

---

## Spring Data JPA

The persistence layer uses:

* Spring Data JPA
* Hibernate
* Repository abstractions
* Entity relationships
* Query methods
* Pagination

The application uses JPA to keep persistence logic separated from the service and controller layers.

---

# 🚦 Concurrency & Data Consistency

Real-time auctions introduce concurrency challenges because multiple users may attempt to place bids at nearly the same time.

BidStream therefore performs backend-side validation to ensure:

* The auction is still active
* The bid meets the required criteria
* Invalid bids are rejected
* The highest valid bid is maintained
* Auction state remains consistent

The goal is to prevent inconsistent bidding state when multiple requests are processed concurrently.

---

# 🏗️ Architecture

BidStream follows a layered backend architecture.

```text
                         Client
                           │
               ┌───────────┴───────────┐
               │                       │
               ▼                       ▼
          REST APIs                WebSocket
               │                       │
               └───────────┬───────────┘
                           │
                           ▼
                     Service Layer
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
       Security / JWT             Business Logic
              │                         │
              └────────────┬────────────┘
                           │
                 ┌─────────┴─────────┐
                 │                   │
                 ▼                   ▼
            PostgreSQL             Redis
             Persistence           Cache
```

### Architectural Layers

**Controller Layer**

Handles incoming REST and WebSocket requests.

**Service Layer**

Contains business logic, validation, auction workflows, and bid processing.

**Repository Layer**

Handles persistence using Spring Data JPA and Hibernate.

**Security Layer**

Manages JWT authentication and role-based authorization.

**Caching Layer**

Uses Redis for frequently accessed data.

This separation improves maintainability, testability, and clarity of responsibilities.

---

# 📁 Project Structure

```text
src
├── main
│   ├── java
│   │   └── com.bidstream
│   │       ├── auth
│   │       ├── config
│   │       ├── auction
│   │       ├── bid
│   │       ├── product
│   │       ├── favorite
│   │       ├── rating
│   │       ├── security
│   │       ├── websocket
│   │       ├── exception
│   │       └── common
│   │
│   └── resources
│       └── application.properties
│
└── test
    ├── service
    ├── security
    └── integration
```

---

# 🧪 Testing

BidStream includes automated testing using:

* **JUnit 5**
* **Mockito**
* Spring Boot testing utilities

### Test Focus

Testing covers important application behavior such as:

* Service-layer business logic
* Bid validation
* Auction workflows
* Authentication behavior
* Authorization rules
* Security-related components
* Repository/service interactions

The goal is to catch business-logic and security regressions before deployment.

---

# 📖 API Documentation

BidStream uses **SpringDoc OpenAPI** to provide interactive API documentation.

After starting the application:

```text
http://localhost:8080/swagger-ui/index.html
```

The Swagger interface can be used to explore REST endpoints, request parameters, responses, and authentication requirements.

---

# 🐳 Docker & Infrastructure

BidStream supports containerized local development using **Docker** and **Docker Compose**.

### Infrastructure

* Spring Boot application
* PostgreSQL
* Redis

Start the application stack with:

```bash
docker compose up --build
```

Backend:

```text
http://localhost:8080
```

Swagger UI:

```text
http://localhost:8080/swagger-ui/index.html
```

---

# ⚙️ Environment Configuration

Create an environment configuration with the required database, Redis, and JWT values.

Example:

```env
# ----------------------------
# PostgreSQL
# ----------------------------

SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/bidstream
SPRING_DATASOURCE_USERNAME=postgres
SPRING_DATASOURCE_PASSWORD=postgres

# ----------------------------
# JWT
# ----------------------------

JWT_SECRET=your-super-secret-jwt-key

# ----------------------------
# Redis
# ----------------------------

REDIS_HOST=localhost
REDIS_PORT=6379
```

> Never commit real credentials, JWT secrets, passwords, or other sensitive configuration to GitHub.

---

# 💻 Running Locally

## Prerequisites

Install:

* Java 17
* Docker
* Docker Compose
* Git

---

## Clone the Repository

```bash
git clone https://github.com/Pragati4566/bidstream-realtime-auction-platform.git

cd bidstream-realtime-auction-platform
```

---

## Start Infrastructure

Start PostgreSQL and Redis using Docker Compose:

```bash
docker compose up postgres redis
```

---

## Start the Application

Run the Spring Boot application using the configured build system.

For a Gradle-based project:

```bash
./gradlew bootRun
```

For a Maven-based project:

```bash
./mvnw spring-boot:run
```

Use the command matching the actual build configuration of the repository.

---

# 🔄 Real-Time Auction Flow

The core auction interaction follows this general flow:

```text
User
 │
 ▼
Authenticate
 │
 ▼
View Active Auction
 │
 ▼
Connect to WebSocket
 │
 ▼
Place Bid
 │
 ▼
Validate Bid
 │
 ▼
Update Auction State
 │
 ▼
Persist Bid
 │
 ▼
Broadcast Update
 │
 ▼
Other Connected Users
```

This allows multiple clients to stay synchronized with the latest auction state through real-time updates.

---

# 📈 Performance Considerations

BidStream focuses on performance through:

### Redis Caching

Frequently accessed auction and product information can be served from Redis to reduce database reads.

### Pagination

Pagination is used for data-heavy API operations to avoid unnecessarily loading large result sets.

### WebSockets

WebSockets allow real-time updates without continuous client-side polling.

### Backend Validation

Bid validation occurs on the server so concurrent clients cannot independently determine auction state.

---

# 🔐 Engineering Practices

The project emphasizes practical backend engineering practices:

* REST API design
* Layered architecture
* Authentication
* Authorization
* JWT security
* WebSocket communication
* Concurrent request handling
* Data consistency
* Redis caching
* Database persistence
* Pagination
* Automated testing
* API documentation
* Docker-based development
* Centralized exception handling

---

# 🛠️ Technology Stack

| Category                | Technology                  |
| ----------------------- | --------------------------- |
| Language                | Java 17                     |
| Framework               | Spring Boot 3.x             |
| Security                | Spring Security, JWT        |
| Real-Time Communication | Spring WebSocket            |
| Database                | PostgreSQL                  |
| ORM                     | Spring Data JPA, Hibernate  |
| Cache                   | Redis                       |
| Testing                 | JUnit 5, Mockito            |
| API Documentation       | SpringDoc OpenAPI / Swagger |
| Containerization        | Docker                      |
| Orchestration           | Docker Compose              |
| Build Tool              | Gradle / Maven              |

> Keep only the actual build tool used in the repository once the implementation is finalized.

---

# 📌 Development Roadmap

## Phase 1 — Core Auction Platform

* [x] User authentication
* [x] JWT authorization
* [x] Role-based access control
* [x] Product management
* [x] Auction management
* [x] Bid placement
* [x] Winner determination

## Phase 2 — Real-Time Layer

* [x] WebSocket communication
* [x] Real-time bid updates
* [x] Secured WebSocket sessions

## Phase 3 — Performance & Quality

* [x] Redis caching
* [x] Pagination
* [x] Service-layer testing
* [x] Security testing
* [x] Swagger / OpenAPI documentation
* [x] Docker-based development

## Phase 4 — Future Improvements

* [ ] Advanced observability
* [ ] Metrics and monitoring
* [ ] Distributed event processing
* [ ] Message broker integration
* [ ] Load testing
* [ ] Cloud deployment

---

# 📌 Current Project Status

BidStream is a **production-oriented real-time backend project** focused on secure APIs, real-time communication, concurrency handling, caching, and backend software engineering practices.

The current implementation demonstrates:

**Authentication + Authorization + REST APIs + WebSockets + PostgreSQL + Redis + Testing + Docker**

Future improvements will focus on scalability, observability, distributed processing, and cloud deployment.

---

# 🎓 Engineering Concepts Demonstrated

BidStream provides practical exposure to:

* Object-Oriented Programming
* REST API Design
* Backend Service Development
* Layered Architecture
* Authentication & Authorization
* JWT Security
* WebSocket Communication
* Real-Time Systems
* Concurrency
* Data Consistency
* Redis Caching
* PostgreSQL Persistence
* JPA / Hibernate
* Pagination
* Unit Testing
* Integration Testing
* API Documentation
* Docker
* Software Maintainability

---

# 🤝 Contributing

This repository is primarily a personal software engineering project.

For development:

```bash
git checkout -b feature/<feature-name>
```

Implement the change, add or update relevant tests, and validate the application locally before opening a pull request.

---

# 📄 License

This project should contain a valid license file reflecting the licensing terms of the codebase.

If any source code is adapted from an external repository, retain the applicable copyright and license notices and comply with the original license terms.

---

# 👩‍💻 Author

**Pragati Chaudhary**

B.Tech, Electronics and Communication Engineering
Indira Gandhi Delhi Technical University for Women

**GitHub:** https://github.com/Pragati4566

---

## 🚀 Project Focus

BidStream is designed to demonstrate backend engineering beyond basic CRUD development by combining:

**Secure APIs + Real-Time Communication + Concurrency + Caching + Testing + Containerization**

The project focuses on building software that is **secure, consistent, testable, maintainable, and responsive under real-time user interaction**.
