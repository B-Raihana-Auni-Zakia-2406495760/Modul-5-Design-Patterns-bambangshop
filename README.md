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
-   [V] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Subscriber model struct.`
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [V] Commit: `Implement add function in Subscriber repository.`
    -   [V] Commit: `Implement list_all function in Subscriber repository.`
    -   [V] Commit: `Implement delete function in Subscriber repository.`
    -   [V] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
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
1. Di kasus BambangShop, penggunaan trait atau interface tidak selalu diperlukan karena Rust sudah mendukung struct dan implementasi langsung. Karena kita hanya punya satu jenis data spesifik yang menjalankan fungsi sama (notifikasi), maka satu Model Subscriber sudah cukup tanpa perlu menambah kompleksitas interface.
2. Penggunaan DashMap sangat disarankan karena kita membutuhkan lookup dan penghapusan data secara konstan O(1) berdasarkan url yang unik, dan menjaga thread-safety pada sistem yang berjalan secara concurrent.
3. Kita tetap butuh struktur seperti DashMap atau implementasi thread-safe lainnya walaupun kita memakai pola Singleton. Karena Singleton hanya menjamin satu instance secara global, tetapi tidak secara otomatis mencegah terjadinya race condition oleh beberapa thread secara bersamaan.

#### Reflection Publisher-2
1. Pemisahan antara Service, Repository, dan Model dilakukan dengan mengikuti prinsip Single Responsibility Principle (SRP). Model berfokus pada representasi data, Repository menangani interaksi dengan penyimpanan data, dan Service bertanggung jawab terhadap logika bisnis. Ini membuat kode menjadi lebih terstruktur dan mudah untuk diuji.
2. Jika semua tanggung jawab digabung ke dalam Model, maka kompleksitas kode akan meningkat drastis. Model bisa menjadi terlalu besar karena mencakup struktur data, pengelolaan memori, hingga komunikasi eksternal seperti HTTP request. Hal ini juga meningkatkan risiko error karena perubahan di satu bagian dapat memengaruhi bagian lainnya.
3. Ya, Postman sangat membantu karena memungkinkan pengujian API secara terpisah. Kita bisa menyimpan format request dalam bentuk collection, melakukan automasi request, serta melihat response secara langsung tanpa perlu membuat UI.

#### Reflection Publisher-3
