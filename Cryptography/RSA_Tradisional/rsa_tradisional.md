# RSA Tradisional

**Kategori**: Cryptography  
**Flag**: `SCSC26{sm4ll_pr1m3_1s_w34k}`

---
## Deskripsi Singkat

Diberikan parameter RSA dalam berkas `chall.txt`:

- `n` (modulus)
- `e = 65537` (public exponent)
- `c` (ciphertext)
- `p` dan `q` dikosongkan.

Tujuan: dekripsi ciphertext `c` untuk memperoleh flag.

---
## Analisis

Ini adalah skema **RSA tradisional** tanpa modifikasi:

- Public key: `(n, e)`
- Ciphertext: `c = m^e mod n`
- Tanpa `p` dan `q`, normalnya kita tidak bisa menghitung private key `d`.

Dalam CTF, celah paling umum untuk RSA klasik adalah:

1. **Small prime factor**: salah satu faktor `n` (p atau q) dibuat cukup kecil → bisa ditemukan dengan trial division.
2. Setelah `p` ditemukan, `q = n // p` dan seluruh kunci privat dapat direkonstruksi.

---
## Proses Penyelesaian

1. **Parsing parameter**
   - Ambil `n`, `e`, dan `c` dari `chall.txt` sebagai integer.

2. **Cek faktor kecil (common weakness)**
   - Lakukan trial division (loop integer kecil) untuk mencari `p` yang membagi `n`:
     ```python
     for p in range(2, LIMIT):
         if n % p == 0:
             break
     q = n // p
     ```
   - Jika faktor ditemukan cepat → **konfirmasi bahwa ini kasus “small prime”**.

3. **Rekonstruksi kunci privat**
   - Hitung:
     ```python
     phi = (p - 1) * (q - 1)
     d = pow(e, -1, phi)
     ```

4. **Dekripsi ciphertext**
   - Hitung plaintext sebagai integer:
     ```python
     m = pow(c, d, n)
     ```
   - Konversi ke teks:
     ```python
     m_hex = hex(m)[2:]
     if len(m_hex) % 2 == 1:
         m_hex = "0" + m_hex
     flag = bytes.fromhex(m_hex).decode()
     ```

5. **Hasil**
   - String hasil dekripsi berbentuk:
     ```text
     SCSC26{sm4ll_pr1m3_1s_w34k}
     ```

---
## Script Akhir

```python
#!/usr/bin/env python3

from math import isqrt

n = int("2144831537182691127226840733029608917028213290006813425996254255600138294915857888555028089760852947821230571957658788758912243893627426892642042247199424294789573427071703290644668266217757721636207129972073199609043937414663647879045094904418897021363777158941294486149117818217208033779367195636217363694694541")

e = 65537

c = int("1983234254934925761486284406677131831481570946662392531654817098945817202064275913205054573643771458003143250325225227419382435408653586301494982444166548588851767106811874491325904092340088285777126941515587285167887079578329783781806162085914505940944650957530660228135260917298087314905239242563505803779036874")

def find_small_factor(n, limit=1_000_000):
    if n % 2 == 0:
        return 2
    for p in range(3, limit, 2):
        if n % p == 0:
            return p
    raise ValueError(f"Tidak ditemukan faktor kecil hingga {limit}")

# 1. Cari faktor kecil p
p = find_small_factor(n)
q = n // p
print(f"[+] p ditemukan: {p}")
print(f"[+] q = n // p")

# 2. Hitung φ(n)
phi = (p - 1) * (q - 1)

# 3. Hitung kunci privat d
d = pow(e, -1, phi)
print(f"[+] d (private exponent) diperoleh")

# 4. Dekripsi ciphertext
m = pow(c, d, n)

# 5. Konversi integer m ke bytes lalu ke string
m_hex = hex(m)[2:]

if len(m_hex) % 2 == 1:
    m_hex = "0" + m_hex

plaintext = bytes.fromhex(m_hex).decode("utf-8", errors="ignore")

print(f"Flag: {plaintext}")
```

---
## Flag

```text
SCSC26{sm4ll_pr1m3_1s_w34k}}
```