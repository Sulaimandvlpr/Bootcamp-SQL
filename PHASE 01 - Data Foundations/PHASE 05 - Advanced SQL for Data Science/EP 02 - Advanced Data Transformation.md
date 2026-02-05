## Advanced Data Transformation (Pivoting & Regex)

Data di database sering kali disimpan dalam format **Long Format** (banyak baris ke bawah) demi efisiensi penyimpanan. Namun, manusia dan alat visualisasi sering kali membutuhkan **Wide Format** (banyak kolom ke samping). Di bab ini, kita akan menguasai cara mengubah bentuk data tersebut secara programatik.

---

## 1. Pivoting: Baris menjadi Kolom

Pivoting adalah teknik merotasi data dari baris menjadi kolom. Ini sangat berguna untuk membuat laporan perbandingan bulanan atau tahunan.

### A. Manual Pivoting (Standard SQL)

Banyak RDBMS tidak memiliki fungsi `PIVOT` bawaan yang simpel, jadi kita menggunakan teknik **CASE WHEN + Aggregation**.

**Kasus**: Menghitung total penjualan per kategori produk untuk tahun 2023, dipecah per kuartal (Q1, Q2, Q3, Q4).

```sql
SELECT 
    kategori,
    SUM(CASE WHEN EXTRACT(QUARTER FROM tgl_transaksi) = 1 THEN total_harga ELSE 0 END) AS Q1_Sales,
    SUM(CASE WHEN EXTRACT(QUARTER FROM tgl_transaksi) = 2 THEN total_harga ELSE 0 END) AS Q2_Sales,
    SUM(CASE WHEN EXTRACT(QUARTER FROM tgl_transaksi) = 3 THEN total_harga ELSE 0 END) AS Q3_Sales,
    SUM(CASE WHEN EXTRACT(QUARTER FROM tgl_transaksi) = 4 THEN total_harga ELSE 0 END) AS Q4_Sales
FROM penjualan
WHERE EXTRACT(YEAR FROM tgl_transaksi) = 2023
GROUP BY kategori;

```

---

## 2. Unpivoting: Kolom menjadi Baris

Kebalikan dari pivoting. Digunakan ketika data yang kamu terima berbentuk "lebar" (misal hasil ekspor Excel) dan kamu ingin memasukkannya kembali ke database dalam format "panjang" agar mudah di-agregasi.

Dalam PostgreSQL, kita sering menggunakan fungsi `unnest` atau `cross join lateral`.

---

## 3. Regular Expressions (Regex)

Regex adalah pola pencarian teks yang sangat kuat. Jika `LIKE` hanya bisa mencari `%teks%`, Regex bisa mencari pola yang sangat spesifik seperti format email, nomor telepon, atau ekstraksi kode produk di tengah kalimat.

| Simbol | Deskripsi |
| --- | --- |
| `^` | Menandakan awal string. |
| `$` | Menandakan akhir string. |
| `.` | Karakter apa saja (wildcard). |
| `[0-9]` | Mencari angka. |
| `+` | Satu atau lebih kecocokan. |

### Contoh Kasus: Validasi & Ekstraksi Teks

Bayangkan kolom `keterangan` berisi teks acak seperti: *"Order #12345 sukses oleh user@email.com"*. Kamu ingin mengambil emailnya saja.

```sql
-- Menggunakan PostgreSQL syntax (~ untuk regex)
SELECT 
    keterangan,
    substring(keterangan FROM '([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})') AS email_extracted
FROM log_transaksi
WHERE keterangan ~ '@'; -- Filter hanya yang mengandung email

```

---

## Sesi Mentor: Studi Kasus "Market Basket Transformation"

Seorang Data Scientist sering diminta menyiapkan data untuk **Association Rule Mining** (seperti: "Orang yang beli sabun juga beli pasta gigi"). Data asli biasanya berbentuk:

| Transaction_ID | Item |
| --- | --- |
| 101 | Sabun |
| 101 | Pasta Gigi |

Untuk algoritma tertentu, kita perlu mengubahnya menjadi baris tunggal per transaksi. Kita bisa menggunakan **String Aggregation**:

```sql
SELECT 
    transaction_id,
    STRING_AGG(item, ', ') AS basket_list
FROM market_basket
GROUP BY transaction_id;

```

**Hasilnya**: `101 | Sabun, Pasta Gigi`

---

## Mengapa Teknik ini Strategis untuk Portofolio?

1. **Pivoting**: Menunjukkan kamu bisa membuat laporan yang "Business-Ready" langsung dari SQL tanpa perlu bantuan Excel.
2. **Regex**: Menunjukkan kemampuan *Data Cleaning* tingkat lanjut. Kamu bisa mengolah data *Unstructured* (teks mentah) menjadi *Structured*.
3. **String Aggregation**: Menunjukkan kamu paham cara menyiapkan dataset untuk kebutuhan spesifik Data Science (ML modeling).

---

### Kesimpulan Mentor

Transformasi data adalah tentang **perspektif**. Data yang sama bisa bercerita hal yang berbeda jika disajikan dalam bentuk yang berbeda. Kuasai bab ini, dan kamu akan menjadi orang yang paling diandalkan saat tim visualisasi data atau tim ML kesulitan mengolah data mentah.
