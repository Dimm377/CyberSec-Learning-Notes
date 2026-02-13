# TryHackMe - Google Dorking

---

- **Room Link:** [Google Dorking](https://tryhackme.com/room/googledorking)
- **Category:** Exploring Room
- **Difficulty:** Easy

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

- **Question:** What is the extension of a Unix/Linux system configuration file that we might want to hide from "Crawlers"?
- **Answer:** ?
  _(Clue: Ekstensi 4 huruf)_
