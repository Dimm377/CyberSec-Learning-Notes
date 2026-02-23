# TryHackMe: HTTP In Details


---

- **Room Link:** [HTTP In Details](https://tryhackme.com/room/httpindetail)
- **Category:** How The Web Works
- **Difficulty:** easy

---

# Overview

Room HTTP in Detail ini fokus ke pemahaman mendalam tentang protokol komunikasi paling mendasar di internet: **Hypertext Transfer Protocol (HTTP).** Di sini kita belajar gimana data ditransmisikan antara browser dan web server, serta ngerti anatomi di balik setiap permintaan yang kita bikin di web.

### What is HTTP ? (Hypertext Transfer Protocol)

HTTP itu protokol yang kepake setiap kali kita buka sebuah situs web. Protokol ini dikembangin sama Tim Berners-Lee dan timnya antara tahun 1989-1991. HTTP itu aturan-aturan yang dipake buat berkomunikasi sama web server buat ngirim data halaman web, entah itu HTML, gambar, video, dan lainnya.

### What is HTTPS? (HyperText Transfer Protocol Secure)

HTTPS itu versi aman dari HTTP. Data di HTTPS dienkripsi jadi nggak cuma mencegah orang ngintip data yang kita terima dan kirim, tapi juga ngasih jaminan bahwa kita lagi ngunjungin server web yang bener, bukan yang nyamar. Jadi, HTTPS bantu melindungi integritas dan kerahasiaan data kita waktu berkomunikasi di internet.

Waktu kita akses sebuah situs web, browser perlu bikin _request_ ke web server buat dapetin aset kayak HTML, gambar, dll. Sebelum itu, kita perlu kasih tau browser secara spesifik gimana dan di mana ngakses sumber daya ini. Di sinilah URL (Uniform Resource Locator) berperan penting.

### What is A URL ? (Uniform Resource Locator)

URL itu alamat yang dipake buat ngidentifikasi dan ngakses sumber daya di internet. URL kasih tau browser gimana dan di mana nemuin halaman web atau file tertentu. Sebuah URL biasanya terdiri dari beberapa bagian, termasuk protokol (kayak HTTP atau HTTPS), nama domain, dan jalur ke sumber daya spesifik. Contohnya, di URL `https://www.domain.com/index.html`, `https` itu protokolnya, `www.domain.com` itu nama domainnya, dan `/index.html` itu jalur ke file yang diminta.

Berikut fitur-fitur yang ada di URL:

<p align="center">

![URL](../../Assets/Images/URL.png)

</p>

-

- **Scheme:** Ini nentuin protokol apa yang dipake buat ngakses sumber daya, kayak HTTP, HTTPS, FTP (File Transfer Protocol).

- **User:** Beberapa layanan butuh autentikasi buat login, kita bisa masukin username dan password ke dalam URL buat login.

- **Host:** Nama domain atau alamat IP server yang mau diakses.

- **Port:** Port yang bakal disambungin, biasanya 80 buat HTTP dan 443 buat HTTPS, tapi bisa di-host di port mana aja antara 1 - 65535.

- **Path:** Nama file atau lokasi sumber daya yang kita coba akses.

- **Query String:** Info tambahan yang bisa dikirim ke jalur yang diminta. Misalnya, /blog?id=1 bakal ngasih tau jalur blog bahwa kita mau nerima artikel blog dengan id 1.

### HTTP Request & Response

Contoh Request:

```http

GET /index.html HTTP/1.1
Host: domain.com
User-Agent: Mozilla/5.0 (Arch Linux)
Accept: text/html
```

- **Line 1:** Berisi Method `(GET)`, Path target `(/index.html)`, dan versi protokol `(HTTP/1.1)`

- **Line 2:** Header `Host` buat nentuin alamat server tujuan

- **Line 3:** `User-Agent` buat ngasih tau server kalau user lagi pake browser `Mozilla/5.0` di Arch Linux

- **Line 4:** `Accept` buat ngasih tau server tipe konten apa yang bisa diterima `(misal: text/html)`

Contoh Response:

```http
HTTP/1.1 200 OK
Date: Wed, 28 Jan 2026 19:40:00 GMT
Server: Dimm-Arch-Server/2.4
Content-Type: text/html
Content-Length: 173
Connection: keep-alive
Cache-Control: public, max-age=3600

<html>
<head>
    <title>Dimm HTTP Response</title>
</head>
<body>
    <h1>Welcome to Dimm Server !</h1>
    <p>Status: Online</p>
</body>
</html>
```

- **Line 1** `(HTTP/1.1 200 OK)`: Baris status utama. `HTTP/1.1` itu versi protokol, dan `200 OK` itu status code yang ngasih tau client bahwa permintaan berhasil diproses tanpa masalah.

- **Line 2** `(Date: Wed, 28 Jan 2026...)`: Nunjukin waktu (timestamp) kapan respon itu dibuat sama server.

- **Line 3** `(Server: Dimm-Arch-Server/2026.01)`: Ngasih tau client jenis dan versi web server yang dipake (dalam hal ini, server custom milikku di ekosistem Arch).

- **Line 4** `(Content-Type: text/html)`: Njelasin format data yang dikirim. Label `text/html` nginstruksiin browser buat nge-render isi body sebagai halaman web.

- **Line 5** `(Content-Length: 173)`: Ngasih tau ukuran payload data dalam satuan byte. Angka ini harus akurat biar browser tau kapan proses download data selesai.

- **Line 6** `(Connection: keep-alive)`: Instruksi biar koneksi antara browser dan server tetap terbuka buat permintaan selanjutnya, jadi akses terasa lebih cepet dan efisien.

- **Line 7** `(Cache-Control: public, max-age=3600)`: Ngatur kebijakan caching. Ini ngasih tau browser bahwa konten ini boleh disimpen di memori selama 3600 detik (1 jam).

- **Line 8** `(Blank Line)`: Baris kosong wajib yang jadi pemisah antara bagian headers (info administratif) sama bagian body (isi konten).

### HTTP Methods

HTTP method itu cara yang dipake client (kayak browser) buat berkomunikasi sama web server. Method ini nentuin jenis tindakan yang mau dilakuin ke sumber daya tertentu.

Beberapa HTTP method yang umum dipake:

1. **GET (Read):** Dipake buat minta atau ngambil data/informasi dari server.

2. **PUT (Create):** Dipake buat ngirim data baru ke web server dan bikin record baru.

3. **POST (Update):** Dipake buat mperbarui atau ngganti informasi yang udah ada di server.

4. **DELETE (Delete):** Dipake buat ngapus sumber daya atau informasi tertentu dari server secara permanen.

### HTTP Status Code

HTTP Status Codes itu kode berupa angka yang dikirim server sebagai bagian dari respons terhadap permintaan HTTP. Kode ini ngasih info tentang hasil dari permintaan yang dilakuin client.

Kode-kode ini dikelompokin ke dalam lima kategori berdasarkan angka pertamanya biar gampang diidentifikasi:

| Rentang     | Kategori                 | Deskripsi (TryHackMe Reference)                                                                                                     |
| :---------- | :----------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| **100-199** | **Information Response** | Ngasih tau client bahwa permintaan udah diterima dan mereka harus terus ngirim sisa permintaan. Kode ini udah jarang ditemuin.       |
| **200-299** | **Success**              | Nunjukin bahwa permintaan client udah berhasil diproses server.                                                                      |
| **300-399** | **Redirection**          | Dipake buat ngalihin permintaan client ke sumber daya lain, baik halaman web atau website yang beda.                                |
| **400-499** | **Client Errors**        | Ngasih tau client bahwa ada kesalahan di permintaan yang mereka kirim.                                                               |
| **500-599** | **Server Errors**        | Nandain masalah besar yang terjadi di sisi server waktu nangani permintaan.                                                          |

#### **Daftar Status Code Populer**

Beberapa status code yang wajib dipahami:

- **200 OK**: Permintaan berhasil diproses.
- **201 Created**: Berhasil bikin data baru di server (misalnya setelah registrasi akun).
- **301 Moved Permanently**: Halaman yang diminta udah pindah ke URL baru secara permanen.
- **401 Unauthorized**: Akses ditolak karena client belum login/autentikasi.
- **403 Forbidden**: Client nggak punya izin akses ke halaman itu meskipun udah login.
- **404 Not Found**: Server nggak bisa nemuin halaman atau file yang diminta.
- **500 Internal Server Error**: Terjadi kesalahan teknis umum di sisi server.
- **503 Service Unavailable**: Server lagi nggak tersedia karena kelebihan beban atau lagi maintenance.

### HTTP Headers

HTTP headers itu informasi tambahan yang dikirim bareng request atau respons HTTP. Headers ini ngasih konteks dan metadata tentang data yang lagi ditransfer.

### **Request Headers**

- **Host:** Header yang ngasih tau server nama domain yang diinginin client waktu bikin request, contohnya (`domain.com`)

- **User-Agent:** Header yang ngidentifikasi jenis perangkat, sistem operasi, dan aplikasi client yang dipake buat ngakses server.

- **Content-Length:** Ngasih tau server seberapa gede data yang dikirim (misal waktu ngisi formulir) biar server tau berapa banyak data yang harus diterima.

- **Accept-Encoding:** Ngasih tau server jenis kompresi apa yang didukung browser (kayak gzip) biar data bisa dikirim lebih cepet.

- **Cookie:** Data yang dikirim balik ke server buat bantu server nginget informasi client sebelumnya.

#### **Response Headers**

- **Set-Cookie:** Perintah buat browser supaya nyimpen cookie.

- **Cache-Control:** Durasi penyimpanan data di cache browser.

- **Content-Type:** Jenis format data yang dikembaliin (HTML, CSS, dll).

- **Content-Encoding:** Metode kompresi yang dipake server.

### Cookies

HTTP itu protokol **stateless**, artinya server nggak nginget permintaan sebelumnya dari client yang sama. Cookies diciptain buat ngatasi masalah ini dengan nyimpen informasi kecil di sisi browser client.

#### **Cara Kerja Cookies**

1. **Set-Cookie (Response):** Waktu login, server ngirim header `Set-Cookie` yang berisi ID unik (session token).
2. **Storage:** Browser bakal nyimpen token itu di memori lokal.
3. **Cookie (Request):** Setiap kali client buka halaman baru di situs yang sama, browser otomatis nyelipin header `Cookie` berisi token tadi buat konfirmasi kalau yang buka halaman masih orang yang sama.

#### **Kegunaan Utama Cookies**

- **Session Management:** Njaga status login atau isi keranjang belanja.
- **Personalization:** Nginget preferensi user (kayak Dark Mode atau bahasa).
- **Tracking:** Ngelacak aktivitas user buat kepentingan analitik atau iklan.
