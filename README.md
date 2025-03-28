# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create SubscriberRequest model struct.`
    -   [v] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [v] Commit: `Implement add function in Notification repository.`
    -   [v] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [v] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Commit: `Implement receive_notification function in Notification service.`
    -   [v] Commit: `Implement receive function in Notification controller.`
    -   [v] Commit: `Implement list_messages function in Notification service.`
    -   [v] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. Why use RwLock<> instead of Mutex<>?
    i used RwLock<> because it allows multiple readers to access the data concurrently, which is useful for scenarios where notifications are being read frequently. However, it ensures that only one writer can modify the data at a time. This is important for performance, as we want to allow reading operations to happen simultaneously without blocking each other. On the other hand, Mutex<> would lock the entire data for reading and writing, which could reduce performance when there are frequent reads.

2. Why did Rust not allow mutating static variables directly?
    Rust does not allow directly mutating static variables for safety reasons. Unlike Java, where static variables can be mutated via static functions, Rust enforces strict ownership and borrowing rules. This ensures thread safety and prevents data races. The use of lazy_static and synchronization mechanisms like RwLock ensures that we can safely mutate the static variable in a controlled manner, maintaining Rust's memory safety guarantees.
#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs?
    I haven't explored things outside the steps in the tutorial, particularly the src/lib.rs file. The tutorial was very focused on the NotificationService, NotificationRepository, and related files, and I wanted to stick closely to the instructions to ensure the core functionality worked correctly before branching out. If I were to explore the lib.rs file, I would likely look for shared utility functions or global variables that might help in structuring the application more effectively.
2. explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?
    The Observer pattern has been very helpful in easily adding new subscribers to the system. The pattern allows me to decouple the logic of notifying users from the actual business logic, so whenever a new subscriber needs to be added, it's just a matter of creating a new instance and subscribing it to the publisher. This is true even when spawning multiple instances of the Receiver; the Observer pattern makes it straightforward to plug in more subscribers, as the logic is already structured to handle them. If I were to spawn multiple instances of the Main app, it would still be relatively simple to integrate them since the communication via the publisher-subscriber mechanism remains consistent across all instances.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection?
    I have tried enhancing the documentation in my Postman collection, which has been very useful. Adding detailed explanations and comments for each request in the Postman collection has made it easier to test the system and communicate the process to my team. As for writing my own tests, I have started creating some basic ones to verify the notification system's functionality. This has helped ensure that the system works as expected, especially with multiple subscribers involved.