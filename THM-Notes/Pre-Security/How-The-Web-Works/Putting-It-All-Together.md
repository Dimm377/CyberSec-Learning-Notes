# TryHackMe: How The Web Works


---

- **Room Link:** [Putting It All Together](https://tryhackme.com/room/puttingitalltogether)
- **Category:** How The Web Works
- **Difficulty:** easy

---

### Putting It All Together

Room ini merangkum seluruh proses komunikasi web, dari permintaan pengguna sampai pengiriman konten dari server.

#### **1. Komponen Infrastruktur Tambahan**

Selain Client dan Server, ada komponen penting lainnya:

- **Load Balancer:** Distribusiin traffic ke beberapa server dan ngelakuin **Health Check**.
- **CDN (Content Delivery Networks):** Membuat akses lebih cepat dengan mengirim file statis dari lokasi server terdekat.
- **WAF (Web Application Firewall):** Lindungin web server dari serangan cyber (Hacking/DoS).
- **Database:** Menyimpan data dinamis yang diproses sama aplikasi backend.

#### **2. Mekanisme Web Server**

Web server pakai **Virtual Hosts** buat ngejalanin beberapa situs web di satu server fisik yang sama, dengan ngebedain permintaan berdasarkan nama domain.

- **Static vs Dynamic Content:**
  - **Static:** File yang dikirim langsung apa adanya (HTML, CSS, Gambar).
  - **Dynamic:** Konten yang bisa berubah-ubah tergantung permintaan user (hasil pencarian, profil user), biasanya diproses sama aplikasi backend.

#### **3. Alur Permintaan Website**

Secara garis besar, urutan prosesnya:

1. Mencari alamat IP lewat DNS (Cache -> Recursive -> Root -> Authoritative).
2. Traffic difilter sama **WAF**.
3. Permintaan diarahin sama **Load Balancer**.
4. Terhubung ke server lewat **Port 80/443**.
5. Server memproses permintaan dan berinteraksi sama **Database**.

> **Note:** Mengerti infrastruktur ini penting banget buat pentester, agar tau titik lemah mana yang bisa dieksploitasi -- apakah itu di level DNS, WAF bypass, atau Database injection.
