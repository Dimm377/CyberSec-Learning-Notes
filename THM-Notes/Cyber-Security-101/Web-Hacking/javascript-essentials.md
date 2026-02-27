# TryHackMe: Javascript-Essentials

- **Room Link:** [Javascript-essentials](https://tryhackme.com/room/javascriptessentials)
- **Category:** Web Hacking
- **Difficulty:** Easy

## Introduction

JavaScript adalah bahasa scripting populer yang memungkinkan kita sebagai developer untuk menambahkan fitur interaktif ke dalam website yang sudah memiliki struktur (HTML) dan tampilan (CSS).

Tanpa JS, website cuma jadi web statis. Dengan JS, kita bisa membuat website itu hidup.

**Learning Objectives:**

- Memahami dasar-dasar javascript
- Integrasi Javascript ke HTML
- Menyalahgunakan dialogue function
- Bypassing Control Flow Statements
- Exploring Minified files

## Essentials Concept

### Variables

**Variables** adalah sebuah container (wadah) yang memungkinkan kita menyimpan nilai data di dalam nya, bayangkan variable adalah kotak berlabel tempat kita dapat menyimpan data secara berbeda, contoh nya kita punya kotak berlabel `age` maka yang ada di dalam kotak tersebut bisa masukkan angka **(7,27,15)**

**3 cara mendeklarasikan variables di javascript:**

- Gunakan **Let** ketika ingin nilai di dalam nya berubah
- Pakai **Const** kalau ingin nilai nya tetap
- Lupakan **Var** Sudah kuno ngapain make ini yang penting 2 diatas itu sekarang

### Data Types

di JS, tipe data menentukan tipe nilai yang bisa ditampung oleh sebuah variable

**Contoh Tipe Data:**

- Number: Mewakili bilangan integer dan float (misalnya 42, 3.14)
- String: Mewakili rangkaian karakter, ditandai dengan tanda kutip (misalnya, 'halo', "dunia")
- Boolean: Tipe data yang hanya ada 2 value **True & False**, sering digunakan untuk memvalidasi sebuah kondisi
- Object: objek adalah kumpulan properti, di mana setiap properti didefinisikan sebagai pasangan nilai kunci. Objek memungkinkan kita mengelompokkan data dan fungsi terkait menjadi satu, contoh `{ name: 'Alice', age: 25 }`
- Array: array adalah tipe objek khusus yang digunakan untuk menyimpan daftar nilai dalam satu variabel, contoh `let numbers = [1, 2, 3, 4, 5];`
- Undefined: Variabel yang sudah dideklarasikan namun belum diberi nilai, contoh `let x;`

### Functions

Functions dalam JavaScript adalah blok kode yang dapat digunakan kembali untuk melakukan tugas tertentu

```javascript
function sapaDunia() {
  console.log("Halo, selamat belajar JavaScript!");
}

// Cara manggilnya:
sapaDunia();
```

### Why do you have to use functions?

- Reusability: tidak perlu menulis kode yang sama sebanyak 10 kali. Cukup tulis satu kali di dalam fungsi, lalu panggil kapan pun dibutuhkan seperti prinsip **DRY (Don't Repeat yourself)**

- Organized: Kode nya jadi lebih rapi dan lebih mudah dibaca nantinya

- Modular: memecah mecah masalah besar jadi tugas-tugas kecil yang lebih gampang di maintenance.

### Loops

Loops (Perulangan) digunakan untuk menjalankan blok kode yang sama berulang kali selama kondisi tertentu terpenuhi. Ini sangat berguna buat ngurangin redundansi kode.

**Jenis-jenis Loops di JavaScript:**

1.  **For Loop:** Paling umum dipake, kita tau berapa kali loop harus jalan.
    ```javascript
    // Struktur: for (inisialisasi; kondisi; increment)
    for (let i = 0; i < 5; i++) {
        console.log("Perulangan ke-" + i);
    }
    // Output: 0, 1, 2, 3, 4
    ```

2.  **While Loop:** Jalan terus selama kondisinya `true`.
    ```javascript
    let i = 0;
    while (i < 5) {
        console.log("While loop ke-" + i);
        i++;
    }
    ```

3.  **Do...While Loop:** Mirip `while`, tapi minimal jalan **sekali** dulu baru cek kondisi.
    ```javascript
    let i = 0;
    do {
        console.log("Do-While loop ke-" + i);
        i++;
    } while (i < 5);
    ```

4.  **For...Of Loop (Array):** Enak banget buat nge-loop isi array.
    ```javascript
    const buah = ["Apel", "Jeruk", "Mangga"];
    for (const item of buah) {
        console.log("Buah: " + item);
    }
    ```

    ```

**Answer the questions below:**

- **Question:** What term allows you to run a code block multiple times as long as it is a condition?
- **Answer:** ?

---

## JavaScript Overview

Task ini sebenernya cuma review singkat dan demo script sederhana.
Ilustrasinya ada kode JS buat nambahin angka.

**Poin Penting:**
*   **Interpreted Language:** JavaScript itu bahasa yang diterjemahkan baris per baris saat dijalankan (oleh browser), bukan dicompile jadi file binary dulu seperti C++ atau Java.
*   **Variable Manipulation:** Kita bisa ubah nilai variabel kapan saja (kalau pakai `let` atau `var`).

**Answer the questions below:**

- **Question:** What is the code output if the value of x is changed to 10?
- **Answer:** ?


- **Question:** Is JavaScript a compiled or interpreted language?
- **Answer:** ?

---

## Integrating JavaScript in HTML

Di task ini, kita diminta buat eksperimen pakai file yang sudah disediain: `external_test.html`.
Tujuannya agar paham bedanya **Internal** vs **External** JS.

**1. Internal JavaScript**
Kode JS ditulis langsung di dalem file HTML pakai tag `<script>`.
*   **Contoh:**
    ```html
    <!-- Di dalam file HTML -->
    <script>
        console.log("Hello from Internal JS");
    </script>
    ```

**2. External JavaScript**
Kode JS dipisah ke file sendiri (ekstensi `.js`), terus dipanggil di HTML pakai atribut `src`.
*   **Skenario Room:**
    *   Kita punya file HTML: `external_test.html`.
    *   Kita punya file JS: `thm_external.js`.
    *   Cara nge-link-nya:
        ```html
        <!-- Di dalam external_test.html -->
        <script src="thm_external.js"></script>
        ```
*   **Kenapa pakai External?**
    *   Bisa dipake ulang di banyak halaman (Reusability).
    *   Kode lebih rapi & bersih.

**Answer the questions below:**

- **Question:** Which type of JavaScript integration places the code directly within the HTML document?
- **Answer:** ?

- **Question:** Which method is better for reusing JS across multiple web pages?
- **Answer:** ?

- **Question:** What is the name of the external JS file that is being called by "external_test.html"?
- **Answer:** ?

- **Question:** What attribute links an external JS file in the script tag?
- **Answer:** ?

---

## Abusing Dialogue Functions

JavaScript punya 3 fungsi bawaan buat menampilkan pop-up (dialogue box) ke user. Ini sering dipake buat interaksi, tapi bisa juga disalahgunain buat nge-prank (atau serangan XSS).

**3 Fungsi Utama:**
1.  **`alert("Pesan")`**
    *   Cuma menampilkan pesan + tombol OK.
    *   *Abuse:* Kalo ditaruh di loop tanpa henti (`while(true)`), browser bakal hang/crash karena pop-up muncul terus.
2.  **`prompt("Input Sesuatu")`**
    *   Minta input teks dari user.
    *   Nilai yang diinput bisa disimpen di variabel.
    *   Contoh: `let nama = prompt("Siapa nama kamu?");`
3.  **`confirm("Yakin?")`**
    *   Menampilkan pilihan OK (return `true`) atau Cancel (return `false`).

**Skenario di Room:**
Kita dikasih file `invoice.html` yang isinya ada script nakal.
Pas dibuka, dia bakal nge-loop menampilkan alert "Hacked" berkali-kali.

**Answer the questions below:**

- **Question:** In the file "invoice.html", how many times does the code show the alert "Hacked"?
- **Answer:** ?

- **Question:** Which of the JS interactive elements should be used to display a dialogue box that asks the user for input?
- **Answer:** ?

- **Question:** If the user enters "Tesla", what value is stored in the carName variable in `carName = prompt("What is your car name?")`?
- **Answer:** ?

---

## Bypassing Control Flow Statements

Di Client-Side (Browser), kita punya kontrol penuh atas kode JavaScript yang berjalan. Artinya, validasi yang cuma ngandelin JS saja itu **TIDAK AMAN**.

**Konsep:**
*   Website pakai `if-else` buat mengecek sesuatu (misal: Umur > 18, Password bener/salah).
*   Kita bisa manipulasi alur ini lewat **Browser Console** atau **Debugger**.
*   Caranya: Ubah nilai variabel di tengah jalan, atau ubah logikanya.

**Skenario Room:**
*   Ada pengecekan umur.
*   Ada form login admin.

**Answer the questions below:**

- **Question:** What is the message displayed if you enter the age less than 18?
- **Answer:** ?

- **Question:** What is the password for the user admin?
- **Answer:** ?

---

## Exploring Minified Files

Kode JS di production biasanya di-**Minify** dan di-**Obfuscate**.
*   **Minification:** Hapus spasi, enter, komentar. Tujuannya agar file kecil & load cepat.
*   **Obfuscation:** Membuat kode jadi ribet dibaca manusia (ubah nama variabel jadi `a`, `b`, `x`, dll). Tujuannya agar susah dicolong/dibaca orang lain.

**Cara Baca (Deobfuscate):**
*   Pakai fitur **"Pretty Print"** (tanda `{}` di DevTools Chrome/Firefox).
*   Pakai tool online seperti **JSNice** atau **Beautifier**.

**Skenario Room:**
Kita dikasih file `hello.html` dan script yang sudah di-obfuscate. Kita harus baca nilai variabel `age` dari kode hex yang ribet.

**Answer the questions below:**

- **Question:** What is the alert message shown after running the file "hello.html"?
- **Answer:** ?

- **Question:** What is the value of the age variable in the following obfuscated code snippet?
- **Answer:** ?

---

## Best Practices

Agar kode JavaScript kita aman dan tidak gampang dihack, ada beberapa aturan main (Best Practices):

1.  **Don't rely solely on Client-Side Validation**
    *   Jangan cuma ngandelin JS buat validasi (seperti cek umur atau password tadi).
    *   *Alasannya:* JS berjalan di browser user, jadi user bisa matiin atau manipulasi kodenya. Validasi WAJIB lakuin lagi di **Server-Side**.

2.  **Don't Include Untrusted Libraries**
    *   Hati-hati kalau mengambil script dari internet (CDN atau library asing).
    *   Kalau script itu disusupi malware, website kita juga kena dampaknya (Supply Chain Attack).

3.  **Hati-hati menyimpan Secrets**
    *   JANGAN PERNAH simpen API Key, Password, atau Token di dalam kode JS.
    *   Ingat, **View Source** itu gampang banget. Siapapun bisa melihat.

**Answer the questions below:**

- **Question:** Is it a good practice to blindly include JS in your code from any source (yea/nay)?
- **Answer:** ?

---

## Conclusion

Selesai sudah room **Javascript Essentials** ini!

**Kita sudah belajar:**
1.  **Basic JS:** Variable, Data Types, Functions, Loops.
2.  **HTML Integration:** Internal vs External JS.
3.  **Dialogue Functions:** `alert`, `prompt`, `confirm`.
4.  **Security Concepts:**
    *   Client-Side Bypass (Control Flow).
    *   Minification & Obfuscation.
    *   Bahaya menyimpan rahasia di JS.

JavaScript itu powerful banget buat membuat web interaktif, tapi kalau tidak ati-ati, bisa jadi celah keamanan yang fatal. *Stay curious and keep learning!*





