# TryHackMe: SOC Fundamentals

- **Room Link:** [SOC Fundamentals](https://tryhackme.com/room/socfundamentals)
- **Kategori:** Defensive Security
- **Difficulty:** easy

## Task 1: Introduction to SOC

Seiring dengan kemajuan teknologi, tanggung jawab kita dalam menjaga keamanan data juga semakin besar. Saat ini, data krusial (yang biasa disebut _secrets_ atau rahasia perusahaan) tidak lagi disimpan di dalam lemari arsip fisik, melainkan berbentuk data digital. Sebuah organisasi bisa mengelola berjuta juta data konfidensial di dalam jaringan dan sistem mereka.

Semua bentuk modifikasi, kehilangan, maupun tindakan yang tidak sah pada data ini mampu menyebabkan kerugian financial maupun reputasi yang luar biasa besar (Bayangkan sebuah bank yang kehilangan data nasabahnya). Di sisi lain, _Threat Actors_ (Hacker Jahat) setiap harinya terus berevolusi menemukan cara baru untuk mengeksploitasi kerentanan di dalam jaringan tersebut.

Praktek keamanan cyber tradisional (seperti hanya memasang antivirus atau _firewall_ biasa) sudah tidak lagi cukup untuk menahan gelombang ancaman modern. Oleh karena itu, mendedikasikan sebuah tim khusus berkemampuan tinggi untuk mengelola keamanan data organisasi adalah hal yang sangat vital. Tim inilah yang disebut dengan **SOC**.

### Apa itu SOC?

**SOC** (_Security Operations Center_) adalah sebuah fasilitas terdedikasi yang dioperasikan oleh tim spesialis keamanan komputer.

Tujuan utama tim operasi keamanan ini adalah untuk:

1. **Memantau (Monitoring)** jaringan dan sumber daya organisasi secara terus-menerus (24 jam sehari, 7 hari seminggu).
2. **Mengidentifikasi** setiap keanehan atau gerak-gerik aktivitas mencurigakan.
3. Mencegah sebelum kerusakan (kerugian eksploitasi) benar-benar terjadi.

Secara singkat, mereka adalah garda terdepan dari lini pertahanan _blue team_ dalam ranah Cyber Security. disini kita akan menyelami key concept mengenai bagaimana sebuah SOC beroperasi, yang mana merupakan salah satu pilar fundamental di medan Defensive Security.

**(Learning Objectives):**

- Membangun pondasi pemahaman tentang SOC (_Security Operations Center_).
- Taktik Deteksi (_Detection_) dan Respon (_Response_) di dalam SOC.
- Memahami peran Orang (_People_), Proses (_Processes_), dan Teknologi (_Technology_) dalam ekosistem SOC.
- Latihan Hands-On

## Task 2: Purpose And Component

Fokus dari tim SOC adalah memastikan kapabilitas **Detection dan Response** tetap solid dan selalu siaga. Untuk mencapai tujuan ini, tim SOC dipersenjatai dengan berbagai solusi keamanan berlapis. Solusi-solusi ini bertugas untuk mengintegrasikan seluruh jaringan perusahaan serta sistem-sistem krusial agar dapat dipantau dari satu _dashboard_ central. Lewat pemantauan (monitoring) terpusat yang berjalan terus-menerus inilah, tim SOC mampu mendeteksi serta menetralisir setiap insiden keamanan sebelum berubah menjadi bencana.

![SOC Operations Diagram](../../Assets/Images/SOC.png)

### Detection

- **Detect vulnerabilities:**
  Kerentanan (_vulnerability_) adalah sebuah cacat keamanan atau titik lemah yang bisa disalahgunakan oleh penyerang untuk menembus batasan otorisasi sistem. Cacat ini bisa bersembunyi di mana saja, mulai dari sistem operasi _server_ hingga program aplikasi spesifik yang di-_install_ di komputer. Sebagai contoh, tim SOC mungkin mendeteksi adanya sekelompok komputer bersistem operasi MS Windows di dalam jaringan yang belum diperbarui (_patching_) untuk menambal sebuah _Exploit_ publik terbaru. Walaupun secara teknis proses perbaikan perangkat lunak (menambal kerentanan) **bukanlah** tugas murni dari SOC, tetapi kerentanan yang dibiarkan terekspos akan secara langsung memperlemah keamanan seluruh perusahaan. Oleh karena itu, SOC wajib menyoroti risiko ini.

- **Detect unauthorized activity:**
  Pertimbangkan sebuah ancaman krusial di mana penyerang (_Threat Actor_) berhasil meretas _username_ beserta kata sandinya dari salah satu karyawan yang sah, lalu menggunakannya untuk menyusup ke dalam sistem jaringan perusahaan. Sangat vital bagi SOC untuk mendeteksi aktivitas tak berizin semacam ini _(Unauthorized Activity)_ bergerak secepat kilat sebelum data di dalam jaringan berhasil dieksfiltrasi. Berbagai indikator logis, seperti ketidakwajaran geolokasi IP asal di mana login tersebut dilakukan, bisa dimanfaatkan sebagai petunjuk utama oleh mesin deteksi otomatis SOC.

- **Detect intrusions:**
  Intrusi (_Intrusion_) merujuk pada akses ilegal dan tidak berizin yang menembus sistem atau jaringan perusahaan. Skenario klasik dari intrusi adalah ketika _attacker_ berhasil mengeksploitasi celah keamanan pada aplikasi _web_ yang menghadap ke internet. Skenario fatal lainnya adalah ketika pengguna internal di dalam jaringan tanpa sengaja mengunjungi situs web berbahaya yang mengakibatkan perangkat mereka terinfeksi oleh perangkat lunak jahat (_malware_).

### Response

- **Support with the incident response:**
  Segera setelah sebuah ancaman atau insiden keamanan terdeteksi dengan valid, serangkaian langkah taktis langsung dieksekusi untuk merespons serangan tersebut (_Incident Response_). Fokus utama dari manuver tanggap darurat ini adalah untuk meminimalkan dampak kerusakan dan menelusuri akar penyebab (_Root-Cause Analysis_) terjadinya insiden. Dalam skenario ini, tim SOC memainkan peran pendukung yang sangat krusial dengan memberikan intelijen ancaman dan data dari mesin deteksi mereka untuk memandu (_Incident Response Team_) melakukan tugasnya.

### The Trinity Of SOC

Terdapat tiga buah pilar utama yang mendukung berdirinya sebuah SOC. Dengan beroperasinya ketiga pilar ini secara sinergis, tim SOC dapat berevolusi menjadi sebuah garda terdepan yang sangat mapan (_mature_) dan super efisien dalam mendeteksi sekaligus merespons segala bentuk insiden keamanan. Ketiga pilar krusial tersebut adalah Manusia (_People_), Proses (_Processes_), dan Teknologi (_Technology_).

![3 Pillars of SOC](../../Assets/Images/3P.png)

_People_, _Processes_ , dan _Technology_ hidup saling berdampingan dan tidak dapat dipisahkan di dalam ekosistem SOC. Sebuah pertahanan yang tangguh lahir murni dari ketiga pillar ini: Individu profesional (_People_) yang mengendalikan perangkat dan alat keamanan canggih (_Technology_), bergerak bersama-sama mengamankan jaringan berdasarkan tata letak prosedur taktis yang terkalkulasi dengan presisi (_Processes_). Kematangan (_Maturity_) sebuah tim SOC diukur dari seberapa solid mesin, orang, dan pedoman kerja ini bergerak bersamaan.

Task berikutnya akan membahas masing-masing pilar ini satu per satu dan mempelajari bagaimana pilar-pilar tersebut merupakan bagian penting dari SOC.

## Task 3: People

Terlepas dari masifnya evolusi otomatisasi yang telah mengambil alih sebagian besar tugas di Cyber security, peran Manusia (_People_) di dalam SOC akan selalu memegang kendali yang paling esensial. Teknologi dan solusi keamanan otomatis memang sanggup menjaring serta membunyikan ribuan deteksi tanda bahaya (_alerts_) setiap detiknya. Namun, sistem mentah ini tidak jarang memicu timbulnya _False Positives_ alias peringatan palsu, yang pada akhirnya malah menciptakan kebisingan yang luar biasa padat (_Alert Fatigue_) di dalam ekosistem SOC. Di sinilah insting manusia sangat dibutuhkan untuk memvalidasi mana ancaman yang sungguhan dan mana yang hanya sekadar _noise_ lalu bereaksi sesuai SOP.

Bayangkan kamu adalah anggota tim pemadam kebakaran yang bertugas memantau sebuah dashboard pemantauan pusat dari seluruh alarm pendeteksi api di kota. Misalkan secara kebetulan muncul rentetan peringatan darurat secara membabi buta dari ratusan lokasi yang berbeda beda. Saat kamu mengerahkan seluruh unit untuk menuju ke lokasi-lokasi tersebut, sangat mengecewakan ketika mengetahui bahwa mayoritas alarm tersebut hanyalah asap biasa dari aktivitas memasak warga, bukan kebakaran sungguhan. Terlalu banyak peringatan palsu seperti ini pada akhirnya hanya akan membakar habis tenaga, waktu, serta sumber daya operasional tim secara sia-sia. Hal yang persis sama kerap terjadi pada sistem deteksi SOC sebelum adanya validasi analitis dari manusia.

Dalam implementasi SOC di lapangan, jika sebuah solusi keamanan dibiarkan beroperasi sepenuhnya tanpa campur tangan dari manusia (_Human Intervention_), fokus tim pasti akan teralihkan untuk mengurus masalah-masalah sepele dan peringatan yang tidak relevan. Kehadiran pilar Manusia (_People_) mutlak dibutuhkan untuk memandu solusi keamanan otomatis tadi dalam menyaring aktivitas yang sungguh-sungguh berbahaya, sehingga eksekusi insiden respons (_Incident Response_) dapat diluncurkan secara cepat dan tepat sasaran.

Kumpulan orang-orang hebat yang berdiri di balik pilar ini secara umum dikenal sebagai Tim SOC (_The SOC Team_). Tim operasi ini terstruktur dengan memikul peran (Roles) serta tanggung jawab spesifik (_Responsibilities_) berikut:

![SOC Roles Diagram](../../Assets/Images/People-SOC.png)

- **SOC Analyst (Level 1):** Seluruh peringatan yang ditangkap oleh security solution akan disaring oleh analis lini pertama ini. Mereka adalah responden pertama (_first responders_) untuk setiap deteksi. Analis SOC Level 1 bertugas melakukan triad peringatan dasar (_alert triage_) demi memilah apakah suatu anomali berbahaya atau sekadar _False Positive_, lalu melaporkan ancaman yang presisi melalui jalur eskalasi yang seharusnya.
- **SOC Analyst (Level 2):** Ketika analis Level 1 selesai melakukan filtering lapisan pertama, beberapa deteksi mungkin menuntut investigasi yang lebih mendalam. Analis Level 2 turun tangan untuk menyelami investigasi lanjutan dengan mengkorelasikan (_correlate_) log data dari berbagai sumber intelijen untuk mendeduksi insiden keamanan yang sebenarnya.
- **SOC Analyst (Level 3):** Analis Level 3 adalah para ahli profesional sangat berpengalaman yang secara proaktif melakukan (_Threat Hunting_) dan memberi panduan taktis dalam aktivitas respons insiden (_Incident Response_). Deteksi berskala kritis kerap kali menuntut respons struktural yang kompleks, mulai dari melokalisasi penyebaran (_containment_), membasmi agen ancaman (_eradication_), hingga pemulihan sistem (_recovery_). Di titik ini, jam terbang tinggi sang analis Level 3 sangat diandalkan.
- **Security Engineer:** Para _Engineer_ bertugas melakukan (_deployment_) dan mengonfigurasikan seluruh arsitektur infrastruktur security solution yang dioperasikan oleh para analyst. Keahlian murni mereka ditujukan demi menjaga keandalan dan kelancaran operasi mesin pertahanan di balik layar.
- **Detection Engineer:** _Security Rules_ (Aturan Deteksi Keamanan) ibarat tulang punggung logika yang mensuplai otak solusi keamanan agar bisa secara akurat melacak aktivitas peretas. Meski analis Level 2 dan 3 sering kali merumuskan aturan deteksi secara mandiri, beberapa tim SOC independen lebih memilih menugaskan satu insinyur khusus (_Detection Engineer_) agar dapat fokus meracik serta mempertajam _Rules_ pendeteksian ini secara sempurna.
- **SOC Manager:** SOC Manager mengurus perputaran roda _Processes_ serta dukungan manajerial kepada anggota tim. Manager SOC wajib menjadi jembatan diplomatis utama ke ranah eksekutif, yaitu **CISO** (_Chief Information Security Officer_), untuk melaporkan perkembangan terkait tingkat kekuatan dan postur pertahanan (_Security Posture_) dari ekosistem organisasi mereka.

> **Note:** jumlah dan variasi susunan peran di dalam tim SOC ini sejatinya bersifat fleksibel, dapat membesar atau menyusut secara dinamis tergantung seberapa masif dan rentan sistem infrastruktur dari corporate yang mereka lindungi.

---

**Answer the questions below:**

- Apa istilah untuk fenomena "kebisingan" luar biasa padat akibat tumpukan ribuan peringatan rentan _False Positives_ yang dihaasilkan oleh solusi keamanan otomatis?
- Mengapa sebuah sistem deteksi SOC tetap mutlak membutuhkan pilar Manusia (_Human Intervention_) ketimbang beroperasi 100% secara otomatis?
- Siapakah _first responders_ yang bertugas melakukan _alert triage_ dasar terhadap setiap deteksi awal untuk menyaring mana yang _False Positive_?
- Siapakah analis yang turun tangan menyelami investigasi lebih mendalam dengan mengkorelasikan (_correlate_) log data dari berbagai sumber?
- Siapakah analis berpengalaman yang memburu ancaman rumit secara proaktif (_Threat Hunting_) dan menjadi komando utama saat eksekusi _Incident Response_ kritis berlangsung?
- Profesi apa yang memegang kendali atas urusan _deployment_ dan kelancaran arsitektur infrastruktur solusi keamanan di belakang layar SOC?
- Jabatan apa yang difokuskan khusus secara independen untuk meracik dan mempertajam kerangka logika deteksi serangan alias _Security Rules_?
- Apa gelar sang pengerak tim yang bukan cuma mengatur roda _Processes_, tapi juga jadi diplomat eksekutif ke ranah CISO?
