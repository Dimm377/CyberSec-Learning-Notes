# TryHackMe: OWASP Top 10 (2025)

- **Room Link:** [OWASP Top 10 (2025): IAAA Failures](https://tryhackme.com/room/owasptop102025iaaafailures)
- **Category:** OWASP Top 10 (2025)
- **Difficulty:** Easy

---

## Introduction

Room ini akan membedah 3 kategori dari **OWASP Top 10 (2025)** yang berkaitan dengan kegagalan dalam penerapan **Identity, Authentication, Authorisation, dan Accountability (IAAA)** pada aplikasi.

Kamu akan mempelajari teori dan langsung mempraktikkannya melalui tantangan (*challenges*) yang tersedia. Kategori yang akan dibahas meliputi:

1. **A01: Broken Access Control**
2. **A07: Authentication Failures**
3. **A09: Logging & Alerting Failures**

Room ini dirancang untuk pemula dan tidak memerlukan pengetahuan keamanan sebelumnya.

---

## What is IAAA ?

**IAAA** adalah cara sederhana untuk memahami bagaimana pengguna dan tindakan mereka diverifikasi dalam aplikasi. Setiap kategori berperan sangat penting dan kamu **tidak bisa melompati levelnya**. Jika level sebelumnya tidak dilakukan, maka level berikutnya tidak bisa dijalankan.

Keempat item tersebut adalah:

* **Identity (Identitas):** Akun unik (misal: user ID/email) yang mewakili seseorang atau layanan. -> *Siapa kamu?*
* **Authentication (Autentikasi):** Membuktikan identitas tersebut (misal: password, OTP, passkeys). -> *Buktikan kalau itu benar kamu*
* **Authorisation (Autorisasi):** Apa yang boleh dilakukan oleh identitas tersebut. -> *Kamu boleh ngapain aja di sini?*
* **Accountability (Akuntabilitas):** Mencatat dan memberi peringatan tentang siapa melakukan apa, kapan, dan dari mana. -> *Siapa yang mencatat jejakmu?*

Tiga kategori dari **OWASP Top 10:2025** yang dibahas di room ini berkaitan dengan kegagalan dalam penerapan IAAA. Kelemahan di sini bisa sangat fatal karena memungkinkan penyerang untuk mengakses data pengguna lain atau mendapatkan hak akses lebih (*privilege*) dari yang seharusnya mereka miliki.

---

## A01: Broken Access Control

**Broken Access Control** terjadi ketika server tidak memeriksa dengan benar **siapa yang boleh mengakses apa** di setiap permintaan. Masalah ini muncul karena aplikasi terlalu percaya pada input dari sisi pengguna (*client*).

Celah yang paling umum di kategori ini adalah **IDOR (Insecure Direct Object Reference)**.

### Apa itu IDOR?
Bayangkan kamu sedang melihat data akunmu dan muncul parameter di URL seperti `?accountID=7`. Jika kamu iseng mengganti angkanya menjadi `?accountID=6` dan tiba-tiba kamu bisa melihat atau bahkan mengedit data orang lain, itulah IDOR.

### Tipe-Tipe Privilege Escalation
Dalam praktiknya, kegagalan kontrol akses ini terbagi menjadi dua:

1.  **Horizontal Privilege Escalation:**
    *   Mengakses data milik pengguna lain yang level atau perannya sama denganmu.
    *   **Analogi:** Kamu penyewa apartemen, dan kamu bisa masuk ke unit tetanggamu yang juga penyewa biasa.
2.  **Vertical Privilege Escalation:**
    *   Meloncat dari pengguna biasa ke tindakan yang hanya boleh dilakukan oleh admin.
    *   **Analogi:** Kamu penyewa biasa, tapi tiba-tiba bisa masuk ke ruang panel utama gedung atau ruang manajer.

**Ingat:** Jika kamu bisa memanipulasi ID di URL untuk melihat data sensitif (misalnya mencari user yang punya saldo lebih dari $1 juta), berarti sistem tersebut memiliki celah keamanan yang serius.

---

## A07: Authentication Failures

Jika *Access Control* (A01) bicara tentang apa yang boleh kamu lakukan, maka **Authentication** bicara tentang membuktikan siapa kamu. **Authentication Failures** terjadi ketika aplikasi tidak bisa memverifikasi identitas pengguna dengan andal.

### Masalah Umum pada Autentikasi:
*   **Username Enumeration:** Penyerang bisa menebak apakah sebuah username ada di database atau tidak (misal melalui pesan error yang berbeda).
*   **Weak Passwords:** Penggunaan password yang gampang ditebak dan tidak adanya sistem penguncian (*account lockout*) setelah beberapa kali gagal login.
*   **Logic Flaws:** Celah dalam alur login atau registrasi.
*   **Insecure Session Handling:** Penanganan cookie atau sesi yang tidak aman, sehingga bisa dicuri atau dimanipulasi.

### Contoh Kasus: Account Confusion
Ini adalah cara licik untuk mengelabui aplikasi agar memberikan akses ke akun orang lain.
*   **Skenario:** Kita tahu ada user bernama `admin`. Kita mencoba mendaftar akun baru dengan nama yang sangat mirip, misalnya `aDmiN`.
*   **Kenapa ini berhasil?** Jika aplikasi tidak menstandarisasi penulisan username (misalnya mengubah semuanya jadi huruf kecil sebelum disimpan) atau tidak mengecek keunikan secara mendalam, aplikasi mungkin akan bingung dan menganggap kamu adalah admin asli saat kamu login.

Ini adalah bentuk kegagalan serius dalam tahap **Authentication** pada model IAAA.

---

## A09: Logging & Alerting Failures

Pernah lihat gedung yang punya CCTV tapi tidak ada rekamannya atau tidak ada satpam yang memantau? Itu adalah gambaran **Logging & Alerting Failures**.

Dalam keamanan siber, **Logging** adalah pondasi dari **Accountability** (Akuntabilitas). Artinya, kita harus bisa membuktikan: *siapa melakukan apa, kapan, dan dari mana.*

### Apa yang Terjadi Jika Logging Gagal?
Tanpa catatan yang baik, tim keamanan (*defenders*) tidak bisa mendeteksi atau menyelidiki serangan. Kegagalan ini biasanya terlihat seperti:
*   **Missing Authentication Events:** Tidak mencatat kapan seseorang login atau logout.
*   **Vague Error Logs:** Log yang terlalu umum sehingga tidak memberikan informasi berguna.
*   **No Alerting on Brute-force:** Sistem diam saja saat ada ribuan kali kegagalan login atau perubahan hak akses yang mencurigakan.
*   **Short Retention:** Catatan log dihapus terlalu cepat sebelum sempat diselidiki.
*   **Tampering Risks:** Log disimpan di tempat yang bisa dijangkau dan diubah oleh penyerang untuk menghapus jejak mereka.

### Kenapa Ini Bahaya?
Bayangkan ada penyerang masuk ke sistem. Jika tidak ada log yang mencatat aktivitas mereka, kita tidak akan pernah tahu:
1.  Dari mana asal serangan (IP address)?
2.  Akun mana saja yang sudah dikompromi?
3.  Data sensitif apa yang sudah diakses atau dicuri?

Tanpa akuntabilitas yang kuat, sebuah aplikasi ibarat rumah tanpa pintu yang bisa dimasuki siapa saja tanpa meninggalkan jejak.

---

## Conclusion

Room **OWASP Top 10 (2025): IAAA Failures** ini mengajarkan kita bahwa mengamankan identitas dan akses bukan cuma soal pasang password. 

Kita harus memastikan:
1.  **Access Control** yang ketat agar user tidak bisa saling intip data (**Authorisation**).
2.  **Authentication** yang kuat agar identitas tidak mudah dipalsukan atau dikelabui.
3.  **Logging & Alerting** yang sigap agar setiap tindakan mencurigakan terekam dan bisa ditindaklanjuti (**Accountability**).

Penerapan **IAAA** yang solid adalah kunci utama untuk menjaga aplikasi dari penyalahgunaan akses yang fatal.
