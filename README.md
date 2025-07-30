# Spring Academy Notes

This repository contains my personal notes, code examples, and projects from the courses I am taking at the [Spring Academy](https://spring.academy/) by Broadcom.

The purpose of this repository is to document my learning journey through the various Spring Framework topics, serving as a reference for key concepts and practical implementations.

## Fundamental Spring Boot Concepts

Here is a summary of the most important points from the initial course notes.

### Spring Framework vs. Spring Boot

While often used together, it's crucial to understand that Spring Boot is not a replacement for the Spring Framework, but rather an extension built on top of it to improve developer productivity.

## 📚 Spring Framework vs. Spring Boot

| Feature                  | Spring Framework                                      | Spring Boot                                         |
|--------------------------|--------------------------------------------------------|-----------------------------------------------------|
| Startup Configuration    | ❌ Manual configuration via XML or Java classes        | ✅ Auto-configuration based on classpath            |
| Embedded Web Server      | ❌ Requires external deployment (WAR in Tomcat, etc.)  | ✅ Ships with embedded Tomcat/Jetty                 |
| Dependencies Management  | ❌ Manual (add each dependency separately)             | ✅ Starter dependencies (`spring-boot-starter-*`)   |
| Production Readiness     | ❌ Needs custom setup                                  | ✅ Includes Actuator, metrics, health checks        |
| Ideal For                | Highly customized enterprise apps                     | Quick microservice & REST API development           |




#### The Spring Framework: The Foundation

The Spring Framework provides the core, foundational infrastructure for building enterprise Java applications. It is highly modular and focuses on flexibility and extensibility. Key features include:

*   **Core Infrastructure:** Provides fundamental features like Inversion of Control (IoC) via the `ApplicationContext` and Aspect-Oriented Programming (AOP).
*   **Data Integration:** Offers robust support for JDBC, JPA (Java Persistence API), and JMS (Java Message Service).
*   **Transaction Management:** Includes declarative transaction management, simplifying how data operations are handled.
*   **Manual Configuration:** Requires developers to explicitly configure the application, which offers great control but can be verbose and time-consuming.

#### Spring Boot: The Accelerator

Spring Boot is an opinionated project that radically simplifies the process of building production-ready applications on top of the Spring Framework. Its primary goal is to get you up and running as quickly as possible. Key features include:

*   **Auto-Configuration:** Intelligently configures your application based on the dependencies (JARs) present on the classpath.
*   **Starter Dependencies:** Provides "starter" packages (e.g., `spring-boot-starter-web`) that bundle common dependencies together, simplifying build configuration.
*   **Embedded Servers:** Includes embedded servlet containers like Tomcat or Jetty, so you can run your application as a standalone JAR without needing to deploy a WAR file.
*   **Production-Ready Features:** Comes with out-of-the-box features like Actuator endpoints for monitoring and managing your application.

In short, the **Framework** provides the *features*, while **Boot** provides a faster way to *use* those features. Understanding this distinction is key to avoiding suboptimal solutions and technical debt.

### Inversion of Control (IoC) and Dependency Injection (DI)

*   **Key Concept:** One of the core pillars of Spring. Instead of your code creating its own dependencies (objects it needs to function), the Spring container is responsible for creating them and "injecting" them where needed.
*   **Practical Advantage:** This allows for decoupling code from its dependencies. For example, you can configure the application to use an in-memory database for local development and a PostgreSQL database for production, all without changing a single line of your application's code.
*   **Terminology:** Although technically different, in the Spring ecosystem, the terms *Inversion of Control (IoC)* and *Dependency Injection (DI)* are often used interchangeably.

### Spring Initializr

*   **Purpose:** The recommended tool for starting any new Spring Boot project.
*   **How it works:** It's a web-based project generator (`start.spring.io`) where you define your project's metadata (name, Java version, etc.) and select the initial dependencies you need (e.g., "Spring Web," "Spring Data JPA"). Upon completion, it generates a compressed project ready to be imported into your IDE.

## Implementing GET Endpoints

### Key Concepts of REST and HTTP

*   **REST (Representational State Transfer):** An architectural style for managing the state (data) of "Resources" (objects).
*   **CRUD:** An acronym for the four basic data operations: **C**reate, **R**ead, **U**pdate, and **D**elete.
*   **Mapping CRUD to HTTP Methods:**
    *   `POST` for **Create** (Responds with `201 CREATED`).
    *   `GET` for **Read** (Responds with `200 OK`).
    *   `PUT` for **Update** (Responds with `204 NO CONTENT`).
    *   `DELETE` for **Delete** (Responds with `204 NO CONTENT`).
*   **Request & Response:** An HTTP **Request** contains a Method (verb), a URI (endpoint), and an optional Body. A **Response** contains a Status Code and a Body (often JSON).

### Implementation with Spring Boot

*   **`@RestController`:** A class-level annotation that marks a class as a request handler for REST APIs. Spring manages these objects as Beans in its IoC container.
*   **`@GetMapping("/path/{id}")`:** A method-level annotation that maps a method to handle HTTP GET requests for a specific path. The `{id}` part is a dynamic path variable.
*   **`@PathVariable`:** An annotation used on a method parameter to extract the value from the corresponding path variable (e.g., `{id}`) and inject it into the method.
*   **`ResponseEntity`:** A Spring class used to build a complete HTTP response. It allows you to control the **status code**, **headers**, and the **response body**. The `ResponseEntity.ok(body)` method is a convenient shortcut to return a `200 OK` status with a given body.

## Repositories & Spring Data

At this point in our development journey, we’ve got a system which returns a hard-coded Cash Card record from our Controller. However, what we really want is to return real data from a database.

Spring Data works with Spring Boot to make database integration simple. Before we jump in, let’s briefly talk about Spring Data’s architecture.

### Controller-Repository Architecture

The **Separation of Concerns** principle states that well-designed software should be modular, with each module having distinct and separate concerns from any other module.

Up until now, our codebase only returns a hard-coded response from the Controller. This setup violates the Separation of Concerns principle by mixing the concerns of a Controller (a web interface abstraction) with the concerns of reading and writing data to a data store. To solve this, we’ll use the **Repository pattern**.

A common architectural framework that divides these layers is called **Layered Architecture**. We can think of our Repository and Controller as two layers:
-   **Controller Layer**: Near the client, receives and responds to web requests.
-   **Repository Layer**: Near the data store, reads from and writes to it.

The Repository is the interface between the application and the database, providing a common abstraction that makes it easier to switch databases if needed. Spring Data provides a collection of robust data management tools, including implementations of the Repository pattern.

### Choosing a Database

For our database, we’ll use an embedded, in-memory database called **H2**.
-   **Embedded**: It's a Java library, added as a dependency.
-   **In-memory**: It stores data in memory only, not in permanent storage.

There are tradeoffs to using an in-memory database. It's great for development (no separate RDBMS installation, clean state on every run), but a persistent database is needed for production. This can lead to a **Dev-Prod Parity** mismatch, where the application behaves differently in development versus production. Fortunately, H2 is highly compatible with other relational databases, minimizing this issue.

### Auto-Configuration

This showcases one of the most powerful features of Spring Boot: **Auto-Configuration**. By simply adding the Spring Data and H2 dependencies, Spring Boot will automatically configure the application to communicate with H2. No manual configuration is needed.

### Spring Data’s CrudRepository

We’ll use Spring Data’s `CrudRepository`. It seems magical at first. The following is a complete implementation of all CRUD operations:

```java
interface CashCardRepository extends CrudRepository<CashCard, Long> {
}
```

With just this interface, a caller can use predefined methods like `findById`:

```java
cashCard = cashCardRepository.findById(99);
```

Where is the implementation? `CrudRepository` is an interface. Spring Data provides the implementation for us at runtime based on the specific Spring Data framework used (like Spring Data JDBC). The Spring runtime then exposes the repository as a bean that can be injected anywhere in the application.

While `CrudRepository` is convenient, it generates standard SQL. For more complex use cases, you might need to write custom SQL statements. For now, its out-of-the-box methods are sufficient.

**Summary**: To manage data, we use the Repository pattern for separation of concerns, with Spring Data's `CrudRepository` providing automatic CRUD operations. Spring Boot's auto-configuration simplifies database setup, and we'll use the H2 in-memory database for development.

## Implementing POST Endpoints

To add a "Create" operation to our API, we need to consider several aspects of the HTTP request and response.

*   **ID Generation**: The server will be responsible for generating the unique ID for each new resource. This is a simple and common approach, as databases are efficient at managing unique keys.
*   **HTTP Method**: We will use the `POST` method for creating new resources.
*   **Idempotence**: A `POST` request is **not idempotent**. This means that making the same `POST` request multiple times will result in multiple new resources being created, each with a different server-generated ID. This is in contrast to idempotent methods like `GET`, `PUT`, and `DELETE`, where repeated requests have the same effect as a single request.
*   **Request Body**: The client sends the data for the new resource (e.g., the `amount`) in the request body, typically as JSON. The ID is omitted since the server will generate it.
*   **Response**:
    *   **Status Code**: A successful creation should return `201 CREATED`. This is more specific than `200 OK` because it explicitly confirms that a new resource was created.
    *   **Headers**: The response should include a `Location` header containing the URI of the newly created resource (e.g., `Location: /cashcards/100`). This allows the client to easily retrieve the new resource.
*   **Spring Web Convenience**: Spring provides helpful methods like `ResponseEntity.created(uri)` to build a `201 CREATED` response. This method automatically sets the status code and adds the `Location` header, simplifying the controller logic.
