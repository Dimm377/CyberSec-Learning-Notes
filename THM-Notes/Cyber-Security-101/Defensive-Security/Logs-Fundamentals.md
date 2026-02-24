# TryHackMe: Logs Fundamentals

- **Room Link:** [Logs Fundamentals](https://tryhackme.com/room/logsfundamentals)
- **Kategori:** Defensive Security
- **Difficulty:** easy

---

Lanjut ke *room* **Logs Fundamentals**. *Room* ini bakal ngebahas tuntas soal apa itu *Logs*, kenapa penting banget, dan gimana cara kita ngebaca jejak-jejak digital.

## Task 1: Introduction to Logs

Attacker pasti berusaha sebisa mungkin ngehapus jejak (meninggalkan *trace* sesedikit mungkin) di sistem korban biar nggak ketahuan. Tapi kok tim Security tetep bisa mengetahui gimana serangannya terjadi, bahkan nemuin siapa dalangnya?

**Analoginya gini:**
Bayangin ada polisi lagi investigasi kasus orang hilang di gubuk tengah hutan salju.
Polisi nemuin petunjuk: pintu kayu hacur, atap jebol, jejak sepatu di salju, sama rekaman CCTV dari tetangga kejauhan. Dengan nyatuin kepingan-kepingan petunjuk (_traces_) ini, polisi akhirnya bisa tau kronologi dan siapa penjahatnya.

Nah, **Logs** di dunia cyber itu ibarat kepingan-kepingan petunjuk yang ditinggalin sama maling tadi. Kumpulan _traces_ dari berbagai sumber bakal digabungin buat jadi petunjuk menuju si kriminal.
