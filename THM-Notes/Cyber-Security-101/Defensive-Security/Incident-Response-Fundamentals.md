# TryHackMe: Incident Response Fundamentals

- **Room Link:** [Incident Response Fundamentals](https://tryhackme.com/room/incidentresponsefundamentals)
- **Kategori:** Defensive Security
- **Difficulty:** easy

## Task 1: Introduction to Incident Response

Bayangin kamu tinggal di jalan yang rawan kejahatan dengan banyak barang mahal di rumah. Pasti kepikiran buat punya security guard dan beberapa kamera CCTV di rumah. Menyembunyikan barang berharga di ruangan bawah tanah tersembunyi juga ide bagus kalau ada penyusup yang berhasil masuk ke rumah. Ini semua langkah-langkah yang kamu rencanain buat keamanan rumah, bahkan sebelum serangan terjadi.

Selain langkah-langkah proaktif ini, pernah gak kamu pikirin gimana semuanya bakal berjalan kalau seseorang berhasil melewati mekanisme keamanan eksternal dan dapet akses ke rumahmu? Kamu juga harus ngambil beberapa langkah lain setelah rumahmu diserang.

Ayo bawa konsep ini ke dunia digital. Kamu mungkin pernah denger tentang serangan cyber terhadap organisasi yang bikin mereka rugi ribuan dolar. Banyak kasus kayak gini yang dilaporin setiap hari di internet. Ini disebut sebagai **Cyber Security Incidents**. Sama kayak skenario di atas, di mana kamu merencanakan keamanan rumahmu, cyber security incidents juga butuh perencanaan dan sumber daya buat menghindari kerugian besar.

Incident Response menangani sebuah insiden dari awal sampai akhir. Mulai dari deploy keamanan di berbagai area buat mencegah insiden, sampe melawan insiden tersebut dan meminimalisir dampaknya — incident response adalah panduan yang menyeluruh.

Room ini bakal bantu kamu memahami konsep-konsep penting dari incident response dan ngasih kesempatan buat menyelesaikan insiden pertamamu secara praktis.

### Learning Objectives

- Overview tentang apa itu insiden dan severity level-nya
- Jenis-jenis insiden yang umum terjadi
- Fase fase Incident Response dari framework SANS dan NIST
- Tools buat Incident Detection dan Response beserta peran PlayBooks
- Incident Response Plan

## Task 2: What are Incidents?

Berbagai proses (interaktif & background) jalan di perangkat kita dan menghasilkan **events**. Events ini dimasukkin ke **security solutions** sebagai **logs** buat mendeteksi aktivitas berbahaya. Tantangan sebenarnya dimulai setelah security solution berhasil mendeteksi aktivitas mencurigakan dan memicu _alert_.

Tim keamanan bakal menganalisis _alerts_ ini:

- **False Positive:** Alert yang keliatan bahaya, tapi ternyata aman. (Contoh: Alert transfer data gede → ternyata cuma proses backup ke cloud).
- **True Positive:** Alert yang bener-bener berbahaya. (Contoh: Alert email masuk → ternyata email Phishing buat eksploitasi user).

True Positive alerts inilah yang disebut sebagai **Incidents (Insiden)**.

### Incident Severity

Kalau ada banyak insiden terjadi bersamaan, tim keamanan butuh prioritas berdasarkan dampaknya (_severity level_):
- **Low / Medium / High / Critical**
- Skala **Critical** selalu jadi prioritas tertinggi buat ditangani duluan.

<p align="center">
<img src="../../Assets/Images/Severity.png" alt="Incident Severity Levels">
</p>

* *Contoh*: Dari gambar di atas, **Title** merujuk ke ID tiket dari sebuah alert/insiden. Tim SOC akan memprioritaskan penyelesaian tiket **TKS-21** dan **MCA-36** terlebih dahulu karena statusnya **Critical**, sedangkan tiket dengan status Low seperti IAM-01 bisa ditunda pengerjaannya.

## Task 3: Types of Incidents

Insiden keamanan siber itu ada banyak jenisnya, bukan cuma sebatas hacking biasa. Berbagai jenis insiden ini bisa terjadi secara mandiri atau serentak pada satu korban.

- **Malware Infections:** Malware itu program jahat yang dirancang buat merusak sistem, jaringan, atau aplikasi. Mayoritas insiden keamanan berhubungan dengan malware infection. Malware biasanya menyebar lewat file (teks, dokumen berlampiran jahat, file `.exe`, dll).
- **Security Breaches:** Terjadi saat orang yang gak punya izin (unauthorized) berhasil mengakses data rahasia. Ini bahaya banget karena banyak bisnis yang bergantung sama data penting ini.
- **Data Leaks:** Kebocoran informasi rahasia ke pihak yang gak berwenang. Attacker biasanya pake data bocor ini buat **memeras korban** atau **ngerusak reputasi** target mereka. Beda dengan Security Breaches (yang disengaja), Data Leaks kadang bisa terjadi cuma karena **human error** atau **salah konfigurasi**.
- **Insider Attacks:** Insiden yang berasal dari *dalam* organisasi itu sendiri. Contohnya: Karyawan yang lagi marah/kecewa nancepin Flashdisk isi malware ke komputer di hari terakhir dia kerja. Serangan ini **sangat berbahaya** karena orang dalem pastinya punya akses (privilege) yang jauh lebih besar ke resource perusahaan dibanding outsider.
- **Denial of Service (DoS) Attacks:** _Availability_ (Ketersediaan) adalah salah satu dari 3 pilar utama cyber security (CIA Triad). Percuma ngelindungin data kalau datanya gak bisa diakses. DoS attack adalah insiden di mana attacker nge-banjiri sistem/jaringan dengan *request palsu* bertubi-tubi. Akibatnya, resource server habis dan user asli yang sah (legitimate users) jadi gak bisa ngakses layanannya.

Tiap insiden ini punya potensi ngerusak yang unik, dan dampaknya gak bisa dipukul rata. Insiden yang kecil di satu perusahaan bisa jadi malapetaka di perusahaan lain. **Contoh:** Perusahaan A mungkin gak terlalu kerasa rugi kalau kena Data Leak (karena datanya gak penting), tapi mereka bisa rugi besar kalau kena **DoS Attack** karena bisnis utama mereka bergantung dari ngelayanin _customer_ lewat _website_.

---

### Questions

- Apa bedanya insiden Security Breaches dengan Data Leaks?
- Kenapa Insider Attack dianggap lebih berbahaya dari pada serangan dari luar (pihak eksternal)?
- Apa contoh paling umum dari insiden Denial of Service (DoS)?




