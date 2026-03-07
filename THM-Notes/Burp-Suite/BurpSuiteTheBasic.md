# TryHackMe: Burp Suite Basics

---

- **Room Link:** [Burp Suite Basics](https://tryhackme.com/room/burpsuitebasics)
- **Category:** Web Security / Tools
- **Difficulty:** Easy

---

## Overview

*Room* ini ngasih pondasi ilmu soal cara kerja **Burp Suite**, alat yang wajib menyala tiap harinya di laptop seorang *Pentester* atau *Bug Hunter*. Di sini kamu bakal diajari gimana triknya "berdiri di tengah-tengah" jalan tol antara *browser* si korban dan *server* web, murni buat membajak, menyetop, dan memanipulasi *traffic* data sesuai niat liarmu.

---

## Membedah Dashboard

*Dashboard* itu pusat kendali (*command center*) di Burp Suite. Kalau kamu kebetulan lagi buka versi **Community 2026.1.1**, fitur ini bakal menonjolkan layar ringkasan bagian penting di sistem kamu:

- **Task Log**: Ibarat buku absen. Menampilkan riwayat *log* dari *task/crawler* otomatis yang lagi (atau barusan kelar) jalan.
- **Issue Activity**: Ini nih layar favorit, tempat nongolnya temuan kerentanan keamanan (pasnya sih buat versi Pro, kalau di versi Community cuma ngasih tau letak temuan minor aja).
- **Shortcut sakti**: Pencet `Ctrl + Shift + D` buat langsung loncat balik ke tab *Dashboard* kapan pun lagi nyasar.

---

## Kekuatan Utama: The Burp Proxy

Nah, fitur *Proxy* ini adalah nyawanya Burp Suite. Dia memerankan posisi penipu ulung alias *Man-in-the-Middle*:

- **Interception**: Kekuatannya buat menghadang/menyita *request* HTTP di tengah wara-wiri, menahan alurnya *sebelum* sempet nembus nyampe ke *server*. Mau mengubah data tiket pesawat jadi gratis? Di sinilah tempat mengedit nominalnya.
- **HTTP History**: Kalau kamu lagi males menahan, dia bertugas diem-diem sbg tukang catat *log* semua riwayat paket *request* yang berlalu-lalang buat bahan kamu menyusun analisis ntar malem.
- **Intercept is on/off**: Berwujud toogle *switch/button* simpel buat mutusin nasib trafik—mau kamu tahan atau biarkan mengalir bebas (*off*).

---

## Target Scoping & Site Map

Di belantara internet, kalau kamu ngelepas rante proxy, Burp bakal mencatat smua sampah trafik latar (kayak paket *telemetry* Google atau *update window*s). Biar fokus ga ambyar, kamu butuh The Scope.

- **Site Map**: Fitur visual keren membentuk jaring laba-laba. Dia ngasih gambaran peta struktur folder hirarki dan dahan file yang terintip terang dari web aplikasi targetmu.
- **Scope**: Kekang pembatas lu lho! Bikin *traffic log* kamu disaring (di-*filter*) super bersih. Burp cuma bakal pasang mata dan telinga menangkap gerak-gerik trafik dari domain/URL yang emang udah disahkan jadi wilayah sasarannya doang.

---

## Mengulik Manual pake Repeater

Kalo *Proxy* tujuannya buat menghadang, *Repeater* itu adalah bengkel rahasia favorit buat analis yang hobi mengulik racikan parameter racun (*payload*) pelan-pelan:

- **Function**: Daripada cape mondar-mandir mengeklik *reload/refresh* di muka Google Chrome, fitur ini membantu kamu menekan tombol "Gass Kirim (*Send*)" ke sebuah *request* secara berulang-ulang pakai sisa modifikasi parameter yang tiap bijinya lo ganti beda-beda.
- **Shortcut Ninja**: Cukup tekan `Ctrl + R` (buat menendang lemparan *request* mentahannya dari *Proxy* biar pindah kamar ke *Repeater* secara kilat).

---

## Kenalan sama Sang Intruder

Kalau capek mengirim manual pakai *Repeater*, *Intruder* turun panggung menangani serbuan otomatisnya di level skala *brute-force* ringan:

- **Gaya Jurus Payload (Types)**: Sniper (menembak brutal ke *satu titik* inceran lho doang), Battering Ram, Pitchfork (menyerang beberapa sela tapi barengan teratur), dan Cluster Bomb (Tipe paling edan mengetes semua kombinasi kombinatorik gila dari berbagai *payload* silang).
- **Throttling (Aturan Cekik)**: Camkan baik-baik bro, kalau modal kamu cuma versi pak tani/gratisan (**Community Edition**), kecepatan tempur tembakan Intruder dibatasi secara sadis (*throttled* lemot abis) sama pemilik app.

---

## Decoder, Comparer, & BApp Store

*Skill set* perkakas reparasi sampingan tapi jasanya gede selangit bikin alat ini ga kekejar aplikasi tetangga:

- **Decoder**: Mesin giling andalan buat bongkar pasang (*encoding/decoding*) bongkahan rantai teks (kayak Base64, serpihan URL `%20`, sampe ASCII *Hex*) bolak-balik dalam hitungan detik tanpa lu harus mikir buka *tab* Google.
- **Comparer**: Sesuai namanya, matanya jeli buat mencari letak perbedaan (*diffing* visual) dari jejeran dua *response* HTTP panjang. Mantap abis buat menemukan perbedaan satu huruf atau lamanya durasi respon server (kepake banget buat menganalisa *blind SQL injection*!).
- **Extender (BApp Store)**: Semacem *Play Store*-nya alat *hacker*. Toko modding lho buat belanja dan menempelkan ekstensi *plugin* dewa macem **Logger++** atau **Turbo Intruder** buat mendongkrak otot Burp Suite biar berasa versi berbayar.

---

## Attack Flow Awareness

Burp Suite ini mutlak memegang peranan panggung utama pas kamu masuk di tahap pendaftaran target (**Reconnaissance**) dan jebolin pintu depan (**Initial Access**) pas menghadapi aplikasi web:

| Fase Retas | Jatah Kerjanya Burp Suite |
| :--- | :--- |
| **Reconnaissance** | Mengandalkan *Site Map* buat meraba-raba dan memetakan rancang bangun seluruh pelosok kota aplikasi web, membantu menemukan ruang rahasia (*endpoint*) yang sengaja disembunyikan dari publik. |
| **Initial Access** | Mengawinkan kekuatan *Proxy* + *Repeater* buat mengorak-ngarik parameter, menemukan, sampe sukses mengeksekusi kerentanan gawat (kayak menendang *SQLi*, menitip *XSS*, mengintip data *IDOR*, dll). |
| **Post-Exploitation** | Kerjaan membongkar lencana otentikasi macem *session tokens* dan deretan *cookies* buat mencuri identitas Admin (*Privilege Escalation*). |

**Gimana Blue Team (Bek) Melihat Ini:**
- **WAF (Web Application Firewall):** Satpam digital ini matanya awas. Dia bisa mengendus dan ngamuk kalau liat wujud pola rentetan *request* brutal nan ga wajar yang dimuntahkan dari *Burp Intruder* (kayak misal ribuan *request* aneh masuk ke satu alamat *login* cuma dalam sedetik).
- **Rate Limiting:** Trik jitu musuh buat membatasi/mengerem leher jumlah laju akses *request* per alamat IP biar serangan otomatis lo (*brute-force*) jalan kayak siput.
- **Logging:** Sadar diri! Segala macem pergerakan ga masuk akal yang lewat celah *server* itu pasti otomatis tercatat (*logged*) mati buat bahan bukti lab forensik penyidik kalau perusahaan tau-tau kena musibah.

---

## Real-World Relevance

- **Bug Bounty:** Udah rahasia umum, Burp Suite itu ibarat pedang Excalibur buat para *Bug Hunter*. Kalau kamu mengintip *platform* tajir macem HackerOne atau Bugcrowd, dipastikan 90% laporan maut di sana itu hasil nemu pakai *combo Proxy* + *Repeater*.
- **Web App Pentest:** Di taruhan karir profesional, seorang *pentester* jelas pakai Burp Suite Pro (berbayar sultan) buat menyalakan *active vulnerability scanning* secara masif bin otomatis, sebelom gilirannya dia turun gunung validasi temuan itu manual *exploit* pakai *Repeater*.
- **OWASP Top 10:** Bisa dibilang ampir seluruh penyakit wajib yang nangkring di OWASP Top 10 (SQLi, XSS, Broken Access Control, dll) itu bisa banget dipancing keluar dari kandangnya sekaligus dibantai cuma pakai satu aplikasi ini lho.

---

## Quick Check (Active Recall)

1. Kalau logikanya jalan, kenapa sih ilmu menangkap (*understanding*) bahasa dasar protokol HTTP itu jadi fondasi mutlak sebelum tangannya boleh lincah buka-buka Burp Suite?
2. Emangnya seberapa membantu sih fitur nyeting ***Scope*** target di Burp Suite pas kamu lagi ditugasin merampok aplikasi web skala raksasa gajah keliling?
3. Di kondisi nyebelin kek *Blind SQL Injection* (tempat *database*-nya buta dan cuma respon jeda waktu), kenapa lu wajib berterima kasih sama adanya fitur **Comparer**?
