a. How much data your publisher program will send to the message broker in one run?

Dalam satu kali dijalankan (cargo run), program publisher akan mengirimkan 5 buah pesan ke message broker.

Hal ini terlihat dari pemanggilan fungsi p.publish_event yang dilakukan sebanyak lima kali secara berurutan di dalam fungsi main(). Setiap pemanggilan mengirimkan satu instance UserCreatedEventMessage dengan data yang berbeda (User ID 1 sampai 5, dengan nama Amir, Budi, Cica, Dira, dan Emir). Pesan-pesan ini kemudian akan masuk ke dalam antrean (queue) yang bernama "user_created".

b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?

Kesamaan URL koneksi tersebut berarti kedua program (Publisher dan Subscriber) terhubung ke broker RabbitMQ yang sama.

Dalam arsitektur Message Broker, broker berfungsi sebagai perantara pusat. Agar komunikasi terjadi:

Publisher harus tahu ke mana harus mengirimkan pesan (alamat broker).

Subscriber harus tahu dari mana harus mengambil pesan (alamat broker yang sama).

Karena keduanya menggunakan localhost:5672, ini menandakan bahwa kedua program tersebut berkomunikasi melalui server RabbitMQ yang berjalan di mesin lokal kamu. Jika URL-nya berbeda (misalnya alamat IP yang berbeda), maka Publisher akan mengirim pesan ke tempat yang tidak bisa didengar oleh Subscriber, sehingga pesan tidak akan pernah sampai.