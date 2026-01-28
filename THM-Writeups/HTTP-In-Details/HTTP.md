# TryHackMe: HTTP In Details

**Room Link:** [What Is Networking](https://tryhackme.com/room/whatisnetworking)
**Category:** How The Web Works
**Difficulty:** easy

# Overview

### What is HTTP ? (Hypertext Transfer Protocol)

HTTP adalah protokol yang digunakan setiap kali kita mengunjungi sebuah situs web, Protokol ini dikembangkan oleh Tim Berners-Lee dan timnya antara tahun 1989-1991. HTTP adalah serangkaian aturan yang digunakan untuk berkomunikasi dengan server web untuk mentransmisikan data halaman web, baik itu HTML, gambar, video, dan sebagainya.

### What is HTTPS? (HyperText Transfer Protocol Secure)

HTTPS adalah versi aman dari HTTP, data di HTTPS dienkripsi sehingga tidak hanya mencegah orang melihat data yang kita terima dan kirim, tetapi juga memberikan jaminan bahwa kita sedang mengunjungi server web yang benar dan bukan sesuatu yang menyamar. Dengan demikian, HTTPS membantu melindungi integritas dan kerahasiaan data Anda saat
berkomunikasi di internet

saat kita sedang mengakses sebuah situs web, browser perlu membuat request ke server web untuk mendapatkan aset seperti HTML, gambar, dll, Sebelum itu kita perlu memberi tahu browser secara spesifik bagaimana dan di mana mengakses sumber daya ini. Di sinilah URL (Uniform Resource Locator) berperan penting

### What is A URL ? (Uniform Resource Locator)

URL adalah alamat yang digunakan untuk mengidentifikasi dan mengakses sumber daya di internet, URL memberi tahu browser bagaimana dan di mana menemukan halaman web atau file tertentu, Sebuah URL biasanya terdiri dari beberapa bagian, termasuk protokol (seperti HTTP atau HTTPS), nama domain, dan jalur ke sumber daya spesifik. Contohnya, dalam URL `https://www.domain.com/index.html`, `https` adalah protokol, `www.domain.com` adalah nama domain, dan `/index.html` adalah jalur ke file yang diminta.

Berikut fitur-fitur yang ada di URL:

<p align="center">
<img src="../../Assets/Images/URL.png" width="400">
</p>

-

- **Scheme:** Ini menginstruksikan protokol apa yang digunakan untuk mengakses sumber daya seperti HTTP, HTTPS, FTP (File Transfer Protocol).

- **User:** Beberapa layanan memerlukan authentikasi untuk login, kita dapat memasukkan nama pengguna dan kata sandi ke dalam URL untuk login.

- **Host:** Nama domain atau alamat IP server yang ingin diakses.

- **Port:** Port yang akan disambungkan, biasanya 80 untuk HTTP dan 443 untuk HTTPS, tetapi ini dapat dihosting di port mana pun antara 1 - 65535.

- **path:** Nama file atau lokasi sumber daya yang kita coba akses
