# TryHackMe: Wireshark: The Basics


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/wiresharkthebasics)
- **Category:** Networking / Analysis tool
- **Difficulty:** Easy

---

## Task 1: Introduction

**Wireshark** adalah alat analisis paket jaringan (*open-source*) yang paling populer di dunia. Ibarat sebuah **mikroskop** atau **mesin X-Ray** buat jaringan — dia bikin kamu bisa ngeliat apa yang sebenarnya terjadi di dalam kabel jaringan atau udara (Wi-Fi), ngebedah setiap bit data yang lewat.

**Kegunaan Utama:**
- **Sniffing:** Ngintip traffic yang lagi jalan secara *live*.
- **Investigating PCAP:** Ngebedah rekaman traffic (file `.pcap`) yang udah diambil sebelumnya.

### Learning Objectives (Tujuan Belajar):
Di room ini, kamu bakal fokus nguasain:
1. Navigasi dan konfigurasi dasar Wireshark.
2. Ngebedah paket buat nyari informasi di setiap lapisan **TCP/IP**.
3. Cara pakai **Display Filters** buat nyaring data yang kamu butuhin.

---

## Tool Overview

### Use Cases (Kapan Pakai Wireshark?)
Wireshark bisa dipake buat banyak hal:
1. **Troubleshooting Jaringan:** Nyari titik masalah (*failure points*) atau kemacetan (*congestion*) di jaringan.
2. **Mendeteksi Anomali Keamanan:** Nyari *host* mencurigakan, pemakaian *port* yang ga wajar, atau *traffic* nakal.
3. **Mempelajari Protokol:** Ngulik detail protokol kayak *response code* HTTP atau isi *payload* data.

> **Catatan Penting:** Wireshark **BUKAN** *Intrusion Detection System* (IDS). Dia ga bisa nyegah serangan atau ngasih peringatan otomatis. Dia cuma ngasih jalan buat kamu ngebedah paket sedalam mungkin dan **tidak memodifikasi** paket, murni cuma ngebaca. Jadi, urusan deteksi itu bergantung penuh sama insting kamu sebagai analis.

### GUI and Data
Tampilan utama Wireshark dibagi jadi 5 bagian penting:

| Bagian GUI | Penjelasan Fungsinya |
| ---------- | -------------------- |
| **Toolbar** | Menu utama yang isinya *shortcuts* buat *packet sniffing*, *filtering*, nyortir, ngeringkas, ekspor, atau gabungin file `.pcap`. |
| **Display Filter Bar** | Kolom penelusuran utama buat ngetik *query filter* (nyaring data yang lagi ditampilin). |
| **Recent Files** | Daftar file rekaman yang barusan dibuka. Tinggal klik buat ngelanjutin analisis. |
| **Capture Filter and Interfaces** | Tempat ngatur *capture filter* (aturan saringan **sebelum** mulai ngerekam) dan milih jalur antarmuka (misal: `eth0`, `lo`, atau `ens33`). |
| **Status Bar** | Baris paling bawah yang ngasih tau status *tool*, profil, sama jumlah paket. |

Gambar di bawah nunjukin tampilan utama Wireshark, bagian yang disorot udah dijelasin di tabel atas:

<p>
<img src="../../Assets/Images/main-Wiresharkl.png" alt="Wireshark GUI" width="800px" />
</p>

### Loading a PCAP File

Sebelum mulai ngebedah data, kamu harus masukin file rekamannya (`.pcap` atau `.pcapng`). Caranya lumayan simple:
1. Cukup lewat menu **File > Open**,
2. **Drag & drop** file langsung ke jendela Wireshark, atau
3. **Klik ganda** aja di file `.pcap` yang mau dianalisis.

Begitu filenya masuk, layar kamu bakal penuh sama barisan data yang sekilas mirip bahasa *matrix*. Biar ga *overwhelmed*, kamu cuma perlu fokus ke **3 jendela utama (*panes*)** ini. 

Ibarat kamu lagi jadi detektif yang meriksa surat, 3 jendela ini mewakili tingkat kedalaman analisisnya:

| Jendela Utama (*Pane*) | Fungsi & Analogi Detektif |
| ---------------------- | ------------------------- |
| **1. Packet List** | **Ibarat Buku Tamu.** Ini adalah daftar panjang semua paket (mobil kurir) yang ketangkep. Di sini kamu bisa ngeliat rangkuman *high-level*: nomor urut, jam lewat (*Time*), siapa pengirim (*Source*), ke mana tujuannya (*Destination*), jalur yang dipake (*Protocol*), dan info singkatnya. |
| **2. Packet Details** | **Ibarat Ngebongkar Isi Amplop Surat.** Kalau kamu klik salah satu baris di *Packet List*, jendela tengah ini bakal mengurai isi paketnya berlapis-lapis sesuai teori *OSI Layers*. Dari mulai bungkus luar (lapisan fisik/MAC) sampai ke inti dalamnya (protokol aplikasi kayak HTTP). |
| **3. Packet Bytes** | **Ibarat Ngeliat DNA / Atom dari Suratnya.** Ini adalah bentuk data paling mentah. Bagian ini nampilin data paket dalam wujud *Hexadecimal* (angka *hex*) dan *ASCII* (teks yang bisa dibaca manusia). Kadang kamu bisa nemu teks *password* atau *payload* tersembunyi yang bertebaran di sini. |

Di pojokan layar Wireshark, ada informasi tambahan buat *tracking*:
- **File Name:** Nama file pcap yang lagi dibuka (ada di *tab browser* atas, dan *status bar* sudut kiri bawah).
- **Total Packets:** Total baris catatan (jumlah paket) yang sukses terekam (di *status bar* paduan kanan bawah).

Biar ada bayangan, cek gambar di bawah yang menyoroti 3 jendela utama tadi:

<p>
<img src="../../Assets/Images/Pcap.png" alt="Wireshark PCAP Loading Interface" width="800px" />
</p>

---

### Colouring Packets

Pernah merhatiin kenapa barisan paket di Wireshark punya warna beda-beda? Itu bukan buat hiasan. Wireshark pake sistem pewarnaan biar mata kamu gampang ngenalin masalah (anomali) atau nemuin protokol tertentu tanpa harus meriksa detailnya satu per satu. 

Wireshark punya **Dua Jenis Metode Pewarnaan**:

| Jenis Pewarnaan | Karakteristik | Cara Akses |
| :--- | :--- | :--- |
| **Permanent Rules** | Aturan paten yang disimpan di profilmu. Aturan ini bakal tetep ada tiap kali kamu buka Wireshark di sesi berikutnya. | Klik menu **View > Coloring Rules** *(Bisa juga buat bikin rule custom-mu sendiri)* |
| **Temporary Rules** | Aturan sementara yang cuma aktif selama sesi Wireshark jalan. Kalau aplikasinya ditutup, warnanya bakal ilang. | Klik kanan di baris paket > **Colorize Conversation** (atau via menu **View > Conversation Filter**) |

Biar tampilan warnanya aktif (atau kalau mau dimatiin biar polos matanya ga capek), kamu bisa pake setelan **View > Colorise Packet List**.

Coba cek penampakan *Coloring Rules Default* bawaan Wireshark di bawah ini:

<p>
<img src="../../Assets/Images/Coloring-filter.png" alt="Wireshark Default Coloring Rules" width="800px" />
</p>

---

### Traffic Sniffing

Selain kemampuan ngebaca file rekaman (PCAP), keahlian unggulan Wireshark adalah kemampuannya buat *sniffing* atau memonitor *traffic* jaringan secara *live* saat itu juga. 

Di bagian kiri *Toolbar* atas, ada 3 tombol berguna buat melakukan *sniffing*:

- **Blue Shark Button (Start):** Mulai nangkep (*capturing*) *traffic* yang lagi lalu-lalang di jaringan.
- **Red Square Button (Stop):** Menghentikan penangkapan sementara kalau kamu udah selesai analisis.
- **Green Arrow Button (Restart):** Ngulang proses *sniffing* bersih dari awal lagi.

Pas *sniffing* lagi jalan, kamu bisa ngecek bagian *Status Bar* (paling bawah layar) buat mastiin dua hal ini:
1.  **Interface yang Dipakai:** (Contoh: `eth0`) Biar kamu ga salah nyadap jalur.
2.  **Jumlah Paket:** (Contoh: `Packets: 122`) Nunjukin udah seberapa banyak data yang sukses kekumpul.

Biar lebih jelas posisi tombol sama statusnya, lihat referensi GUI ini ya:

<p>
<img src="../../Assets/Images/Shark-Button.png" alt="Wireshark Sniffing Controls" width="800px" />
</p>

---

### Merge PCAP Files

Wireshark juga punya fitur oke buat ngegabungin dua file `.pcap` yang pisah jadi satu file utuh. 

1. Pastiin kamu udah buka satu file pcap utama di Wireshark.
2. Buka menu **File > Merge**.
3. Pilih file pcap kedua yang mau digabungin.
4. Wireshark bakal ngasih tau estimasi total paket dari file kedua. 
5. Tinggal klik **Open**, dan dua file itu bakal nyatu jadi satu _merged workspace_ (tandanya namanya jadi *Untitled*).

**Ingat:** Karena hasil gabungannya ini sifatnya masih *temporary*, **jangan lupa klik Save As** buat nyimpen file PCAP gabungannya ke laptop sebelum kamu mulai sibuk analisis.

### View File Details

Penting banget buat tau identitas asli dari file pcap yang lagi kamu analisis, apalagi kalau bos nyodorin banyak file pcap dari berbagai kasus yang beda-beda. Di bagian *File Details* ini, kamu bisa ngeliat info krusial dan statistik penting biar tau file mana yang diseriusin duluan:
- **File hash** (SHA256/MD5 buat bukti forensik ke pengadilan biar terbukti filenya belum diotak-atik)
- **Capture time** (Kapan *traffic* ini direkam kejadiannya)
- **Komentar file** (Catatan obrolan dari analis lain yang megang file ini sebelumnya)
- **Interface & Statistics** (Berapa banyak paket dan byte data raksasa yang kerekam)

Ada **dua cara** cepat buat ngebuka jendela detail ini:
1. Lewat menu atas: **Statistics > Capture File Properties**
2. Lewat *shortcut* GUI: Cukup klik **icon pcap kecil di pojok kiri bawah** (persis di sebelah nama file).

<p>
<img src="../../Assets/Images/View-details.png" alt="Wireshark View File Details" width="800px" />
</p>

---

## Packet Dissection

*Packet dissection* (bedah paket) itu intinya adalah nerjemahin detail isi paket ke dalam wujud protokol dan kolom-kolom yang gampang buat dibaca. Wireshark punya banyak banget dukungan protokol bawaan buat dibedah, dan kamu bahkan bisa bikin *script* pembedah (*dissector scripts*) kamu sendiri.

> **Note:** Kemampuan ini bergantung banget sama seberapa paham kamu soal konsep **OSI Model**. Jadi pastiin pondasi OSI Layer kamu udah kuat biar ngebaca beginian kerasa gampang.

### Packet Details (Ngebedah Lapis Demi Lapis)

Masih inget **Jendela Tengah (*Packet Details*)** yang udah dibahas sebelumnya? Nah, ini adalah *core* / inti kemampuannya Wireshark. Saat kamu klik salah satu paket di *Packet List* atas, Wireshark bakal ngebuka "amplop" paket tersebut berlapis-lapis sesuai sama konsep OSI Model.

Pecahan lapisan ini biasanya ada **5 sampai 7 tingkat**. Di bawah ini adalah contoh ngebedah Paket HTTP nomor urut 27. Kamu bisa liat blok merah yang nandain pembagian lapisannya mulai dari `Frame` (paling liar/fisik) sampai `Hypertext Transfer Protocol` (paling abstrak/aplikasi).

<p>
<img src="../../Assets/Images/Packet-dissections.png" alt="Wireshark Packet Dissection (OSI Layers)" width="800px" />
</p>

Setiap kali kamu ngeklik barisan informasi di **Packet Details**, perhatiin deh kalau di jendela bawahnya (**Packet Bytes**) bakal ngasih *highlight* (warna biru) ke tampilan bytes mentah (*hex*) dari info yang lagi kamu pilih. Ini bukti kuat kalau apa yang ditampilin di *Details* adalah terjemahan langsung dari data mentahnya (*Bytes*).

<p>
<img src="../../Assets/Images/detail-packet.png" alt="Packet Details to Packet Bytes Correlation" width="800px" />
</p>

Kalau kamu perbesar (*zoom-in*) jendela *Packet Details*, kamu bisa liat ada **7 lapisan utama** yang nelanjangin detail paket dari fisik naik ke aplikasi:

1. **`frame/packet`** (Rangkuman paket mentah & dari *interface* mana nangkepnya)
2. **`source [MAC]` / Ethernet II** (Alamat fisik (*MAC Address*) dari pengirim & penerima)
3. **`source [IP]` / IPv4/v6** (Alamat logika (*IP Address*) dari pengirim & penerima)
4. **`protocol` / TCP/UDP** (Jalur komunikasi, misal TCP *port* 80 / 443)
5. **`protocol errors`** (Sisa atau gabungan / *reassembly* dari potongan TCP kalau ada)
6. **`application protocol`** (Protokol tingkat atasnya, contoh: HTTP, DNS, FTP)
7. **`application data`** (Inti data yang lagi dikirim, misal isi teks *HTML*, dll)

<p>
<img src="../../Assets/Images/detail-pane.png" alt="A closer view of the Details Pane" width="800px" />
</p>

---

### The Frame (Layer 1)

Ini adalah lapisan terdasar yang ngewakilin spesifikasi **Physical** (Fisik) pada model OSI. 
Bagian ini murni cuma nyeritain "informasi penangkapan" paket, bukan ngomongin isi datanya. Di sini kamu bisa ngecek detail meta-data kayak:
- **Frame Number:** Paket ke berapa yang lagi kamu liat.
- **Arrival Time:** Cap waktu spesifik kapan sinyal/paket ini nyampe.
- **Length:** Ukuran asli paket di kabel keras (*bytes on wire*) vs ukuran yang berhasil direkam Wireshark (*captured*).
- Status rekaman dan interface yang dipake.

Biar lebih jelas, kamu bisa liat isinya pas bagian ini dibuka (*expand*):

<p>
<img src="../../Assets/Images/the-frame.png" alt="Layer 1: The Frame" width="800px" />
</p>

---

### Source [MAC] (Layer 2)

Ini adalah lapisan *Data Link*. Di sini kamu mulai ngebahas soal identitas perangkat keras di jaringan lokal. Bagian ini nampilin **MAC Address** dari jeroan mesin si pengirim (*Source*) dan mesin si penerima (*Destination*). 

Kalau kamu *expand* bagian *Ethernet II* ini, kamu nggak cuma merhatiin nomor seri MAC-nya doang, tapi kadang bisa ketahuan juga vendor/merek *network card* (NIC) apa yang dipake sama *device* tersebut (misal: Xerox, Intel, Apple, dll).

<p>
<img src="../../Assets/Images/source-Mac.png" alt="Layer 2: Source MAC Address" width="800px" />
</p>

---

### Source [IP] (Layer 3)

Naik satu tingkat ke fungsi *routing*, ini ngewakilin *Network Layer* di OSI. Kalau di Layer 2 kamu mainnya MAC Address (sesama mesin lokal), di Layer 3 ini kamu main pake **IPv4/IPv6 Address** (publik/antar jaringan luas).

Di bagian *Internet Protocol Version 4/6* ini, kamu bisa nguliti banyak info penting kayak:
- Siapa IP sumbernya (Source) & siapa targetnya (Destination).
- Panjang alamat *header*.
- *Time to Live* (TTL) — umur paket sebelum layu dan dibuang dari jaringan (ini info emas buat nebak *Operating System* mesin korban).
- Protokol di atasnya mau pake apa (misal TCP atau UDP).

<p>
<img src="../../Assets/Images/Source-IP.png" alt="Layer 3: Source IP Address" width="800px" />
</p>

---

### Protocol (Layer 4)

Kita masuk ke ranah *Transport* OSI Layer. Di bagian ini Wireshark bakal ngejabarin cara lalu-lintas datanya dikirim, yang paling sering nongol itu **TCP** (jalur aman, paket dijamin nyampe berurutan) atau **UDP** (jalur cepat, ngebut, bodo amat kalau ada paket ilang di jalan).

Bagian *Transmission Control Protocol* (kalau pake TCP) itu detail banget. Kamu bisa nginvestigasi:
- **Source Port & Destination Port:** (Contoh: *Port* 80 buat web HTTP biasa, *Port* 443 buat HTTPS aman).
- **Sequence (Seq) & Acknowledgment (Ack) Number:** Ini sistem "nomor antrean" paket TCP biar datanya nggak nyasar dan dikonfirmasi sama si empunya rumah.
- **Flags:** Status spesifik dari koneksinya (Misal: ada bendera SYN buat ngajak kenalan/koneksi, FIN buat mutusin koneksi pamit pulang, PSH buat kirim data).

<p>
<img src="../../Assets/Images/Protocols.png" alt="Layer 4: Protocol (TCP/UDP)" width="800px" />
</p>

---

### Protocol Errors (Sisa/Gabungan Layer 4)

Lapisan yang kelima ini sejatinya masih sisaan (ekstensi) dari Layer 4 (Transport). Kamu nggak bakal selalu nemuin blok ini di setiap paket.

Tapi, kadang kalau datanya kelewat gemuk (misal waktu ngirim file gambar atau kodingan HTML panjang), paketnya bakal di-*slice* alias dipotong-potong paksa jadi banyak segmen TCP kecil. Nah, Wireshark cukup pinter buat mungutin dan ngerakit ulang (*Reassembled*) bongkahan-bongkahan data TCP yang pecah tadi, biar kamu bisa liat wujud aslinya ngegabung di baris ini sebelum dilempar naik ke Layer ke-7 (Aplikasi).

<p>
<img src="../../Assets/Images/Protocols-error.png" alt="5th Layer: Protocol Errors / Reassembled TCP" width="800px" />
</p>

---

### Application Protocol (Layer 5/7)

Akhirnya nyampe juga di lapisan yang paling deket dan sering dipake manusia sehari-hari: *Application Layer*. Di sinilah Wireshark membedah protokol spesifik dari aplikasi yang lagi dipake, contohnya **HTTP** pas lagi *browsing*, **FTP** buat mindahin transfer file, atau **DNS** buat nanya alamat web domain.

Di contoh SS bawah ini dia pake *Hypertext Transfer Protocol* (HTTP). Kamu bisa nyekrop harta karun penting kayak:
- *Request* apa yang lagi diminta si klien (misal `GET /page.html`).
- *Response Code* jawaban dari servernya (misal `200 OK` dapet, atau `404 Not Found` ditolak).
- Jenis konten (*Content-Type*: text/html, image/png).
- Terus informasi teknis *header* lainnya kayak nama *Host/Server* dan jam *Date*.

<p>
<img src="../../Assets/Images/App-protocols.png" alt="Layer 6: Application Protocol (HTTP/FTP/etc)" width="800px" />
</p>

---

### Application Data (The Payload)

Ini adalah ujung perjalanan *(Application Data / Line-based text data)* yang bakal nampilin secara terang-terangan **isi asli** dari paket yang dikirim.

Kalau protokolnya nggak dienkripsi (misal HTTP biasa, bukan HTTPS), kamu bisa baca pesan curhatannya tanpa halangan sama sekali. Di contoh SS, Wireshark ngebongkar dan berani nampilin file *script/HTML* mentahnya (bisa kelihatan kode tag `<html><head>...`).

Bayangin kalau skenarionya kamu lagi nangkep *traffic* dari *form login* HTTP, nah *password* aslinya si korban (*plain text*) bakal nongol jelas di depan mata di baris ini. Mengerikan kan kalau kecolongan? Makanya web modern wajib pake SSL/TLS (HTTPS).

<p>
<img src="../../Assets/Images/App-Data.png" alt="Layer 7: Application Data / Payload" width="800px" />
</p>

---

## Packet Navigation

### Packet Numbers

Wireshark ngitung dan ngasih nomor unik (berurutan) buat setiap paket yang berhasil ditangkep. Ini bakal berguna banget pas proses analisis file *capture* yang ukurannya raksasa. 

Dengan adanya identitas di kolom **No.** ini, kamu jadi lebih gampang nginget posisi, manggil ulang paket spesifik, dan nyari referensi *event* tertentu tanpa bingung. Nomor urut ini juga nyambung dan persis sama dengan angka yang tertera di bagian `Frame` pada jendela *Packet Details*.

### Go to Packet

Karena setiap paket udah punya nomor urut yang pasti, kamu bisa manfaatin fitur lompat cepat tanpa harus repot *scroll* manual pelan-pelan.

Fitur *Go to Packet* ini sangat ngebantu kamu buat:
- Lompat bolak-balik ke nomor paket spesifik dengan instan.
- Ngelacak *frame* demi *frame* dan ngikutin alur percakapan / pertukaran data (*conversation*) tertentu jadi jauh lebih gampang.

Cara ngakses fiturnya simpel banget:
1. Klik menu baris atas: **Go > Go to Packet...**, terus masukin nomor paket yang dicari.
2. Atau bisa langsung ngetik nomor di kolom teks penelusuran paket, dan pake tombol *Previous/Next Packet* di bagian *toolbar* utama.

---

### Find Packets

Selain lompat ke nomor tertentu, Wireshark punya fitur penelusuran (*Find*) buat nyari paket berdasarkan isi datanya. Ini *skill* penting banget buat nangkep pola serangan (kayak *Intrusion*) atau jejak *error* mesin di tengah lautan *network traffic*.

Ada dua aturan main pas lagi nyari supaya langsung ketemu:
1. **Pilih Tipe Input:** Wireshark nolak nyari asal-asalan. Dia cuma nerima 4 jenis input (*Display filter*, *Hex*, *String*, dan *Regex*). Pencarian pake *String* (teks biasa) dan *Regex* (pola khusus) itu yang paling sering dipake analis di lapangan.
2. **Pilih Jendela Target Spesifik:** Wireshark terbagi jadi 3 jendela utama (*Packet List*, *Packet Details*, *Packet Bytes*). Nah, kalau kata kunci yang dicari nyelip di dalem *Packet Details* (lapisan OSI) tapi kamu masang target penelusurannya di *Packet List* (tabel ringkasan depan), Wireshark bakal lapor kalau dia ga nemu apa-apa.

Cara manggil kotak penelusurannya: Klik menu **Edit > Find Packet...** (Atau mending pake *shortcut keyboard* standar kalau ada).

### Mark Packets

Buat nandain temuan penting (biar ga ilang atau kelewat pas asyik *scroll* nyari hal lain), gunain fitur **Mark Packets**. 

Fitur mark ini kerjanya murni kayak stabilo. Paket yang ditandain bakal otomatis ganti warna latar jadi **Hitam**, numpuk di atas pewarnaan *default*-nya protokol. Ini bikin fokus matamu ga gampang pecah dan ngebantu banget kalau suatu saat *packet* tersebut mau di-*ekspor* khusus buat barang bukti penyidikan.

Cara pakainya simpel:
- Lewat baris menu atas: **Edit > Mark/Unmark Packet**
- Atau cara yang lebih masuk akal: **Klik kanan di baris paket spesifik > Mark/Unmark Packet**

> **Catatan Penting:** Tanda stabilo hitam ini sifatnya cuma sementara, alias langsung ilang begitu sesi analisismu kelar. Kalau aplikasi Wireshark ditutup tanpa menyimpen ulang filenya, semua status *"Marked"* itu bakal ikutan musnah.

---

### Packet Comments

Selain ditandain pake stabilo (*Marking*), kamu juga bisa nyisipin catatan digital alias *Comments* di paket tertentu. Fitur ini ngebantu banget:
- Sebagai pengingat (*reminder*) buat dirimu sendiri pas lagi jalanin investigasi panjang.
- Sebagai alat komunikasi / penunjuk titik penting kalau file PCAP-nya mau dianalisis keroyokan bareng analis SOC lain.

Beda sama *Mark Packets* yang raib kalau ditutup, **Packet Comments bakal permanen** nempel di dalam file PCAP sampai kamu sendiri yang ngehapusnya (*note:* pastiin pilih ekstensi `.pcapng` pas nge-*save* biar komentarnya tetep utuh).

Cara nambahinnya: **Klik kanan pada paket target > Packet Comments...**

### Export Packets

Pernah ngebayangin buka file *capture* yang ukurannya sampai jutaan paket? Berat banget, sumpah. Karena Wireshark itu cuma mesin pembaca statis (bukan *IDS* otomatis), kamulah yang ditugaskan milih mana paket normal dan mana paket jahat.

Kalau kamu udah nemu kumpulan paket (*scope*) yang isinya aktivitas mencurigakan (*suspicious*), kamu bisa misahin paket-paket spesifik itu ke dalam file PCAP baru yang lebih terisolasi. Tujuannya biar datamu kelihatan bersih dan kamu ga perlu repot nge-*share* 99% paket *noise* ke analis lainnya.

Cara ngekstraknya: Klik menu **File > Export Specified Packets...** 

### Export Objects (Files)

Ini dia trik andalan para *Security Analyst*. Wireshark ternyata sanggup lho buat **mengekstrak ulang file utuh** (misal gambar, PDF, dokumen bajakan, atau *malware* / *.exe*) yang diem-diem lewat di jaringan tersebut.

Syarat mutlaknya cuma satu: protokol waktu pas ngirim harus nggak digembok enkripsi (alias masih polos) atau minimal protokol pendukung standar (*HTTP*, *FTP*, *SMB*, *DICOM*, *IMF*, *TFTP*). Jadi misal ada penjahat nyebar virus cuma via HTTP, kamu bisa langsung nyedot *malware* mentahnya pake Wireshark buat dibedah lebih dalam di ruang isolasi lab (*sandbox*).

Cara narik filenya: Klik menu **File > Export Objects > [Pilih Protokol terkait, misal HTTP]**

---

### Time Display Format

Secara bawaan (*default*), Wireshark bakal ngurutin paket berdasarkan waktu penangkapan dengan format **"Seconds Since Beginning of Capture"** (Rekaman detik ke-sekian sejak tombol *Start* ditekan). 

Format detik ini emang oke buat narasi durasi singkat, tapi **jadi mimpi buruk kalau dipake buat investigasi**. Bayangin kamu disuruh nyocokin log *error* *Server* yang formatnya jam dan tanggal (misalnya kejadian terekam jam `10:30 Pagi`), dicocokin sama riwayat detik Wireshark (misal di detik ke `4502`). Bikin pening kepala.

Makanya, *best practice* (kebiasaan emas) analis kawakan adalah mutlak ngubah format tampilannya dulu ke **UTC Time** (*Coordinated Universal Time*) atau **Local Time** supaya patokan jamnya sinkron dan gampang dicari kapan kejadian *event* aneh itu persisnya numpang lewat.

Cara ngerubah format jamnya:
Klik opsi menu atas **View > Time Display Format > [Pilih format yang menurutmu paling pas, misal UTC Date and Time]**

---

### Expert Info

Wireshark bukan cuma modal nampilin baris data mentah, tapi dia sediain fitur buat mendeteksi gejala aneh (anomali) atau masalah (*problems*) teknis protokol. Rangkuman peringatan intelijen ini dikumpulin di satu wadah bernama **Expert Info**.

> **Peringatan:** Analisis sistem Expert Info ini murni bertindak sebagai *suggestion* (saran/dugaan awal). Jadi bakal selalu ada kemungkinan terjadinya *false positive* (disangka malapetaka padahal fajar wajar) atau *false negative* (disangka normal padahal bom waktu). Jangan ditelen mentah-mentah hasil tebakannya!

Expert info membagi level peringatannya ke dalem tingkatan status **Keparahan (Severity)** yang dibedain lewat bendera warna:

| Severity (Keparahan) | Colour (Warna) | Penjelasan Info |
| :--- | :---: | :--- |
| **Chat** | <span style="color:#007BFF">**Blue**</span> | Informasi standar soal alur kerja rutin (*usual workflow*). Sangat normal dan bukan sinyal ancaman. |
| **Note** | <span style="color:#17A2B8">**Cyan**</span> | Kejadian yang cukup unik (*notable events*), biasanya memuat catatan kode *error* wajar di level aplikasi. |
| **Warn** | <span style="color:#FFC107">**Yellow**</span> | Peringatan lumayan serius. Ngasih laporan *error code* yang nggak lumrah alias nimbulin masalah (*problem statements*). |
| **Error** | <span style="color:#DC3545">**Red**</span> | Tanda silang darurat! Menandakan ada paket data pecah berantakan atau cacat teknis (*malformed packets*). |

Selain dinilai dari seberapa parah, peringatan ini juga dikelompokin dari jenis asal usul **Kasusnya (Group)**:

| Group A | Info Group A | Group B | Info Group B |
| :--- | :--- | :--- | :--- |
| **Checksum** | Bukti itungan validasi *checksum* paket nggak sinkron. | **Deprecated** | Protokol jaman old yang nekat digunain (*deprecated*). |
| **Comment** | Ada analis yang ninggalin jejak komentar (*Packet comment*). | **Malformed** | Nemu paket yang strukturnya berantakan nggak wangun (*malformed*). |

---

## Packet Filtering

Wireshark punya *filter engine* sakti yang ngebantu analis ngebabat habis *traffic* yang ga penting (*noise*) biar bisa fokus nemuin kejadian spesifik. 

Di Wireshark, *filter* itu dibagi jadi dua mesin utama:
1. **Capture Filters:** Dipake buat **Nangkep** paket inceran aja *sebelum* gerbong *traffic*-nya masuk dan disimpen ke *memory* aplikasi (misal: "cuma mau nangkep paket HTTP port 80 aja, sisanya buang").
2. **Display Filters:** Dipake buat "**Nampilin**" paket tertentu aja dari file PCAP yang *udah berhasil ditangkep* secara utuh. (Fokus kamu di *basic room* ini murni cuma bakal mainan *Display Filter*).

Sebenernya nulis aturan/sintaks *filter* secara manual itu lumayan ribet. Bersyukurnya, Wireshark punya antarmuka gampang. Ada satu aturan (*Golden Rule*) buat analis jaringan yang mager nulis rumus *queries* sendiri:

> **"If you can click on it, you can filter and copy it"** 
> *(Kalau kamu bisa ngeklik bagian itu, berarti kamu niscaya bisa ngefilternya).*

### Apply as Filter

Ini adalah cara paling *basic* tapi super ampuh buat *filtering*, kamu ga perlu repot ngafalin sintaks panjang-panjang. 

Misal nemu *IP Address* atau Protokol tertentu lewat depan mata di jendela *Packet List* atau *Packet Details*, kamu tinggal:
1. Blok/Klik baris target yang mau dijaring.
2. Klik kanan.
3. Pilih panggilannya **Apply as Filter** > Pilih cabangnya (misal: *Selected* buat filter yang persis nyocokin temuanmu, atau *Not Selected* buat ngebuang jauh-jauh traffic pengganggu itu).

Otomatis, rumusnya bakal nongol terisi sendiri di kolom *Display Filter Bar* atas. Praktis banget kan? 

Penting buat nyatet: Total paket asli bawaan file dibandingin total paket sisa yang berhasil nembus filter bakal selalu ketauan di **Status Bar** paling bawah (pojok kanan).

---

### Conversation Filter

Kalau jurus *Apply as Filter* di atas biasanya memfilter cuma dari **satu entitas mutlak** (misalnya jaring cuma nampilin rumah Si A), maka beda cerita dengan *Conversation Filter*.

Terkadang pas kamu lagi nyelidikin satu aktivitas yang baunya nggak enak, kamu pasti butuh ngeliat **seluruh riwayat percakapannya**. Kamu kepengen liat semua paket yang lari ganti-gantian bolak-balik antara dua alamat IP atau *Port* spesifik tersebut. Nah, di sinilah wujud *Conversation Filter* berjasa.

Fitur ini bertugas buat ngulitin "percakapan saksi utama" dan **nyembunyiin total** semua lautan paket pelengkap (*noise*) yang nggak relevan dari *pane* utamamu.

Cara aksesnya:
- **Klik kanan > Conversation Filter > [Pilih target sasarannya, misal IPv4 atau TCP]**
- Atau via baris menu atas: **Analyse > Conversation Filter**

---

### Colourise Conversation

Opsi ini sebenarnya beda-beda tipis deketnya sama *Conversation Filter* barusan, tapi ada satu beda yang nge-jreng: **Dia sama sekali ga nyembunyiin paket dari aktor lain**.

Ketimbang memfilter dan mengurangi jumlah paket yang tampil di layar, opsi *Colourise Conversation* cuma naruh warna stabilo doang buat narik perhatian blok paket yang saling tektokan. Warnanya bakal nimpa aturan pewarnaan (*Colouring Rules*) bawaan. Efeknya, rantai dialog inceran kamu langsung keliatan nyala terang nonjol di tumpukan PCAP.

Cara pakainya:
1. **Klik kanan paket target > Colourise Conversation** (atau via menu **View > Colourise Conversation**).
2. Kalau investigasi udah kelar dan kamu pengen ngebalikin warnanya kayak sediakala, tekan **View > Colourise Conversation > Reset Colourisation**.

### Prepare as Filter

Nah, kalau fitur *Apply as Filter* (yang kita bahas di awal) itu ibarat "Sekali klik, filter lansung jalan", si *Prepare as Filter* ini lebih rileks pakenya layaknya "Bikin draf dulu, eksekusinya nanti".

Opsi ini ngena banget pas kamu niat meracik *Display Filter* yang syaratnya kompleks berlapis (butuh paduan logika **AND** / **OR**). Si Wireshark cuma bertugas ngebantu **ngetikin** sintaks algoritmanya di kolom pencarian atas, tapi bakalan diam nunggu kamu neken tombol `Enter` secara manual buat mutusin eksekusinya.

Cara aksesnya persis sama kaya sebelahnya: **Klik kanan > Prepare as Filter**. Barulah kamu tempelin aja gabungan logikanya pake pilihan opsi semacam *... and selected* atau *... or selected*.

---

### Apply as Column

Secara bawaan (*default*), *Packet List* tabel di Wireshark pelit nampilin jendela data (cuma ngasih kolom standar No, Time, Source, Destination, Protocol, Length, Info). Terus gimana dong kalau kamu pengen mantau nilai spesifik daleman lapisan OSI (misalnya manjangin ukuran besaran *header*) persis nongkrong di halaman depan tanpa ngabisin waktu ngebukanya satu-satu?

Solusi nyamannya: pake fitur **Apply as Column**. Fitur ini ngasih kebebasan buat ngerampas satu nilai/parameter sakti dari kedalaman *Packet Details*, lalu menyulapnya buat nongkrong selamanya jadi "kolom permanen baru" di halaman muka tabel *Packet List*. Ini efisien parah buat kerjaan nyocokin satu parameter kunci ngelintasin ribuan paket sekaligus dengan sekilas mata!

Cara pakainya gampang:
Cari *value* incerannya di jendela *Packet Details* > **Klik kanan > Apply as Column**. (Oiya, kalau posisinya jelek, arah kolom barunya ini bebas ditarik geser, atau disembunyiin kapan pun dengan ngklik kanan di *header* tabel atasannya).

### Follow Stream

Wireshark emang kerjanya nanduk data mentah (*raw traffic*) pake porsi sekecil sisa serpihan paket (terfragmentasi). Sebagai analis jaringan, stres mah iya kalau dititah ngebaca potongan sisa ini satu per satu. Biar waras dan tau makna aplikasinya secara utuh, kita butuh alat penerjemah rekonstruksi.

Fitur luhur **Follow Stream** ibarat mesin penyusun *teka-teki puzzle otomatis*. Opsi ini bakal nyusun ulang dan ngerangkai (*reconstruct*) pecahan dari ratusan debu paket tadi jadi satu aliran utuh dialog aplikasi (*Application-level data*). 

Kalau naasnya lajur jaringannya telanjang bulat alias bodong nggak dienkripsi (seperti HTTP biasa, FTP, atau fasilitas Telnet), menu ini langsung berubah wujud jadi **senjata pamungkas**. Kamu bisa dengan gampang membaca wujud *username*, *password*, atau curhatan chat utuh aslinya di wujud baris (*plain text*) layar kepisah. Di dialog papannya stream ini, teks berwarna **merah** nandain bacotan sang *Client* (menuju Server), dan arah teks **biru** berarti itu tektokan balasannya si *Server* (ke Client).

Cara mbukanya standar:
**Klik kanan di paket data target > Follow > [Pilih Stream salurannya, misal TCP / UDP / HTTP Stream]**

---

## Simple Display Filter Queries

Selain mengandalkan *cheat* klik kanan antarmuka, analis tulen di lapangan lebih nyaman dan cepet geraknya pake ngetik *query filter* dasar langsung di baris penelusuran (*Display Filter Bar*). Wireshark punya banyak banget opsi, tapi kamu bakal kita bekelin filter yang paling esensial dan mendasar.

### Filter By Protocol Name
Metode pencarian tersolid dan lugu. Pas kamu pengen ngerucutin saringan khusus buat merhatiin satu protokol doang (ngebabat spesies trafik liar lainnya):
- Langsung ketik nama jenis protokolnya di *search bar* *(wajib diketik dalam huruf kecil semua)*.
- Contoh amunisinya: `http`, `ftp`, `dns`, `arp`, `icmp`, `ssh`.

### Filter By Protocol Port Number
Pusing nyari karna aplikasinya iseng pake *port* ajaib, atau saat kamu ditarget cuma boleh peduli fokus ke nomor gembok *port*-nya si tersangka ngabain embel-embel si jaringannya apa wujud protokolnya.

Sintaksnya gampang, tinggal paduin jenis nama *Transport Layer*-nya (`tcp` atau `udp`) digabung lekat kata *port* dan nyebutin angkanya.
- Pola formatnya: `<protocol>.port == <nomer port>`
- Contoh skenario real saringannya: `tcp.port == 80` (Hanya bakalan rela ngelolosin wujud traffic HTTP yang numpang jalurnya di port 80 doang).

### Filter By IP
Kebutuhan pas lagi ngebongkar arsip PCAP, rata-rata kamu diwajibin buat ngisolasi jalur *traffic* cuma murni terpaku ke satu alamat IP target aja (entah IP si *Server* atau ngintai mesin IP si *Attacker*).

Pondasi utama sintaks dasarnya saat menarget IP general ini (entah dia yang ngirim pesen atau yang dapet paketan) itu namanya adalah `ip.addr`.
- Contoh pola manggilnya: `ip.addr == <IP address>`
- Contoh penerapan eksekusinya: `ip.addr == 192.168.1.2` (Bakal langsung nyaring jaringan supaya ngeluarin wujud paket yang baris *Source* ato *Destination*-nya mengandung IP tersebut).

---

## Real-World Relevance

Di skenario ketatnya rutinitas *Security Operations Center (SOC)* atau simulasi medan perang *Red Teaming* di dunia nyata, Wireshark ibarat "Mata Elang" analis:
- **Analisis Malware & Incident Response:** *Malware* jadul atau skrip C2 (*Command and Control*) kampungan kadang masih nekat berkomunasi murni via HTTP atau DNS biasa tanpa pelindung enkripsi sepeserpun. Pasukan SOC bakal menggunakan *Follow Stream* dan *Export Objects* yang barusan kamu pelajari buat merakit bongkahan *malware payload* langsung nge-ekstraknya utuh dari *network traffic* sebelum mengeksekusinya di dalam ruang kurungan lab forensik *sandbox*.
- **Pencurian Kredensial (Cleartext Sniffing):** Dalam fase lompat melintang antar mesin (*Lateral Movement*), penyerang yang udah mujur nembus Intranet sasaran bisa kapan aja gelar Wireshark (atau wujud terminal alias `tcpdump` di OS target) buat nyadap lalin dan ngerampok akun dewa (*admin creds*) staf dalem yang masih teledor dikirim via FTP, Telnet tua, atau parahnya aplikasi web internal yang pada alergi di-instal gembok sertifikat SSL/HTTPS.
- **Kesimpulan Risiko:** Teknik serangan berbasis penyadapan murni ini ngeri banget! Kenapa? Karena perbuatannya nguping nyadap alat sekelas Wireshark sama sekali kagak ninggalin *log* bising cipratan serangan yang mancing ngamuknya alarm *Intrusion Detection System (IDS)* manapun—alasan amannya karena sifatnya yang cuma "duduk, mendengarkan/nguping", bebas nangkep info ga ngelakuin aksi ofensif nembakin meriam *exploit*. 
