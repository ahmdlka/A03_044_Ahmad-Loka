# A03\_044\_Ahmad-Loka

**Tugas Program Komnum 2025**
Final Project Komputasi Numerik yang dikerjakan oleh saya Ahmad Loka sebagai salah satu anggota dari kelompok A03

---

### Bahas Fungsi Main

```python
f = input("Masukkan fungsi f(x) nya: ")
f = sympify(f)
xl = float(input("Masukkan batas bawahnya (XL): "))
xu = float(input("Masukkan batas atasnya (XU): "))
x_real = float(input("Masukkan X sebenarnya: "))
bagi_dua(xl, xu, x_real, f)
```

1. Masukkan input buat fungsinya dalam bentuk string, contoh: `f = x*3 + 10*x*2 - 7*x - 196`
2. Jadikan inputan fungsi tersebut menjadi operasi matematika menggunakan `sympify`
3. Input nilai (float) `Xl`, `Xu`, dan `Xsebenarnya`
4. Masuk ke sub-fungsi `bagi_dua`

---

### Bahas Sub-fungsi `bagi_dua`, `Et`, dan `Ea`

#### Fungsi `error_true`

```python
def error_true(x_real, xr):
    return abs((x_real - xr) / x_real) * 100
```

1. Untuk menemukan nilai **Error True**, yang mana nilai yang didapat dibandingkan dengan nilai variabel sebenarnya

#### Fungsi `error_aprox`

```python
def error_aprox(xr, xr_old):
    return abs((xr - xr_old) / xr) * 100 if xr != 0 else float('inf')
```

1. Untuk menemukan nilai **Error Aprox**, yang mana nilai yang didapat dibandingkan dengan nilai sebelumnya
2. Pastikan bahwa `Xr != 0`, jika tidak `float('inf')` karena angka 0 bisa menjadi pembagi

---

### Fungsi `bagi_dua`

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

        if f_xl * f_xr < 0:
            xu = xr
        elif f_xl * f_xr > 0:
            xl = xr
        else:
            if i == 0:
                print(f"iterasi {i+1}: xr = {xr}, Et = {et_value}, Ea = Belum bisa dicari")
            else:
                ea_value = round(error_aprox(xr, xr_old), 2)
                print(f"iterasi {i+1}: xr = {xr}, Et = {et_value}, Ea = {ea_value}")
            break 

        if i == 0:
            print(f"iterasi {i+1}: xr = {xr}, Et = {et_value}, Ea = Belum bisa dicari")
        else:
            ea_value = round(error_aprox(xr, xr_old), 2)
            print(f"iterasi {i+1}: xr = {xr}, Et = {et_value}, Ea = {ea_value}")

        if 0 <= et_value < 1:
            break

        xr_old = xr
        i += 1
```

1. Mengubah `X` dalam bentuk string menjadi bentuk variabel math
2. Set `Xr_old = 0`, hal ini berguna untuk menentukan `Ea` nanti
3. `while True`, dan diakhiri ketika `0 < Et < 1`
4. `xr` diperoleh dari `(Xl + Xu) / 2`, lalu dibulatkan dengan syntax `round`
5. `f_xl` dan `f_xr` dimasukkan ke dalam fungsi memakai `f.subs` dan dihitung menggunakan `evalf()`
6. Mencari `Et value` dengan masuk ke subfungsi `Et` lalu dibulatkan
7. Kalau semisal `f_xl * f_xr < 0`, maka pada iterasi selanjutnya `xu = xr`; sebaliknya, `xl = xr`
8. Selanjutnya ngeprint setiap iterasi, dan ada special case di `Ea` ketika index-nya masih 0
9. Lalu cari `Ea value`
10. Perbarui nilai `Xr_old` atau `Xr` sebelumnya
11. Increment index
