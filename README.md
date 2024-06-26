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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
Pola desain Observer memungkinkan objek untuk mendapatkan pemberitahuan tentang perubahan yang terjadi pada objek lain. Biasanya, hal ini dicapai dengan menggunakan trait pada Rust atau interface untuk mendefinisikan metode-metode yang ingin digunakan. Namun, pada BambangShop, tidak ada variasi dalam perilaku yang diharapkan dari observer, sehingga penggunaan interface atau trait tidak sepenuhnya diperlukan. Sebagai alternatif, menggunakan sebuah struktur Model tunggal yang mencakup data mengenai observer dan perilaku yang diperlukan untuk memberi tahu sudah cukup. Dengan demikian, penggunaan satu struktur Model untuk observer sudah memadai dalam konteks ini.
2.  id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
Jika hanya perlu menyimpan ID atau URL dari pelanggan, menggunakan Vec (list) sudah memadai. Namun, penggunaan DashMap (map/dictionary) lebih disarankan karena memberikan keunggulan dalam kecepatan akses dan manipulasi data.
Dengan DashMap, pengecekan terhadap ID atau URL yang sudah ada akan menjadi lebih sederhana. Selain itu, DashMap juga memfasilitasi akses dan pembaruan entri yang sudah ada dengan kecepatan yang lebih tinggi, yang dapat meningkatkan efisiensi dan kinerja aplikasi.
3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
Singleton pattern memastikan bahwa hanya ada satu instance dari sebuah kelas di seluruh aplikasi.
Penggunaan DashMap untuk variabel statis SUBSCRIBERS merupakan pilihan terbaik karena memungkinkan akses ke struktur data yang sama dari mana pun dalam aplikasi serta menjaga keamanan konkurensi. Dengan demikian, DashMap memberikan solusi yang efektif untuk mengelola keamanan konkurensi dalam lingkungan multi-threaded yang penting dalam pengembangan program Rust.
Meskipun pola Singleton dapat diimplementasikan dalam Rust, namun penggunaannya tidak sepenuhnya diperlukan dalam kasus ini karena DashMap telah menyediakan fungsionalitas yang diperlukan. Oleh karena itu, penggunaan DashMap adalah opsi yang tepat untuk kebutuhan variabel statis SUBSCRIBERS dalam Bambangshop.
#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
   Prinsip SRP (Single Responsibility Principle) menekankan pentingnya kelas yang hanya bertanggung jawab pada satu tugas tertentu. Oleh karena itu, ada kebutuhan untuk memisahkan Service dan Repository dari Model, sehingga Model hanya fokus pada definisi struktur data.
   Prinsip desain ini menyoroti pentingnya pemisahan komponen yang berbeda sehingga setiap komponen bertanggung jawab atas tugasnya sendiri. Dengan memindahkan logika bisnis ke Service dan operasi penyimpanan data ke Repository, Model dapat berperan sebagai representasi struktur data saja.
   Pendekatan ini memungkinkan Model untuk menjadi lebih fleksibel dan mudah menyesuaikan perubahan dalam struktur data atau penyimpanan data tanpa perlu memikirkan metode yang tidak relevan.
2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
   Jika kita hanya mengandalkan Model tanpa membagi tanggung jawab menjadi Service dan Repository, ini akan menyebabkan model bertanggung jawab atas representasi data dan logika bisnis. Dampaknya adalah terciptanya kelas-kelas yang memiliki tanggung jawab berlebihan yang tidak sesuai dengan prinsip SRP.
   Kesalahan ini dapat mengakibatkan peningkatan kompleksitas kode, kesulitan dalam pemeliharaan dan perluasan kode, interaksi antar-model yang lebih rumit dan terikat satu sama lain, serta ketergantungan yang kuat antar-kode.
   Sebagai contoh, jika suatu Produk harus memberi tahu Subscriber tentang perubahan, mungkin akan berinteraksi langsung dengan objek Subscriber untuk mengirimkan Notifikasi. Namun, ini bukan pendekatan yang ideal karena melanggar prinsip enkapsulasi dan menyulitkan pemahaman terhadap kode.
   Oleh karena itu, tanpa pembagian yang tepat atas tanggung jawab, kompleksitas kode untuk setiap Model akan meningkat. Ini dapat menyebabkan kode sulit dipahami, diuji, dan dipelihara.
3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
   Postman bisa digunakan untuk melakukan pengujian program dengan mengirimkan respons ke endpoint API. Ini memungkinkan simulasi tanpa harus menggunakan HTML.
   Postman dapat mengirimkan permintaan HTTP, yang memfasilitasi pengiriman permintaan ke endpoint API dengan parameter yang dapat disesuaikan, seperti header, query parameter, dan request body.
   Postman dilengkapi dengan fitur pengujian otomatis yang memungkinkan pembuatan dan pelaksanaan serangkaian pengujian otomatis untuk memverifikasi perilaku endpoint API, termasuk status kode respons, isi body respons, dan sebagainya.
   Selain itu, Postman memiliki fitur variabel lingkungan yang memudahkan dalam beralih antara lingkungan yang berbeda, seperti pengembangan, penyiapan, dan produksi saat menguji API.
#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
   Pada module ini, kita menggunakan variasi model Push dari Observer Pattern yang dapat dilihat dari penggunaan request HTTP POST untuk mengirimkan notifikasi kepada customer yang telah berlangganan.
   Notifikasi ini dipicu oleh create, promote, atau delete produk.
2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
   Keuntungan dari pendekatan ini adalah mengurangi penggunaan sumber daya jaringan dan komputasi karena Subscriber hanya mengambil data saat diperlukan. Selain itu, Subscriber memiliki kendali penuh atas waktu pengambilan data yang memungkinkan mereka untuk menghindari pengambilan data yang tidak diperlukan.
   Namun, kelemahannya adalah bahwa pelanggan harus secara aktif meminta pembaruan, yang dapat menyebabkan keterlambatan dalam mendapatkan informasi terbaru. Ini tidak efisien dalam situasi di mana pembaruan harus segera diterima. Selain itu, implementasi Model Pull dapat memerlukan penambahan logika di sisi pelanggan untuk mengelola permintaan dan pembaruan data dengan efisien.
3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
   Proses notifikasi akan dilakukan secara berurutan di mana setiap notifikasi diproses satu per satu. Kami menunggu hingga notifikasi sebelumnya selesai sebelum memproses yang berikutnya.
   Jika terdapat banyak notifikasi yang perlu dikirim, proses ini dapat menjadi lambat dan menyebabkan keterlambatan dalam memberikan respons.
   Namun, dengan menggunakan multi-threading, notifikasi dapat diproses secara paralel, yang dapat mempercepat proses serta meningkatkan responsivitas aplikasi. Selain itu, pendekatan ini lebih efisien ketika ingin mengirim banyak notifikasi.
