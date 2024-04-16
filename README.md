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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

## 1. Pada observer patterns, subscriber adalah intercafe (salah satu trait rust untuk mengaplikasikan implementasi konkret) yang membuat kita lebih fleksibel dan memudahkan kita juga dalam mengembangkan kode. Hal itu saya sebutkan karena kita dapat menambah jenis subscriber baru tanpa mengubah kode yang ada. Pada modul ini, bambangshop memiliki kemungkinan menambah jenis subscriber sehingga lebih baik jika kita mendefinisikan subscriber sebagai trait.


## 2. Pada modul ini, dashmap lebih cocok untuk menangani id uni program dan url subscriber daripada vec. Mengapa? hal ini dikarenakan DashMap bersifat concurrent yang bisa mencari value dengan key sehingga memungkinkan program bekerja dengan kompleksitas O(1). Sedangkan vec adalah array yang bisa terus bertumbuh dan kurang efisien karena dapat menghasilkan kompleklsitas O(N).


## 3. Singleton pattern memastikan bahwa sebuah kelas hanya memiliki satu instance dan menyediakan akses ke instance tersebut secara global. Karena itu, ketika hal ini diterapkan akan membuat variable subscriber dipastikan hanya ada satu untuk seluruh aplikasi. Tetapi, pada Rust kita juga memerlukan sinkronisasi akses untuk mencegah isu-isu concurrency. Disitulah dashmap yang bersifat concurrent digunakan agar memungkinkan beberapa thread melakukan sesuatu seperti baca tulis ke Map yang ada dengan aman. Penggunaan dashmap pada modul ini sudah tepat karena telah mengamankan akses threadnya (thread safe).

#### Reflection Publisher-2

## 1. Dengan memisahkan Service dan Repository dari sebuah Model, kita menciptakan struktur yang lebih terorganisir dalam kode kita. Ini berarti kita secara eksplisit mendefinisikan peran yang jelas untuk setiap komponen: Service bertanggung jawab atas logika bisnis, sementara Repository mengelola akses data. Konsep ini mengikuti prinsip-prinsip clean code dengan membuat kode kita lebih terstruktur dan mudah dipahami. Dengan membagi tanggung jawab ini, kita menghindari kebingungan dan pencampuran antara logika aplikasi dan operasi database. Lebih dari itu, dengan menerapkan pemisahan ini, kita menerapkan prinsip Single Responsibility Principle (SRP). Setiap komponen dalam sistem memiliki tanggung jawab yang jelas dan spesifik. Service bertanggung jawab atas logika bisnis, sedangkan Repository hanya bertanggung jawab atas operasi database. Dengan cara ini, kita menciptakan sistem yang lebih mudah dipelihara dan diperluas di masa depan. Pemisahan ini memungkinkan kita untuk membuat perubahan pada logika bisnis tanpa mempengaruhi bagaimana data diakses atau sebaliknya. Hal ini memberikan fleksibilitas yang sangat dibutuhkan dalam pengembangan perangkat lunak.

## 2. Apabila kita hanya mengandalkan Model, kompleksitas akan merajalela di dalam setiap entitas seperti Program, Subscriber, dan Notification. Hal ini menjurus pada pelanggaran terhadap prinsip Single Responsibility Principle, di mana setiap model akan memikul beban tanggung jawab yang terlalu banyak. Mereka tidak hanya mengatur logika bisnis, tetapi juga mengurus akses data dan interaksi dengan model lainnya secara mandiri. Dengan pendekatan ini, kita berisiko membuat aplikasi kita menjadi sangat terikat satu sama lain, sehingga setiap perubahan pada salah satu model bisa berdampak besar pada keseluruhan sistem. Akibatnya, kemudahan dalam pemeliharaan sistem menjadi terkendala karena sulit untuk memahami dan mengelola kompleksitas yang terjadi di dalamnya. Oleh karena itu, penting bagi kita untuk memisahkan tanggung jawab antara logika bisnis, akses data, dan interaksi antarmodel dengan menerapkan struktur yang lebih terstruktur, seperti menggunakan Service dan Repository. Dengan cara ini, kita dapat menciptakan aplikasi yang lebih mudah dipelihara dan dikembangkan seiring berjalannya waktu.

## 3. Aplikasi POSTMAN ini sangat berguna untuk menguji API. Ini memungkinkan kita untuk memastikan bahwa fungsionalitas aplikasi berjalan seperti yang diharapkan dengan mengirimkan permintaan HTTP ke API yang dituju dan mengevaluasi respons yang diterima. Salah satu fitur yang menonjol bagi saya adalah kemampuan untuk membuat koleksi (collection) dari permintaan HTTP. Ini memudahkan kita untuk mengelompokkan, menyimpan, dan berbagi permintaan HTTP dalam satu tempat yang teratur. Saat bekerja pada proyek kelompok atau proyek individu, fitur ini menjadi sangat berharga karena memudahkan kolaborasi dan memastikan bahwa tim memiliki akses yang mudah terhadap semua permintaan yang diperlukan. Selain itu, kegunaan fitur pengujian API yang mudah sangat membantu dalam memastikan kualitas aplikasi kita. Dengan Postman, kita dapat dengan cepat dan efisien menguji berbagai skenario, mengidentifikasi bug, dan memperbaiki masalah sebelum aplikasi dideploy ke lingkungan produksi. Secara keseluruhan, Postman adalah alat yang sangat berharga dalam pengembangan perangkat lunak, terutama ketika bekerja dengan API. Fitur-fiturnya yang intuitif dan kuat membuatnya menjadi pilihan yang solid untuk proyek-proyek saat ini dan di masa depan.

#### Reflection Publisher-3

## 1. Tutorial ini menerapkan varian Observer pattern dalam bentuk push model. Konsep ini diimplementasikan dengan Publisher yang mengirimkan notifikasi kepada Subscribers melalui NotificationService. Ketika suatu perubahan terjadi, misalnya pembuatan, penghapusan, atau penerbitan produk, Publisher akan memanggil metode notify untuk memberitahukan Subscribers tentang peristiwa tersebut. Dengan pendekatan ini, Subscribers akan secara aktif menerima pembaruan tanpa harus meminta informasi secara eksplisit, sehingga memungkinkan sistem untuk secara dinamis merespons perubahan dengan lebih efisien dan fleksibel.

## 2. Pendekatan pull model menawarkan kelebihan berupa kebebasan bagi Subscriber untuk menentukan kapan mereka ingin menerima data yang dikirim oleh Publisher. Ini memberikan fleksibilitas yang signifikan dan memberikan kontrol penuh atas aliran data kepada Subscriber. Dengan Subscriber memutuskan kapan untuk mengambil data, mereka dapat menyesuaikan pemrosesan informasi sesuai dengan kebutuhan dan kondisi saat itu. Namun, keberadaan keuntungan ini juga diimbangi dengan beberapa kekurangan. Salah satunya adalah kebutuhan akan logika tambahan di sisi penerima untuk mengatur kapan notifikasi diterima dari Publisher. Hal ini menambah kompleksitas implementasi dan dapat menyulitkan manajemen aliran data di sisi Subscriber. Selain itu, pendekatan pull model juga dapat menyebabkan penundaan dalam pembaruan data bagi Subscriber yang tidak mengambil data secara aktif. Dalam skenario di mana Subscriber tidak secara teratur meminta pembaruan, informasi yang diperoleh dapat menjadi usang atau tidak akurat karena penundaan dalam pengambilan data yang diperlukan. Dengan demikian, sementara pull model memberikan fleksibilitas yang besar bagi Subscriber, keberadaannya juga menghadirkan tantangan tambahan dalam manajemen aliran data yang efisien dan tepat waktu.

## 3. Jika kita tidak menggunakan multi-threading dalam proses notifikasi, aplikasi dapat mengalami kinerja yang lambat ketika banyak notifikasi perlu dikirimkan ke Subscribers secara bersamaan. Ini terjadi karena pengiriman notifikasi dilakukan secara bergiliran atau berurutan satu per satu. Dalam situasi di mana pengiriman notifikasi kepada beberapa Subscriber dilakukan secara bersamaan, Subscriber selanjutnya harus menunggu hingga pengiriman notifikasi ke Subscriber sebelumnya selesai. Dalam konteks ini, kecepatan pengiriman notifikasi tergantung pada seberapa cepat setiap pengiriman notifikasi kepada setiap Subscriber selesai dilakukan. Tanpa multi-threading, aplikasi harus menunggu hingga notifikasi selesai dikirimkan ke satu Subscriber sebelum melanjutkan ke Subscriber berikutnya. Hal ini dapat menyebabkan penundaan yang signifikan, terutama ketika jumlah Subscriber yang menerima notifikasi cukup besar. Oleh karena itu, penggunaan multi-threading dapat menjadi solusi yang efektif untuk meningkatkan kinerja dalam kasus ini. Dengan multi-threading, aplikasi dapat mengirimkan notifikasi kepada beberapa Subscriber secara bersamaan, tanpa harus menunggu pengiriman sebelumnya selesai. Ini dapat mempercepat proses pengiriman notifikasi secara signifikan dan mengurangi waktu penundaan yang dialami oleh Subscriber.