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
