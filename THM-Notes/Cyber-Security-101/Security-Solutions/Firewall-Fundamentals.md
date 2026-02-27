# TryHackMe: Firewall Fundamentals

- **Room Link:** [Firewall Fundamentals](https://tryhackme.com/room/firewallfundamentals)
- **Category:** Security Solutions
- **Difficulty:** easy

## What is Purpose of Firewall

Pernahkah kamu memperhatikan satpam (*security guard*) yang berjaga di pintu masuk *mall*, bank, atau rumah? Tugas utama mereka adalah mengawasi setiap orang yang keluar-masuk, dan memastikan tidak ada penyusup yang bisa masuk tanpa izin. Satpam ini bertindak sebagai **tembok penghalang** antara pengunjung luar dan area gedung.

Nah, sama seperti fungsi satpam tersebut, **Firewall** adalah sistem keamanan yang bertugas mengawasi ribuan lalu lintas data (*traffic*) yang masuk dan keluar dari sebuah jaringan digital kita.

Firewall bertindak sebagai **tembok pertahanan utama** yang memisahkan jaringan internal kita yang aman dari jaringan eksternal (Internet). Berbekal aturan khusus (*rules*), Firewall akan memutuskan apakah suatu data diizinkan masuk atau akan diblokir mentah-mentah.

<P align="center">
<img src="../../Assets/Images/Firewall.png" alt="Firewall" width="600px"/>
</p>

### Learning Objectives

- Tipe-tipe firewall
- Firewall rules dan komponennya
- Windows Defender Firewall
- Linux firewall (iptables)

## Type Of Firewall

<P align="center">
<img src="../../Assets/Images/Firewall-type.png" alt="Type of Firewall" width="800px"/>
</p>

nyatanya ada banyak **tipe Firewall berbeda** di luar sana—masing-masing dirancang dengan tujuannya sendiri

Yang membuat keren Firewall adalah beda jenis Firewall, beda juga tempat kerjanya. Mereka beroperasi di **lapisan (*layer*) OSI Model yang berbeda-beda**. Ibaratnya, ada satpam yang hanya memeriksa KTP di pagar depan, ada juga yang sampai memeriksa isi tas sampai ke dalam-dalamnya di ruang VIP

Berikut adalah beberapa kategori Firewall yang wajib kita tahu:

### 1. Stateless Firewall (Satpam Pemalas)

*Stateless Firewall* ini ibarat **satpam pemalas** yang jaga di gerbang depan (Beroperasi di **OSI Layer 3 & 4**).
Dia kerja super cepat buat meriksa *header* paket data (ngecek IP dari mana, mau ke mana, dan lewat Port berapa). TAPI kelemahannya: dia **GAK PUNYA INGATAN**.
- Dia gak peduli paket ini kelanjutan dari koneksi yang wajar atau bukan.
- Kalau ada paket aneh diblokir hari ini, pas paket itu datang lagi besok, si firewall ini bakal nanya dari awal lagi karena lupa kalau kemarin paket itu udah diblokir.

### 2. Stateful Firewall (Satpam Rajin)

Beda dari yang *Stateless*, si **Stateful Firewall** ini satpamnya bawa buku catatan (*State Table*) kemana-mana (Beroperasi di **OSI Layer 3 & 4** juga).
- Kalau ada paket yang diizinin masuk, dia bakal catat koneksi itu. Jadi pas ada paket balasan dari saluran yang sama, dia bakal otomatis izinin masuk **tanpa perlu diinterogasi dari nol lagi**.
- Begitu juga kalau ada paket berbahaya yang dia usir, dia bakal catet cirinya buat nolak paket serupa di masa depan secara otomatis. Jauh lebih aman

### 3. Proxy Firewall (Asisten Kurir)

Nah ini beda kelas. *Proxy Firewall* (atau *Application-level Gateway*) ini kerjanya udah di ruangan VIP (**OSI Layer 7**).
Kelemahan *firewall* sebelumnya adalah mereka cuma cek "kulit/amplop"-nya doang, gak berani buka isinya. *Proxy* memecahkan masalah itu:
- Dia bertindak layaknya **Asisten Kurir Pribadi**. Pas kamu mau kunjungi server luar, si *Proxy* yang bakal repot-repot pergi nemuin server itu, ngambil datanya, lalu nganterin balik ke kamu.
- Karena dia yang ngambilin barang, dia bakal ngebongkar isinya dulu. Kalau isinya *malware*, langsung dia buang.
- Nilai plusnya: IP asli kamu jadi anonim/tersembunyi karena yang maju berhadapan sama internet adalah si *Proxy*.

### 4. Next-Generation Firewall (NGFW) (Unit Pasukan Khusus)

Ini adalah *Firewall Last Boss* sejuta umat zaman sekarang. NGFW beroperasi nyapu bersih dari **OSI Layer 3 sampai Layer 7**.
Dia bukan cuma satpam, tapi udah kayak **Sistem Pertahanan Militer Lengkap**:
- Punya fitur *Deep Packet Inspection* (mengecek paket sampai ke dalem-dalemnya).
- Punya sistem IPS (*Intrusion Prevention System*) buat mencegah ancaman/serangan secara *Real-Time*.
- Bisa nebak pola serangan dan mendeskripsi lalu lintas yang disamarkan (*SSL/TLS decryption*). Singkatnya: NGFW itu *All-in-One Security*

> **Note:**
> BONUS:

### Web Application Firewall (WAF)

Kalau *Firewall* biasa tugasnya mengamankan pintu server secara umum, WAF punya tugas spesifik: **Mengamankan Aplikasi Web/Website**. Dia berada di depan *web server* buat mencegah serangan-serangan peretas *web* tingkat tinggi kayak *SQL Injection* atau *Cross-Site Scripting (XSS)*.

### Ringkasan Karakteristik Firewall

Biar makin gampang buat *review* atau nentuin *firewall* mana yang pas buat dipakai, cek tabel di bawah ini:

| Tipe Firewall | Karakteristik Utama |
| :--- | :--- |
| **Stateless Firewall** | - Cuma bisa penyaringan dasar (*Basic filtering*)<br>- Gak punya rekaman koneksi sebelumnya (*No track*)<br>- Cocok buat jaringan yang butuh kecepatan tinggi karena kerjanya ngebut |
| **Stateful Firewall** | - Bisa mengenali lalu lintas data lewat pola (*Recognize traffic by patterns*)<br>- Bisa dikasih aturan yang lumayan rumit (*Complex rules*)<br>- Benar-benar memantau jaringan dan mencatat koneksi yang sedang jalan |
| **Proxy Firewall** | - Berani membongkar dan inspeksi isi paket datanya<br>- Punya fitur penyaringan konten (*Content filtering*)<br>- Pegang kendali penuh atas aplikasi keamanan<br>- Bisa mendekripsi dan inspeksi paket data yang disandikan pakai SSL/TLS |
| **Next-Generation Firewall**  | - Perlindungan paling mutakhir (*Advanced threat protection*)<br>- Udah bawaan punya sistem IPS (*Intrusion Prevention System*)<br>- Bisa menganalisa hal-hal aneh secara heuristik (berdasarkan kecerdasan buatan)<br>- Sama kayak Proxy, jago dekripsi dan inspeksi paket data SSL/TLS |
| **Web Application Firewall (WAF)** | - Khusus dipasang di depan Web Server buat melindungi Aplikasi Web / Website<br>- Fokus mencegah serangan *hacker* web spesifik kayak *SQL Injection* & *Cross-Site Scripting (XSS)* |

## Rules in Firewall

Firewall memberikan kita **kekuasaan penuh (*control*)** untuk mengatur *traffic* yang masuk dan keluar dari jaringan.

Meski dari *Firewall* sudah dibekali aturan keamanan standar, bagian paling seru untuk seorang Analis/Admin adalah kita bisa membuat **Aturan Sendiri (*Custom Rules*)** yang spesifik.

**Contoh Kasus (Skenario Akses SSH):**
Normalnya, buat menjaga server dari *hacker* , kita bakal pasang aturan: *"Firewall Tolak mentah-mentah (*Deny*) semua orang asing dari luar yang mencoba masuk ke server kita lewat jalur SSH (Port 22)."*

Tapi, masalah muncul kalau kebetulan kita (sebagai admin) sedang kerja *remote* dari *cafe* dan butuh akses SSH ke server itu. Di sinilah saksinya *Custom Rules* bekerja. Kita tinggal tambahin instruksi pengecualian:
*"Tolak semua akses SSH **KECUALI** kalau orang itu datang dari alamat IP `192.168.100.55` (Ini IP WiFi *cafe* tempat kita nongkrong). Kalau si IP itu yang minta masuk, bukain pintunya (*Allow*)"*

Intinya: **Aturan Firewall (*Firewall Rules*) itu adalah hukum mutlak buatan sendiri.** Kita yang menentukan siapa yang harus ditolak, dan siapa pengecualian yang boleh masuk.

Setiap kali kita bikin satu hukum/aturan buat si Firewall, ada **6 komponen dasar** yang wajib kita tentukan ini dia isinya:

1. **Source Address (IP Asal):** Siapa tamunya? Ini adalah alamat IP perangkat si pengirim data.
2. **Destination Address (IP Tujuan):** Mau ketemu siapa di dalam? Ini adalah alamat IP perangkat target yang mau dituju.
3. **Port (Pintu Jalur):** Mau lewat pintu nomor berapa? (Misal: Port 80 buat HTTP, Port 22 buat SSH).
4. **Protocol (Bahasa Komunikasi):** Mereka bakal komunikasi pakai bahasa apa? (TCP, UDP, ICMP, dll).
5. **Action (Status Eksekusi):** Perintah terakhir buat si firewall. Kalau semua ciri-ciri di atas terpenuhi, mau diapakan paketnya? Diizinkan masuk, atau ditendang keluar
6. **Direction (Arah Lalu Lintas):** Aturan ini diatur untuk orang luar yang mau **masuk** (*Inbound / Ingress*) atau orang dalem yang mau **keluar** dari jaringan kita (*Outbound / Egress*)?

### Type of Actions

Komponen *Action* adalah Keputusan yang diambil *firewall* setelah mencocokkan paket data dengan daftar aturan. Ada 3 jenis keputusan utama yang bisa kita tentukan:

1. **Allow (Silahkan Lewat):**
   Ini adalah lampu hijau. Kalau paket datanya di-*Allow*, *firewall* bakal membukakan pintu dan membiarkan paket itu masuk dengan aman.
   *Contoh Implementasi:* Mengizinkan semua karyawan di jaringan kita agar bisa *browsing* *website* (HTTP) ke Internet.
   | Action | Source (Asal) | Destination (Tujuan) | Protocol | Port | Direction (Arah) |
   | :---: | :---: | :---: | :---: | :---: | :---: |
   | **Allow** | `192.168.1.0/24` | Any (Mana saja) | TCP | 80 | Outbound |
   *(Cara Bacanya: "Firewall, tolong **Izinkan (Allow)** jaringan karyawan kita (`192.168.1.0/24`) jalan **Keluar (Outbound)** ke **Website Manapun (Any)** lewat **Port 80 (TCP)**.")*

2. **Deny (Tolak / Blokir):**
   Ini adalah lampu merah mutlak. Paket akan **ditolak dan diblokir** seketika (*bisa dengan cara di-Drop diam-diam atau di-Reject terang-terangan*). Ini adalah pondasi utama pengamanan server untuk mencegah *traffic* jahat/mencurigakan.
   *Contoh Implementasi:* Menolak semua *traffic* asing (Internet) yang mencoba masuk ke server (*Inbound*) lewat jalur *remote* SSH.
   | Action | Source (Asal) | Destination (Tujuan) | Protocol | Port | Direction (Arah) |
   | :---: | :---: | :---: | :---: | :---: | :---: |
   | **Deny** | Any (Siapa saja) | `192.168.1.0/24` | TCP | 22 | Inbound |
   *(Cara Bacanya: "Firewall, tolong **Blokir (Deny)** dari **Siapapun (Any)** yang mau **Masuk (Inbound)** ke jaringan kita (`192.168.1.0/24`) lewat jalur **Port 22 (TCP/SSH)**.")*

3. **Forward (Teruskan):**
   Nah, *Action* satu ini beda. Dia bukan cuma jaga pintu, tapi bertindak sebagai *Gateway/Router*. Kalau paket diberi label *Forward*, *firewall* bakal capture paket itu dan langsung **meneruskan paket** ke segmen jaringan lain di dalam.
   *Contoh Implementasi:* Meneruskan semua *traffic* pengunjung internet yang mau buka *port* web (80) langsung ke *Web Server* internal kita.
   | Action | Source (Asal) | Destination (Tujuan) | Protocol | Port | Direction (Arah) |
   | :---: | :---: | :---: | :---: | :---: | :---: |
   | **Forward** | Any (Siapa saja) | `192.168.1.8` | TCP | 80 | Inbound |
   *(Cara Bacanya: "Firewall, tolong **Teruskan (Forward)** pengunjung **Dari Mana Saja (Any)** yang mau **Masuk (Inbound)** lewat **Port 80 (TCP)**, langsung kirim ke komputer server kita di alamat `192.168.1.8`.")*

### Directionality of Rules (*Arah Lalu Lintas*)

Berdasarkan kemana data itu pergi, aturan *firewall* bisa dibagi lagi jadi 3 kategori besar yang gampang dibedakan:

1. **Inbound Rules (Aturan Masuk):**
   Ini aturan mutlak buat "Tamu dari Luar". *Rule* ini berfokus buat nyaring siapa saja pengunjung internet luar yang boleh menyentuh perangkat/jaringan internal kita.
   *Contoh:* Mengizinkan *traffic* masuk ke *port* 80 supaya orang-orang di luar sana bisa mengunjungi Web Server publik milik perusahaan kita.

2. **Outbound Rules (Aturan Keluar):**
   Ini kebalikannya, yaitu aturan khusus buat "Orang Dalem" yang mau keluar rumah. *Rule* ini mengatur perangkat di jaringan kita sendiri yang mencoba ngakses internet/dunia luar.
   *Contoh:* Memblokir semua komputer di kantor agar tidak bisa sembarangan keluar ngirim email (*Outbound Port 25 - SMTP*), **KECUALI** mesin *Mail Server* resmi kita. (Ini taktik cerdas biar kalau ada komputer karyawan yang kena virus, virusnya nggak bisa nyebar ngirim *spam email* ke luar!).

3. **Forward Rules (Aturan Teruskan):**
   Aturan ini ibarat jalur *bypass*/transit. Dibuat khusus buat mem- *forward* (meneruskan) *traffic* dari luar biar langsung nyampe ke server tujuan di dalam jaringan kita.
   *Contoh:* Kalau ada *traffic* HTTP (Port 80) yang datang bermaksud mau buka website, si Firewall bakal nangkap tu paket, terus langsung **meneruskan jalurnya** ke alamat IP *Web Server* internal kita.

## Windows Defender Firewall

Kita tidak perlu beli *hardware* mahal buat merasakan bagaimana *firewall* bekerja. Microsoft sudah berbaik hati memberikan kita **Windows Defender Firewall**, fitur *Security Guard* bawaan gratisan di semua OS Windows.

Fitur ini sudah cukup untuk mencakup semua fungsionalitas dasar seperti memblokir program mencurigakan, mengizinkan aplikasi tertentu, sampai membuat *Custom Rules* (*Inbound/Outbound*) seperti yang kita bahas di atas.

**Cara Mengakses Windows Defender Firewall:**
Cukup pencet tombol Windows + R di *keyboard*, terus ketik aja *"wf.msc"* di kolom *search*. Dari situ kita bisa mulai melakukan konfigurasi firewall.

### Network Profiles

Secara cerdas, OS Windows punya fitur bernama **Network Location Awareness (NLA)**. Fitur ini bisa mendeteksi kita lagi terhubung ke jaringan di mana, lalu otomatis menampilkan settingan security(*Firewall Profile*) yang paling pas buat tempat itu.

Secara *default*, ada dua profil yang dipakai si Firewall Windows:

1. **Private Networks (Jaringan Pribadi / Aman):**
   Ini profil santai. Aktif otomatis waktu kita connect ke WiFi rumah atau WiFi kantor yang emang sudah disetting *Trusted* (Dipercaya). Aturannya lebih longgar, karena komputer sekeliling kita adalah keluarga atau teman satu kerjaan (bisa *sharing printer*, *file sharing*, dll).

2. **Guest or Public Networks (Jaringan Publik / Rawan):**
   Ini profil restricted. Otomatis aktif waktu kita numpang WiFi gratisan di kafe, restoran, atau bandara. Di tempat ini, kita dikelilingi ribuan perangkat asing yang berpotensi jadi *hacker*.
   *Skenario:* Saat profil Publik ini aktif, *firewall* bisa kita atur untuk **Menutup Pintu Rapat-Rapat (Blokir total semua Inbound Connection)** ke perangkat kita, dan cuma mengizinkan kita buat akses internet (*Outbound*). Kerennya, saat kita pulang dan tersambung lagi ke WiFi rumah, aturan ketat ini bakal dicabut otomatis tanpa harus kita atur manual.

## Linux Firewall (Netfilter & iptables)

Kalau sebelumnya kita membahas firewall bawaannya Windows, sekarang gimana nasib *user* Linux? Tenang saja, Linux justru surganya *firewall*. Di dalam OS Linux, ada sebuah *framework* (kerangka kerja) canggih yang tertanam langsung di inti sistemnya (*kernel*). Namanya adalah **Netfilter**.

### Netfilter

*Netfilter* adalah mesin penggerak utama alias **pondasi dari semua fungsionalitas *firewall* di Linux** (mulai dari penyaringan paket data, *Network Address Translation/NAT*, sampai pelacakan jejak koneksi).

Karena *Netfilter* ini cuma sekadar mesin di belakang layar, kita butuh alat (*utilities*/aplikasi) buat bisa komunikasi dan memberi perintah ke mesin ini. Nah, berikut adalah 3 alat paling populer yang bertugas menerjemahkan perintah kita ke *Netfilter*:

1. **iptables:**
   Ini adalah pelopor dan alat *firewall* **paling populer & paling banyak dipakai** di berbagai distro Linux. Dia berinteraksi langsung sama *Netfilter* buat menyediakan ratusan fitur super canggih untuk mengontrol lalu lintas jaringan.
2. **nftables:**
   Ini adalah versi *Next-Gen* alias penerus sah dari `iptables`. Dia bawa kemampuan *filtering* dan *NAT* yang jauh lebih cepat dan canggih, tapi tetep berakar di atas pondasi *Netfilter*.
3. **firewalld:**
   Sama-sama nyuruh si *Netfilter*, tapi *utility* satu ini cara kerjanya beda. Dia didesain punya aturan *pre-defined* (aturan siap pakai) dan mengenalkan konsep Zona Jaringan (*Network Zones*) biar penggunanya tidak perlu pusing memikirkan aturan dari nol.

### UFW (Uncomplicated Firewall)

Sesuai namanya (*Uncomplicated* = tidak bikin ribet), **UFW** adalah alat penyelamat buat pemula atau admin yang males berurusan sama *syntax* `iptables` yang panjang dan rumit.

UFW pada dasarnya bertindak sebagai perantara (*frontend*) yang ramah pengguna. Apapun perintah simpel yang diketik di UFW, dia yang bakal menerjemahkan perintah itu jadi aturan `iptables` di belakang layar.

Berikut adalah beberapa command dasar UFW yang wajib diketahui (*Note: Butuh akses `sudo` atau root*):

1. **Mengecek Status Firewall:**
   Untuk cek Firewall lagi (*inactive*) atau lagi kerja (*active*).
   ```bash
   user@arch:~$ sudo ufw status
   Status: inactive
   ```

2. **Menyalakan / Mengaktifkan Firewall:**
   Untuk mengaktifkan Firewall (sekaligus bikin dia otomatis nyala tiap PC *restart*).
   ```bash
   user@arch:~$ sudo ufw enable
   Firewall is active and enabled on system startup
   ```

3. **Mematikan Firewall:**
   Untuk mematikan Firewall.
   ```bash
   user@arch:~$ sudo ufw disable
   Firewall stopped and disabled on system startup
   ```
