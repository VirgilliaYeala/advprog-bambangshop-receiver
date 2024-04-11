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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Jawaban : 
Dalam kasus ini, penggunaan RwLock<> diperlukan karena kita ingin memungkinkan multiple referensi ke data Vec Notifications secara aman, namun tetap membatasi akses mutable satu per satu. RwLock<> memungkinkan multiple referensi ke data tetapi hanya satu referensi mutable yang diperbolehkan pada satu waktu, sehingga memastikan keamanan akses ke data saat dibaca dan ditulis secara konkuren. Mengapa tidak menggunakan Mutex<>? Karena Mutex<> hanya mengizinkan satu pemilik pada satu waktu, yang tidak cocok untuk kasus di mana kita membutuhkan banyak pembaca yang berbagi data tanpa memblokir satu sama lain.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Jawaban : 
Dalam Rust, tidak diperbolehkan untuk melakukan mutasi pada variabel statik melalui fungsi statik. Hal ini karena Rust menempatkan keamanan sebagai prioritas utama, dan mengizinkan mutasi pada variabel statik melalui fungsi statik dapat menimbulkan bahaya dalam lingkup konkurensi dan pembagian data. Rust menerapkan kebijakan yang ketat terhadap mutasi pada data yang bersifat statis untuk mencegah terjadinya race condition dan konflik data saat eksekusi konkuren. Sebagai gantinya, Rust menyarankan penggunaan mekanisme lain, seperti RwLock<> atau Mutex<>, untuk memastikan akses yang aman dan terkendali ke data statis.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Jawaban : 
Ya, saya telah menjelajahi bagian-bagian di luar langkah-langkah yang disediakan dalam tutorial, termasuk file lib.rs. File ini mengatur konfigurasi aplikasi dan menyediakan utilitas yang diperlukan, seperti variabel REQWEST_CLIENT yang memastikan efisiensi penggunaan sumber daya dengan satu instance reqwest::Client, serta struktur data APP_CONFIG yang memuat konfigurasi dari file .env. Selain itu, tipe alias Result dan Error digunakan untuk penanganan error, sementara struktur ErrorResponse dan fungsi compose_error_response membantu dalam merangkum dan memberikan respons error yang jelas kepada pengguna atau klien aplikasi. Dengan memahami bagian-bagian ini, saya dapat meningkatkan pemahaman saya tentang konfigurasi aplikasi, penanganan error, dan penggunaan utilitas secara efisien dalam pengembangan aplikasi.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?


Jawaban : 
Pola Observer memungkinkan penambahan lebih banyak pelanggan dengan mudah, karena memisahkan logika subjek (pengirim notifikasi) dari pelanggan (penerima notifikasi). Dengan pola Observer, setiap kali subjek mengalami perubahan, ia secara otomatis memberi tahu semua pelanggan yang terdaftar tentang perubahan tersebut. Ini memungkinkan penambahan pelanggan baru tanpa memodifikasi subjek. Namun, ketika memunculkan lebih dari satu instansi Aplikasi Utama, penambahan ke sistem mungkin tidak selesai mudah, karena setiap instansi dapat memiliki konfigurasi dan keadaan yang berbeda. Ini memerlukan manajemen yang lebih rumit untuk memastikan konsistensi antara instansi dan koordinasi di antara mereka.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Jawaban : 
Saya telah mencoba membuat Tes sendiri dan meningkatkan dokumentasi pada koleksi Postman saya. Meningkatkan dokumentasi pada koleksi Postman membantu dalam memperjelas dan mendokumentasikan endpoint API, parameter, dan contoh permintaan/respons, yang sangat berguna untuk komunikasi tim dan pengguna lainnya. Selain itu, membuat Tes sendiri memungkinkan saya untuk menguji fungsionalitas aplikasi secara lebih terperinci dan memastikan bahwa aplikasi berperilaku sesuai dengan yang diharapkan. Kedua fitur ini sangat bermanfaat dalam pekerjaan saya, karena membantu dalam pengujian dan dokumentasi yang lebih baik dari aplikasi yang sedang dikembangkan.