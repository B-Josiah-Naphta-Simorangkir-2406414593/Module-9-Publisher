a. How much data your publisher program will send to the message broker in one run?

Dalam satu kali dijalankan (cargo run), program publisher akan mengirimkan 5 buah pesan ke message broker.

Hal ini terlihat dari pemanggilan fungsi p.publish_event yang dilakukan sebanyak lima kali secara berurutan di dalam fungsi main(). Setiap pemanggilan mengirimkan satu instance UserCreatedEventMessage dengan data yang berbeda (User ID 1 sampai 5, dengan nama Amir, Budi, Cica, Dira, dan Emir). Pesan-pesan ini kemudian akan masuk ke dalam antrean (queue) yang bernama "user_created".

b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?

Kesamaan URL koneksi tersebut berarti kedua program (Publisher dan Subscriber) terhubung ke broker RabbitMQ yang sama.

Dalam arsitektur Message Broker, broker berfungsi sebagai perantara pusat. Agar komunikasi terjadi:

Publisher harus tahu ke mana harus mengirimkan pesan (alamat broker).

Subscriber harus tahu dari mana harus mengambil pesan (alamat broker yang sama).

Karena keduanya menggunakan localhost:5672, ini menandakan bahwa kedua program tersebut berkomunikasi melalui server RabbitMQ yang berjalan di mesin lokal kamu. Jika URL-nya berbeda (misalnya alamat IP yang berbeda), maka Publisher akan mengirim pesan ke tempat yang tidak bisa didengar oleh Subscriber, sehingga pesan tidak akan pernah sampai.

RabbitMQ running image: ![RabbitMQ image](image/rabbitmq.png)

![Terminal image](image/terminal.png)
Ketika perintah cargo run dijalankan pada proyek publisher, program tersebut membuat koneksi ke RabbitMQ dan mengirimkan 5 buah event berupa pesan JSON (User ID 1-5). RabbitMQ kemudian menampung pesan-pesan tersebut di dalam antrean.

Karena program subscriber sudah dalam kondisi berjalan (listening), broker langsung meneruskan (push) pesan-pesan tersebut ke subscriber. Subscriber kemudian memprosesnya dengan mencetak pesan ke layar terminal secara berurutan. Ini menunjukkan bahwa sistem message broker berhasil memediasi pengiriman data secara asinkron antar dua aplikasi yang berbeda.

Spiking Graph image: ![Spiking Graph image](image/spiking_graph.png)

Lonjakan (spike) pada grafik tersebut merepresentasikan aktivitas pengiriman pesan dari program publisher. Berikut detailnya:

Penyebab Spike: Setiap kali saya menjalankan perintah cargo run pada proyek publisher, program tersebut mengirimkan 5 buah pesan ke broker dalam waktu yang sangat singkat. Lonjakan tajam ke atas pada grafik menunjukkan adanya kenaikan kecepatan publikasi pesan (Publish rate).

Hubungan dengan Publisher: Jika publisher dijalankan secara berulang-ulang, kita akan melihat deretan lonjakan pada grafik yang sesuai dengan momen eksekusi program.

Warna Grafik: Biasanya, garis berwarna kuning/emas menunjukkan Publish rate (pesan masuk ke broker), sedangkan garis berwarna ungu menunjukkan Deliver/Consumer ack rate (pesan keluar dari broker menuju subscriber). Karena subscriber kita berjalan sangat cepat, lonjakan pengiriman biasanya langsung diikuti oleh lonjakan penerimaan, menandakan pesan diproses hampir secara instan.