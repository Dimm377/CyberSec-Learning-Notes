# TryHackMe: Public Key Cryptography

**Room Link:** [TryHackMe](https://tryhackme.com/room/publickeycrypto)
**Category:** Cryptography / Security
**Difficulty:** Easy

# TryHackMe Room Write-up: Public Key Cryptography Basics

## 1. Overview

Room **Public Key Cryptography Basics** di platform TryHackMe merupakan modul fundamental yang dirancang untuk memberikan pemahaman mendalam mengenai sistem kriptografi asimetris. Fokus utama dari room ini adalah melatih praktisi keamanan dalam memahami bagaimana pasangan kunci publik dan privat digunakan untuk menjamin aspek **Authentication**, **Authenticity**, dan **Integrity** dalam komunikasi data modern.

## 2. Key Concepts Learned

- **Asymmetric vs Symmetric Encryption**: Perbedaan mendasar terletak pada penggunaan kunci. Kriptografi asimetris menggunakan sepasang kunci (publik dan privat), sedangkan simetris hanya menggunakan satu kunci.
- **Key Exchange Performance**: Enkripsi asimetris secara komputasi lebih lambat dibandingkan simetris. Oleh karena itu, kegunaan utamanya adalah untuk melakukan pertukaran kunci simetris yang akan digunakan untuk enkripsi data massal selanjutnya.
- **Integer Factorization**: Dasar keamanan algoritma RSA yang mengandalkan tingkat kesulitan matematis dalam memfaktorkan perkalian dua bilangan prima besar ($p$ dan $q$).
- **Modular Exponentiation**: Operasi matematika utama dalam Diffie-Hellman yang memungkinkan dua pihak menyepakati rahasia bersama tanpa pernah mengirimkan rahasia tersebut melalui jaringan publik.

## 3. Step-by-Step Walkthrough

### Task 1 & 2: Introduction and Common Use

Pada bagian awal, dilakukan pemahaman mengenai analogi gembok (lock).

- **Methodology**: Memahami bahwa kunci publik adalah gembok terbuka yang dapat digunakan siapa saja untuk mengunci pesan, namun hanya pemegang kunci privat yang memiliki kunci fisik untuk membukanya.
- **Reasoning**: Asimetris digunakan sebagai "handshake" awal untuk memverifikasi identitas sebelum beralih ke enkripsi simetris yang lebih cepat.

### Task 3: RSA

Tantangan pada bagian ini adalah melakukan perhitungan manual untuk menentukan komponen kunci RSA.

- **Langkah 1**: Menentukan Modulus ($n$) dengan rumus $n = p \times q$.
- **Langkah 2**: Menentukan Euler's Totient ($\phi(n)$) dengan rumus $\phi(n) = (p - 1) \times (q - 1)$.
- **Insight**: Ketelitian dalam perhitungan prima sangat krusial karena kesalahan satu angka akan membatalkan seluruh proses enkripsi dan dekripsi.

### Task 4: Diffie-Hellman Key Exchange

Fokus pada tugas ini adalah memahami alur pertukaran nilai publik untuk menghasilkan kunci rahasia bersama.

- **Proses**: Masing-masing pihak menghitung nilai publik mereka sendiri ($A$ dan $B$) menggunakan generator ($g$), modulus ($p$), dan kunci rahasia masing-masing ($a$ atau $b$).
- **Hasil Akhir**: Melalui rumus $Key = B^a \pmod p$ atau $Key = A^b \pmod p$, kedua pihak akan mendapatkan nilai kunci yang identik.

### Task 5: SSH Implementation

Penerapan praktis dilakukan dengan menginspeksi file kunci privat yang tersedia pada sistem target.

- **Approach**: Menggunakan utilitas pembaca teks untuk memeriksa struktur file kunci.
- **Analysis**: Identifikasi algoritma dilakukan dengan melihat _header_ file. Keberadaan string `-----BEGIN RSA PRIVATE KEY-----` secara eksplisit menunjukkan penggunaan algoritma RSA.

## 4. Tools & Commands Used

- **SSH**: Protokol komunikasi aman yang dianalisis penggunaannya terhadap kriptografi asimetris.
- **cat**: Digunakan untuk menampilkan konten file sensitif guna mengidentifikasi _cryptographic headers_.
- **Linux Terminal**: Lingkungan eksekusi utama untuk melakukan navigasi direktori dan manajemen kunci.

## 5. Security Takeaways

- **Private Key Hygiene**: Kunci privat tidak boleh meninggalkan mesin lokal dan harus dilindungi dengan izin file yang ketat (seperti `chmod 600`).
- **Algorithm Selection**: Meskipun RSA masih standar, algoritma berbasis _Elliptic Curve_ mulai lebih disukai karena menawarkan keamanan yang setara dengan ukuran kunci yang lebih kecil.
- **Manual Calculation Awareness**: Memahami matematika di balik enkripsi membantu analis keamanan dalam mengidentifikasi potensi kelemahan pada implementasi kustom.

## 6. Conclusion

Penyelesaian room ini memberikan pemahaman konkret bahwa keamanan internet modern tidak hanya bergantung pada kekuatan algoritma, tetapi pada cara kunci-kunci tersebut dipertukarkan dan disimpan. Keterampilan dalam mengidentifikasi tipe kunci dan memahami alur pertukaran rahasia adalah aset vital bagi setiap praktisi keamanan ofensif maupun defensif.
