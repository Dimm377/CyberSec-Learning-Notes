# TryHackMe - Google Dorking

---

- **Room Link:** [Google Dorking](https://tryhackme.com/room/googledorking)
- **Category:** Exploring Room
- **Difficulty:** Easy

---

## Task 1: Ye Ol' Search Engine

Google itu sebenernya "tukang index". Dia punya bot (spider/crawler) yang jalan-jalan terus di internet buat nyatet semua website yang dia temui.

Cara kerjanya:
1.  **Crawling:** Bot jalan-jalan nyari konten.
2.  **Indexing:** Konten disimpen di database raksasa Google.
3.  **Searching:** Pas kita ngetik di Google, dia nyari di database-nya itu, bukan langsung ke website aslinya saat itu juga.

---

## Task 2: Let's Learn About Crawlers

Crawler itu kayak agen rahasia yang tugasnya ngumpulin informasi.

**Istilah Penting:**
*   **Crawler / Spider:** Program yang otomatis jelajahin internet.
*   **Index:** Database tempat nyimpen hasil jelajah si crawler.

**Apa yang diambil Crawler?**
Dia ngambil macem-macem, tapi yang paling penting itu **Keywords**. Dari keywords ini Google tau website kita itu tentang apa (misal: "Resep Masakan", "Tutorial Hacking", dll).

**Answer the questions below:**

- **Question:** Name the key term of what a "Crawler" is used to do?
- **Answer:** ?
  _(Clue: Tujuannya buat ngisi database...)_

- **Question:** What is the name of the technique that "Search Engines" use to retrieve this information about websites?
- **Answer:** ?
  _(Clue: Nama lain jalan-jalannya spider)_

- **Question:** What is an example of the type of contents that could be gathered from a website?
- **Answer:** ?
  _(Clue: Kata kunci)_

---

## Task 3: Enter: Search Engine Optimisation (SEO)

SEO itu seni bikin website kita disukai sama Google biar muncul di halaman pertama.

**Topik SEO:**
1.  **Keywords:** Kata kunci yang relevan.
2.  **Meta Title:** Judul halaman yang muncul di tab browser & hasil pencarian.
3.  **Sitemap:** Peta situs.

Task ini sebenernya nyuruh kita pake tool kayak "SEO Site Checkup" buat ngecek performa website `tryhackme.com` vs `googledorking.cmnatic.co.uk`.

**Answer the questions below:**

- **Question:** Using the SEO Site Checkup tool on "tryhackme.com", does TryHackMe pass the "Meta Title Test"? (Yea / Nay)
- **Answer:** ?
  _(Clue: Yea)_

- **Question:** Does "tryhackme.com" pass the "Keywords Usage Test?" (Yea / Nay)
- **Answer:** ?
  _(Clue: Nay)_

- **Question:** From a "rating score" perspective alone, what website would list first? tryhackme.com or googledorking.cmnatic.co.uk
- **Answer:** ?
  _(Clue: Yang skornya lebih gede)_

- **Question:** With the same tool and domain in Question #3 (previous) How many pages use "flash"?
- **Answer:** ?
  _(Clue: 0)_

---

## Task 4: Beepboop - robots.txt

Task ini ngebahas soal file sakral buat mesin pencari, yaitu **`robots.txt`**.

**Apa itu robots.txt?**
File teks biasa yang dipake webmaster buat ngasih tau crawler (kayak Googlebot, Bingbot, dll) halaman mana yang boleh diambil (index) dan mana yang enggak.

**Poin Penting:**

1.  **Lokasi File:** Wajib ada di root domain.
    *   Misal website `ablog.com`, filenya pasti ada di `ablog.com/robots.txt`.
    *   Ga bisa ditaruh di subfolder kayak `ablog.com/site/robots.txt`.

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
  _(Clue: Di root domain)_

- **Question:** If a website was to have a sitemap, where would that be located?
- **Answer:** ?
  _(Clue: Nama filenya sitemap, ekstensi xml)_

- **Question:** How would we only allow "Bingbot" to index the website?
- **Answer:** ?
  _(Clue: User-agent: ...)_

- **Question:** How would we prevent a "Crawler" from indexing the directory "/dont-index-me/"?
- **Answer:** ?
  _(Clue: Pake perintah Disallow)_

- **Question:** What is the typical file structure of a "Sitemap"?
- **Answer:** ?
  _(Clue: Format file sitemap)_

- **Question:** What real life example can "Sitemaps" be compared to?
- **Answer:** ?
  _(Clue: Peta konvensional)_

- **Question:** Name the keyword for the path taken for content on a website.
- **Answer:** ?
  _(Clue: Awalannya R)_

---

---

## Task 6: What is Google Dorking?

Ini dia menu utamanya: **Google Dorking**.
Teknik pake operator pencarian Google buat nemuin hasil yang spesifik banget. Di dunia security, ini dipake buat **Reconnaissance** (nyari info target).

### 1. Basic Operators (Operator Dasar)
- **`site:`** -> Nyari di domain spesifik.
  - Contoh: `site:tryhackme.com` (Cuma hasil dari THM).
  - Contoh: `site:.go.id` (Cuma hasil dari domain pemerintah Indonesia).
- **`filetype:`** (atau `ext:`) -> Nyari jenis file tertentu.
  - Contoh: `filetype:pdf` (Dokumen PDF).
  - Contoh: `ext:log` (File log, bahaya kalo bocor!).
- **`inurl:`** -> Nyari kata di dalam URL.
  - Contoh: `inurl:admin` (Kemungkinan halaman admin).
- **`intitle:`** -> Nyari kata di Judul Halaman (Tab Browser).
  - Contoh: `intitle:login` (Halaman login).
- **`intext:`** -> Nyari kata di dalam isi artikel/body halaman.
  - Contoh: `intext:"password"` (Nyari kata password di halaman).

### 2. Advanced Combinations (Combo Sakti)
Gabungin operator di atas buat hasil yang ngeri.

**a. Nyari Halaman Login / Admin Panel:**
- `site:target.com intitle:"login"` -> Halaman login di target.
- `inurl:admin intitle:"login"` -> Halaman login admin secara umum.

**b. Nyari File Sensitif / Bocor:**
- `filetype:log intext:"password"` -> Nyari file log yang isinya ada kata password.
- `intitle:"index of" "backup"` -> **Directory Listing** (Folder kebuka) yang isinya file backup.
- `site:gov.id filetype:pdf "rahasia"` -> Dokumen rahasia instansi pemerintah (Jangan disalahgunain!).

**c. Nyari Config File:**
- `ext:conf` atau `ext:cnf` -> File konfigurasi server.
- `inurl:wp-config.txt` -> Konfigurasi WordPress yang bocor.

### 3. Google Dorking Database (GHDB)
Ada database isinya ribuan dork yang udah ditemuin orang lain: **Exploit-DB (GHDB)**. Isinya dork buat nyari CCTV, printer online, password, dll.

**Answer the questions below:**

- **Question:** What would be the format used to query the site bbc.co.uk about flood defences?
- **Answer:** ?
  _(Clue: site:bbc.co.uk ...)_

- **Question:** What term would you use to search by file type?
- **Answer:** ?
  _(Clue: filetype:)_

- **Question:** What term can we use to look for login pages?
- **Answer:** ?
  _(Clue: intitle: ...)_
