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

You can download the Postman Collection JSON here: https://api.postman.com/collections/1518342-1ab29a75-95d2-4e61-a257-0971e15b75a0?access_key=PMAT-01HT9WP343FDK3QKB7HGE2AKHJ

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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. It depends on the use of the observer design pattern. If we have an observer that has many types so that it consists of various classes, it will be necessary to create an observer using a trait. However, in BambangShop the observer, Subscriber, is only one class, so there is no need to create a trait, unless in the future there will be new observer additions.  

2. We need to use DashMap because with DashMap we will have a mapping of each product type to each subscriber who wants it. If we change it to a vector, then we need to create two vectors for each product where one vector stores the url and the other the subscriber, which will make it difficult to change the data.  

3. Based on my understanding, we need Dashmap instead of Hashmap because dashmap is a built in data structure which is a hashmap suitable for multithreading. In our case, we need it because BambangShop application will use multi threading and the SUBSCRIBER Map will be accessed by many threads. For Singleton, singleton serves to ensure that as long as the program runs there will only be 1 instance of the object. This is useful so that we can always ensure that the list of subscribers to our products is only in 1 dash map and not scattered in various data structures.  

#### Reflection Publisher-2
1. Service needs to be separated from Repository to fulfill the single responsibility principle. The separation of Service functions for modules that have the function of getting and processing data obtained from the repository while the Repository Layer functions as a layer that is useful for accessing the data base, changing and deleting the contents of the data base. The separation of these two layers helps in the development and maintainability of the code.  

2. If we only use the model layer without other layers, it will create a program that has a high coupling so that if there is a change, it will require a lot of changes to be made to the code.  

3. In my opinion, Postman is very useful for testing applications that we have made and seeing whether the application will return a response that matches our expectations based on the requests we make. I can also customize the desired method such as CRUD so that I can see whether the data retrieved is correct or incorrect through Postman.  

#### Reflection Publisher-3
1. In the case of this tutorial, the push model used can be seen in the algorithm section where when something happens to the object module such as create, delete or update, the notification service will call a method that will iterate through all subscribers to get the latest updates.  

2. If we use the pull method, then each subscriber must determine for themselves whether the data changes that occur are relevant to them. The advantage is that the observer is free to determine what data he will pull and when he will pull it. The disadvantage is that the observer needs to know the structure of the data source in order to do the aforementioned things.  

3. What will happen is in the notificationservice code when the notificationservice needs to notify each subscriber, it will create a long queue if there are a lot of subscribers which makes sending notifications to each subscriber hampered due to the bottle neck of computing capabilities.  