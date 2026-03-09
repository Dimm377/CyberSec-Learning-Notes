# TryHackMe: Security Principles

- **Room Link:** [Security Principles](https://tryhackme.com/room/securityprinciples)
- **Category:** Cyber Security 101 - Build Your Cyber Security Career
- **Difficulty:** Easy

---

### Learning Objectives
Di *room* ini, kita akan membedah pondasi utama cybersecurity, yaitu:
- Memahami fungsi keamanan: **CIA Triad** (*Confidentiality, Integrity, Availability*).
- Mengenal kebalikan dari CIA, yaitu **DAD Triad** (*Disclosure, Alteration, Destruction/Denial*).
- Mengenal model keamanan dasar seperti **Bell-LaPadula**.
- Mempelajari prinsip keamanan penting: **Defense-in-Depth**, **Zero Trust**, dan **Trust but Verify**.
- Mengenal standar **ISO/IEC 19249**.
- Memahami perbedaan antara **Vulnerability**, **Threat**, dan **Risk**.

---

## 1. Introduction

Saat ini, kata **Security** atau keamanan sudah menjadi semacam **Istilah populer** di perusahaan teknologi. Hampir semua perusahaan berani mengklaim bahwa layanan atau produk mereka itu aman. Tapi pertanyaannya: **Aman buat siapa?**

Sebelum kita masuk ke teori-teori prinsip keamanan yang kompleks, hal pertama yang wajib kamu pahami adalah **siapa lawan yang sedang kamu hadapi**. 

### Balita vs Mata-mata Industri

Bayangkan kamu sedang menjaga sebuah laptop, strategi keamanan yang kamu berikan akan sangat bergantung pada siapa lawan yang sedang kamu hadapi:

1. **Kasus A (Lawan Balita):** Kamu cuma mau mencegah seorang balita (anak kecil) supaya nggak asal pencet *keyboard* dan menghapus tugas sekolahmu. 
   - *Solusi:* Cukup ditaruh di tempat tinggi atau pakai *password* sederhana saja sudah sangat aman.
2. **Kasus B (Lawan Mata-mata):** Laptop kamu berisi desain rahasia teknologi senilai miliaran rupiah, dan targetnya adalah **Mata-mata Industri** yang profesional.
   - *Solusi:* Memakai cara yang sama seperti kasus A tentu sudah masuk kategori konyol/nggak cukup, di sini kamu butuh **Appropriate Security Controls** (kontrol keamanan yang sesuai porsinya), seperti:
     - Enkripsi data tingkat tinggi (biar datanya nggak bisa dibaca kalau dicuri).
     - Authentikasi berlapis (*Multi-Factor Authentication*) menggunakan biometric atau kunci fisik.
     - Tim **Security Operations Center (SOC):** Seperti memiliki tim detektif yang memantau laptopmu 24 jam sehari dari kejauhan, mereka akan langsung bereaksi jika ada pergerakan atau akses yang tidak wajar.
     
     

### 100% Security Is a Myth

Satu prinsip yang paling keras di dunia *cyber security* adalah: **Tidak ada sistem yang 100% aman**

Sistem sekuat apa pun pasti punya celah, entah itu di kodenya, di orang yang mengelolanya, atau di perangkat kerasnya. Karena itulah, tujuan kita bukan membangun **"sistem yang tidak bisa dibobol"**, melainkan memperkuat **Security Posture** (kondisi keamanan kita).

Tujuannya? Untuk membuat biaya dan waktu yang harus dikeluarkan oleh lawan (*Adversary*) menjadi sangat mahal dan menyulitkan, sehingga mereka berpikir ulang untuk menyerang.

**Gembok Rumah vs Maling Pro**
Bayangkan rumahmu, kamu bisa pasang pagar tinggi, CCTV, kabel berduri, hingga satpam 24 jam
- **Apakah rumahmu 100% aman?** Tidak, kalau ada agen rahasia sekelas *Mission Impossible* pakai helikopter dan alat tercanggih di dunia, mereka tetap bisa masuk.
- **Lalu untuk pasang itu semua?** Supaya maling biasa menyerah saat melihat pertahananmu yang rumit, lalu dia memilih pindah ke rumah tetangga yang pagarnya terbuka lebar.

**Inti Pembelajaran:**
Sangatlah lucu kalau kamu menerapkan cara pengamanan **balita** untuk menghadapi **mata-mata profesional**. Karena itulah, **mengenal lawan (Adversary)** adalah langkah wajib supaya kita bisa mempelajari pola serangan mereka dan menerapkan **Kontrol Keamanan** yang memang sesuai porsinya.

## The CIA Triad

Sebelum kita mengeklaim suatu sistem itu "aman", kita perlu membedah apa saja unsur yang membentuk keamanan tersebut. Di dunia *cybersecurity*, ada tiga tiang utama yang menjadi standar penilaian, yaitu **CIA Triad**:

### 1. Confidentiality (Kerahasiaan)
Menjamin bahwa data hanya bisa diakses oleh orang yang memang berhak.

- **Contoh 1: Resep Rahasia Perusahaan.** Perusahaan seperti KCF atau Coca-Cola punya resep rahasia yang cuma diketahui segelintir orang, Kalau resepnya bocor ke saingan, mereka rugi besar.
- **Contoh 2: Chat/DM Pribadi.** Saat kamu kirim pesan curhat ke teman lewat aplikasi chat, pesan tersebut di-enkripsi agar cuma kamu dan temanmu yang bisa baca. Pihak lain (termasuk hacker atau penyedia layanan) tidak boleh tahu isinya.

### 2. Integrity (Integritas)
Menjamin bahwa data tidak bisa diubah secara tidak sah, dan kita bisa mendeteksi jika ada perubahan yang terjadi.

- **Contoh 1: Saldo Rekening Bank.** Bayangkan kamu punya saldo 100 miliar, tiba-tiba besok berubah jadi 100 juta tanpa ada transaksi apa pun, Itu artinya integritas datanya dirusak, Saldo harus tetap benar dan tidak boleh berubah secara tidak sah.
- **Contoh 2: Isi Surat Keputusan (SK).** Jika isi sebuah surat keputusan resmi pemerintah diubah satu kata saja (misal dari "Ditolak" jadi "Disetujui") oleh pihak luar, maka seluruh sistem birokrasi bisa kacau.

### 3. Availability (Ketersediaan)
Menjamin bahwa sistem atau layanan selalu siap digunakan kapan pun dibutuhkan oleh pengguna yang sah.

- **Contoh 1: Nomor Darurat (112).** Saat ada kebakaran atau keadaan darurat, nomor telepon darurat harus bisa dihubungi setiap detik, Kalau sistem teleponnya mati, nyawa orang bisa jadi taruhannya.
- **Contoh 2: Layanan Video Streaming.** Bayangkan malam minggu kamu sedang mau menonton film favorit, tapi aplikasinya terus-menerus *loading* atau servernya *down*, Pengguna pasti merasa dirugikan karena layanan yang mereka bayar tidak tersedia saat dibutuhkan.

---

### Penting: Skala Prioritas CIA
Tiga unsur di atas tidak selalu memiliki beban prioritas yang sama. Tergantung pada apa yang sedang kita amankan, terkadang salah satu unsur jauh lebih penting dibanding yang lain.

**Contoh: Pengumuman Kelulusan Kampus**
- **Confidentiality:** Tidak terlalu penting, karena pengumumannya memang bersifat publik.
- **Integrity:** **Sangat Kritis.** Jangan sampai nama orang yang tidak lulus diubah menjadi lulus di daftar pengumuman.
- **Availability:** Cukup penting agar semua mahasiswa bisa melihat hasilnya tanpa kendala teknis.

---

## Authenticity & Non-repudiation

Selain tiga pilar CIA, ada dua konsep tambahan yang sangat krusial agar sebuah sistem bisa dipercaya:

### 1. Authenticity (Keaslian)
Memastikan bahwa data atau transaksi benar-benar berasal dari sumber yang asli, bukan palsu atau hasil manipulasi.

- **Contoh:** Jika sebuah perusahaan menerima pesanan 1000 unit mobil, mereka harus yakin kalau pesanan itu benar-benar dari pembeli asli, bukan *prank* atau pesanan fiktif dari kompetitor.

### 2. Non-repudiation (Anti penyangkalan)
Memastikan bahwa pengirim data atau pelaku transaksi tidak bisa menyangkal perbuatannya di kemudian hari.

- **Contoh:** Masih di kasus pesanan 1000 mobil tadi. Setelah mobil dikirim, jangan sampai si pembeli bisa bilang, "Eh, saya nggak pernah pesan mobil itu kok". Sistem harus punya bukti kuat (seperti tanda tangan digital) yang membuat mereka tidak bisa mengelak.

---

## Parkerian Hexad

Pada tahun 1998, Donn Parker mengusulkan **Parkerian Hexad**, yaitu kumpulan enam elemen keamanan. Empat di antaranya sudah kita bahas (Availability, Integrity, Authenticity, Confidentiality). Mari kita bahas dua elemen sisanya:

### 1. Utility (Kegunaan)
Fokus pada seberapa bermanfaat informasi tersebut. Data mungkin ada, tapi kalau tidak bisa digunakan, nilainya nol (*no utility*).

- **Contoh:** Kamu punya laptop dengan penyimpanan terenkripsi. Kamu masih punya laptopnya, disk-nya utuh (masih ada *Availability*), tapi kalau kamu **kehilangan kunci dekripsinya**, datanya jadi tidak berguna. Kamu punya datanya, tapi tidak bisa dibaca.

### 2. Possession (Kepemilikan/Kendali)
Fokus pada perlindungan informasi dari pengambilan, penyalinan, atau pengendalian oleh pihak yang tidak sah.

- **Contoh 1:** Seorang penyerang mencuri *backup drive* (hardisk cadangan) milikmu. Meskipun kamu mungkin masih punya data aslinya di laptop, fakta bahwa orang lain memegang kendali atas salinan datamu berarti kamu telah kehilangan *Possession*.
- **Contoh 2:** *Ransomware*, Penyerang mengenkripsi datamu dan meminta tebusan. Meskipun datanya masih ada di komputermu, kamu kehilangan kendali untuk mengakses atau mengelolanya.

## DAD
