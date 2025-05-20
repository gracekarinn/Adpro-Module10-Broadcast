### 2.1

![Screenshot](/public/2.1.png)

Saat seorang klien mengirim pesan, pesan itu terlebih dahulu diterima oleh server, lalu server meneruskannya ke seluruh klien yang sedang terhubung, termasuk pengirimnya sendiri. Mekanisme ini berjalan karena server menyimpan informasi tentang semua klien yang terkoneksi dan akan terus memantau hingga salah satu dari mereka mengirimkan pesan yang kemudian akan disebar.


### 2.2

![Screenshot](/public/2.2.png)

Pada gambar tersebut, terlihat bahwa port yang digunakan oleh klien dan server tidak sama. Sserver menunggu koneksi di port 8080, sedangkan klien mencoba terhubung ke websocket di port 2000. Karena tidak ada websocket yang aktif di port 2000, klien mengalami error *ConnectionRefused*. Ini menunjukkan bahwa klien sudah mencoba beberapa kali untuk membuat koneksi, tetapi tidak pernah berhasil. Agar aplikasi dapat berjalan dengan normal, seperti sebelumnya, solusi yang perlu dilakukan adalah mengganti baris kode di `client.rs` menjadi menggunakan port yang sama dengan server, yaitu mengubah bagian `ClientBuilder::from_uri(Uri::from_static("ws://127.0.0.1:2000"))` ke port 8080. 

Keduanya menggunakan websocket protokol. Prefix ws:// secara eksplisit menunjukkan bahwa klien akan mencoba membuka koneksi menggunakan protokol WebSocket (bukan TCP biasa atau HTTP biasa).

### 2.3

Pada setiap contoh yang saya tampilkan sejauh ini, setiap pesan yang ditampilkan dari server menyertakan informasi tentang klien pengirim pesan tersebut. Hal ini bisa dilakukan dengan cara memodifikasi bagian kode di `src/bin/server.rs` yang bertanggung jawab untuk mengirim pesan ke seluruh klien yang terhubung. Tepatnya, pada baris `bcast_tx.send(text.into())?;`, saya ubah menjadi `bcast_tx.send(format!("{addr:?}: {text:?}"))?;`. Dengan perubahan ini, setiap kali ada klien yang mengirim pesan, server akan menyebarkan pesan tersebut ke semua klien beserta informasi IP dan port pengirimnya.