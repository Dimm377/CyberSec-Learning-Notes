# TryHackMe: Security Principles

- **Room Link:** [Security Principles](https://tryhackme.com/room/securityprinciples)
- **Category:** Cyber Security 101 - Build Your Cyber Security Career
- **Difficulty:** Easy

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
- **Lalu buat apa pasang itu semua?** Supaya maling biasa menyerah saat melihat pertahananmu yang rumit, lalu dia memilih pindah ke rumah tetangga yang pagarnya terbuka lebar.

**Inti Pembelajaran:**
Sangatlah lucu kalau kamu menerapkan cara pengamanan **balita** untuk menghadapi **mata-mata profesional**. Karena itulah, **mengenal lawan (Adversary)** adalah langkah wajib supaya kita bisa mempelajari pola serangan mereka dan menerapkan **Kontrol Keamanan** yang memang sesuai porsinya.
