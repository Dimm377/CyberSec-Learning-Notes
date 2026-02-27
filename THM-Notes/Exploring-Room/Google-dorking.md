# TryHackMe - Google Dorking

---

- **Room Link:** [Google Dorking](https://tryhackme.com/room/googledorking)
- **Category:** Exploring Room
- **Difficulty:** Easy

---

## Ye Ol' Search Engine

Google itu sebenernya "tukang index". Dia punya bot (spider/crawler) yang jalan-jalan terus di internet buat nyatet semua website yang dia temui.

Cara kerjanya:
1.  **Crawling:** Bot jalan-jalan mencari konten.
2.  **Indexing:** Konten disimpen di database raksasa Google.
3.  **Searching:** Pas kita ngetik di Google, dia mencari di database-nya itu, bukan langsung ke website aslinya saat itu juga.

---

## Let's Learn About Crawlers

Crawler itu seperti agen rahasia yang tugasnya ngumpulin informasi.

**Istilah Penting:**
*   **Crawler / Spider:** Program yang otomatis jelajahin internet.
*   **Index:** Database tempat menyimpan hasil jelajah si crawler.

**Apa yang diambil Crawler?**
Dia mengambil macem-macem, tapi yang paling penting itu **Keywords**. Dari keywords ini Google tau website kita itu tentang apa (misal: "Resep Masakan", "Tutorial Hacking", dll).

**Answer the questions below:**

- **Question:** Name the key term of what a "Crawler" is used to do?
- **Answer:** ?

- **Question:** What is the name of the technique that "Search Engines" use to retrieve this information about websites?
- **Answer:** ?

- **Question:** What is an example of the type of contents that could be gathered from a website?
- **Answer:** ?

---

## Enter: Search Engine Optimisation (SEO)

SEO itu seni membuat website kita disukai sama Google agar muncul di halaman pertama.

**Topik SEO:**
1.  **Keywords:** Kata kunci yang relevan.
2.  **Meta Title:** Judul halaman yang muncul di tab browser & hasil pencarian.
3.  **Sitemap:** Peta situs.

Task ini sebenernya memerintah kita pakai tool seperti "SEO Site Checkup" buat mengecek performa website `tryhackme.com` vs `googledorking.cmnatic.co.uk`.

**Answer the questions below:**

- **Question:** Using the SEO Site Checkup tool on "tryhackme.com", does TryHackMe pass the "Meta Title Test"? (Yea / Nay)
- **Answer:** ?

- **Question:** Does "tryhackme.com" pass the "Keywords Usage Test?" (Yea / Nay)
- **Answer:** ?

- **Question:** From a "rating score" perspective alone, what website would list first? tryhackme.com or googledorking.cmnatic.co.uk
- **Answer:** ?

- **Question:** With the same tool and domain in Question #3 (previous) How many pages use "flash"?
- **Answer:** ?

---

## Beepboop - robots.txt

Task ini membahas soal file sakral buat mesin pencari, yaitu **`robots.txt`**.

**Apa itu robots.txt?**
File teks biasa yang dipake webmaster buat memberi tau crawler (seperti Googlebot, Bingbot, dll) halaman mana yang boleh diambil (index) dan mana yang enggak.

**Poin Penting:**

1.  **Lokasi File:** Wajib ada di root domain.
    *   Misal website `ablog.com`, filenya pasti ada di `ablog.com/robots.txt`.
    *   Ga bisa ditaruh di subfolder seperti `ablog.com/site/robots.txt`.

2.  **Sitemap:** Peta situs yang isinya daftar semua link di website. Biasanya lokasinya di `/sitemap.xml`.

3.  **User-agent:** Identitas crawler yang mau diatur.
    *   `User-agent: *` = Berlaku buat semua bot.
    *   `User-agent: Bingbot` = Khusus buat botnya Bing.

4.  **Disallow:** Perintah larangan.
    *   `Disallow: /` = Ga boleh index seluruh website.
    *   `Disallow: /admin/` = Ga boleh index folder admin.
    *   `Disallow: /dont-index-me/` = Ga boleh index folder rahasia ini.

5.  **Extensi File:** File konfigurasi Linux seringkali punya ekstensi `.conf`. Kalo kesebar bisa bahaya!

**Answer the questions below:**

- **Question:** Where would "robots.txt" be located on the domain "ablog.com"?
- **Answer:** ?

- **Question:** If a website was to have a sitemap, where would that be located?
- **Answer:** ?

- **Question:** How would we only allow "Bingbot" to index the website?
- **Answer:** ?

- **Question:** How would we prevent a "Crawler" from indexing the directory "/dont-index-me/"?
- **Answer:** ?

- **Question:** What is the typical file structure of a "Sitemap"?
- **Answer:** ?

- **Question:** What real life example can "Sitemaps" be compared to?
- **Answer:** ?

- **Question:** Name the keyword for the path taken for content on a website.
- **Answer:** ?

---

---

## What is Google Dorking?

Ini dia menu utamanya: **Google Dorking**.
Teknik pakai operator pencarian Google buat nemuin hasil yang spesifik banget. Di dunia security, ini dipake buat **Reconnaissance** (mencari info target).

### 1. Basic Operators (Operator Dasar)
- **`site:`** -> Mencari di domain spesifik.
  - Contoh: `site:tryhackme.com` (Cuma hasil dari THM).
  - Contoh: `site:.go.id` (Cuma hasil dari domain pemerintah Indonesia).
- **`filetype:`** (atau `ext:`) -> Mencari jenis file tertentu.
  - Contoh: `filetype:pdf` (Dokumen PDF).
  - Contoh: `ext:log` (File log, bahaya kalo bocor!).
- **`inurl:`** -> Mencari kata di dalam URL.
  - Contoh: `inurl:admin` (Kemungkinan halaman admin).
- **`intitle:`** -> Mencari kata di Judul Halaman (Tab Browser).
  - Contoh: `intitle:login` (Halaman login).
- **`intext:`** -> Mencari kata di dalam isi artikel/body halaman.
  - Contoh: `intext:"password"` (Mencari kata password di halaman).

### 2. Advanced Combinations (Combo Sakti)
Gabungin operator di atas buat hasil yang ngeri.

**a. Mencari Halaman Login / Admin Panel:**
- `site:target.com intitle:"login"` -> Halaman login di target.
- `inurl:admin intitle:"login"` -> Halaman login admin secara umum.

**b. Mencari File Sensitif / Bocor:**
- `filetype:log intext:"password"` -> Mencari file log yang isinya ada kata password.
- `intitle:"index of" "backup"` -> **Directory Listing** (Folder kebuka) yang isinya file backup.
- `site:gov.id filetype:pdf "rahasia"` -> Dokumen rahasia instansi pemerintah (Jangan disalahgunain!).

**c. Mencari Config File:**
- `ext:conf` atau `ext:cnf` -> File konfigurasi server.
- `inurl:wp-config.txt` -> Konfigurasi WordPress yang bocor.

### 3. Google Dorking Database (GHDB)
Ada database isinya ribuan dork yang sudah ditemuin orang lain: **Exploit-DB (GHDB)**. Isinya dork buat mencari CCTV, printer online, password, dll.

**Answer the questions below:**

- **Question:** What would be the format used to query the site bbc.co.uk about flood defences?
- **Answer:** ?

- **Question:** What term would you use to search by file type?
- **Answer:** ?

- **Question:** What term can we use to look for login pages?
- **Answer:** ?
