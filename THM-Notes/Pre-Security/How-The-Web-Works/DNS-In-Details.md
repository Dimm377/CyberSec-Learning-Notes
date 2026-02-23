# TryHackMe: DNS In Details


---

- **Room Link:** [DNS In Details](https://tryhackme.com/room/dnsindetail)
- **Category:** How The Web Works
- **Difficulty:** easy

---

# Overview

### What is DNS ?

DNS (Domain Name System) itu cara simpel buat kita berkomunikasi sama perangkat di internet tanpa harus ngapalin angka-angka kompleks.

### Domain Hierarchy

Domain Hierarchy itu sistem penamaan terstruktur yang bentuknya kayak pohon terbalik. Setiap tingkatan dipisahin pake tanda (`.`) dan dibaca dari kanan ke kiri. Sistem ini mastiin setiap alamat di internet itu unik dan bisa dikelola secara terdistribusi.

Struktur Domain Hierarchy:

<p align="center">
<img src="../../Assets/Images/DNS.png" alt="DNS">
</p>

### 1. Root Domain

Root domain itu tingkat paling tinggi di sistem domain hierarchy. Diwakili sama tanda titik (`.`) dan ada di puncak nama domain. Contoh: `domain.com`, si `.` itu adalah root domain. Root domain penting banget buat resolusi DNS karena bantu menavigasi seluruh struktur domain.

### 2. TLD (Top Level Domain)

TLD itu bagian paling kanan dari nama domain. Contoh: `domain.com`, tepat setelah titik terakhir yaitu `.com`.

TLD dibagi jadi dua kategori utama:

1. **gTLD (Generic Top Level Domain):** Dipake buat ngasih tau pengguna tentang tujuan atau bidang situs tersebut.

Contoh:

- **.com:** Buat tujuan komersial (Commercial)

- **.org:** Buat situs organisasi (Organization)

- **.edu:** Buat instansi pendidikan (Education)

- **.gov:** Buat instansi pemerintah (Government)

2. **ccTLD (Country Code Top Level Domain):** Dipake buat nunjukin lokasi geografis atau asal negara dari sebuah situs (domain).

Contoh:

- **.co.uk:** Buat situs yang berbasis di Inggris (United Kingdom)

- **.id:** Buat situs negaraku, yaitu Indonesia

### Nama domain di era Modern (sekarang)

- .online

- .club

- .biz

- dan banyak lagi

### DNS Record Types

DNS pake berbagai tipe record buat nyimpen data spesifik. Ini daftar yang paling umum:

- **A Record:** Record ini dipetakan ke alamat IPv4, misalnya 104.26.10.229

- **AAAA Record:** Record ini dipetakan ke alamat IPv6, misalnya 2606:4700:20::681a:be5

- **CNAME Record:** CNAME (Canonical Name) itu tipe record yang fungsinya memetakan nama alias (nama samaran) ke nama domain lain yang bersifat canonical (nama asli). Bedanya sama A Record, CNAME nggak langsung ngarah ke alamat IP tapi ngarahin query ke domain lain.

- **MX Record:** MX (Mail Exchange) Record itu tipe record yang fungsinya nentuin mail server mana yang bertanggung jawab nerima email buat sebuah domain.

- **TXT Record:** TXT (Text) Record itu tipe DNS yang ngebolehin pemilik domain buat masukin informasi berbasis teks ke sistem DNS. Beda sama record DNS lain yang strukturnya kaku buat alamat IP atau server, TXT record itu kolom teks bebas yang fleksibel banget buat berbagai keperluan administratif dan keamanan.

### Making A Request

Proses ini adalah urutan langkah yang dilakuin sistem buat menerjemahkan nama domain jadi IP Address, melibatkan koordinasi antar beberapa server DNS yang berbeda.

1. **Local Cache Check:** Sebelum minta ke jaringan luar, sistem bakal cek dulu secara internal. Ini mekanisme pertahanan pertama buat meminimalkan latensi dan beban traffic internet.

2. **Recursive Resolver:** Kalau info-nya nggak ketemu secara lokal, tugas pencarian diserahin ke Recursive Resolver. Ini server yang jadi perantara buat client.

3. **The Root Server:** Root Name Server itu infrastruktur yang jadi tulang punggung (_backbone_) dari seluruh sistem DNS di internet. Berada di puncak hierarki DNS, server ini jadi otoritas pertama yang nanganin permintaan kalau info-nya nggak ada di cache.

4. **TLD (Top Level Domain Server):** TLD Name Server jadi jembatan antara infrastruktur pusat (Root) dan server spesifik milik organisasi. Server ini bertanggung jawab ngelola informasi tentang semua domain yang ada di bawah satu ekstensi tertentu (kayak .com, .org, atau .id).

5. **Authoritative DNS Server:** Authoritative DNS server itu server yang bertanggung jawab nyimpen DNS record buat nama domain tertentu. Server ini nyimpen database asli dari berbagai tipe record (kayak A, MX, TXT). Semua perubahan, penambahan, atau penghapusan DNS record di domain kamu cuma bisa dilakuin di server otoritatif ini.

### TTL (Time To Live)

TTL itu parameter numerik di setiap DNS Record yang nentuin berapa lama (dalam detik) informasi itu masih valid sebelum dianggap kedaluwarsa sama sistem caching.

Mekanisme kerja TTL:

1. **Penerimaan Data:** Waktu Recursive Resolver dapet jawaban dari Authoritative Server, jawaban itu dateng bareng nilai TTL (misalnya: 3600, yang artinya 1 jam).

2. **Caching:** Resolver bakal nyimpen data itu di memorinya dan mulai hitung mundur dari 3600 ke 0.

3. **Penggunaan Cache:** Selama angkanya belum nyampe 0, kalau ada orang lain yang nanya domain yang sama, Resolver bakal langsung kasih jawaban dari memorinya tanpa nanya lagi ke server otoritatif.

4. **Refresh:** Begitu angkanya nyampe 0, data dianggap "basi". Resolver bakal hapus data itu dan lakuin pencarian ulang (Re-query) buat mastiin alamat IP-nya masih sama atau udah berubah.
