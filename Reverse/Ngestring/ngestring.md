# Ngestring

**Kategori**: Reverse Engineering
**Flag**: `SCS26{b4s1c_st4t1c_4n4lys1s_w1th_str1ngs}`

---
## Deskripsi Challenge

Diberikan sebuah file bernama `ngestring` dan format flag yang dicantumkan `SCSC26{...}`.

---
## Analisis Singkat

Karena soal mencantumkan format flag, asumsi awal yang paling masuk akal adalah **flag tersimpan di dalam binary sebagai string**. Untuk memverifikasi, digunakan utilitas `strings` untuk mengekstrak string yang terbaca dari file, lalu dilakukan penyaringan dengan `grep` untuk menemukan pola prefix flag.

---
## Proses Penyelesaian

Jalankan perintah berikut:

```bash
strings ngestring | grep SC
```

Perintah tersebut menampilkan string yang mengandung prefix flag, sehingga flag dapat diambil langsung dari output.

![Placeholder: Output](./1.png)

---
## Flag

`SCS26{b4s1c_st4t1c_4n4lys1s_w1th_str1ngs}`