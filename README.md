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
