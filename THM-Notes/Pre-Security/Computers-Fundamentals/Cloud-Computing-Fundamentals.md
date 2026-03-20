# TryHackMe: Cloud Computing Fundamentals

- **Room Link:** [Cloud Computing Fundamentals](https://tryhackme.com/room/cloudcomputingfundamentals)
- **Category:** Pre-Security
- **Difficulty:** Easy

---

## Introduction

Bayangkan kamu punya ide brilian: yaitu membuat aplikasi latihan *cyber security* buat teman seangkatan. Awalnya lancar saat hanya 2-3 orang yang mengakses lewat laptop pribadimu. Tapi, apa jadinya saat aplikasi itu mulai viral?

Di sinilah hambatannya mulai terasa. Temanmu yang lagi di luar negeri pasti mengeluh *lag* parah karena data harus bolak-balik ke laptopmu yang jauh di sana. Belum lagi kalau 100 orang masuk secara bersamaan, laptopmu yang kentang itu pasti bakal panas, *hang*, terus *crash* gara-gara nggak kuat menampung beban sebanyak itu. Dan masalah paling simpel: kalau laptopmu mati atau internet rumahmu putus, ya aplikasinya ikutan mati.

Inilah kenapa ide brilian seringkali terhambat oleh hardware fisik yang terbatas. **Cloud Computing** hadir buat memecahkan masalah ini.

Gampangnya, *Cloud Computing* itu kita kayak menyewa tenaga komputer (CPU, RAM, storage) lewat internet. Jadi, alih-alih pusing mengurusi hardware di kamar, kamu tinggal memakai infrastruktur raksasa milik penyedia layanan cloud.

### What is Cloud Computing?

Cloud itu sebenarnya **hasil pengembangan lanjut** dari teknologi virtualisasi dan kontainer yang sudah kita bahas di [Virtualisation Basics](Virtualisation-Basics.md). Berkat teknologi itu, ribuan aplikasi bisa berjalan bersamaan di hardware yang sama tanpa saling mengganggu. Kita pun bisa menambah atau mengganti kapasitas server cuma dalam hitungan detik.

Dunia IT sudah melewati evolusi panjang sampai ke titik ini:
1.  **Physical Server**: Satu server fisik cuma buat satu aplikasi. Boros, mahal, dan kalau mati ya aplikasinya tamat.
2.  **Virtualisation**: Satu server fisik bisa menampung banyak server virtual. Lebih efisien, tapi kamu masih harus mengelola semuanya sendiri.
3.  **Cloud Computing**: Resource dibagi secara fleksibel lewat internet. Kamu bayar sesuai pemakaian dan bisa menambah kapasitas (*scale*) sesuka hati.

---

## Benefits of Cloud Computing

Pindah ke cloud bukan cuma soal mengikuti tren, tapi soal efisiensi nyata. Ada beberapa karakteristik utama yang bikin cloud jadi pilihan nomor satu perusahaan saat ini:

*   **Scalability**: Kalau websitemu tiba-tiba ramai (misal saat promo Black Friday), kamu bisa menambah server dengan gampang. Kalau sudah sepi, tinggal dikurangi lagi agar hemat biaya.
*   **On-demand**: Kamu bisa membuat atau menghapus server kapan saja tanpa perlu menunggu berminggu-minggu sampai hardware fisik datang.
*   **Pay as you go**: Tidak ada biaya besar di awal untuk beli hardware. Kamu cukup membayar apa yang kamu gunakan saja.
*   **High Availability**: Aplikasi tetap berjalan walaupun ada sebagian sistem yang gagal, karena datamu tersebar di berbagai infrastruktur yang tangguh.
*   **Global Access**: User dari belahan dunia mana pun bisa mengakses aplikasimu dengan cepat melalui server terdekat dari lokasi mereka.

---

## Deployment Models & Service Models

Cara perusahaan menggunakan cloud itu tidak semuanya sama. Ada yang mau berbagi, ada yang mau eksklusif. Ini yang disebut **Deployment Types**:

1.  **Public Cloud**: Infrastruktur yang dipakai bersama banyak orang/perusahaan lewat internet. Murah dan skalanya besar. Contoh: **AWS**, **Google Cloud**, **Azure**.
2.  **Private Cloud**: Infrastruktur yang dikhususkan hanya untuk satu organisasi saja. Kontrolnya lebih ketat dan lebih aman, biasanya dipakai oleh bank atau pemerintah.
3.  **Hybrid Cloud**: Gabungan keduanya. Data sensitif disimpan di Private Cloud, sementara traffic yang besar dan umum dilempar ke Public Cloud.

### Service Models: IaaS, PaaS, SaaS

Biasanya pemula bingung bedanya ketiga ini. Cara paling gampang memahaminya adalah dengan analogi **sewa tempat tinggal**:

*   **IaaS (Infrastructure as a Service)**: Seperti sewa **tanah kosong**. Kamu dapat server, storage, dan jaringan, tapi Sistem Operasi (Windows/Linux) dan aplikasinya harus kamu instal dan urus sendiri.
    *   *Contoh*: **AWS EC2**, Google Compute Engine.
*   **PaaS (Platform as a Service)**: Seperti sewa **rumah siap huni**. Infrastruktur dan OS sudah diurus pihak penyedia. Kamu tinggal fokus membangun dan menjalankan kodemu saja tanpa pusing urusan server.
    *   *Contoh*: **Heroku**, Google App Engine.
*   **SaaS (Software as a Service)**: Seperti tinggal di **hotel**. Semuanya sudah disediakan, kamu tinggal pakai lewat browser. Tidak perlu mengelola apa-apa.
    *   *Contoh*: **Gmail**, **Zoom**, **Spotify**.

---

## The Cloud Ecosystem

Di dunia nyata, hampir semua raksasa digital yang kamu pakai itu menggunakan layanan cloud.

*   **Netflix**: Menghosting seluruh platformnya di AWS agar bisa melayani jutaan penonton tanpa *down*.
*   **Spotify**: Menggunakan layanan cloud untuk menyimpan jutaan lagu dan melakukan analisis data user secara cepat.
*   **Instagram**: Menyimpan miliaran foto dan video agar bisa diakses cepat dari seluruh dunia.

Kenapa mereka mau bayar mahal ke cloud provider? Supaya mereka bisa fokus **mengembangkan produk**, bukan sibuk memperbaiki server yang rusak.

> **for your information:**
> **AWS (Amazon Web Services)** — Pemimpin pasar cloud saat ini karena jangkauan globalnya paling luas.
> **Scalability** — Kemampuan sistem untuk menambah atau mengurangi sumber daya sesuai kebutuhan beban kerja.
> **EC2 (Elastic Compute Cloud)** — Layanan server virtual dari AWS, salah satu contoh nyata dari model IaaS.

---

### Real-World Relevance

*   **Apakah teknik ini umum di pentest nyata?**
    Sangat umum. Hampir semua target pentest saat ini (kecuali internal network yang sangat jadul) ada di cloud. Misal, kamu harus tahu cara menemukan *misconfigured S3 Bucket* atau mencari celah di *serverless functions*.
*   **Variasi teknik ini di dunia nyata?**
    Sekarang ada tren **Multi-cloud**, di mana satu perusahaan pakai AWS dan Azure sekaligus untuk menghindari ketergantungan pada satu provider.
*   **Resiko jika sistem cloud tidak aman?**
    Kalau konfigurasinya salah, data jutaan user bisa bocor ke publik hanya karena satu "checkbox" keamanan yang lupa dicentang di dashboard cloud.

---

### Pertanyaan Singkat

1. Apa bedanya **High Availability** dengan **Scalability**?
2. Kalau kamu cuma mau pakai software lewat web tanpa mau pusing urusan server, model layanan apa yang kamu pilih?
3. Di antara AWS, Azure, dan GCP, mana yang saat ini menjadi pemimpin pasar global?
