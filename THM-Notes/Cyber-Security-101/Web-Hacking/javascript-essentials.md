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

## Essentials Concept

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

### Why do you have to use functions?

- Reusability: tidak perlu menulis kode yang sama sebanyak 10 kali. Cukup tulis satu kali di dalam fungsi, lalu panggil kapan pun dibutuhkan kayak prinsip **DRY (Don't Repeat yourself)**

- Organized: Kode nya jadi lebih rapi dan lebih mudah dibaca nantinya

- Modular: memecah mecah masalah besar jadi tugas-tugas kecil yang lebih gampang di maintenance.
