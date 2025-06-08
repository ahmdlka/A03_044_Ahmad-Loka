# **A03\_044\_Ahmad Loka Arziki**

## **Final Project Komputasi Numerik 2025**

Disusun oleh Ahmad Loka Arziki, mahasiswa Teknik Informatika ITS, sebagai bagian dari kelompok A03.

---

## ✨ **Penjelasan Fungsi `main()`**

```python
f = input("Masukkan fungsi f(x) nya: ")
f = sympify(f)
xl = float(input("Masukkan batas bawah (XL): "))
xu = float(input("Masukkan batas atas (XU): "))
x_real = float(input("Masukkan nilai X sebenarnya: "))
bagi_dua(xl, xu, x_real, f)
```

### Alur:

1. **Input fungsi f(x)** dalam bentuk string, misalnya:
   `x*3 + 10*x**2 - 7*x - 196`
2. **Konversi fungsi** ke bentuk simbolik matematika dengan `sympify()`.
3. **Input nilai batas bawah (`xl`)**, batas atas (`xu`), dan nilai `x_real` (akar sebenarnya).
4. Panggil fungsi utama `bagi_dua()`.

---

## 🔍 **Sub-Fungsi: Error dan Metode Bagi Dua**

### 🔹 `error_true(x_real, xr)`

```python
def error_true(x_real, xr):
    return abs((x_real - xr) / x_real) * 100
```

* Menghitung **True Error (Et)**, yaitu selisih antara akar eksak dan pendekatan saat ini.

---

### 🔹 `error_aprox(xr, xr_old)`

```python
def error_aprox(xr, xr_old):
    return abs((xr - xr_old) / xr) * 100 if xr != 0 else float('inf')
```

* Menghitung **Aproksimasi Error (Ea)** antara hasil iterasi saat ini dengan sebelumnya.
* Jika `xr == 0`, maka hasilnya `inf` untuk menghindari pembagian dengan nol.

---

### 🔹 `bagi_dua(xl, xu, x_real, f)`

```python
def bagi_dua(xl, xu, x_real, f):
    x = symbols('x')
    xr_old = 0
    i = 0

    while True:
        xr = round((xl + xu) / 2, 2)
        f_xl = f.subs(x, xl).evalf()
        f_xr = f.subs(x, xr).evalf()

        et_value = round(error_true(x_real, xr), 2)

        # Update batas
        if f_xl * f_xr < 0:
            xu = xr
        elif f_xl * f_xr > 0:
            xl = xr
        else:
            if i == 0:
                print(f"Iterasi {i+1}: xr = {xr}, Et = {et_value}%, Ea = Belum bisa dicari")
            else:
                ea_value = round(error_aprox(xr, xr_old), 2)
                print(f"Iterasi {i+1}: xr = {xr}, Et = {et_value}%, Ea = {ea_value}%")
            break

        # Cetak iterasi
        if i == 0:
            print(f"Iterasi {i+1}: xr = {xr}, Et = {et_value}%, Ea = Belum bisa dicari")
        else:
            ea_value = round(error_aprox(xr, xr_old), 2)
            print(f"Iterasi {i+1}: xr = {xr}, Et = {et_value}%, Ea = {ea_value}%")

        # Stop jika error true sudah cukup kecil
        if 0 <= et_value < 1:
            break

        xr_old = xr
        i += 1
```

### Penjelasan Langkah:

1. Buat simbol `x` untuk evaluasi fungsi.
2. Inisialisasi nilai awal `xr_old = 0` dan counter `i = 0`.
3. Loop hingga error true di bawah 1%.
4. Hitung titik tengah `xr`, kemudian evaluasi `f(xl)` dan `f(xr)`.
5. Tentukan error true.
6. Tentukan apakah akar ada di \[xl, xr] atau \[xr, xu], lalu update batasnya.
7. Cetak hasil iterasi dengan format:

   * Jika iterasi pertama: Ea belum bisa dicari.
   * Iterasi selanjutnya: tampilkan nilai Ea.
8. Update nilai `xr_old` dan iterasi.

---
