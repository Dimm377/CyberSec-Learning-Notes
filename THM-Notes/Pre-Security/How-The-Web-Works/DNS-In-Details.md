# TryHackMe: DNS In Details


---

- **Room Link:** [DNS In Details](https://tryhackme.com/room/dnsindetail)
- **Category:** How The Web Works
- **Difficulty:** easy

---

# Overview

### What is DNS ?

DNS (Domain Name System) memberikan cara sederhana bagi kita untuk berkomunikasi dengan perangkat di internet tanpa mengingat angka-angka kompleks

### Domain Hierarchy

Domain Hierarchy merupakan sistem penamaan terstruktur yang menyerupai pohon terbalik, setiap tingkatan dipisahkan dengan tanda (`.`) dan dibaca dari kanan ke kiri, memastikan bahwa setiap alamat di internet bersifat unik dan dapat dikelola secara terdistribusi

struktur Domain Hierarchy:

<p align="center">
![DNS](../../Assets/Images/DNS.png)
</p>

### 1. Root Domain

Root domain adalah tingkat tertinggi dalam sistem domain hierarchy, diwakili oleh tanda titik (`.`) dan berada di puncak nama domain, contoh `domain.com`, `.` adalah root domain ,Root domain sangat penting untuk resolusi DNS, karena membantu menavigasi seluruh struktur domain

### 2. TLD (Top Level Domain)

TLD adalah bagian paling kanan dari nama domain, contoh `domain.com`, tepat setelah titik terakhir `.com`,

TLD dibagi menjadi dua kategori utama:

1. **gTLID (Generic Top Level Domain):** Digunakan untuk memberitahu pengguna tentang tujuan atau bidang situs tersebut

Contoh:

- **.com:** Digunakan untuk tujuan komersial (Commercial)

- **.org:** Digunakan untuk situs organisasi (Organitation)

- **.edu:** Digunakan untuk instansi pendidikan (Education)

- **.gov:** Digunakan untuk instansi pemerintah (Goverment)

2. **ccTLD (Country Code Top Level Domain):** Digunakan untuk menunjukkan lokasi geografis atau asal negara dari sebuah situs (domain)

contoh:

- **.co.uk:** Untuk situs yang berbasis di Inggris (United Kingdom)

- **.id:** Untuk situs negaraku yaitu indonesia

### Nama domain di era Modern (sekarang)

- .online

- .club

- .biz

- dan banyak

### DNS Record Types

DNS menggunakan berbagai tipe record untuk menyimpan data spesifik. Berikut adalah daftar informasi yang paling umum ditemukan

- **A Record:** Catatan ini ditetapkan ke alamat IPv4, misalnya 104.26.10.229

- **AAAA Record:** Catatan ini ditetapkan ke alamat IPv6, misalnya 2606:4700:20::681a:be5

- **CNAME Record:** CNAME (Canonical Name) adalah tipe resource record yang berfungsi untuk memetakan sebuah nama alias (nama samaran) ke nama domain lain yang bersifat canonical (nama asli). Berbeda dengan A Record yang langsung mengarah pada alamat IP, CNAME mengarahkan query ke entitas domain lainnya

- **MX Record:** MX (Mail Exchange) Record adalah tipe catatan sumber daya yang berfungsi untuk menetapkan server pengiriman surat (mail server) yang bertanggung jawab atas penerimaan email untuk sebuah domain

- **TXT Record:** (Text) Record adalah tipe DNS yang memungkinkan pemilik domain untuk memasukkan informasi berbasis teks ke dalam sistem DNS, Berbeda dengan catatan DNS lainnya yang memiliki struktur kaku untuk alamat IP atau server, TXT record merupakan kolom teks bebas yang sangat fleksibel untuk berbagai keperluan administratif dan keamanan.

### Making A Request

Proses ini adalah urutan langkah yang dilakukan oleh sistem untuk menerjemahkan nama domain menjadi IP Address, melibatkan koordinasi antara beberapa server DNS yang berbeda

1. **Local Cache Check:** Sebelum melakukan permintaan ke jaringan luar, sistem akan melakukan pemeriksaan internal, Ini adalah mekanisme pertahanan pertama untuk meminimalkan latensi dan beban lalu lintas internet

2. **Recursive Resolver:** Jika informasi tidak ditemukan secara lokal, tugas pencarian diserahkan kepada Recursive Resolver. Ini adalah server yang bertindak sebagai perantara bagi client

3. **The Root Server:** Root Name Server merupakan infrastruktur yang berfungsi sebagai tulang punggung (backbone) dari seluruh sistem DNS di internet. Berada di puncak hierarki DNS, server ini menjadi otoritas pertama yang menangani permintaan jika informasi tidak ditemukan dalam cache.

4. **TLD (Top Level Domaim Server):** TLD Name Server berfungsi sebagai jembatan antara infrastruktur pusat (Root) dan server spesifik milik organisasi. Server ini bertanggung jawab untuk mengelola informasi mengenai semua domain yang berada di bawah satu ekstensi tertentu (seperti .com, .org, atau .id).

5. **Authoritative DNS Server:** Authoritative DNS server adalah server yang bertanggung jawab menyimpan DNS record untuk nama domain tertentu, Server ini menyimpan basis data asli dari berbagai tipe record (seperti A, MX, TXT). dan Segala bentuk perubahan, penambahan, atau penghapusan catatan DNS pada domain Anda hanya dapat dilakukan dan ditetapkan pada server otoritatif ini.

### TTL (Time To Live)

TTL adalah parameter numerik dalam setiap DNS Record yang menentukan durasi (dalam satuan detik) validitas informasi tersebut sebelum dianggap kedaluwarsa oleh sistem caching

Mekanisme kerja TTL:

1. **Penerimaan Data:** Saat Recursive Resolver mendapatkan jawaban dari Authoritative Server, jawaban tersebut datang bersama nilai TTL (misalnya: 3600, yang berarti 1 jam).

2. **Caching:** Resolver akan menyimpan data tersebut di memorinya dan mulai menghitung mundur dari 3600 ke 0.

3. **Penggunaan Cache:** Selama angka tersebut belum mencapai 0, jika ada orang lain yang menanyakan domain yang sama, Resolver akan langsung memberikan jawaban dari memorinya tanpa bertanya lagi ke server otoritatif.

4. **Refresh:** Begitu angka mencapai 0, data dianggap "basi". Resolver akan menghapus data tersebut dan melakukan proses pencarian ulang (Re-query) untuk memastikan alamat IP-nya masih sama atau sudah berubah.
