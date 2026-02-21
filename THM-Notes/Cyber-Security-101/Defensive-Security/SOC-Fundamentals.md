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

### Detection

- **Detect vulnerabilities:**
  Kerentanan (_vulnerability_) adalah sebuah cacat keamanan atau titik lemah yang bisa disalahgunakan oleh penyerang untuk menembus batasan otorisasi sistem. Cacat ini bisa bersembunyi di mana saja, mulai dari sistem operasi _server_ hingga program aplikasi spesifik yang di-_install_ di komputer. Sebagai contoh, tim SOC mungkin mendeteksi adanya sekelompok komputer bersistem operasi MS Windows di dalam jaringan yang belum diperbarui (_patching_) untuk menambal sebuah _Exploit_ publik terbaru. Walaupun secara teknis proses perbaikan perangkat lunak (menambal kerentanan) **bukanlah** tugas murni dari SOC, tetapi kerentanan yang dibiarkan terekspos akan secara langsung memperlemah keamanan seluruh perusahaan. Oleh karena itu, SOC wajib menyoroti risiko ini.

- **Detect unauthorized activity:**
  Pertimbangkan sebuah ancaman krusial di mana penyerang (_Threat Actor_) berhasil meretas _username_ beserta kata sandinya dari salah satu karyawan yang sah, lalu menggunakannya untuk menyusup ke dalam sistem jaringan perusahaan. Sangat vital bagi SOC untuk mendeteksi aktivitas tak berizin semacam ini _(Unauthorized Activity)_ bergerak secepat kilat sebelum data di dalam jaringan berhasil dieksfiltrasi. Berbagai indikator logis, seperti ketidakwajaran geolokasi IP asal di mana login tersebut dilakukan, bisa dimanfaatkan sebagai petunjuk utama oleh mesin deteksi otomatis SOC.

- **Detect intrusions:**
  Intrusi (_Intrusion_) merujuk pada akses ilegal dan tidak berizin yang menembus sistem atau jaringan perusahaan. Skenario klasik dari intrusi adalah ketika _attacker_ berhasil mengeksploitasi celah keamanan pada aplikasi _web_ yang menghadap ke internet. Skenario fatal lainnya adalah ketika pengguna internal di dalam jaringan tanpa sengaja mengunjungi situs web berbahaya yang mengakibatkan perangkat mereka terinfeksi oleh perangkat lunak jahat (_malware_).

### Response

- **Support with the incident response:**
  Segera setelah sebuah ancaman atau insiden keamanan terdeteksi dengan valid, serangkaian langkah taktis langsung dieksekusi untuk merespons serangan tersebut (_Incident Response_). Fokus utama dari manuver tanggap darurat ini adalah untuk memitigasi (meminimalkan) dampak kerusakan dan menelusuri akar penyebab (_Root-Cause Analysis_) terjadinya insiden. Dalam skenario ini, tim SOC memainkan peran pendukung yang sangat krusial dengan memberikan intelijen ancaman dan data dari mesin deteksi mereka untuk memandu (_Incident Response Team_) melakukan tugasnya.
