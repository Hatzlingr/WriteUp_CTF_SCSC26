# basis64

**Kategori**: Crypto 
**Flag**: `SCSC26{g1t4r_kup3t1k_b4ss_kub3t0t}`

---
## Deskripsi Challenge

Diberikan sebuah file `desc.md` berisi pesan berikut:

> I learn base64 for fun, can you decode it?  
> `U0NTQzI2e2cxdDRyX2t1cDN0MWtfYjRzc19rdWIzdDB0fQ==`

Tujuan challenge adalah mendapatkan flag dengan mendekode string yang diberikan.

---
## Analisis Singkat

String yang diberikan dapat diidentifikasi sebagai **Base64** karena:
- Menggunakan karakter yang umum pada Base64 (A–Z, a–z, 0–9, dan simbol tertentu).
- Memiliki **padding `=`** di akhir (`==`), ciri khas Base64 untuk menyamakan panjang data menjadi kelipatan 4.
- Deskripsi challenge secara eksplisit menyebut basis64.

Solusi cukup melakukan **Base64 decoding** untuk memperoleh plaintext flag.

---
## Proses Penyelesaian

1. Salin string Base64 dari file `desc.md`:
    
    ```
    U0NTQzI2e2cxdDRyX2t1cDN0MWtfYjRzc19rdWIzdDB0fQ==
    ```
    
1. Decode menggunakan Python (modul `base64` bawaan):
    - `base64.b64decode()` mengubah Base64 menjadi bytes asli.
    - `.decode()` mengonversi bytes menjadi string agar terbaca.
    
    Jalankan script berikut:
    ```python
    import base64
    
    s = "U0NTQzI2e2cxdDRyX2t1cDN0MWtfYjRzc19rdWIzdDB0fQ=="
    print(base64.b64decode(s).decode())
    ```
    
2. Output yang dihasilkan adalah flag.

---
## Flag

`SCSC26{g1t4r_kup3t1k_b4ss_kub3t0t}`