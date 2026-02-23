# TryHackMe: SOC Fundamentals

- **Room Link:** [SOC Fundamentals](https://tryhackme.com/room/socfundamentals)
- **Kategori:** Defensive Security
- **Difficulty:** easy

## Task 1: Introduction to SOC

Seiring teknologi makin canggih, tanggung jawab kita buat ngejaga keamanan data juga ikut membesar. Sekarang, data penting (yang biasa disebut _secrets_ atau rahasia perusahaan) nggak lagi disimpan di lemari arsip fisik, semuanya udah jadi data digital. Satu organisasi aja bisa nyimpen jutaan data rahasia di jaringan dan sistem mereka.

Kalau data ini sampai diubah, hilang, atau diakses sama pihak yang nggak berhak, kerugiannya bisa luar biasa gede, baik dari sisi finansial maupun reputasi (Bayangin aja kalau sebuah bank kehilangan data nasabahnya). Di sisi lain, _Threat Actors_ (Hacker Jahat) setiap hari terus cari cara baru buat mengeksploitasi celah-celah keamanan di jaringan.

Praktik keamanan cyber tradisional (kayak cuma pasang antivirus atau _firewall_ biasa) udah nggak cukup buat nahan serangan modern. Makanya, punya tim khusus yang fokus mengelola keamanan data organisasi itu jadi hal yang super penting. Tim inilah yang disebut **SOC**.

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

Fokus utama tim SOC itu sederhana: pastikan kemampuan **Detection dan Response** selalu siap tempur. Buat mencapai hal itu, tim SOC dibekali berbagai _security solutions_ berlapis. Solusi-solusi ini tugasnya menghubungkan seluruh jaringan perusahaan dan sistem-sistem penting supaya bisa dipantau dari satu _dashboard_ pusat. Lewat monitoring terpusat yang jalan terus 24/7 inilah, tim SOC bisa mendeteksi dan menetralisir insiden keamanan sebelum jadi bencana.

![SOC Operations Diagram](../../Assets/Images/SOC.png)

### Detection

- **Detect vulnerabilities:**
  Kerentanan (_vulnerability_) itu ibarat lubang atau titik lemah di sistem yang bisa dimanfaatkan penyerang buat menerobos masuk. Lubang ini bisa sembunyi di mana aja, mulai dari OS _server_ sampai aplikasi yang di-_install_ di komputer. Contohnya, tim SOC mungkin nemu sekelompok komputer Windows di jaringan yang belum di-_patch_ buat nutup _Exploit_ publik terbaru. Walaupun urusan _patching_ **bukan** tugas langsung SOC, tapi kerentanan yang dibiarkan terbuka bakal bikin keamanan seluruh perusahaan jadi lemah. Makanya, SOC wajib menyoroti risiko ini.

- **Detect unauthorized activity:**
  Bayangin ada penyerang (_Threat Actor_) yang berhasil nyolong _username_ dan password salah satu karyawan, lalu pakai kredensial itu buat nyusup ke jaringan perusahaan. SOC harus bisa ngedeteksi aktivitas ilegal kayak gini (_Unauthorized Activity_) secepat mungkin sebelum data berhasil dicuri. Petunjuk-petunjuk logis seperti lokasi IP yang nggak wajar (misalnya login dari negara yang nggak biasa) bisa jadi sinyal kuat buat mesin deteksi otomatis SOC.

- **Detect intrusions:**
  Intrusi (_Intrusion_) itu intinya akses ilegal yang berhasil masuk ke sistem atau jaringan perusahaan. Contoh klasiknya: _attacker_ berhasil nge-_exploit_ celah keamanan di aplikasi web yang terhubung ke internet. Contoh lain yang nggak kalah fatal: pengguna internal tanpa sadar buka situs berbahaya, dan perangkat mereka langsung kena _malware_.

### Response

- **Support with the incident response:**
  Begitu ancaman atau insiden keamanan terdeteksi valid, langkah-langkah taktis langsung dijalankan buat merespons serangan tersebut (_Incident Response_). Fokus utamanya adalah meminimalkan kerusakan dan melacak akar penyebab kejadian (_Root-Cause Analysis_). Di sini, tim SOC berperan sebagai pendukung yang sangat penting, mereka nyediain data intelijen ancaman dan hasil deteksi buat bantu _Incident Response Team_ bekerja.

### The Trinity Of SOC

Ada tiga pilar utama yang jadi pondasi sebuah SOC. Kalau ketiga pilar ini jalan bareng dengan kompak, tim SOC bisa berkembang jadi lini pertahanan yang matang (_mature_) dan super efisien dalam mendeteksi serta merespons insiden keamanan. Tiga pilar itu adalah: Manusia (_People_), Proses (_Processes_), dan Teknologi (_Technology_).

![3 Pillars of SOC](../../Assets/Images/3P.png)

_People_, _Processes_, dan _Technology_ itu satu paket yang nggak bisa dipisahkan di dalam SOC. Pertahanan yang kuat lahir dari kombinasi ketiganya: orang-orang profesional (_People_) yang ngoperasiin alat keamanan canggih (_Technology_), sambil ngikutin prosedur yang sudah dirancang dengan matang (_Processes_). Kematangan (_Maturity_) sebuah tim SOC diukur dari seberapa kompak ketiga elemen ini jalan bareng.

Task berikutnya akan membahas masing-masing pilar ini satu per satu dan mempelajari bagaimana pilar-pilar tersebut merupakan bagian penting dari SOC.

## Task 3: People

Mau se-canggih apa pun otomatisasi di dunia Cyber Security, peran Manusia (_People_) di SOC tetap yang paling penting. Solusi keamanan otomatis memang bisa menangkap ribuan _alerts_ setiap detiknya. Tapi masalahnya, sistem ini juga sering banget ngebangkitkan _False Positives_ (peringatan palsu), yang ujung-ujungnya malah bikin kebisingan luar biasa padat di SOC, fenomena ini disebut _Alert Fatigue_. Di sinilah naluri manusia dibutuhkan buat memvalidasi mana ancaman beneran dan mana yang cuma _noise_.

Bayangkan kamu adalah anggota tim pemadam kebakaran yang bertugas memantau sebuah dashboard pemantauan pusat dari seluruh alarm pendeteksi api di kota. Misalkan secara kebetulan muncul rentetan peringatan darurat secara membabi buta dari ratusan lokasi yang berbeda beda. Saat kamu mengerahkan seluruh unit untuk menuju ke lokasi-lokasi tersebut, sangat mengecewakan ketika mengetahui bahwa mayoritas alarm tersebut hanyalah asap biasa dari aktivitas memasak warga, bukan kebakaran sungguhan. Terlalu banyak peringatan palsu seperti ini pada akhirnya hanya akan membakar habis tenaga, waktu, serta sumber daya operasional tim secara sia-sia. Hal yang persis sama kerap terjadi pada sistem deteksi SOC sebelum adanya validasi analitis dari manusia.

Kalau di lapangan, sebuah solusi keamanan yang dibiarkan jalan sendiri tanpa campur tangan manusia (_Human Intervention_), tim SOC pasti bakal sibuk ngurusin hal-hal sepele dan peringatan yang nggak relevan. Manusia (_People_) dibutuhkan buat ngarahin sistem otomatis tadi supaya fokus ke aktivitas yang beneran berbahaya, jadi _Incident Response_ bisa dijalankan dengan cepat dan tepat sasaran.

Orang-orang yang berdiri di balik pilar ini dikenal sebagai Tim SOC (_The SOC Team_). Tim ini punya struktur peran (_Roles_) dan tanggung jawab (_Responsibilities_) masing-masing:

![SOC Roles Diagram](../../Assets/Images/People-SOC.png)

- **SOC Analyst (Level 1):** Semua peringatan yang ditangkap _security solution_ bakal disaring duluan sama analis lini pertama ini. Mereka itu _first responders_ buat setiap deteksi. Tugasnya melakukan _alert triage_ dasar, milah mana yang beneran bahaya dan mana yang cuma _False Positive_, terus laporin ke jalur eskalasi yang tepat.
- **SOC Analyst (Level 2):** Kalau analis L1 udah selesai nyaring, beberapa deteksi mungkin butuh investigasi lebih dalam. Analis L2 turun tangan buat nge-_correlate_ log data dari berbagai sumber intelijen dan nemuin insiden keamanan yang sebenarnya.
- **SOC Analyst (Level 3):** Analis L3 itu para ahli berpengalaman yang secara proaktif berburu ancaman (_Threat Hunting_) dan jadi komando utama saat _Incident Response_ kritis. Deteksi level kritis biasanya butuh respons yang kompleks, mulai dari _containment_ (lokalisasi penyebaran), _eradication_ (pembasmian ancaman), sampai _recovery_ (pemulihan sistem). Di titik ini, jam terbang tinggi analis L3 sangat diandalkan.
- **Security Engineer:** Para _Engineer_ ini yang nge-_deploy_ dan ngonfigurasi seluruh infrastruktur _security solution_ yang dipakai para analyst. Fokus mereka murni buat menjaga semua mesin pertahanan tetap jalan lancar di balik layar.
- **Detection Engineer:** _Security Rules_ itu ibarat tulang punggung logika yang bikin solusi keamanan bisa ngelacak aktivitas peretas secara akurat. Meski analis L2 dan L3 sering bikin aturan deteksi sendiri, beberapa tim SOC lebih milih punya satu orang khusus (_Detection Engineer_) yang fokus meracik dan mempertajam _Rules_ deteksi ini.
- **SOC Manager:** SOC Manager yang ngatur jalannya _Processes_ dan support manajerial buat seluruh anggota tim. Manager SOC juga jadi jembatan utama ke level eksekutif, yaitu **CISO** (_Chief Information Security Officer_), buat ngelaporin kondisi pertahanan (_Security Posture_) organisasi.

> **Note:** jumlah dan variasi susunan peran di dalam tim SOC ini sejatinya bersifat fleksibel, dapat membesar atau menyusut secara dinamis tergantung seberapa masif dan rentan sistem infrastruktur dari corporate yang mereka lindungi.

---

**Answer the questions:**

- Apa istilah untuk fenomena "kebisingan" luar biasa padat akibat tumpukan ribuan peringatan rentan _False Positives_ yang dihaasilkan oleh solusi keamanan otomatis?
- Mengapa sebuah sistem deteksi SOC tetap mutlak membutuhkan pilar Manusia (_Human Intervention_) ketimbang beroperasi 100% secara otomatis?
- Siapakah _first responders_ yang bertugas melakukan _alert triage_ dasar terhadap setiap deteksi awal untuk menyaring mana yang _False Positive_?
- Siapakah analis yang turun tangan menyelami investigasi lebih mendalam dengan mengkorelasikan (_correlate_) log data dari berbagai sumber?
- Siapakah analis berpengalaman yang memburu ancaman rumit secara proaktif (_Threat Hunting_) dan menjadi komando utama saat eksekusi _Incident Response_ kritis berlangsung?
- Profesi apa yang memegang kendali atas urusan _deployment_ dan kelancaran arsitektur infrastruktur solusi keamanan di belakang layar SOC?
- Jabatan apa yang difokuskan khusus secara independen untuk meracik dan mempertajam kerangka logika deteksi serangan alias _Security Rules_?
- Apa gelar sang pengerak tim yang bukan cuma mengatur roda _Processes_, tapi juga jadi diplomat eksekutif ke ranah CISO?

## Task 4: Processes

Sebelumnya, kita udah bahas berbagai peran dan tanggung jawab orang-orang di Tim SOC. Kayak mesin, setiap peran itu butuh Prosedur atau SOP (_Processes_) yang spesifik biar bisa jalan. Contohnya, analis SOC Level 1 sebagai _first responder_ harus ngejalanin proses _Triage alert_ buat validasi tingkat bahaya suatu keanehan. kita dalami beberapa kerangka Proses penting yang jadi roda pertahanan di SOC.

### Alert Triage

_Alert triage_ adalah pondasi operasional utama dari sebuah tim SOC. Langkah pertama yang selalu dieksekusi terhadap peringatan keamanan apa pun adalah melakukan alert triage. Proses ini berpusat pada analisis spesifik terhadap peringatan yang muncul guna mengklasifikasikan tingkat keparahannya (_severity_) dan membantu analis mengurutkan skala prioritas penanganannya. Inti dari melakukan _alert triage_ yang sukses adalah kemampuan untuk menemukan jawaban atas kaidah **5 W** (_5 Ws_). Apa sajakah 5 W tersebut?

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

Setiap peringatan berbahaya yang berhasil dideteksi dan divalidasi harus segera dilaporkan ke analis tingkat atas (seperti analis L2 atau L3) agar bisa mendapatkan penanganan dan resolusi yang tepat waktu (_timely response_). Peringatan-peringatan mentah ini akan diubah statusnya menjadi **tiket eskalasi** (_tickets_) dan ditugaskan secara spesifik kepada personel yang relevan. Praktik terbaik dalam meracik sebuah laporan ancaman adalah dengan mencantumkan penjabaran menyeluruh dari metodologi **5 Ws** yang digabungkan dengan ulasan analisis yang mendalam. Selain itu, wajib disertakan pula berbagai tangkapan layar (_screenshots_) sebagai bukti autentik dari pemicu atau aktivitas mencurigakan tersebut.

### Incident Response and Forensics

Dalam beberapa kasus, peringatan yang dilaporkan bisa jadi merujuk pada aktivitas eksploitasi berbahaya yang berskala sangat kritis. Saat skenario terburuk ini terjadi, tim tingkat atas (seperti analis L3 atau _Incident Responder_) akan memulai manuver Tanggap Darurat (_Incident Response_). Pada titik ini pula, aktivitas forensik digital (_Forensics Analysis_) yang terperinci tidak jarang ikut dilibatkan. Objektif utama dari Forensic operation ini adalah membongkar seluk beluk akar penyebab insiden (_Root Cause_) dengan cara menganalisis sisa-sisa jejak (_Artifacts_) yang tertinggal di dalam mesin komputer maupun di network traffic.

## Task 5: Technology

Punya _People_ yang ahli dan _Processes_ yang solid saja belum cukup tanpa adanya _security solutions_ (solusi keamanan) yang mumpuni. Pilar _Technology_ di dalam SOC merujuk langsung pada perangkat dan solusi keamanan itu sendiri. Solusi-solusi ini bertugas meminimalkan pekerjaan manual tim SOC dalam mendeteksi dan merespons ancaman secara efisien.

Jaringan sebuah organisasi terdiri dari banyak sekali perangkat dan aplikasi. Kalau tim keamanan harus mendeteksi dan merespons ancaman di setiap _device_ atau aplikasi satu per satu secara manual, itu bakal makan tenaga dan sumber daya yang luar biasa besar. Di sinilah _security solutions_ berperan: mereka mengumpulkan seluruh informasi dari perangkat dan aplikasi yang ada di jaringan ke satu titik pusat, lalu mengotomatisasi kemampuan deteksi dan respons.

Mari kita kenali beberapa _security solutions_ utama yang jadi senjata andalan tim SOC:

- **SIEM (Security Information and Event Management):** SIEM adalah tool paling populer yang hampir pasti ada di setiap lingkungan SOC. Tool ini mengumpulkan log dari berbagai perangkat jaringan (yang disebut _log sources_), kemudian mencocokkannya dengan _detection rules_ yang sudah dikonfigurasi di dalamnya. _Rules_ ini berisi logika untuk mengidentifikasi aktivitas mencurigakan. Kalau ada yang cocok (_match_), SIEM langsung kasih alert ke tim SOC. SIEM modern bahkan sudah melampaui pendekatan _rule-based_ biasa, mereka sudah dilengkapi _user behavior analytics_ dan kemampuan _threat intelligence_, ditambah algoritma _machine learning_ yang bikin deteksinya makin tajam.

> **Note:** SIEM hanya menyediakan kemampuan **Detection** di dalam ekosistem SOC.

- **EDR (Endpoint Detection and Response):** EDR memberikan visibilitas _real-time_ dan historis yang sangat detail terhadap aktivitas di setiap _endpoint_ (komputer, laptop, server). Tool ini beroperasi langsung di level _endpoint_ dan bisa menjalankan respons otomatis (_automated responses_). EDR punya kemampuan deteksi yang luas untuk _endpoint_, jadi kamu bisa menginvestigasi insiden secara mendalam dan meresponsnya cuma dalam beberapa klik.
- **Firewall:** Firewall murni berfungsi untuk keamanan jaringan (_network security_). Dia bertindak sebagai tembok pembatas antara jaringan internal dan jaringan eksternal (seperti Internet). Firewall memantau _traffic_ yang masuk dan keluar, lalu menyaring _traffic_ yang tidak sah (_unauthorized_). Firewall juga punya _detection rules_ sendiri yang membantu mengidentifikasi dan memblokir _traffic_ mencurigakan sebelum sempat masuk ke jaringan internal.
