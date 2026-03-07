# TryHackMe: REMnux Getting Started

- **Room Link:** [REMnux Getting Started](https://tryhackme.com/room/remnuxgettingstarted)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

## Introduction

Menganalisis software yang berpotensi berbahaya (_malware_) bisa jadi tugas yang menegangkan, terutama saat terjadi insiden keamanan (_security incident_). Analis diburu waktu untuk memberikan hasil seakurat mungkin tanpa membahayakan sistem yang ada.

Di sinilah **REMnux** berperan.

### What is REMnux?

**REMnux** adalah distro Linux khusus yang dirancang khusus untuk keperluan **Reverse Engineering Malware**.

Analogi simpelnya: Kalau Kali Linux adalah toolkit untuk anak _Offensive Security_ (Red Team) buat ngebobol sistem, maka REMnux adalah **"meja operasi"** untuk anak _Defensive Security_ (Blue Team / Malware Analyst) buat membedah virus.

Apa keunggulannya?
- Sudah ter-install ratusan _tools_ untuk analisis memori, jaringan, dan bedah file statis. (Contoh: Volatility, YARA, Wireshark, oledump, INetSim).
- Dirancang seperti lingkungan _sandbox_ yang aman untuk mengeksekusi dan mengobservasi software berbahaya tanpa resiko menginfeksi komputer pribadimu.
- Siap pakai (_ready to go_) — gak perlu pusing install tools satu per satu secara manual.

### Learning Objectives

Setelah menyelesaikan room ini, kamu akan menguasai cara:
- Eksplorasi _tools_ bawaan di dalam REMnux VM.
- Menggunakan _tools_ untuk menganalisis dokumen berbahaya (_malicious documents_) secara efektif.
- Mensimulasikan jaringan palsu (_fake network_) untuk mengelabui malware selama analisis.
- Familiar dengan _tools_ yang digunakan untuk menganalisis _memory images_.

---

## File Analysis Tools

Saat sedang menghadapi insiden keamanan, sering banget musuhnya nyamar jadi *file* kantoran biasa (*Excel, Word, PDF*). Nah, di REMnux, kamu punya banyak pisau bedah khusus buat nanganin ginian.

### Membedah Dokumen Jahat dengan `oledump.py`

Pernah dapet email berlampiran *Excel* yang tiba-tiba bikin komputer lemot? Kemungkinan besar *file* itu menyimpan *malware* berbasis *Macro*. Buat ngebongkarnya dengan aman secara statis (tanpa perlu membuka aplikasinya), kamu bisa pakai **`oledump.py`**.

**Apa itu OLE2?**
Kalo kamu bingung, OLE2 (*Object Linking and Embedding*) itu teknologi jadul buatan Microsoft. Pada dasarnya, *file* OLE2 kayak sebuah *zip folder* transparan, dia bisa menyimpan banyak tipe data berbeda (teks, gambar, racun *script*) berdesakan di dalem satu *file* utuh. *Tool* ini jago banget buat mengekstrak dan menelanjangi isi kandungan OLE2 tadi.

**Cara Pakainya:**
Misal kamu dapet *file* mencurigakan bernama `agenttesla.xlsm`. Cukup panggil *tool*-nya di terminal:

```bash
oledump.py agenttesla.xlsm
```

**Membaca Hasil Output:**
Hasilnya bakal keluar struktur direktori dari dalem *file* Excel itu.
- Kolom pertama adalah nomor urut (*index*), sering juga disebut **data streams**.
- Kalau ada huruf **`M`** atau **`m`** besar/kecil di samping nomornya (misal: `A4: M 688 'VBA/ThisWorkbook'`), itu tanda bahaya merah, huruf 'M' itu berarti ada *script* VBA Macro yang disisipkan di dalem partisi tersebut.
- Kolom angka di sebelahnya menunjukkan ukuran *byte* dari partisi itu, disusul nama partisinya.

Tugas kamu sebagai analis adalah membedah lebih dalem indeks nomor yang ada huruf 'M'-nya buat melihat isi nyatanya menulis *script* racun jenis apa.

**Menyelam Lebih Dalam ke Data Stream Jahat:**
Nah, anggap saja di *output* sebelumnya kita curiga sama baris nomor 4 (`A4: M ...`). Kita bisa menyuruh *oledump* buat nampilin isi mentahan dari *stream* nomor 4 itu aja pakai *flag* `-s` (*select*):

```bash
oledump.py agenttesla.xlsm -s 4
```

Masalahnya, hasil *output*-nya biasanya masih dalam wujud *hex dump* (kode campur aduk) yang bikin mata pusing. 

**Membaca Script Asli (Decompress):**
Biar *script* VBA yang ada di dalem situ bisa dibaca manusia dengan waras, kamu perlu nambahin *flag* ekstra spesifik: `--vbadecompress`.

```bash
oledump.py agenttesla.xlsm -s 4 --vbadecompress
```

Sekarang barisan *script* aslinya bakal kelihatan. Kamu nggak perlu pusing membaca dan mengerti seluruh baris *script*-nya sampai hafal. 
Sebagai analis pemula, insting yang harus kamu tajamkan adalah mencari anomali kasat mata, misalnya:
- Alamat *Public IP* aneh yang bukan mikik perusahaanmu.
- Tulisan berakhiran `.exe` atau `.pdf`.
- Perintah *download* atau eksekusi *shell/CMD*.

**Membersihkan Sampah Script (Deobfuscation) Pakai CyberChef**
Ngomong-ngomong soal anomali, pas kamu *scroll* hasil *decompress* tadi, kamu gampang banget nemuin baris *script* panjang yang bentuknya penuh simbol aneh, misalnya:
`"^p*o^*w*e*r*s^^h*e*l^*l* *^-w*i*n^*d*o*w^*s*t*y^*l*e*..."`

Itu bukan *error*. Pembuat *malware*-nya sengaja menyisipkan karakter sampah (kayak `*` dan `^`) biar *tools* antivirus bingung membacanya. Teknik ngumpetin wujud asli ini namanya **Obfuscation**.

Dilihat dari baris *script* (contoh variabel `Sqtnew`), ketahuan kalau ada perintah `Replace` buat membuang karakter `*` dan `^` sebelum *script*-nya dieksekusi diam-diam di komputer korban. 

Tugas kamu sekarang adalah membuang sampah itu secara manual biar wujud aslinya kelihatan, tools yang paling pas buat kerjaan ginian adalah **CyberChef** (*swiss army knife*-nya anak *cybersec*).

*Langkah praktek:*
1. Buka **CyberChef** (bisa dari *browser* di dalam mesin REMnux atau _online_).
2. *Copy* kalimat acak-acakan tadi dan *paste* ke kotak **Input** di CyberChef.
3. Di sebelah kiri (*Operations*), cari dan geser fitur **Find/Replace** ke kolom *Recipe* (seret dua kali karena kita mau membuang dua simbol berbeda).
4. Di Find/Replace pertama: Isi kolom *Find* dengan tanda bintang `*`, biarkan kotak *Replace* **kosong melompong** (alias dihapus).
5. Di Find/Replace kedua: Isi kolom *Find* dengan tanda caping `^`, dan biarkan kotak *Replace* kosong juga.

dan di kotak **Output** pojok kanan bawah, deretan sampah itu bakal terbaca jelas sebagai perintah jahat (*Powershell* tersembunyi).
