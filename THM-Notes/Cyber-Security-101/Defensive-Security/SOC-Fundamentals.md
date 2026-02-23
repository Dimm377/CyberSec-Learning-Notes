# TryHackMe: SOC Fundamentals

- **Room Link:** [SOC Fundamentals](https://tryhackme.com/room/socfundamentals)
- **Kategori:** Defensive Security
- **Difficulty:** easy

## Task 1: Introduction to SOC

Seiring teknologi makin canggih, tanggung jawab kita buat ngejaga keamanan data juga ikut membesar:

- Data penting (yang biasa disebut _secrets_ atau rahasia perusahaan) sekarang udah gak disimpen di lemari arsip fisik — semuanya udah jadi data digital.
- Satu organisasi aja bisa nyimpen jutaan data rahasia di jaringan dan sistem mereka.

Kalau data ini sampai diubah, hilang, atau diakses sama pihak yang gak berhak:

- Kerugiannya bisa luar biasa gede, baik dari sisi finansial maupun reputasi.
- Bayangin aja kalau sebuah bank kehilangan data nasabahnya.
- Di sisi lain, _Threat Actors_ (Hacker Jahat) setiap hari terus cari cara baru buat mengeksploitasi celah-celah keamanan.

Praktik keamanan cyber tradisional (kayak cuma pasang antivirus atau _firewall_ biasa) udah gak cukup buat nahan serangan modern. Makanya, punya tim khusus yang fokus mengelola keamanan data organisasi itu jadi super penting. Tim inilah yang disebut **SOC**.

### Apa itu SOC?

**SOC** (_Security Operations Center_) adalah sebuah fasilitas terdedikasi yang dioperasikan oleh tim spesialis keamanan komputer.

Tujuan utama tim operasi keamanan ini adalah untuk:

1. **Memantau (Monitoring)** jaringan dan sumber daya organisasi secara terus-menerus (24 jam sehari, 7 hari seminggu).
2. **Mengidentifikasi** setiap keanehan atau gerak-gerik aktivitas mencurigakan.
3. Mencegah sebelum kerusakan (kerugian eksploitasi) benar-benar terjadi.

Singkatnya, mereka adalah barisan terdepan pertahanan _blue team_ di dunia Cyber Security. Di sini kita bakal belajar konsep-konsep dasar tentang gimana SOC beroperasi, yang merupakan salah satu pilar paling penting di Defensive Security.

**(Learning Objectives):**

- Membangun pondasi pemahaman tentang SOC (_Security Operations Center_).
- Taktik Deteksi (_Detection_) dan Respon (_Response_) di dalam SOC.
- Memahami peran Orang (_People_), Proses (_Processes_), dan Teknologi (_Technology_) dalam ekosistem SOC.
- Latihan Hands-On

## Task 2: Purpose And Component

Fokus utama tim SOC itu sederhana: pastiin kemampuan **Detection dan Response** selalu siap tempur.

- Tim SOC dibekali berbagai _security solutions_ berlapis.
- Solusi-solusi ini ngehubungin seluruh jaringan perusahaan dan sistem penting supaya bisa dipantau dari satu _dashboard_ pusat.
- Lewat monitoring terpusat 24/7 inilah, tim SOC bisa mendeteksi dan menetralisir insiden keamanan sebelum jadi bencana.

![SOC Operations Diagram](../../Assets/Images/SOC.png)

### Detection

- **Detect vulnerabilities:**
  Kerentanan (_vulnerability_) itu ibarat lubang di sistem yang bisa dimanfaatin penyerang buat menerobos masuk.
  - Lubang ini bisa sembunyi di mana aja — dari OS _server_ sampe aplikasi yang di-_install_ di komputer.
  - Contoh: tim SOC nemu sekelompok komputer Windows yang belum di-_patch_ buat nutup _Exploit_ publik terbaru.
  - Walaupun _patching_ **bukan** tugas langsung SOC, kerentanan terbuka bakal bikin keamanan perusahaan lemah. Makanya, SOC wajib menyoroti risiko ini.

- **Detect unauthorized activity:**
  Bayangin ada penyerang yang berhasil nyolong _username_ dan password karyawan, lalu pake kredensial itu buat nyusup ke jaringan.
  - SOC harus bisa ngedeteksi aktivitas ilegal kayak gini (_Unauthorized Activity_) secepat mungkin.
  - Petunjuk kayak lokasi IP yang gak wajar (login dari negara yang gak biasa) bisa jadi sinyal kuat buat mesin deteksi otomatis SOC.

- **Detect intrusions:**
  Intrusi itu akses ilegal yang berhasil masuk ke sistem atau jaringan perusahaan.
  - Contoh: _attacker_ berhasil nge-_exploit_ celah keamanan di aplikasi web.
  - Contoh lain: pengguna internal tanpa sadar buka situs berbahaya, dan perangkat mereka langsung kena _malware_.

### Response

- **Support with the incident response:**
  Begitu ancaman terdeteksi valid, langkah-langkah taktis langsung dijalanin buat merespons serangan (_Incident Response_).
  - Fokus utamanya: meminimalkan kerusakan dan melacak akar penyebab kejadian (_Root-Cause Analysis_).
  - Tim SOC nyediain data intelijen ancaman dan hasil deteksi buat bantu _Incident Response Team_ bekerja.

### The Trinity Of SOC

Ada tiga pilar utama yang jadi pondasi sebuah SOC:

- **Manusia** (_People_)
- **Proses** (_Processes_)
- **Teknologi** (_Technology_)

Kalau ketiga pilar ini jalan bareng dengan kompak, tim SOC bisa berkembang jadi lini pertahanan yang matang (_mature_) dan super efisien.

![3 Pillars of SOC](../../Assets/Images/3P.png)

_People_, _Processes_, dan _Technology_ itu satu paket yang gak bisa dipisahin di dalam SOC:

- Pertahanan yang kuat lahir dari orang-orang profesional yang ngoperasiin alat keamanan canggih, sambil ngikutin prosedur yang udah dirancang matang.
- Kematangan (_Maturity_) sebuah tim SOC diukur dari seberapa kompak ketiga elemen ini jalan bareng.

Task berikutnya akan membahas masing-masing pilar ini satu per satu dan mempelajari bagaimana pilar-pilar tersebut merupakan bagian penting dari SOC.

## Task 3: People

Mau se-canggih apa pun otomatisasi, peran Manusia (_People_) di SOC tetap yang paling penting.

- Solusi keamanan otomatis emang bisa nangkep ribuan _alerts_ setiap detiknya.
- Tapi masalahnya, sistem ini juga sering banget ngebangkitin _False Positives_ (peringatan palsu).
- Ujung-ujungnya malah bikin kebisingan padat di SOC — fenomena ini disebut **_Alert Fatigue_**.
- Di sinilah naluri manusia dibutuhin buat validasi mana ancaman beneran dan mana yang cuma _noise_.

**Analogi:** Bayangin kamu anggota tim pemadam kebakaran yang mantau dashboard alarm di seluruh kota.

- Tiba-tiba muncul rentetan peringatan darurat dari ratusan lokasi.
- Kamu kerahin seluruh unit, tapi ternyata mayoritas alarm itu cuma asap dari aktivitas masak warga — bukan kebakaran.
- Terlalu banyak peringatan palsu cuma bakar habis tenaga, waktu, dan sumber daya tim secara sia-sia.
- Hal yang persis sama terjadi di sistem deteksi SOC sebelum ada validasi dari manusia.

Kalau solusi keamanan dibiarkan jalan sendiri tanpa campur tangan manusia (_Human Intervention_), tim SOC pasti sibuk ngurusin hal-hal sepele. Manusia dibutuhin buat ngarahin sistem otomatis supaya fokus ke aktivitas yang beneran berbahaya.

Orang-orang di balik pilar ini dikenal sebagai **Tim SOC** (_The SOC Team_). Tim ini punya struktur peran (_Roles_) dan tanggung jawab (_Responsibilities_) masing-masing:

![SOC Roles Diagram](../../Assets/Images/People-SOC.png)

- **SOC Analyst (Level 1):**
  _First responders_ buat setiap deteksi. Tugasnya lakuin _alert triage_ dasar — milah mana yang beneran bahaya dan mana yang cuma _False Positive_, terus laporin ke jalur eskalasi yang tepat.

- **SOC Analyst (Level 2):**
  Kalau L1 udah selesai nyaring, beberapa deteksi butuh investigasi lebih dalam. L2 turun tangan buat nge-_correlate_ log data dari berbagai sumber dan nemuin insiden yang sebenarnya.

- **SOC Analyst (Level 3):**
  Para ahli berpengalaman yang proaktif berburu ancaman (_Threat Hunting_) dan jadi komando utama saat _Incident Response_ kritis.
  - Deteksi level kritis butuh respons kompleks: _containment_ → _eradication_ → _recovery_.

- **Security Engineer:**
  Yang nge-_deploy_ dan ngonfigurasi seluruh infrastruktur _security solution_. Fokusnya menjaga semua mesin pertahanan jalan lancar di balik layar.

- **Detection Engineer:**
  Fokus meracik dan mempertajam _Security Rules_ — tulang punggung logika yang bikin solusi keamanan bisa ngelacak aktivitas peretas secara akurat.

- **SOC Manager:**
  Ngatur jalannya _Processes_ dan support manajerial buat seluruh tim. Juga jadi jembatan ke **CISO** (_Chief Information Security Officer_) buat ngelaporin kondisi pertahanan organisasi.

> **Note:** jumlah dan variasi susunan peran di dalam tim SOC ini sejatinya bersifat fleksibel, dapat membesar atau menyusut secara dinamis tergantung seberapa masif dan rentan sistem infrastruktur dari corporate yang mereka lindungi.

---

**Answer the questions:**

- Apa istilah buat fenomena "kebisingan" akibat tumpukan ribuan peringatan _False Positives_ yang dihasilkan solusi keamanan otomatis? → **?**
- Kenapa sistem deteksi SOC tetap butuh pilar Manusia (_Human Intervention_) ketimbang operasi 100% otomatis? → **?**
- Siapa _first responders_ yang lakuin _alert triage_ dasar buat nyaring _False Positive_? → **?**
- Siapa analis yang turun tangan buat investigasi lebih dalam dengan nge-_correlate_ log data? → **?**
- Siapa analis berpengalaman yang proaktif berburu ancaman (_Threat Hunting_) dan jadi komando utama saat _Incident Response_ kritis? → **?**
- Profesi apa yang megang kendali atas _deployment_ infrastruktur solusi keamanan di balik layar SOC? → **?**
- Jabatan apa yang fokus khusus buat meracik dan mempertajam _Security Rules_? → **?**
- Apa gelar penggerak tim yang ngatur _Processes_ dan jadi jembatan ke CISO? → **?**

## Task 4: Processes

Sebelumnya, kita udah bahas berbagai peran dan tanggung jawab orang-orang di Tim SOC.

- Kayak mesin, setiap peran itu butuh Prosedur atau SOP (_Processes_) yang spesifik biar bisa jalan.
- Contohnya, analis SOC Level 1 sebagai _first responder_ harus jalanin proses _Triage alert_ buat validasi tingkat bahaya.

Yuk kita dalami beberapa kerangka Proses penting yang jadi roda pertahanan di SOC.

### Alert Triage

_Alert triage_ itu pondasi operasional utama tim SOC — langkah pertama yang selalu dieksekusi buat setiap peringatan keamanan.

- **Tujuan:** Analisis peringatan buat klasifikasi tingkat keparahan (_severity_) dan urutin prioritas penanganan.
- **Kunci sukses:** Nemuin jawaban atas kaidah **5 W** (_5 Ws_).

![5 Ws of Alert Triage](../../Assets/Images/5WS.png)

Berikut ini adalah rangkaian pertanyaan 5W yang idealnya harus terjawab selama analis melakukan triad alert atas sebuah peringatan:

**Contoh Kasus Peringatan (_Alert_):** `Malware detected on Host: GEORGE PC`

| 5 Ws       | Penjelasan Kasus (_Answers_)                                                                                                                                                           |
| :--------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **What?**  | _Malicious file_ (berkas mencurigakan) berhasil terdeteksi bersarang di salah satu komputer _host_ di dalam jaringan organisasi.                                                       |
| **When?**  | File tersebut pertama kali terdeteksi hidup pada pukul `13:20`, tanggal `5 Juni 2024`.                                                                                                 |
| **Where?** | File ini ditemukan bersarang di dalam directory komputer milik _host_ bernama: `"GEORGE PC"`.                                                                                          |
| **Who?**   | File ini terdeteksi dieksekusi oleh pengguna (_user_) lokal bernama `George`.                                                                                                          |
| **Why?**   | Setelah diselidiki mendalam, file tersebut rupanya diunduh oleh `George` dari sebuah situs web penyedia _software_ bajakan bajakan demi mendapatkan _software_ berbayar secara gratis. |

### Reporting

Setiap peringatan berbahaya yang udah divalidasi harus segera dilaporkan ke analis tingkat atas (L2 atau L3).

- Peringatan mentah diubah jadi **tiket eskalasi** (_tickets_) dan ditugasin ke personel yang relevan.
- Praktik terbaik bikin laporan: cantumkan penjabaran **5 Ws** + ulasan analisis mendalam.
- Wajib sertakan _screenshots_ sebagai bukti autentik dari aktivitas mencurigakan.

### Incident Response and Forensics

Dalam beberapa kasus, peringatan yang dilaporkan bisa merujuk ke aktivitas eksploitasi yang sangat kritis.

- Saat skenario terburuk terjadi, analis L3 atau _Incident Responder_ mulai manuver **Tanggap Darurat** (_Incident Response_).
- Forensik digital (_Forensics Analysis_) juga sering dilibatkan.
- Tujuan utamanya: membongkar akar penyebab insiden (_Root Cause_) dengan analisis jejak (_Artifacts_) di mesin komputer maupun network traffic.

## Task 5: Technology

Punya _People_ yang ahli dan _Processes_ yang solid aja belum cukup tanpa _security solutions_ yang mumpuni. Pilar _Technology_ merujuk langsung ke perangkat dan solusi keamanan itu sendiri.

- Jaringan organisasi terdiri dari banyak perangkat dan aplikasi.
- Kalau harus deteksi dan respons ancaman satu per satu secara manual, itu bakal makan tenaga luar biasa besar.
- _Security solutions_ berperan ngumpulin seluruh informasi ke satu titik pusat, lalu otomatisin kemampuan deteksi dan respons.

Beberapa _security solutions_ utama yang jadi senjata andalan tim SOC:

- **SIEM (Security Information and Event Management):**
  Tool paling populer yang hampir pasti ada di setiap SOC.
  - Ngumpulin log dari berbagai perangkat jaringan (_log sources_).
  - Cocokkin sama _detection rules_ yang udah dikonfigurasi.
  - Kalau ada yang _match_, SIEM langsung kasih alert ke tim SOC.
  - SIEM modern udah dilengkapi _user behavior analytics_, _threat intelligence_, dan algoritma _machine learning_.

> **Note:** SIEM cuma nyediain kemampuan **Detection** di dalam ekosistem SOC.

- **EDR (Endpoint Detection and Response):**
  Kasih visibilitas _real-time_ dan historis yang detail terhadap aktivitas di setiap _endpoint_ (komputer, laptop, server).
  - Beroperasi langsung di level _endpoint_ dan bisa jalanin respons otomatis.
  - Bikin kita bisa investigasi insiden mendalam dan respons cuma dalam beberapa klik.

- **Firewall:**
  Berfungsi buat keamanan jaringan (_network security_) — tembok pembatas antara jaringan internal dan eksternal.
  - Mantau _traffic_ masuk dan keluar, lalu saring _traffic_ yang gak sah.
  - Punya _detection rules_ sendiri buat identifikasi dan blok _traffic_ mencurigakan.
