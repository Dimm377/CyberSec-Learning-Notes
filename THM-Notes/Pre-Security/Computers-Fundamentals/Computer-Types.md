# TryHackMe: Computer Types

- **Room Link:** [Computer Types](https://tryhackme.com/room/computertypes)
- **Category:** Pre-Security
- **Difficulty:** Easy

## Introduction

Pernahkah kamu membayangkan kalau **kulkas** di rumahmu bisa jadi celah keamanan?

Cerita dimulai saat Sophia mencoba menyambungkan perangkat baru ke WiFi rumahnya dan menemukan sinyal aneh bernama "NexusCool Fridge X17". Awalnya dia tertawa, membayangkan apa bisa mendownload makanan dingin lewat WiFi. Tapi kemudian dia sadar: tetangganya baru saja membeli kulkas pintar, sebuah kulkas yang di dalamnya punya komputer dan bisa terhubung ke internet.

Ini membuat Sophia berpikir. Komputer bukan lagi sekadar laptop atau HP yang sering kita pegang. Mereka sudah ada di benda-benda di sekitar kita, mulai dari peralatan dapur sampai bel pintu. Sophia sering membaca berita tentang perangkat pintar yang bertingkah aneh atau diretas, tapi dia baru benar-benar merenungkan apa sebenarnya mesin-mesin ini.

Di dunia cyber security, memahami jenis-jenis komputer ini sangat krusial karena **setiap jenis punya cara kerja dan risiko yang berbeda.**

### Learning Objectives

Setelah menyelesaikan room ini, kamu akan paham:
*   Jenis-jenis komputer yang kamu pakai langsung setiap hari (seperti laptop dan smartphone).
*   Komputer-komputer yang bekerja di balik layar tanpa kamu sadari (server, perangkat IoT, *embedded systems*).
*   Kenapa setiap jenis komputer dirancang berbeda dan cocok untuk tugas yang berbeda pula.

> **for your information:**
> **IoT** (_Internet of Things_) — Jaringan benda-benda fisik (seperti kulkas, lampu, atau termostat) yang ditanami sensor, software, dan teknologi lainnya dengan tujuan untuk saling bertukar data dengan perangkat lain melalui internet.

## Sophia's Summer of Hidden Computers - Month 1

Selama bulan pertamanya magang di Nova Labs, Sophia belajar sebuah pelajaran penting:
1.  **Tidak semua komputer dibuat untuk berpindah tempat.**
2.  **Tidak semua komputer dibuat untuk diduduki di depannya oleh manusia secara langsung.**

Sophia dikenalkan dengan empat jenis komputer yang sekilas terlihat mirip, tapi punya tujuan yang sangat berbeda.

### The Computers You Sit in Front Of

Ada komputer yang dirancang untuk kamu duduki dan operasikan langsung (punya layar dan keyboard), dan ada yang bekerja diam-diam di balik layar. Berikut perbandingannya:

| Jenis Komputer | Layar & Keyboard? | Tujuan Utama |
| :--- | :--- | :--- |
| **Laptop** | Ya | Komputasi harian yang bisa dibawa kemana-mana (*portable*). |
| **Desktop** | Ya | Performa stabil di satu lokasi tetap. |
| **Workstation** | Ya | Tugas profesional yang butuh presisi dan reliabilitas tinggi. |
| **Server** | Tidak (Biasanya) | Menyediakan layanan ke banyak pengguna melalui jaringan. |

---

### Differences: PC, Workstation, and Server

Biar kamu tidak bingung, mari kita bedah bedanya berdasarkan beban kerja mereka:

#### 1. Laptop & Desktop (Personal Computers)
Ini komputer yang paling sering kita sentuh setiap hari. Perbedaan utamanya ada di **mobilitas**:
*   **Laptop**: Dirancang untuk dibawa kemana-mana. Cocok untuk email dan dokumen, tapi kalau dipaksa kerja berat terus-menerus, performanya akan menurun karena **pendinginan** di ruang sekecil itu memang sulit.
*   **Desktop**: Menang di **upgradability** dan **cooling** yang jauh lebih baik. Karena pakai daya langsung dari tembok (bukan baterai), desktop bisa lari kencang lebih lama dengan stabil.

#### 2. Workstation
Pikirkan ini sebagai Desktop versi flagship. Secara bentuk mirip desktop, tapi komponennya jauh lebih kuat (penjelasan tentang komponen ada di catatan [Inside a Computer System](Inside-a-Computer-System.md)). Workstation memprioritaskan **akurasi dan reliabilitas**. Biasanya pakai komponen khusus (seperti memori ECC) untuk mengurangi error saat melakukan perhitungan rumit seperti simulasi 3D atau *reverse engineering* malware.

#### 3. Server
Berbeda dengan tiga di atas, Server seringkali tidak punya monitor atau keyboard yang terpasang. Mereka dirancang untuk **menyala 24/7 tanpa henti**, melayani ribuan permintaan pengguna secara bersamaan. Kamu mungkin tidak pernah menyentuh server secara fisik, tapi dialah yang menjalankan semua *tools* dan web yang kamu pakai setiap hari. (Penjelasan lebih lengkap tentang bagaimana server melayani permintaan ada di catatan [Client-Server Basics](Client-Server-Basics.md))

> **Common Mistake:** Mengira server itu harus selalu berupa mesin raksasa di ruangan dingin. Secara software, komputer apapun bisa jadi server, tapi secara hardware, Server dirancang untuk menyala **24/7 tanpa henti** dan punya komponen yang sangat handal.

---

### Real-World Relevance

Kenapa kita harus peduli bedanya?
*   **Physical Security**: Laptop lebih rentan hilang/dicuri dibanding desktop. Maka, enkripsi disk (**BitLocker/FileVault**) jadi harga mati di laptop.
*   **Target Serangan**: Server adalah target utama (*high-value target*) karena menyimpan data banyak orang. Menyerang 1 laptop user mungkin cuma dapat 1 akun, tapi menjebol 1 server bisa dapat ribuan data user.
*   **Resource Exhaustion**: Workstation punya resource besar. Jika attacker berhasil masuk, mereka bisa menggunakan kekuatan prosesornya untuk melakukan *cryptojacking* (menambang crypto secara ilegal).

> **for your information:**
> **Uptime** — Durasi waktu sebuah sistem terus beroperasi tanpa gangguan atau *restart*. Server sangat mengejar angka *uptime* 99.9% atau lebih.

## Sophia's Summer of Hidden Computers - Month 2

Pada bulan keduanya, Sophia mulai menyadari keberadaan komputer yang selama ini tidak pernah dia sentuh secara langsung. Komputer paling kuat yang dimiliki kebanyakan orang saat ini muat di dalam saku, tapi jutaan komputer lainnya bersembunyi di balik benda sehari-hari: pintu, lampu, hingga mesin pembuat kopi.

### Hidden Computers in Everyday Objects

Ini dia jenis-jenis komputer yang sering ada di sekitar kita, tapi jarang kita anggap sebagai "komputer":

| Jenis | Apa Itu? | Contoh |
| :--- | :--- | :--- |
| **Smartphone** | Komputer seukuran saku yang dioptimalkan untuk daya tahan baterai dan konektivitas. | iPhone, Android phone |
| **Tablet** | Komputer berbasis layar sentuh dengan layar lebih lebar. | iPad, drawing tablet |
| **IoT Device** | Perangkat yang terhubung ke jaringan dengan satu tujuan spesifik. | Termostat pintar, bel pintu pintar (*smart doorbell*). |
| **Embedded Computer** | Komputer yang ditanamkan/dibangun di dalam perangkat lain. | Kontroler mesin kopi, sensor pintu otomatis. |

---

### IoT vs Embedded System

Banyak yang bingung membedakan keduanya karena sama-sama kecil dan punya tugas tunggal. Kunci perbedaannya ada pada **Konektivitas**:

*   **IoT** (_Internet of Things_): Terhubung ke jaringan (internet/lokal) untuk mengirim data atau menerima perintah. Contoh: Lampu pintar yang bisa kamu matikan lewat HP.
*   **Embedded System**: Biasanya tidak terhubung ke apa pun. Mereka melakukan tugasnya di dalam mesin, seringkali selama bertahun-tahun tanpa ada yang tahu mereka ada di sana.

### Real-Life Examples
Sophia melewati pintu otomatis setiap hari di laboratorium. Dia baru sadar ada chip kecil di dalam bingkai pintu yang bertugas mendeteksi gerakannya dan memberi sinyal ke motor untuk membuka pintu. Itulah *embedded computing* — tidak terlihat, handal, dan ada di mana-mana.

---

### Role in Cyber Security

Jangan remehkan perangkat kecil ini. Di perspektif seorang *attacker*:
1.  **IoT sebagai Pintu Masuk**: Perangkat IoT sering kali punya keamanan yang lemah (misal: password default yang tidak diganti). Hacker bisa meretas lampu pintar untuk masuk ke jaringan WiFi utama rumah atau kantor.
2.  **Spying & Data Leak**: *Smart doorbell* atau *fitness tracker* menyimpan data privasi. Jika bocor, lokasi dan kebiasaan korban bisa terlacak.
3.  **Botnet**: Ribuan perangkat IoT yang terinfeksi bisa digabungkan menjadi "pasukan" (**botnet**) untuk melakukan serangan **DDoS** raksasa yang bisa merubuhkan website besar.

> **for your information:**
> **Botnet** — Kumpulan perangkat yang terhubung ke internet dan telah terinfeksi malware, sehingga bisa dikendalikan oleh penyerang dari jarak jauh tanpa sepengetahuan pemiliknya.

## Why Computers Come in Different Flavors

Mungkin kamu bertanya-tanya hal yang sama dengan Sophia: "Kenapa tidak buat satu komputer saja yang bisa melakukan semuanya?"

Jawabannya sederhana: **karena setiap desain adalah sebuah *trade-off*.**

Tidak ada komputer yang terbaik secara mutlak. Yang ada hanya **alat yang tepat untuk tugas yang tepat.** 

### Core Concept: The Trade-offs

Saat merancang komputer, selalu ada sesuatu yang harus dikorbankan untuk mendapatkan fitur yang lain:

1.  **Mobilitas vs Performa**: Komputer yang kecil dan bisa dibawa kemana-mana (seperti laptop atau smartphone) harus mengorbankan performa maksimal. Mereka tidak bisa bekerja terlalu berat dalam waktu lama karena baterainya terbatas dan sulit untuk didinginkan.
2.  **Reliabilitas vs Biaya**: Membuat sistem yang sangat handal (seperti server atau workstation) itu mahal. Mereka butuh komponen cadangan (*redundancy*), seperti PSU cadangan atau disk ekstra, agar jika satu bagian rusak, sistem tetap menyala.
3.  **Tujuan vs Fleksibilitas**: Perangkat IoT bekerja dalam diam tanpa butuh perhatianmu karena mereka fokus pada satu tugas. Laptopmu lebih fleksibel, tapi lebih rumit untuk dikelola.

---

### Real-World Relevance

Memahami *trade-off* ini membantu kita dalam memilih strategi pertahanan:
*   **Redundancy**: Di sistem kritis, kita harus memastikan tidak ada *Single Point of Failure* (satu titik kegagalan yang merubuhkan seluruh sistem).
*   **Specialization**: Saat melakukan *cracking* data dalam jumlah besar, jangan pakai laptop harianmu. Pakailah workstation atau server khusus yang didesain untuk beban kerja tersebut agar tidak merusak perangkatmu sendiri.

---

## Summary

Laporan akhir Sophia dimulai dengan kalimat: "Saya datang dengan pemikiran bahwa komputer itu harus punya layar dan keyboard. Saya pergi dengan kesadaran bahwa mereka ada di mana-mana, terutama di tempat-tempat yang tidak saya lihat."

Di room ini, kita telah membahas delapan jenis komputer dan mempelajari bagaimana keputusan dibuat saat memilih satu jenis di atas yang lain. Komputer yang paling bagus tidak selalu yang tercepat atau yang paling keren tampilannya. Kadang-kadang, mereka adalah chip-chip yang menjaga pintu tetap terbuka, pesawat tetap terbang, dan mesin kopi tetap menyeduh.

---

### Questions

*   Mengapa laptop tidak bisa memiliki performa yang sama kuatnya dengan server raksasa secara terus-menerus?
*   Apa yang dimaksud dengan **redundancy** pada sistem server?
*   Sebutkan satu contoh perangkat yang termasuk dalam kategori **embedded system** yang sering kamu temui!
