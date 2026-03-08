# TryHackMe: FlareVM Arsenal of Tools

- **Room Link:** [FlareVM Arsenal of Tools](https://tryhackme.com/room/flarevmarsenaloftools)
- **Category:** Defensive Security Tooling
- **Difficulty:** Easy

## Introduction

Kalau di materi sebelumnya kita sudah bahas **REMnux** sebagai meja operasi *malware* berbasis Linux, sekarang saatnya kita kenalan sama saudara kandungnya di dunia Windows: **FlareVM**.

**FlareVM** (singkatan dari *Forensics, Logic Analysis, and Reverse Engineering*) adalah seperangkat kotak perkakas khusus rakitan tim FLARE dari **FireEye**. Isinya sudah dipenuhi ratusan program spesialis yang murni diciptakan buat ngebantu *reverse engineers*, analis *malware*, dan investigator forensik. Ibaratnya, kalau kamu mau menguliti sistem Windows atau ngebongkar wujud asli (*reverse engineer*) dari sebuah virus `.exe`, ini VM andalan.

### Learning Objectives

- Mengenal dan mengeksplorasi isi kotak perkakas di dalam FlareVM.
- Praktik langsung pakai berbagai *tools* bawaannya untuk menganalisis proses/*process* mencurigakan di memori.
- Familiar sama *tools* yang biasa dipakai buat investigasi statis (*static analysis*) dokumen jahat atau *file* biner (*binaries*).

---

## Arsenal of Tools (Persenjataan FlareVM)

Di dalam FlareVM, *tools*-nya dikelompokkan berdasarkan fungsinya biar kamu lebih gampang nyarinya saat lagi menangani kasus.

### 1. Reverse Engineering & Debugging
*Reverse engineering* itu ibarat kamu membongkar sebuah mesin atau barang elektronik (seperti jam mekanik atau radio) yang sudah jadi pabrikan. Tujuannya murni untuk mencari tahu komponen apa saja yang ada di dalamnya dan bagaimana cara alat itu bekerja. Sedangkan *debugging* adalah proses saat alat itu rusak, lalu kamu menganalisis bagian mana yang macet dan mencoba memperbaikinya.

Senjata andalan di kategori ini:
- **Ghidra**: *Software reverse engineering* high class yang dirilis gratis oleh NSA (Badan Intelijen Amerika).
- **x64dbg**: *Debugger* *open-source* favorit untuk membedah *file binaries* format 32-bit maupun 64-bit.
- **OllyDbg**: *Debugger* klasik yang sangat handal membedah instruksi program sampai ke level bahasa *Assembly* (bahasa mesin tingkat rendah).
- **Radare2**: *Platform reverse engineering open-source* berbasis teks yang sangat ringan dan bertenaga.
- **Binary Ninja**: *Tool* visual yang rapi untuk membantu membedah (*disassembling*) dan menyusun ulang (*decompiling*) kode biner.
- **PEiD**: Alat pendeteksi untuk mencari tahu apakah sebuah program itu dibungkus pakai pelindung kode (*packer*, *cryptor*) atau dicetak pakai perangkat lunak pembuat aplikasi (*compiler*) jenis apa.

### 2. Disassemblers & Decompilers
Dua jenis *tools* ini sangat penting dalam analisis *malware*. Tugas utamanya ibarat **Penerjemah Bahasa**, komputer berkomunikasi menggunakan angka dan bahasa mesin yang tidak bisa dibaca manusia biasa. Alat ini bertugas menerjemahkan surat rahasia tersebut ke dalam format logika (seperti kode *C++* atau *Assembly*) yang lebih mudah dipahami oleh analis.

Alat yang sering dipakai:
- **CFF Explorer**: Editor khusus (*PE editor*) untuk menganalisis dan membongkar struktur *file* berformat *Portable Executable* (khas Windows seperti `.exe` atau `.dll`).
- **Hopper Disassembler**: Paket komplit yang bisa merangkap jadi *debugger*, *disassembler*, sekaligus *decompiler*.
- **RetDec**: *Decompiler open-source* tangguh buat menerjemahkan kembali bahasa mesin agar lebih gampang dibaca.

### 3. Static & Dynamic Analysis
Ada dua metode utama dalam menginvestigasi sebuah *malware*. Ibarat pos satpam yang sedang memeriksa paket kotak mencurigakan:
> - **Static Analysis:** Membedah wujud fisik, tulisan pengirim, dan isi kotak tersebut menggunakan alat *X-Ray* **tanpa** membuka kotaknya secara langsung (metode aman).
> - **Dynamic Analysis:** Mengambil paket tersebut, memasukkannya ke dalam ruangan isolasi tebal (*sandbox*), lalu sengaja **membuka dan memicunya** untuk melihat efek apa yang terjadi pada ruangan tersebut (metode tes langsung).

Alat pemantau andalannya:
- **Process Hacker**: Versi "Task Manager" super canggih. Sangat informatif untuk memantau program apa saja yang sedang berjalan (*process watcher*) dan melihat penggunaan memori sistem.
- **PEview**: Alat ringan untuk mengintip isi dari komponen pembangun *file* `.exe` (berjenis PE).
- **Dependency Walker**: Alat yang digunakan untuk melihat daftar *file library* pendukung (DLL bawaan Windows) apa saja yang diam-diam "dipanggil" dan dibutuhkan oleh suatu aplikasi.
- **DIE (Detect It Easy)**: Alat serbaguna untuk mendeteksi apakah suatu *file* dilindungi oleh pengunci (*packer*, *cryptor*) dan alat pembuat (*compiler*) tertentu.
