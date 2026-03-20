# TryHackMe: Computer Types

- **Room Link:** [Computer Types](https://tryhackme.com/room/computertypes)
- **Category:** Pre-Security
- **Difficulty:** Easy
---

## Introduction

Ketika sebagian besar orang mendengar kata "komputer", yang terbayang adalah laptop atau desktop. Kenyataannya, komputer ada dalam bentuk yang jauh lebih beragam — mulai dari server yang melayani jutaan pengguna secara bersamaan, hingga chip kecil yang tertanam di dalam mesin cuci atau sistem kendali lift.

Untuk seorang praktisi cyber security, memahami jenis-jenis komputer ini bukan sekadar pengetahuan umum. Setiap jenis punya arsitektur, keterbatasan, dan permukaan serangan (_attack surface_) yang berbeda. Strategi pertahanan yang tepat untuk sebuah laptop tidak akan sama dengan strategi untuk server atau perangkat IoT.

> **for your information:** **Attack surface** adalah keseluruhan titik dalam sebuah sistem yang berpotensi bisa dieksploitasi oleh penyerang — semakin kompleks sistemnya, semakin luas attack surface-nya.

Setelah menyelesaikan room ini, kamu akan paham:

- Jenis-jenis komputer yang digunakan langsung setiap hari — laptop, desktop, workstation.
- Komputer yang bekerja di balik layar tanpa interaksi langsung — server, perangkat IoT, embedded systems.
- Kenapa setiap jenis dirancang berbeda dan apa implikasinya dari sisi keamanan.

---

## Personal Computers — Laptop, Desktop, and Workstation

Tiga jenis ini adalah komputer yang paling sering berinteraksi langsung dengan pengguna. Ketiganya punya layar dan keyboard, tapi dirancang untuk kebutuhan yang berbeda.

### Laptop

Laptop dirancang untuk mobilitas — seluruh komponennya dikemas dalam satu unit yang bisa dibawa kemana saja. Konsekuensinya ada di dua hal:

- **Performa terbatas** — komponen laptop harus hemat daya karena bergantung pada baterai. CPU dan GPU laptop umumnya tidak sekuat versi desktop dengan harga yang sama.
- **Pendinginan terbatas** — ruang yang sempit membuat sistem pendingin tidak bisa bekerja seoptimal desktop. Jika dipaksa menjalankan beban kerja berat secara terus-menerus, suhu akan naik dan performa akan turun secara otomatis (_thermal throttling_).

Dari sisi keamanan, laptop punya risiko fisik yang tidak dimiliki desktop: **mudah hilang atau dicuri**. Ini membuat enkripsi disk menjadi kebutuhan utama, bukan opsional.

> **for your information:** **BitLocker** adalah fitur enkripsi disk bawaan Windows yang mengenkripsi seluruh isi drive sehingga data tidak bisa dibaca tanpa kunci dekripsi yang benar. **FileVault** adalah padanannya di macOS. **Thermal throttling** adalah mekanisme otomatis di mana prosesor menurunkan kecepatannya sendiri untuk mencegah kerusakan akibat panas berlebih.

### Desktop

Desktop beroperasi dari satu lokasi tetap dan mendapat daya langsung dari stopkontak — tidak bergantung pada baterai. Ini memberi dua keunggulan utama dibanding laptop:

- **Performa lebih konsisten** — tidak ada keterbatasan daya baterai yang mempengaruhi kecepatan prosesor.
- **Pendinginan lebih efektif** — ruang yang lebih besar memungkinkan sistem pendingin bekerja lebih baik, sehingga komponen bisa berjalan di kecepatan penuh lebih lama.

Desktop juga lebih mudah di-upgrade — RAM, storage, dan GPU bisa diganti atau ditambah kapasitasnya tanpa mengganti seluruh unit.

### Workstation

Workstation adalah komputer yang dirancang untuk beban kerja profesional yang membutuhkan presisi tinggi dan keandalan konsisten — seperti simulasi 3D, rendering video, analisis data besar, atau reverse engineering malware.

Secara fisik mirip desktop, tapi menggunakan komponen dengan spesifikasi lebih ketat:

- **ECC Memory** (_Error-Correcting Code Memory_) — jenis RAM khusus yang secara otomatis mendeteksi dan memperbaiki error data selama pemrosesan. Krusial untuk kalkulasi yang tidak boleh ada kesalahan sekecil apapun.
- **Prosesor kelas server** — seperti Intel Xeon atau AMD EPYC, yang dioptimalkan untuk stabilitas jangka panjang di bawah beban kerja berat.

> **for your information:** **Reverse engineering** dalam konteks malware analysis adalah proses membongkar dan menganalisis kode program berbahaya untuk memahami cara kerjanya, tanpa memiliki akses ke source code aslinya.

---

## Server

Server adalah komputer yang dirancang khusus untuk **melayani permintaan dari komputer lain melalui jaringan** — bukan untuk dioperasikan secara langsung oleh satu pengguna. Server menjalankan layanan seperti website, database, email, atau file storage yang bisa diakses oleh banyak pengguna secara bersamaan.

Beberapa karakteristik yang membedakan server dari PC biasa:

- **Tidak selalu punya monitor atau keyboard** — server dikelola dari jarak jauh melalui jaringan, bukan dengan duduk di depannya.
- **Dirancang untuk uptime tinggi** — server diharapkan berjalan terus-menerus tanpa gangguan. Komponen server dipilih berdasarkan keandalan, bukan sekadar performa puncak.
- **Redundansi komponen** — server enterprise sering punya dua PSU, beberapa disk yang dikonfigurasi dalam **RAID**, dan koneksi jaringan ganda. Jika satu komponen gagal, komponen cadangan langsung mengambil alih.

> **for your information:** **Uptime** adalah durasi waktu sebuah sistem beroperasi tanpa gangguan atau restart. Server kelas enterprise menargetkan uptime 99.9% atau lebih — artinya downtime yang diizinkan kurang dari 9 jam per tahun. **RAID** (_Redundant Array of Independent Disks_) adalah konfigurasi beberapa disk yang bekerja bersama untuk meningkatkan performa atau keandalan data.

> **Common Mistake:** Server tidak harus berupa mesin raksasa di ruangan khusus. Secara software, komputer manapun bisa dikonfigurasi sebagai server. Yang membedakan server kelas enterprise adalah komponen dan arsitekturnya yang dirancang khusus untuk keandalan dan beban kerja tinggi secara terus-menerus.

Dari sisi keamanan, server adalah target bernilai tinggi. Kompromi pada satu server bisa berarti akses ke data ribuan atau jutaan pengguna sekaligus — jauh lebih menguntungkan bagi penyerang dibanding menargetkan satu laptop.

---

## Smartphones and Tablets

Smartphone adalah komputer seukuran saku yang dioptimalkan untuk dua hal: **efisiensi daya** dan **konektivitas**. Di dalamnya ada prosesor, RAM, storage, GPS, kamera, dan berbagai sensor lain — semua dikemas dalam form factor yang muat di saku.

Tablet pada dasarnya adalah smartphone dengan layar lebih besar, umumnya dioptimalkan untuk konsumsi konten dan produktivitas ringan.

Dari perspektif keamanan, smartphone menyimpan data yang sangat sensitif: lokasi real-time, riwayat komunikasi, kredensial aplikasi perbankan, hingga data biometrik. Ini menjadikannya target yang sangat menarik.

---

## IoT Devices

**IoT** (_Internet of Things_) merujuk pada perangkat fisik yang ditanamkan kemampuan komputasi dan konektivitas jaringan untuk mengirim atau menerima data — bukan untuk dioperasikan secara interaktif oleh pengguna.

Contoh konkret: termostat pintar, kamera keamanan, lampu yang bisa dikontrol lewat aplikasi, smart doorbell, hingga peralatan medis yang terhubung ke jaringan rumah sakit.

Karakteristik utama perangkat IoT:

- **Terhubung ke jaringan** — inilah yang membedakannya dari embedded system biasa.
- **Tujuan spesifik** — tidak dirancang untuk menjalankan aplikasi sembarangan.
- **Sumber daya terbatas** — prosesor dan memorinya kecil, sehingga tidak bisa menjalankan mekanisme keamanan yang kompleks.

Kombinasi ketiga karakteristik ini menciptakan masalah keamanan yang serius. Perangkat IoT sering kali:

- Menggunakan **kredensial default** (username dan password bawaan pabrik) yang tidak pernah diganti oleh pengguna.
- Tidak mendapat update firmware secara rutin.
- Tidak dimonitor aktivitas jaringannya.

Ini menjadikan IoT sebagai titik masuk yang sering dieksploitasi penyerang untuk menembus jaringan yang lebih besar.

> **for your information:** **Firmware** adalah software tingkat rendah yang tertanam di dalam perangkat keras dan mengendalikan operasi dasarnya. Berbeda dengan aplikasi biasa, firmware tidak bisa diupdate semudah menginstall ulang program.

**IoT sebagai vektor serangan** — penyerang yang berhasil mengkompromis satu perangkat IoT di jaringan rumah atau kantor bisa menggunakannya sebagai titik pivot untuk bergerak ke perangkat lain di jaringan yang sama. Perangkat IoT yang terinfeksi juga bisa dikumpulkan dalam jumlah besar dan dikendalikan sebagai **botnet** untuk melancarkan serangan **DDoS**.

> **for your information:** **Botnet** adalah kumpulan perangkat yang terinfeksi malware dan dikendalikan dari jarak jauh oleh penyerang tanpa sepengetahuan pemiliknya. **DDoS** (_Distributed Denial of Service_) adalah serangan yang membanjiri server atau jaringan target dengan traffic dalam jumlah besar secara bersamaan hingga sistem target tidak bisa merespons permintaan normal.

---

## Embedded Systems

**Embedded system** adalah komputer yang ditanamkan di dalam perangkat lain sebagai bagian dari fungsinya — bukan sebagai perangkat komputasi mandiri yang bisa diprogram ulang secara bebas.

Berbeda dengan IoT, embedded system umumnya **tidak terhubung ke jaringan**. Mereka menjalankan satu fungsi spesifik secara terus-menerus: kontroler di dalam mesin cuci, sistem manajemen bahan bakar di kendaraan, sensor di lift, atau antarmuka di mesin ATM.

Karakteristik embedded system:

- **Sumber daya sangat terbatas** — prosesor dan memori diminimalkan sesuai kebutuhan tugas.
- **Real-time operation** — banyak embedded system harus merespons input dalam batas waktu yang sangat ketat.
- **Masa pakai panjang** — embedded system sering berjalan bertahun-tahun tanpa pernah di-update atau di-restart.

Masa pakai yang panjang tanpa update inilah yang menciptakan risiko keamanan. Kerentanan yang ditemukan bertahun-tahun setelah perangkat di-deploy sering tidak bisa ditambal karena tidak ada mekanisme update, atau karena menghentikan perangkat untuk update bukan pilihan yang praktis.

---

## Why Computers Come in Different Designs

Tidak ada satu desain komputer yang bisa optimal untuk semua kebutuhan. Setiap desain adalah hasil dari serangkaian **trade-off** — keputusan untuk memprioritaskan satu aspek dengan mengorbankan aspek lain.

| Trade-off | Penjelasan |
| :--- | :--- |
| **Mobilitas vs Performa** | Komputer yang portable harus menggunakan komponen hemat daya, yang berarti performa lebih rendah dibanding komputer desktop dengan harga setara. |
| **Reliabilitas vs Biaya** | Sistem yang sangat andal seperti server enterprise membutuhkan komponen redundan dan pengujian ketat — semua ini menambah biaya secara signifikan. |
| **Spesialisasi vs Fleksibilitas** | Perangkat yang fokus pada satu tugas (IoT, embedded) bisa sangat efisien dan hemat daya, tapi tidak bisa dipakai untuk tugas lain di luar fungsinya. |
| **Keamanan vs Kemudahan** | Sistem yang sangat aman cenderung lebih sulit digunakan — lebih banyak autentikasi, lebih banyak pembatasan. Sistem yang mudah digunakan biasanya punya permukaan serangan lebih luas. |

Memahami trade-off ini penting karena keputusan desain yang dibuat oleh produsen secara langsung menentukan **di mana titik lemah sebuah sistem** dan **strategi pertahanan apa yang paling relevan**.

---

## Quick Review

- Apa yang membuat server menjadi target bernilai tinggi dibanding laptop biasa dari perspektif penyerang?
- Kenapa perangkat IoT sering menjadi titik masuk yang dieksploitasi dalam serangan jaringan?
- Jelaskan perbedaan mendasar antara IoT device dan embedded system dari sisi konektivitas dan fungsinya.
