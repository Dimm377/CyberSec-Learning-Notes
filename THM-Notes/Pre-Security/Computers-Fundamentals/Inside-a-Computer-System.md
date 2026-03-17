# TryHackMe: Inside a Computer System

- **Room Link:** [Inside a Computer System](https://tryhackme.com/room/insideacomputersystem)
- **Category:** Pre-Security
- **Difficulty:** Easy

---

## Introduction

Sebelum kamu terjun lebih jauh ke dunia cyber security, ada satu hal fundamental yang wajib kamu pahami: **apa yang sebenarnya sedang kamu lindungi?**

Bayangkan kamu adalah seorang kepala keamanan sebuah kerajaan mewah. Sebelum kamu memasang penjaga di pintu depan atau mengunci jendela, kamu harus tahu dulu seluk-beluk kerajaan tersebut. Di mana letak ruang harta karunnya? Di mana gudang makanannya? Siapa saja yang boleh masuk ke kamar sang komandan? Kalau kamu tidak tahu apa yang ada di kerajaanmu, mustahil kamu bisa melindunginya dengan efektif.

Sama halnya dengan keamanan komputer. Mencoba menjaga sistem yang tidak kamu pahami itu ibarat menjaga kerajaan yang belum pernah kamu lihat. Di room ini, kita akan membedah kerajaan digital kita, yaitu komputer.

### Learning Objectives

Setelah menyelesaikan materi ini, kamu akan mampu:
*   Mengenali berbagai komponen penyusun komputer.
*   Memahami fungsi masing-masing komponen tersebut.
*   Mengetahui bagaimana komponen-komponen ini saling berinteraksi untuk memberikan layanan kepada penggunanya.

Tenang saja, kita akan membahasnya pelan-pelan tanpa terlalu banyak bahasa teknis yang membingungkan. Fokus kita adalah membangun dasar yang kuat.

> **for your information:** 
> **End-to-end security** (Keamanan dari ujung ke ujung) dimulai dengan memahami setiap titik dalam sebuah sistem, termasuk perangkat kerasnya.

---

## Inside a Computer System

Hampir semua komputer yang kamu temui, mulai dari laptop, HP, sampai server canggih, dibangun menggunakan blok penyusun yang sama. Setiap bagian punya tugas spesifik, dan ketika bekerja sama, mereka membuat komputer itu hidup.

Supaya lebih mudah dipahami, kita akan menggunakan analogi **Tubuh Manusia**. Berikut ringkasan cepat sebelum kita bedah satu per satu:

| Komponen | Analogi Tubuh | Fungsi Utama |
| :--- | :--- | :--- |
| **Motherboard** | **Kerangka & Saraf** | Menghubungkan semua komponen. |
| **CPU** | **Otak** | Memproses semua instruksi. |
| **RAM** | **Memori Jangka Pendek** | Menyimpan data sementara agar cepat diakses. |
| **Storage (SSD/HDD)** | **Memori Jangka Panjang** | Menyimpan data secara permanen. |
| **Network Adapter** | **Pita Suara** | Berkomunikasi dengan sistem lain. |
| **PSU** | **Jantung & Paru-paru** | Menyuplai energi ke seluruh komponen. |
| **GPU** | **Korteks Visual** | Memproses dan menampilkan gambar. |
| **I/O Devices** | **Panca Indera & Anggota Tubuh** | Menerima input dan mengeluarkan output. |

---

### Motherboard — Kerangka & Sistem Saraf

Motherboard itu ibarat **kerangka tulang dan sistem saraf** tubuh kita. Dia menjadi tempat semua komponen menempel dan sekaligus menjadi jalur komunikasi antar bagian.

Pada motherboard desktop yang umum, kamu akan menemukan berbagai tempat (socket & slots) tempat komponen lain berkumpul:

| Komponen | Nama Teknis | Fungsi & Detail |
| :--- | :--- | :--- |
| **CPU Socket** | **CPU Socket** | Tempat Otak (prosesor) duduk. Dilengkapi tuas kecil untuk mengunci chip agar tidak goyang. |
| **RAM Slots** | **DIMM Slots** | Tempat Memori Jangka Pendek dipasang. Biasanya butuh **matching pairs** (pasangan yang sama) agar performa maksimal. |
| **Expansion Slots** | **PCI Express x16** | Slot panjang yang diperkuat (reinforced) untuk kartu dengan bandwidth tinggi seperti **GPU**. |
| **Expansion Slots** | **PCI Express x1 / x4** | Slot PCIe yang lebih pendek, biasanya digunakan untuk **Network Card**, capture card, atau kartu ekspansi lainnya. |
| **SATA Ports** | **SATA Connectors** | Konektor data tipis berbentuk huruf L untuk menghubungkan SSD 2.5" atau HDD melalui kabel SATA. |
| **Power Connector** | **24-pin ATX** | Konektor daya utama dari PSU yang menyuplai listrik ke seluruh motherboard dan komponennya. |
| **Rear I/O Ports** | **Back Panel** | Kumpulan pintu di bagian belakang untuk menghubungkan Keyboard, Mouse, Monitor, dan perangkat USB lainnya. |

Setiap komponen lain **menempel atau terhubung melalui** motherboard. Tanpa motherboard, komponen-komponen ini hanyalah suku cadang yang tidak bisa bekerja sama.

> **for your information:**
> **PCB** (_Printed Circuit Board_) — Motherboard pada dasarnya adalah papan sirkuit cetak raksasa. Jalur-jalur tembaga kecil di permukaannya yang disebut **bus** bertugas mengantarkan data dan listrik antar komponen.

---

### CPU — Otak Komputer

**CPU** (_Central Processing Unit_), sering disebut **prosesor**, adalah otak dari komputer. Sama seperti otak kita yang terus-menerus mengeksekusi instruksi (menambah angka, menuangkan susu ke mangkuk, dan sebagainya), CPU melakukan hal yang sama untuk komputer.

Beberapa hal penting tentang CPU:
*   **Multi-Core:** CPU modern punya beberapa *core* (inti prosesor) yang bisa menangani instruksi secara **paralel** (bersamaan). Semakin banyak core, semakin banyak tugas yang bisa dikerjakan sekaligus.
*   **Koneksi:** CPU terpasang di motherboard melalui **CPU Socket**, sebuah konektor khusus yang dirancang agar CPU bisa terhubung dengan aman.

> **Common Mistake:** Saat memasang CPU, perhatikan tanda segitiga kecil di sudut CPU dan socket-nya. Kalau salah posisi, pin CPU bisa bengkok dan rusak permanen.

---

### RAM — Memori Jangka Pendek

**RAM** (_Random Access Memory_) itu ibarat **memori kerja jangka pendek** otak kita. Saat kamu mengerjakan sebuah tugas, otak menyimpan informasi yang relevan secara sementara. RAM melakukan hal yang sama — menyimpan data yang sedang dibutuhkan CPU agar bisa diakses dengan sangat cepat.

Karakteristik penting RAM:
*   **Volatile (Mudah hilang):** Begitu komputer dimatikan atau kehilangan daya, semua isi RAM **langsung hilang**. Ini berbeda dengan penyimpanan permanen seperti SSD.
*   **Teknologi modern:** Modul RAM saat ini menggunakan teknologi seperti **DDR5** atau **DDR6** untuk kecepatan dan performa yang lebih tinggi.

> **Common Mistake:** Banyak pemula bingung membedakan RAM dan Storage. Ingat rumus sederhana ini:
> - **RAM** = meja kerja (semakin lebar, semakin banyak yang bisa dikerjakan sekaligus, tapi bersih saat kamu pulang).
> - **Storage** = lemari arsip (tempat menyimpan file secara permanen).

---

### Storage — Memori Jangka Panjang (SSD & HDD)

SSD dan HDD adalah perangkat penyimpanan yang ibarat **memori jangka panjang** kita. Sama seperti kenangan yang tersimpan secara permanen di otak, data disimpan secara jangka panjang di perangkat ini.

| Fitur | HDD (*Hard Disk Drive*) | SSD (*Solid State Drive*) |
| :--- | :--- | :--- |
| **Teknologi** | Piringan magnetik berputar (*moving parts*) | Chip memori flash (*no moving parts*) |
| **Kecepatan** | Lebih lambat | Jauh lebih cepat |
| **Ketahanan** | Rentan guncangan (karena ada bagian bergerak) | Tahan banting |
| **Harga** | Murah per GB | Lebih mahal per GB |
| **Kapasitas Populer** | 1TB - 4TB | 256GB - 2TB |
| **Koneksi** | **SATA cable** | **SATA cable** atau **PCI Express slot** |

HDD masih populer untuk menyimpan data besar yang jarang diakses (arsip, backup), sedangkan SSD jadi pilihan utama untuk OS dan aplikasi karena kecepatannya.

---

### Network Adapter — Pita Suara

Sama seperti kita menggunakan **pita suara** untuk berkomunikasi dengan lingkungan sekitar, **Network Adapter** (kartu jaringan) membuat komputer bisa berkomunikasi dengan sistem lain.

Dua varian utama:
*   **Wired (Kabel):** Menggunakan kabel Ethernet (RJ-45). Lebih stabil dan cepat.
*   **Wireless (Nirkabel):** Menggunakan sinyal Wi-Fi. Lebih fleksibel tapi bisa terganggu interferensi.

Seringkali network adapter sudah **tertanam langsung** di motherboard (*onboard*). Tapi kalau butuh performa lebih atau fitur khusus, kamu bisa menambahkan kartu jaringan terpisah lewat **PCI Express slot**.

---

### PSU — Jantung & Paru-paru

Setiap sistem butuh energi. Seperti **jantung** yang memompa darah ke seluruh organ, **PSU** (_Power Supply Unit_) menyuplai listrik ke semua komponen komputer.

PSU mengambil daya dari stopkontak (listrik AC) dan mengubahnya menjadi daya DC yang dibutuhkan komponen. Distribusinya lewat berbagai konektor:
*   **Main Motherboard Connector** (24-pin) — menyuplai daya ke motherboard.
*   **CPU Power Connector** (4/8-pin) — daya khusus untuk prosesor.
*   **Molex / SATA Power** — daya untuk storage dan perangkat lainnya.
*   **PCIe Power** (6/8-pin) — daya tambahan untuk GPU.

> **Common Mistake:** Memilih PSU yang watt-nya terlalu kecil untuk kebutuhan komponen adalah kesalahan fatal. Kalau komponen butuh daya lebih besar dari kapasitas PSU, sistem akan gagal menyala atau mati mendadak (*crash*). Selalu hitung kebutuhan daya total sebelum memilih PSU.

---

### GPU — Korteks Visual

**GPU** (_Graphics Processing Unit_) ibarat **korteks visual** otak kita. Mata menangkap informasi visual, lalu korteks visual memproses informasi tersebut menjadi gambar yang kita "lihat". GPU bekerja dengan cara serupa — menerima data dari sistem operasi dan program, lalu mengolahnya menjadi output visual yang ditampilkan ke monitor.

GPU terhubung ke motherboard melalui **PCI Express slot**. Untuk kebutuhan gaming, rendering 3D, atau bahkan *password cracking* di dunia cyber security, GPU yang powerful sangat dibutuhkan.

> **for your information:**
> Di dunia cyber security, GPU dipakai untuk **cracking password** menggunakan tool seperti **Hashcat**. GPU bisa memproses miliaran kombinasi hash per detik karena arsitekturnya yang dirancang untuk komputasi paralel masif.

---

### Input/Output (I/O) — Panca Indera & Anggota Tubuh

Sama seperti kita punya **panca indera** untuk menerima informasi dan **anggota tubuh** untuk bertindak, komputer punya perangkat **Input** dan **Output**.

**Input Devices** (menerima data dari pengguna):
*   Keyboard, Mouse, Microphone, Scanner

**Output Devices** (menampilkan hasil ke pengguna):
*   Monitor, Printer, Speaker

**Konektor umum** untuk perangkat I/O:
*   **USB** — konektor paling universal untuk hampir semua perangkat.
*   **HDMI** — untuk menghubungkan monitor atau TV (audio + video).
*   **DisplayPort** — alternatif HDMI, sering dipakai monitor gaming.

---

### Pertanyaan Singkat

*   Apa perbedaan utama antara **RAM** dan **Storage (SSD/HDD)** dalam hal ketahanan data saat listrik mati?
*   Mengapa kita harus berhati-hati saat memilih daya (**watt**) pada **PSU**?
*   Dalam analogi tubuh manusia, komponen mana yang bertindak sebagai **sistem saraf** yang menghubungkan semua bagian?

## What Happens When You Press the Start Button?
