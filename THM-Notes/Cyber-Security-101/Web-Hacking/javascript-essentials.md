# TryHackMe: Javascript-Essentials

- **Room Link:** [Javascript-essentials](https://tryhackme.com/room/javascriptessentials)
- **Category:** Web Hacking
- **Difficulty:** Easy

## Task 1: Introduction

JavaScript adalah bahasa scripting populer yang memungkinkan kita sebagai developer untuk menambahkan fitur interaktif ke dalam website yang sudah memiliki struktur (HTML) dan tampilan (CSS).

Tanpa JS, website cuma jadi web statis. Dengan JS, kita bisa mmebuat website itu hidup.

**Learning Objectives:**

- Memahami dasar-dasar javascript
- Integrasi Javascript ke HTML
- Menyalahgunakan dialogue function
- Bypassing Control Flow Statements
- Exploring Minified files

## Task 2: Essentials Concept

### Variables

**Variables** adalah sebuah container (wadah) yang memungkinkan kita menyimpan nilai data di dalam nya, bayangkan variable adalah kotak berlabel tempat kita dapat menyimpan data secara berbeda, contoh nya kita punya kotak berlabel `age` maka yang ada di dalam kotak tersebut bisa masukkan angka **(7,27,15)**

**3 cara mendeklarasikan variables di javascript:**

- Gunakan **Let** ketika ingin nilai di dalam nya berubah
- Pakai **Const** kalau ingin nilai nya tetap
- Lupakan **Var** Udah kuno ngapain make ini yang penting 2 diatas itu sekarang

### Data Types

di JS, tipe data menentukan tipe nilai yang bisa ditampung oleh sebuah variable

**Contoh Tipe Data:**

- Number: Mewakili bilangan integer dan float (misalnya 42, 3.14)
- String: Mewakili rangkaian karakter, ditahdai dengan tanda kutip (misalnya, 'halo', "dunia")
- Boolean: Tipe data yang hanya ada 2 value **True & False**, sering digunakan untuk memvalidasi sebuah kondisi
- Object: objek adalah kumpulan properti, di mana setiap properti didefinisikan sebagai pasangan nilai kunci. Objek memungkinkan kita mengelompokkan data dan fungsi terkait menjadi satu, contoh `{ name: 'Alice', age: 25 }`
- Array: arrayc adalah tipe objek khusus yang digunakan untuk menyimpan daftar nilai dalam satu variabel, contoh `let numbers = [1, 2, 3, 4, 5];`
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

- Reusability: tidak perlu menulis kode yang sama sebanyak 10 kali. Cukup tulis satu kali di dalam fungsi, lalu panggil kapan pun dibutuhkan kayak prinsip **DRY (Don't Repeat yourself)**

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
  _(Clue: Perulangan dalam bahasa Inggris)_

---

## Task 3: JavaScript Overview

Task ini sebenernya cuma review singkat dan demo script sederhana.
Ilustrasinya ada kode JS buat nambahin angka.

**Poin Penting:**
*   **Interpreted Language:** JavaScript itu bahasa yang diterjemahkan baris per baris saat dijalankan (oleh browser), bukan dicompile jadi file binary dulu kayak C++ atau Java.
*   **Variable Manipulation:** Kita bisa ubah nilai variabel kapan aja (kalau pake `let` atau `var`).

**Answer the questions below:**

- **Question:** What is the code output if the value of x is changed to 10?
- **Answer:** ?


- **Question:** Is JavaScript a compiled or interpreted language?
- **Answer:** ?
  _(Clue: Dieksekusi langsung oleh browser)_

---

## Task 4: Integrating JavaScript in HTML

Ada 2 cara utama buat masukin kode JS ke dalam HTML:

**1. Internal JavaScript**
Kode ditulis langsung di dalam file HTML, di antara tag `<script>`.
```html
<script>
    console.log("Hello from Internal JS");
</script>
```
*   **Cocok buat:** Kode pendek, testing.
*   **Minus:** Bikin file HTML jadi penuh/berantakan.

**2. External JavaScript**
Kode ditulis di file terpisah (ekstensi `.js`), terus dipanggil di HTML pake atribut `src`.
```html
<script src="script.js"></script>
```
*   **Cocok buat:** Proyek besar, *Code Reusability* (satu file JS bisa dipake banyak halaman).
*   **Plus:** Rapi, caching browser lebih optimal.

**Answer the questions below:**

- **Question:** Which type of JavaScript integration places the code directly within the HTML document?
- **Answer:** ?
  _(Clue: Internal)_

- **Question:** Which method is better for reusing JS across multiple web pages?
- **Answer:** ?
  _(Clue: External)_

- **Question:** What is the name of the external JS file that is being called by "external_test.html"?
- **Answer:** ?
  _(Clue: thm_external.js)_

- **Question:** What attribute links an external JS file in the script tag?
- **Answer:** ?
  _(Clue: src)_


