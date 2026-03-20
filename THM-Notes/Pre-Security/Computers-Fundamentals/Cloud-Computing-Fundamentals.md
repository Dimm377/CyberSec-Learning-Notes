# TryHackMe: Cloud Computing Fundamentals

- **Room Link:** [Cloud Computing Fundamentals](https://tryhackme.com/room/cloudcomputingfundamentals)
- **Category:** Pre-Security
- **Difficulty:** Easy
 
---

## What is Cloud Computing?

Bayangkan kamu sudah selesai membuat aplikasi latihan *cyber security*. Aplikasinya berjalan di laptopmu sendiri, dan semuanya berjalan lancar, sampai temanmu dari negara lain mencoba mengaksesnya dan mengeluh *lemot*. Atau tiba-tiba seratus orang masuk bersamaan, dan aplikasinya langsung *down*. Atau lebih parahnya, laptopmu mati dan aplikasinya ikut tidak bisa diakses.

Masalah ini bukan karena kodemu jelek. Masalahnya adalah **infrastruktur**, kamu mengandalkan satu mesin fisik yang punya keterbatasan kapasitas, lokasi, dan ketersediaan.

**Cloud computing** hadir untuk menyelesaikan ketiga masalah itu sekaligus. Secara sederhana, cloud computing adalah model komputasi di mana kamu menggunakan sumber daya komputasi server, storage, jaringan milik penyedia layanan, yang bisa diakses lewat internet kapan saja, dari mana saja, dan bisa disesuaikan kapasitasnya sesuai kebutuhan.

> **for your information:** **Infrastruktur** dalam konteks ini merujuk pada komponen fisik dan virtual yang menjalankan sebuah sistem, server, storage, jaringan, dan sistem operasi yang mendasarinya.

Secara teknis, cloud dibangun di atas dua teknologi utama:

- **Virtualisation** — memungkinkan satu mesin fisik menjalankan banyak mesin virtual secara bersamaan, sehingga resource tidak terbuang sia-sia.
- **Containers** — unit software ringan yang membungkus aplikasi beserta semua dependensinya, sehingga bisa dibuat dan dihapus dengan sangat cepat.

Keduanya menjadi fondasi yang membuat cloud bisa bersifat fleksibel dan efisien.

---

## How Servers Evolved to Cloud

Cloud tidak muncul tiba-tiba. Ini adalah hasil dari bertahun-tahun evolusi cara bisnis mengelola server mereka. Setiap tahap muncul karena ada masalah nyata yang perlu dipecahkan.

| Era | Cara Kerja | Masalah Utama |
| :--- | :--- | :--- |
| **Physical Server** | Satu server fisik menjalankan satu aplikasi | Mahal, sering tidak terpakai penuh; jika server mati maka aplikasi ikut mati |
| **Virtualisation** | Satu server fisik menjalankan banyak server virtual | Lebih efisien, tapi pengelolaan infrastruktur masih menjadi tanggung jawab sendiri |
| **Cloud Computing** | Resource dibagi antar banyak pengguna, diakses lewat internet | Solusi dari semua masalah di atas — bayar sesuai pemakaian, scale sesuai kebutuhan |

---

## Cloud Benefits and Characteristics

Setelah memahami kenapa cloud muncul, mudah untuk melihat kenapa adopsinya begitu masif. Cloud dirancang langsung untuk menjawab keterbatasan infrastruktur tradisional.

| Karakteristik | Penjelasan |
| :--- | :--- |
| **Scalability** | Kapasitas bisa dinaikkan atau diturunkan sesuai beban, tanpa perlu membeli hardware baru. |
| **On-demand self-service** | Server dan storage bisa dibuat atau dihapus secara instan, tanpa menunggu proses pengadaan fisik. |
| **Pay only for what you use** | Biaya dihitung berdasarkan pemakaian aktual, bukan biaya tetap di muka. |
| **Security** | Penyedia cloud mengelola keamanan infrastruktur fisik dengan standar enterprise. |
| **High availability** | Sistem dirancang redundan, jika satu komponen gagal, komponen lain mengambil alih secara otomatis. |
| **Global access** | Aplikasi bisa diakses dari seluruh dunia dengan latensi rendah karena data center tersebar secara geografis. |

> **for your information:** **Redundan** berarti ada duplikasi komponen penting di dalam sistem, sehingga jika satu bagian gagal, bagian lain sudah siap mengambil alih tanpa mengganggu layanan.

---

## Types of Cloud

### Deployment Types

Cara kamu melakukan deploy cloud environment bergantung pada kebutuhan kontrol, keamanan, dan biaya.

#### Public Cloud

Infrastruktur dimiliki dan dikelola oleh penyedia layanan, lalu digunakan bersama oleh banyak organisasi berbeda. Kamu tidak perlu mengelola hardware sama sekali.

- Biaya paling rendah karena resource dipakai bersama (shared)
- Cocok untuk: startup, aplikasi web publik, workload yang butuh scale cepat
- Contoh penyedia: **AWS** (*Amazon Web Services*), **GCP** (*Google Cloud Platform*), **Azure**

#### Private Cloud

Infrastruktur dibangun dan dioperasikan khusus untuk satu organisasi. Bisa di-host secara internal atau oleh pihak ketiga, tapi tidak dibagi ke siapapun di luar organisasi tersebut.

- Kontrol dan keamanan lebih tinggi
- Cocok untuk: perbankan, pemerintahan, healthcare — industri yang menangani data sensitif dan punya regulasi ketat

#### Hybrid Cloud

Gabungan dari public dan private cloud yang saling terhubung. Organisasi bisa menyimpan data sensitif di private cloud, sementara memanfaatkan public cloud untuk menangani lonjakan traffic.

- Fleksibilitas tinggi
- Cocok untuk: platform e-commerce yang butuh keamanan data transaksi sekaligus kapasitas elastic saat traffic melonjak

---

### Service Models

Selain cara deploy, kamu juga bisa memilih seberapa besar tanggung jawab yang ingin kamu pegang. Ini disebut **service model** — dan ada tiga tingkatan utama.

Cara paling mudah untuk membedakan ketiganya adalah dengan analogi sewa properti.

#### IaaS — Infrastructure as a Service

Seperti menyewa lahan kosong. Kamu mendapat server virtual, storage, dan jaringan — tapi sistem operasi, middleware, dan aplikasinya kamu yang pasang dan kelola sendiri. Kontrol paling besar, tanggung jawab paling besar.

- Cocok untuk: tim yang butuh kontrol penuh atas environment mereka
- Contoh: **AWS EC2** (*Elastic Compute Cloud — layanan virtual server dari AWS*), Google Compute Engine

#### PaaS — Platform as a Service

Seperti menyewa rumah yang sudah lengkap dengan listrik dan air. Infrastruktur dan sistem operasi sudah dikelola oleh penyedia. Kamu hanya fokus membangun, men-deploy, dan menjalankan aplikasi.

- Cocok untuk: developer yang ingin fokus ke kode tanpa urusan server
- Contoh: **Heroku**, Google App Engine

#### SaaS — Software as a Service

Seperti tinggal di hotel. Semuanya sudah tersedia — kamu tinggal masuk dan pakai. Tidak ada yang perlu diinstal atau dikelola. Kamu mengakses software langsung lewat browser atau aplikasi.

- Cocok untuk: pengguna akhir yang butuh software siap pakai
- Contoh: **Gmail**, **Zoom**, **Google Docs**

**Tabel perbandingan tanggung jawab:**

| Yang Dikelola | IaaS | PaaS | SaaS |
| :--- | :---: | :---: | :---: |
| Hardware fisik | Penyedia | Penyedia | Penyedia |
| Jaringan & storage | Penyedia | Penyedia | Penyedia |
| Sistem operasi | **Kamu** | Penyedia | Penyedia |
| Runtime & middleware | **Kamu** | Penyedia | Penyedia |
| Aplikasi | **Kamu** | **Kamu** | Penyedia |
| Data | **Kamu** | **Kamu** | **Kamu** |

---

## Major Cloud Vendors

Ada banyak penyedia layanan cloud, tapi beberapa nama mendominasi pasar global.

| Provider | Keunggulan Utama |
| :--- | :--- |
| **AWS** (Amazon Web Services) | Pemimpin pasar dengan layanan paling lengkap dan jangkauan data center terluas di dunia. |
| **Microsoft Azure** | Kompetitor kuat di segmen enterprise dan hybrid cloud; terintegrasi erat dengan ekosistem Microsoft. |
| **GCP** (Google Cloud Platform) | Unggul di data analytics, machine learning, dan infrastruktur container. |
| **Alibaba Cloud** | Pemain dominan di Asia dengan harga yang kompetitif. |
| **IBM Cloud** | Fokus pada solusi hybrid cloud dan AI untuk kebutuhan enterprise. |
| **Oracle Cloud** | Spesialis untuk aplikasi dan database enterprise. |

AWS tetap menjadi pilihan paling umum karena skala infrastrukturnya dan dukungan untuk semua ukuran bisnis — dari startup hingga perusahaan Fortune 500.

---

## How Companies Are Using the Cloud

Untuk memahami dampak nyata cloud, lihat bagaimana perusahaan besar menggunakannya:

- **Netflix**: Menjalankan seluruh platform streaming-nya di AWS, sehingga mampu melayani jutaan pengguna secara bersamaan dan melakukan scale otomatis saat jam tayang sibuk.
- **Spotify**: Menggunakan cloud untuk mengelola jutaan lagu dan pengguna, serta melakukan deploy fitur baru dengan cepat tanpa gangguan layanan.
- **Instagram**: Mengandalkan cloud untuk menyimpan miliaran foto dan video, sekaligus mengantarkan konten tersebut ke pengguna di seluruh dunia dengan latensi rendah.
- **Platform e-commerce**: Memanfaatkan cloud untuk menghadapi lonjakan traffic ekstrem seperti saat Black Friday, tanpa harus membeli infrastruktur permanen yang mahal.

Pola yang sama berulang di semua kasus: cloud memungkinkan perusahaan fokus pada produk dan inovasi, bukan pada pengelolaan hardware.

---

## Pertanyaan Singkat

1. Apa perbedaan mendasar antara **IaaS**, **PaaS**, dan **SaaS** dari sisi tanggung jawab pengelolaan?
2. Kenapa sebuah perusahaan *healthcare* lebih memilih **private cloud** dibanding **public cloud**?
3. Apa yang dimaksud dengan ***high availability***, dan mekanisme apa yang membuatnya mungkin?
