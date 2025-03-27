# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Is a trait still necessary in BambangShop, or is a single Model struct enough?

In typical Observer pattern implementations, a trait (or interface) defines a shared behavior for all subscribers—like an update() method. But in BambangShop's case, where all subscribers are external Rocket instances receiving notifications through HTTP POST requests, this shared behavior isn’t implemented in code. Instead, the system just sends out requests. So, there's no need for a trait or interface—one simple Subscriber struct holding the necessary data (like url and name) is sufficient.

2. Is using a Vec enough, or should we use DashMap to ensure uniqueness of IDs and URLs?

Using DashMap is the better approach here. While a Vec can store elements, you'd need to manually scan through it to avoid duplicates—this gets inefficient as the list grows. DashMap, on the other hand, allows fast, key-based access and inherently enforces uniqueness for IDs and URLs. Plus, it’s thread-safe, which is important in a multi-threaded web environment where multiple operations might happen at once.

3. Can we use the Singleton pattern instead of DashMap?

The Singleton pattern ensures that only one instance of something—like a global SUBSCRIBERS list—exists. However, it doesn't automatically handle concurrent access. DashMap not only works well as a Singleton (using something like lazy_static!) but also provides built-in thread safety. This makes DashMap a better fit because it supports safe concurrent read/write access without needing extra locking logic.


#### Reflection Publisher-2

1. Why should we separate “Service” and “Repository” from a Model in the MVC pattern?

Separating Service and Repository from the Model follows the Single Responsibility Principle (SRP), which means each component should only handle one specific task. 

- The Repository handles all data-related operations, such as saving, retrieving, or updating records. It abstracts away the database layer. 
- The Service layer deals with the core business logic, keeping it independent from how data is stored or retrieved.

This separation improves modularity and makes the code easier to test and maintain. Changes to how data is stored or how business logic works won’t directly affect the Model, helping avoid bugs and making the system more flexible.

2. What if we only use the Model?

Relying only on the Model for everything leads to tightly coupled and harder-to-manage code.

- Complexity: Models would need to handle data access, validation, and business rules, making them overly complex. 
- Repetition: Shared logic like validation or notifications might get duplicated across multiple models, which can cause inconsistencies. 
- Poor Scalability: Models interacting directly makes it harder to refactor or test, and the code becomes more rigid.

This setup goes against SRP and hurts scalability and maintainability.

3. How does Postman help with testing?

Postman is a great tool for testing REST APIs by allowing you to send simulated HTTP requests and check the responses, even without a frontend.

Helpful features include:
- Collections: Lets you organize and test multiple endpoints neatly.
- Environment Variables: Makes switching between dev and production easy.
- Automated Testing: You can write scripts to ensure endpoints still work after changes.
- Mock Servers: Helps simulate backend responses for testing purposes.

These features make Postman a powerful tool for debugging and validating APIs during both solo and group development work.


#### Reflection Publisher-3
