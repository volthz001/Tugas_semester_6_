---

# 📄 File: `README.md`

```markdown
# Tugas UTS Teknik Kompilasi - Mini Compiler

| **Nama** | Hizkia Siallagan |
|----------|------------------|
| **NIM**  | 231011400154 |
| **Kelas** | 06TPLMP002 |
| **Mata Kuliah** | Teknik Kompilasi |
| **Topik** | Lexer, Parser (EBNF), AST, dan TAC |

---

## 📌 Deskripsi Tugas

Mahasiswa diminta untuk melengkapi sebuah *Mini Compiler* dengan menambahkan dukungan untuk **operator pangkat (`^`)**. Operator pangkat memiliki prioritas lebih tinggi daripada operator perkalian (`*`) dan pembagian (`/`) dalam hirarki matematika.

---

## 🛠️ Fitur yang Diimplementasikan

| No | Fitur | Keterangan |
|----|-------|-------------|
| 1 | Lexical Analysis (Lexer) | Mengenali token berupa angka, variabel, operator `+`, `-`, `*`, `/`, `(`, `)`, dan `^` |
| 2 | Parsing (EBNF) | Membangun AST berdasarkan grammar dengan prioritas operator yang benar |
| 3 | Operator Pangkat (`^`) | Prioritas lebih tinggi dari `*` dan `/` |
| 4 | Three Address Code (TAC) | Menghasilkan kode tiga alamat dari AST |

---

## 📂 Struktur File

```
├── Tugas_UTS_Hizkia_Siallagan.ipynb   # Notebook utama
├── README.md                          # File ini
```

---

## 🚀 Cara Menjalankan

1. Buka file `Tugas_UTS_Hizkia_Siallagan.ipynb` menggunakan **Jupyter Notebook** atau **Google Colab**
2. Jalankan seluruh sel secara berurutan
3. Hasil TAC akan ditampilkan di bagian bawah

### Contoh Input dan Output

**Input:**
```
a ^ 2 + b * c
```
**Output:**
```
t1 = a ^ 2
t2 = b * c
t3 = t1 + t2
```

---

## 🔧 Implementasi Utama

### TUGAS 1: Memperbarui Regex
Menambahkan simbol `^` ke dalam pola regex untuk dikenali sebagai token.

```python
self._tokens = iter(re.findall(r'[a-zA-Z_]\w*|\d+(?:\.\d+)?|[+*/()\-^]', source) + ['?'])
```

### TUGAS 2: Fungsi `power()`
Membuat fungsi khusus untuk menangani operator pangkat.

```python
def power(self):
    node = self.factor()
    while self._current == '^':
        op = self._current
        self.advance()
        node = BinOp(left=node, op=op, right=self.factor())
    return node
```

### TUGAS 3: Memodifikasi `term()`
Mengubah pemanggilan di dalam `term()` dari `factor()` menjadi `power()`.

```python
def term(self):
    node = self.power()
    while self._current in ('*', '/'):
        op = self._current
        self.advance()
        node = BinOp(left=node, op=op, right=self.power())
    return node
```

---

## 📝 Jawaban Pertanyaan Refleksi

### 1. Mengapa `power()` dipanggil di dalam `term()`?

Operator pangkat (`^`) memiliki prioritas lebih tinggi daripada perkalian (`*`) dan pembagian (`/`). Dalam *recursive descent parsing*, fungsi dengan prioritas lebih tinggi dipanggil pada level yang lebih dalam, sehingga `power()` dipanggil di dalam `term()`.

### 2. Apa yang terjadi jika variabel tidak ada di `symbol_table`?

Kompilator akan mendeteksi *semantic error* dan melemparkan exception dengan pesan *"Semantic Error: Undefined variable '...'"*. Proses kompilasi dihentikan dan TAC tidak dihasilkan.

### 3. Mengapa TAC untuk `a ^ 2` muncul sebelum `+`?

Karena AST dibangun dengan prioritas operator yang benar, dan fungsi `generate_tac()` melakukan traversal *post-order*, sehingga anak kiri (`^`) diproses terlebih dahulu sebelum node `+`.

---

## 📦 Requirements

- Python 3.x
- Jupyter Notebook atau Google Colab

Tidak ada *library* eksternal yang diperlukan selain dari pustaka standar Python (`re`).

---

## 📅 Informasi Pengumpulan

| **Tanggal Pengumpulan** | 13 Mei 2026 |
| **Tempat Pengumpulan** | Mentari / E-learning |
| **Repositori** | Public GitHub |

---

## 👤 Kontributor

**Hizkia Siallagan**  
231011400154 / 06TPLMP002

---

*Dibuat untuk memenuhi tugas UTS Mata Kuliah Teknik Kompilasi.*
```

---
