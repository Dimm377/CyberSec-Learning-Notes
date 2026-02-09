# TryHackMe: How The Web Works


---

- **Room Link:** [Putting It All Together](https://tryhackme.com/room/puttingitalltogether)
- **Category:** How The Web Works
- **Difficulty:** easy

---

### Putting It All Together

Room ini merangkum seluruh proses komunikasi web, mulai dari permintaan pengguna hingga pengiriman konten dari server.

#### **1. Komponen Infrastruktur Tambahan**

Selain Client dan Server, terdapat komponen penting lainnya:

- **Load Balancer:** Mendistribusikan traffic ke beberapa server dan melakukan **Health Check**.
- **CDN (Content Delivery Networks):** Mempercepat akses dengan mengirimkan file statis dari lokasi server terdekat.
- **WAF (Web Application Firewall):** Melindungi web server dari serangan cyber (Hacking/DoS).
- **Database:** Menyimpan data dinamis yang diproses oleh aplikasi backend.

#### **2. Mekanisme Web Server**

Web server menggunakan **Virtual Hosts** untuk menjalankan beberapa situs web pada satu server fisik yang sama dengan membedakan permintaan berdasarkan nama domain.

- **Static vs Dynamic Content:**
  - **Static:** File yang dikirim langsung apa adanya (HTML, CSS, Gambar).
  - **Dynamic:** Konten yang bisa berubah-ubah tergantung permintaan user (hasil pencarian, profil user), biasanya diproses oleh aplikasi backend.

#### **3. Alur Permintaan Website**

Secara garis besar, urutan prosesnya adalah:

1. Mencari alamat IP melalui DNS (Cache -> Recursive -> Root -> Authoritative).
2. Traffic difilter oleh **WAF**.
3. Permintaan diarahkan oleh **Load Balancer**.
4. Terhubung ke server melalui **Port 80/443**.
5. Server memproses permintaan dan berinteraksi dengan **Database**.

> **Note:** Memahami infrastruktur ini sangat penting bagi seorang pentester untuk mengetahui titik lemah mana yang bisa dieksploitasi, apakah itu di level DNS, WAF bypass, atau Database injection.
