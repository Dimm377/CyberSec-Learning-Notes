# TryHackMe: Cryptography Basic


---

- **Room Link:** [TryHackMe](https://tryhackme.com/room/cryptographybasics)
- **Category:** Cryptography / Security
- **Difficulty:** Easy

---

## Task 1: Introduction

Room ini yang pertama dari tiga room pengantar tentang cryptography. Di sini kita bakal belajar dasar-dasar yang penting sebelum masuk ke penjelasan yang lebih kompleks.

**Materi yang bakal dipelajari:**

- Istilah-istilah penting dalam cryptography
- Pentingnya cryptography di dunia digital
- Mengenal Caesar Cipher dan algoritma historis lainnya
- Standar cipher simetris (AES, DES, 3DES)
- Cipher asimetris umum
- Matematika dasar yang sering dipake di kriptografi (XOR & Modulo)

## Task 2: Importance of Cryptography

Cryptography itu praktek dan studi tentang teknik komunikasi yang aman dan perlindungan data di mana ada pihak ketiga (lawan). Intinya, lawan gak boleh sampe liat (disclose) atau ngubah (alter) isi pesan tersebut.

Cryptography jadi vital di era digital karena jamin tiga hal utama:

1. **Confidentiality (Kerahasiaan):** Cuma orang yang berhak yang bisa baca datanya.
2. **Integrity (Integritas):** Mastiin data gak berubah pas lagi dikirim.
3. **Authenticity (Keaslian):** Mastiin kalau pengirim data emang beneran orang yang dia klaim.

Selain itu, cryptography juga dipake buat memenuhi standar regulasi internasional.

**Answer the questions below:**

- **Question:** What is the standard required for handling credit card information?
- **Answer:** ?

## Task 3: Plaintext to Ciphertext

Di sini kita belajar gimana data berubah bentuk dari yang bisa dibaca manusia "plaintext" jadi sebuah kode rahasia "ciphertext".

**Key Value:**

- **Plaintext:** Data asli atau pesan sebelum dienkripsi. Pesan ini masih bisa dibaca sama siapa aja (readable), misalnya teks "hello", foto pribadi, informasi penting, dll.
- **Ciphertext:** Hasil dari proses enkripsi. Pesan ini kelihatan berantakan dan gak punya arti buat siapa pun yang gak punya kuncinya (unreadable).
- **Encryption (Enkripsi):** Proses ngubah **Plaintext** jadi **Ciphertext** pake algoritma dan kunci tertentu.
- **Decryption (Dekripsi):** Proses kebalikannya, yaitu ngubah **Ciphertext** balik jadi **Plaintext** biar bisa dibaca lagi.
- **Key (Kunci):** Informasi yang dipake sama algoritma kriptografi buat bikin proses enkripsi/dekripsi jadi unik.

<p align="center">
![Cipher](../../Assets/Images/chiper.png)
</p>

**Answer the questions below:**

- **Question:** What do you call the encrypted plaintext?
- **Answer:** ?

- **Question:** What is the process of turning plaintext into ciphertext called?
- **Answer:** ?

- **Question:** What do you call the process that returns the plaintext?
- **Answer:** ?

## Task 4: Historical Cipher

Ini ngebahas metode enkripsi kuno yang jadi dasar buat cryptography modern. Meskipun sekarang dianggep lemah, ngerti logika di baliknya penting banget.

### 1. Caesar Cipher

Ini salah satu teknik paling tua dan paling simpel.

- **Cara Kerja:** Lakuin pergeseran (shift) setiap huruf dalam alfabet pake jumlah tertentu.
- **Contoh:** Dengan pergeseran 3 (Shift 3), huruf 'A' bakal jadi 'D', 'B' jadi 'E', dan seterusnya.
- **ROT13:** Varian populer dari Caesar Cipher yang nggeser huruf sebanyak 13 posisi. Karena alfabet ada 26 huruf, jalanin ROT13 dua kali di pesan yang sama bakal balikin ke teks asli.

<p align="center">
  ![Encryption](../../Assets/Images/Encryption.png)
  ![Decryption](../../Assets/Images/Decryption.png)
</p>

### 2. Vigenère Cipher

Metode ini lebih canggih karena pake kata kunci (keyword) buat nentuin jumlah pergeseran.

- **Cara Kerja:** Disebut polyalphabetic substitution karena setiap huruf di plaintext bisa digeser pake jumlah yang beda-beda tergantung huruf yang sesuai di keyword-nya.
- **Keunggulan:** Lebih susah dipecahin daripada Caesar Cipher karena frekuensi kemunculan hurufnya gak berpola tetap.

### 3. Enigma Machine

Mesin enkripsi yang dipake tentara Jerman waktu Perang Dunia II.

- **Cara Kerja:** Pake sistem rotor yang terus berputar setiap kali tombol ditekan, jadi satu huruf yang sama bisa berubah jadi huruf yang beda berkali-kali.
- **Sejarah:** Enigma dianggep mustahil dipecahin sampe akhirnya tim yang dipimpin Alan Turing berhasil mecahinnya, yang kemudian jadi cikal bakal komputer modern.

**Answer the questions below:**

- **Question:** What is the name of the popular Caesar cipher variant used for obscuring text online?
- **Answer:** ?

- **Question:** Which cipher uses a keyword to decide the alphabet shift?
- **Answer:** ?

- **Question:** What was the name of the machine the Germans used in World War 2 to encrypt messages?
- **Answer:** ?

- **Question:** Knowing that `Xld Hzhz Apntyel dlhte` was encrypted using Caesar Cipher, what is the original plaintext?
- **Answer:** ?
  > (Penjelasan: Bisa pake tool online kayak [Cryptii](https://cryptii.com/pipes/caesar-cipher). Masukin ciphertext `Xld Hzhz Apntyel dlhte` dan pake **Shift 11** ke arah kanan buat dapetin teks aslinya)\_

## Task 5: Types of Encryption

Dalam kriptografi modern, ada dua kategori utama enkripsi: **Symmetric** (Simetris) dan **Asymmetric** (Asimetris).

### 1. Symmetric Encryption

Pake **kunci yang sama** buat proses enkripsi dan dekripsi (disebut juga _private key cryptography_).

- **Kelemahan:** Tantangannya itu gimana caranya distribusiin kunci secara aman ke penerima.
- **Algoritma Utama:**
  - **DES (Data Encryption Standard):** Diadopsi tahun 1977, pake kunci 56-bit. Udah berhasil di-crack tahun 1999 dalam waktu kurang dari 24 jam.
  - **3DES (Triple DES):** Jalanin DES tiga kali. Kunci 168-bit (efektivitas 112-bit). Udah mulai ditinggalin sejak 2019.
  - **AES (Advanced Encryption Standard):** Standar global saat ini (diadopsi tahun **2001**). Pake kunci 128, 192, atau 256 bit. Aman banget dan efisien.

<p align="center">
![Symmetric](../../Assets/Images/Symmetric.png)
</p>

### 2. Asymmetric Encryption

Pake **sepasang kunci**: _Public Key_ (buat mengenkripsi/mengunci) dan _Private Key_ (buat mendekripsi/membuka).

- **Konsep:** Cuma pemilik _Private Key_ yang bisa buka data yang dikunci pake _Public Key_-nya.
- **Algoritma Utama:**
  - **RSA:** Pake problem matematika faktorisasi bilangan prima besar. Rekomendasi kunci minimal 2048-bit.
  - **Diffie-Hellman:** Dipake buat pertukaran kunci secara aman.
  - **ECC (Elliptic Curve Cryptography):** Lebih efisien; kunci 256-bit ECC setara kekuatannya sama RSA 3072-bit.

<p align="center">
![Asymmetric](../../Assets/Images/Asymmetric.png)
</p>

---

**Answer the questions below:**

- **Question:** Should you trust DES? (Yea/Nay)
- **Answer:** ?

  > (Penjelasan: Karena panjang kuncinya cuma 56-bit, DES udah gak aman lagi terhadap serangan brute force pake komputasi modern.)\_

- **Question:** When was AES adopted as an encryption standard?
- **Answer:** ?

- **Question:** What is the name of the cipher that uses 3 keys and 3 execution stages?
- **Answer:** ?

- **Question:** Which encryption standard uses the same key to encrypt and decrypt?
- **Answer:** ?
## Task 6: Basic Math

Kriptografi modern dibangun di atas fondasi matematika. Task ini ngebahas dua operasi logika dan aritmatika yang paling sering muncul dalam algoritma keamanan.

### 1. XOR Operation (Exclusive OR)

XOR itu operasi logika biner yang ngebandingin dua bit.

- **Aturan Main:** Ngasilin **1** kalau kedua bit beda, dan **0** kalau kedua bit sama.
- **Simbol:** $\oplus$ atau `^`.

**Truth Table XOR:**
| A | B | A $\oplus$ B |
| :--- | :--- | :--- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

- **Kegunaan di Kriptografi:** XOR sering dipake sebagai algoritma enkripsi simetris sederhana. Kalau kita punya Plaintext ($P$) dan Secret Key ($K$), maka Ciphertext ($C$) itu $P \oplus K = C$. Buat dapetin balik Plaintext, tinggal lakuin $C \oplus K = P$.

### 2. Modulo Operation (%)

Modulo itu operasi buat nyari **sisa bagi** dari sebuah pembagian.

- **Contoh:** $23 \pmod 6 = 5$ (karena 23 dibagi 6 itu 3 dengan sisa 5).
- **Sifat Penting:** Modulo gak bisa dibalik (_not reversible_). Kalau $x \pmod 5 = 4$, nilai $x$ bisa berupa 4, 9, 14, dan seterusnya sampe gak terhingga.

---

**Answer the questions below:**

- **Question:** What's 1001 $\oplus$ 1010?
- **Answer:** ?
  _(Cara Hitung: 1$\oplus$1=0, 0$\oplus$0=0, 0$\oplus$1=1, 1$\oplus$0=1)_

- **Question:** What's 118613842 % 9091?
- **Answer:** ?
  _(Cara Hitung: Lu bisa pake Python di terminal Kitty lu: `python3 -c "print(118613842 % 9091)"`)_

- **Question:** What's 60 % 12?
- **Answer:** ?
  _(Cara Hitung: 60 habis dibagi 12, jadi sisanya adalah 0)_
