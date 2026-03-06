# TryHackMe: Wireshark: The Basics


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/wiresharkthebasics)
- **Category:** Networking / Analysis tool
- **Difficulty:** Easy

---

## Task 1: Introduction

**Wireshark** adalah alat analisis paket jaringan (*open-source*) yang paling populer di dunia. Ibarat sebuah **mikroskop** atau **mesin X-Ray** buat jaringan — dia membuat kita bisa melihat apa yang sebenarnya terjadi di dalam jaringan atau udara (Wi-Fi), membedah setiap bit data yang lewat.

**Kegunaan Utama:**
- **Sniffing:** Mengintip traffic yang sedang jalan secara *live*.
- **Investigating PCAP:** Membedah rekaman traffic (file `.pcap`) yang sudah diambil sebelumnya.

### Learning Objectives (Tujuan Kita):
Di room ini, kita bakal fokus menguasai:
1. Navigasi dan konfigurasi dasar Wireshark.
2. Membedah paket buat mencari informasi di setiap lapisan **TCP/IP**.
3. Cara pakai **Display Filters** buat memfilter data yang kita butuhkan.

---

## Tool Overview

### Use Cases (Kapan Kita Pakai Wireshark?)
Wireshark bisa dipakai buat banyak hal:
1. **Troubleshooting Jaringan:** Mendeteksi titik kegagalan (*failure points*) atau kemacetan (*congestion*) di jaringan.
2. **Mendeteksi Anomali Keamanan:** Mencari *host* mencurigakan, penggunaan *port* yang tidak biasa, atau *traffic* nakal.
3. **Mempelajari Protokol:** Menginvestigasi detail protokol seperti *response code* HTTP atau isi *payload* data.

> **Catatan Penting:** Wireshark **BUKAN** *Intrusion Detection System* (IDS). Dia tidak bisa mencegah serangan atau memberi peringatan otomatis. Dia hanya mengizinkan kita membedah paket secara mendalam dan dia **tidak memodifikasi** paket, cuma membacanya. Jadi, mendeteksi anomali itu bergantung penuh pada wawasan si analyst.

### GUI and Data
Tampilan antarmuka utama Wireshark dibagi menjadi 5 bagian penting:

| Bagian GUI | Penjelasan Fungsinya |
| ---------- | -------------------- |
| **Toolbar** | Menu utama yang berisi *shortcuts* untuk *packet sniffing*, *filtering*, menyortir, meringkas, mengekspor, atau menggabungkan file `.pcap`. |
| **Display Filter Bar** | Kolom utama untuk mengetik *query filter* (menyaring data yang lagi dilihat). |
| **Recent Files** | Daftar file yang barusan dibuka/dianalisis. Bisa diklik untuk buka lagi kalau ingin melanjutkan analisis. |
| **Capture Filter and Interfaces** | Tempat mengatur *capture filter* (filter yang jalan **sebelum** merekam) dan memilih capture interface (misal: antarmuka jaringan seperti `eth0`, `lo`, atau `ens33`). |
| **Status Bar** | Baris paling bawah yang menunjukkan status *tool*, profil, dan informasi nomor paket. |

Gambar dibawah menunjukkan tampilan utama Wireshark, juga bagian yang disorot sudah dijelaskan di atas:

<p>
<img src="../../Assets/Images/main-Wiresharkl.png" alt="Wireshark GUI" width="800px" />
</p>

### Loading a PCAP File

Sebelum membedah data, kita harus memasukkan file rekamannya (`.pcap` atau `.pcapng`). Caranya simple:
1. Pakai menu **File > Open**,
2. **Drag & drop** file langsung ke jendela Wireshark, atau
3. Cukup **klik ganda** di file `.pcap` yang mau di analisis.

Begitu file masuk, tampilan layar akan penuh dengan barisan data yang terlihat seperti bahasa *matrix*. Agar tidak *overwhelmed*, Kita cuma perlu fokus ke **3 jendela utama (*panes*)** ini. 

Ibarat kita lagi jadi detektif yang meriksa surat, 3 jendela ini mewakili kedalaman analisisnya:

| Jendela Utama (*Pane*) | Fungsi & Analogi Detektif |
| ---------------------- | ------------------------- |
| **1. Packet List** | **Ibarat Buku Tamu.** Ini adalah daftar panjang semua paket (mobil kurir) yang tertangkap. Di sini Kita bisa lihat rangkuman *high-level*: nomor urut, jam lewat (*Time*), siapa pengirim (*Source*), ke mana tujuannya (*Destination*), jalur yang dipakai (*Protocol*), dan info singkatnya. |
| **2. Packet Details** | **Ibarat Membongkar Isi Amplop Surat.** Kalau Kita klik salah satu baris di *Packet List*, jendela tengah ini bakal mengurai isi paketnya berlapis-lapis sesuai *OSI Layers*. Dari mulai bungkus luar (lapisan fisik/MAC) sampai ke inti jeroannya (protokol aplikasi seperti HTTP). |
| **3. Packet Bytes** | **Ibarat Melihat DNA / Atom dari Suratnya.** Ini adalah bentuk data paling mentah, Bagian ini menampilkan data paket dalam wujud *Hexadecimal* (angka *hex*) dan *ASCII* (teks yang bisa dibaca manusia). Terkadang Kita bisa nemu teks *password* atau *payload* tersembunyi bertebaran di sini. |

Di pojok pojok layar Wireshark yang lagi aktif, ada informasi penunjang tambahan:
- **File Name:** Nama file pcap yang lagi dibuka (ada di *tab browser* atas, dan *status bar* bawah kiri).
- **Total Packets:** Total jumlah baris catatan (jumlah paket) yang berhasil terekam (di *status bar* bawah kanan).

Biar kebayang gimana, liat gambar di bawah ini yang menyorot 3 jendela utama tadi:

<p>
<img src="../../Assets/Images/Pcap.png" alt="Wireshark PCAP Loading Interface" width="800px" />
</p>

---

### Colouring Packets

Pernah merhatiin kenapa baris-baris paket di Wireshark punya warna beda-beda? Itu bukan buat hiasan doang. Wireshark pakai sistem pewarnaan biar mata kita gampang menemukan masalah (anomali) atau protokol tertentu tanpa harus membaca detailnya satu per satu. 

Wireshark punya **Dua Jenis Metode Pewarnaan**:

| Jenis Pewarnaan | Karakteristik | Cara Akses |
| :--- | :--- | :--- |
| **Permanent Rules** | Aturan paten yang disimpen di profil lo. Aturan ini bakal tetep ada tiap kali lo buka Wireshark lagi di sesi berikutnya. | Klik menu **View > Coloring Rules** *(Bisa juga buat bikin rule custom sendiri)* |
| **Temporary Rules** | Aturan sementara yang cuma aktif selama sesi Wireshark. Kalau aplikasinya ditutup, warnanya ilang. | Klik kanan di paket **Colorize Conversation** (atau via menu **View > Conversation Filter**) |

Biar tampilan warnanya aktif (atau kalau mau matiin biar polos aja), bisa pakai opsi **View > Colorise Packet List**.

Coba cek penampakan *Coloring Rules Default* dari Wireshark di bawah ini:

<p>
<img src="../../Assets/Images/Coloring-filter.png" alt="Wireshark Default Coloring Rules" width="800px" />
</p>

---

### Traffic Sniffing

Selain membaca file rekaman (PCAP), keahlian utama Wireshark adalah kemampuannya untuk *sniffing* atau monitoring *traffic* jaringan secara *live* saat itu juga. 

Di bagian kiri *Toolbar* atas, ada 3 tombol penting buat ngendaliin *sniffing*:

- **Blue Shark Button (Start):** Mulai menangkap (*capturing*) *traffic* di jaringan.
- **Red Square Button (Stop):** Menghentikan proses penangkapan sementara jika sudah selesai analisis.
- **Green Arrow Button (Restart):** Mengulang proses *sniffing* dari awal lagi.

Saat *sniffing* lagi dijalankan, kita bisa cek bagian *Status Bar* (paling bawah layar) untuk memastikan dua hal:
1.  **Interface yang Dipakai:** (Contoh: `eth0`) Biar gak salah jalur yang di sniff.
2.  **Jumlah Paket:** (Contoh: `Packets: 122`) Menunjukkan seberapa banyak data yang udah kekumpul.

Biar lebih jelas letak tombol sama statusnya, lihat referensi GUI di bawah:

<p>
<img src="../../Assets/Images/Shark-Button.png" alt="Wireshark Sniffing Controls" width="800px" />
</p>

---

### Merge PCAP Files

Wireshark juga punya fitur untuk menggabungkan dua file `.pcap` yang berbeda jadi satu file. 

1. Pastiin udah buka satu file pcap utama di Wireshark.
2. Buka menu **File > Merge**.
3. Pilih file pcap kedua yang mau digabungin.
4. Wireshark bakal ngasih tau total paket (baris) dari file kedua yang dipilih. 
5. Klik **Open**, dan dua file itu bakal bersatu jadi satu _merged workspace_ (seperti *Untitled*).

**Ingat:** Karena hasil gabungannya masih *temporary*, **jangan lupa klik Save As** untuk menyimpan file *pcap* gabungannya sebelum mulai analisis.

### View File Details

Penting untuk tau identitas asli dari file pcap yang sedang di analisis, apalagi kalau sedang menangani banyak file pcap dari berbagai kasus yang beda. Di bagian *File Details* ini, kita bisa lihat info krusial dan statistik penting biar tau file mana yang harus diprioritasin:
- **File hash** (SHA256/MD5 buat bukti forensik biar tau filenya belum diubah)
- **Capture time** (Kapan traffic ini direkam)
- **Komentar file** (Catatan dari analis sebelumnya)
- **Interface & Statistics** (Berapa banyak paket dan byte data yang kerekam)

Ada **dua cara** cepat buat buka jendela detail ini:
1. Lewat menu atas: **Statistics > Capture File Properties**
2. Lewat *shortcut* GUI: Klik **icon pcap kecil di pojok kiri bawah** (sebelah nama file).

<p>
<img src="../../Assets/Images/View-details.png" alt="Wireshark View File Details" width="800px" />
</p>

---

## Packet Dissection

*Packet dissection* (bedah paket) itu intinya adalah menerjemahkan detail isi paket ke dalam bentuk protokol dan kolom-kolom yang gampang kita baca. Wireshark mendukung banyak protokol bawaan buat dibedah, dan bahkan bisa bikin *script* pembedah (*dissector scripts*) sendiri.

> **Note:** Kemampuan ini bergantung banget dengan seberapa paham kita soal konsep **OSI Model**. Jadi pastikan pondasi OSI Layer kita udah kuat agar membaca ini kerasa gampang.

### Packet Details (Membedah Lapis Demi Lapis)

Masih ingat **Jendela Tengah (*Packet Details*)** yang kita bahas sebelumnya? Nah, ini adalah *core* / inti kemampuannya Wireshark. Saat kita klik salah satu paket di *Packet List* atas, Wireshark bakal membuka "amplop" paket tersebut berlapis-lapis sesuai dengan konsep OSI Model.

Pecahan lapisan ini biasanya ada **5 sampai 7 tingkat**. Di bawah ini adalah contoh ngebedah Paket HTTP nomor urut 27. Kita bisa lihat blok merah yang nandain pembagian lapisannya mulai dari `Frame` (paling liar/fisik) sampai `Hypertext Transfer Protocol` (paling abstrak/aplikasi).

<p>
<img src="../../Assets/Images/Packet-dissections.png" alt="Wireshark Packet Dissection (OSI Layers)" width="800px" />
</p>

Setiap kali kita klik baris informasi di **Packet Details**, perhatikan kalau di jendela bawahnya (**Packet Bytes**) bakal meng-*highlight* (warna biru) tampilan bytes mentah (*hex*) dari informasi yang lagi kita pilih. Ini bukti kalau apa yang ada di *Details* adalah terjemahan langsung dari data mentahnya (*Bytes*).

<p>
<img src="../../Assets/Images/detail-packet.png" alt="Packet Details to Packet Bytes Correlation" width="800px" />
</p>

Kalau kita perbesar (*zoom-in*) jendela *Packet Details*, kita bisa lihat ada **7 lapisan utama** yang mendedahkan detail paket dari fisik ke aplikasi:

1. **`frame/packet`** (Rangkuman paket mentah & dari *interface* mana tangkapannya)
2. **`source [MAC]` / Ethernet II** (Alamat fisik (*MAC Address*) dari pengirim & penerima)
3. **`source [IP]` / IPv4/v6** (Alamat logika (*IP Address*) dari pengirim & penerima)
4. **`protocol` / TCP/UDP** (Jalur komunikasi, misal TCP *port* 80 / 443)
5. **`protocol errors`** (Sisa atau gabungan / reassembly dari segmen TCP jika ada)
6. **`application protocol`** (Protokol tingkat atasnya, contoh: HTTP, DNS, FTP)
7. **`application data`** (Inti data yang lagi dikirim, misal isi teks *HTML*, dll)

<p>
<img src="../../Assets/Images/detail-pane.png" alt="A closer view of the Details Pane" width="800px" />
</p>

---

### The Frame (Layer 1)

Ini adalah lapisan terdasar yang merepresentasikan Layer **Physical** (Fisik) pada model OSI. 
Bagian ini murni menceritakan "informasi penangkapan" paket, bukan membicarakan isi datanya. Di sini kita bisa cek detail meta-data seperti:
- **Frame Number:** Paket ke berapa yang lagi kita lihat.
- **Arrival Time:** Cap waktu spesifik kapan sinyal/paket ini dikirim.
- **Length:** Ukuran asli paket di kabel keras (*bytes on wire*) vs ukuran yang berhasil direkam Wireshark (*captured*).
- Status rekaman dan interface yang dipakai.

Biar lebih jelas, lu bisa liat isinya saat dibuka (*expand*) di bawah ini:

<p>
<img src="../../Assets/Images/the-frame.png" alt="Layer 1: The Frame" width="800px" />
</p>

---

### Source [MAC] (Layer 2)

Ini adalah lapisan *Data Link*. Disini kita mulai membahas soal identitas perangkat keras di jaringan lokal. Bagian ini menampilkan **MAC Address** dari mesin pengirim (*Source*) dan mesin penerima (*Destination*). 

Kalau kita *expand* *Ethernet II* ini, kita tidak hanya melihat nomor seri MAC-nya doang, tapi kadang bisa ketahuan juga vendor/merek *network card* (NIC) apa yang dipake sama *device*-nya (misal: Xerox, Intel, Apple, dll).

<p>
<img src="../../Assets/Images/source-Mac.png" alt="Layer 2: Source MAC Address" width="800px" />
</p>

---

### Source [IP] (Layer 3)

Naik satu tingkat ke fungsi *routing*, ini mewakili *Network* OSI Layer, Kalau di Layer 2 kita mainnya MAC Address (lokal), di Layer 3 kita main pakai **IPv4/IPv6 Address** (publik/antar jaringan).

Di bagian *Internet Protocol Version 4/6* ini, kita bisa menguliti banyak info penting seperti:
- Siapa IP sumbernya (Source) & siapa targetnya (Destination).
- Panjang alamat *header*.
- *Time to Live* (TTL) — umur paket sebelum dibuang dari jaringan (ini berguna untuk mengetahui *Operating System* korban).
- Protokol di atasnya apa (misal TCP atau UDP).

<p>
<img src="../../Assets/Images/Source-IP.png" alt="Layer 3: Source IP Address" width="800px" />
</p>

---

### Protocol (Layer 4)

Masuk ke ranah *Transport* OSI Layer. Di bagian ini Wireshark bakal menjabarkan cara pengiriman datanya, biasanya via **TCP** (jalur aman, terjamin) atau **UDP** (jalur cepat, ngebut, ga peduli paket ilang, ga terjamin).

Bagian *Transmission Control Protocol* (kalau pakai TCP) itu super detail. Kita bisa investigasi:
- **Source Port & Destination Port:** (Contoh: *Port* 80 buat web HTTP, *Port* 443 HTTPS).
- **Sequence (Seq) & Acknowledgment (Ack) Number:** Ini adalah sistem "nomor antrean" paket TCP biar datanya tidak tersesat dan dikonfirmasi sama penerima.
- **Flags:** Status spesifik koneksi (Misal: paket tipe SYN buat mulai koneksi, FIN buat memutus koneksi, PSH buat kirim data).

<p>
<img src="../../Assets/Images/Protocols.png" alt="Layer 4: Protocol (TCP/UDP)" width="800px" />
</p>

---

### Protocol Errors (Sisa/Gabungan Layer 4)

Lapisan ini sejatinya masih kelanjutan (ekstensi) dari Layer 4 (Transport). Kita tidak selalu liat blok ini di setiap paket.

Tapi, kadang kalau datanya terlalu besar (misal ngirim file gambar atau HTML gede), paketnya bakal di-*slice* alias dipotong-potong jadi banyak segmen TCP kecil. Nah, Wireshark cukup pintar untuk mengumpulkan lagi (merakit ulang / *Reassembled*) bongkahan-bongkahan data TCP tersebut, biar kita bisa lihat wujud gabungannya di baris ini sebelum dilempar naik ke Layer ke-7 (Aplikasi).

<p>
<img src="../../Assets/Images/Protocols-error.png" alt="5th Layer: Protocol Errors / Reassembled TCP" width="800px" />
</p>

---

### Application Protocol (Layer 5/7)

Akhirnya kita sampai di lapisan yang biasa dipakai sehari-hari: *Application Layer*. Di sinilah Wireshark membedah protokol spesifik dari aplikasi yang lagi jalan, contohnya **HTTP** buat *browsing*, **FTP** buat transfer file, atau **DNS** buat *resolving* nama domain.

Di contoh bawah ini pakai *Hypertext Transfer Protocol* (HTTP). Kita bisa gali harta karun penting kayak:
- *Request* apa yang diminta klien (misal `GET /page.html`).
- *Response Code* dari server (misal `200 OK` atau `404 Not Found`).
- Jenis konten (*Content-Type*: text/html, image/png).
- Terus informasi teknis lainnya kayak *Host/Server* dan *Date*.

<p>
<img src="../../Assets/Images/App-protocols.png" alt="Layer 6: Application Protocol (HTTP/FTP/etc)" width="800px" />
</p>

---

### Application Data (The Payload)

Ini adalah bagian terakhir *(Application Data / Line-based text data)* yang menampilkan **isi asli** dari paket yang dikirim.

Kalau protokolnya nggak dienkripsi (misal HTTP biasa, bukan HTTPS), kita bisa membaca pesan aslinya tanpa halangan. Di contoh SS lu, Wireshark nge-bedah dan nampilin naskah *script/HTML* mentahnya (dimulai dari `<html><head>...`).

Contohnya kalau skenarionya lagi capture *form login* HTTP, nah *password* polos si target bakal kelihatan di baris ini. Mengerikan kalau disalahgunakan kan? Makanya SSL/TLS (HTTPS) diciptakan

<p>
<img src="../../Assets/Images/App-Data.png" alt="Layer 7: Application Data / Payload" width="800px" />
</p>

---
