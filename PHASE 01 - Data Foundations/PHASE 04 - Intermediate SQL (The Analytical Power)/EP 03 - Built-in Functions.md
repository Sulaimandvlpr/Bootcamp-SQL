## Built-in Functions - Manipulasi & Transformasi Data

Sebagai *Data Scientist*, kamu akan sering menerima data dalam keadaan "mentah" atau berantakan. Mengandalkan Python untuk semua proses pembersihan data (*data cleaning*) bisa sangat lambat jika datanya berukuran besar. **Built-in Functions** memungkinkan kita melakukan pembersihan dan transformasi langsung di dalam *engine* database.

---

## 1. String Functions (Pembersihan Teks)

Data teks sering kali memiliki inkonsistensi (spasi liar, perbedaan huruf besar/kecil, atau format yang menyatu).

* **`TRIM()`**: Menghapus spasi di awal dan akhir string.
* **`UPPER()` / `LOWER()**`: Menyamakan format teks (sangat penting untuk kueri `JOIN` atau `WHERE` agar tidak *case-sensitive*).
* **`CONCAT()`**: Menggabungkan dua atau lebih string.
* **`REPLACE(string, old, new)`**: Mengganti bagian teks tertentu.
* **`SUBSTR(string, start, length)`**: Mengambil potongan teks.

### Contoh Kasus: Normalisasi Nama & Email

```sql
SELECT 
    TRIM(UPPER(first_name)) AS clean_first_name,
    LOWER(CONCAT(first_name, '.', last_name, '@company.com')) AS generated_email
FROM employees;

```

---

## 2. Date & Time Functions (Analisis Temporal)

Analisis tren bisnis sangat bergantung pada manipulasi waktu.

* **`EXTRACT(part FROM date)`**: Mengambil komponen spesifik (YEAR, MONTH, DAY, HOUR).
* **`DATE_TRUNC('part', date)`**: Membulatkan tanggal ke interval terdekat (misal: membulatkan tanggal ke awal bulan untuk laporan bulanan).
* **`AGE(timestamp)`**: Menghitung interval waktu dari sekarang (sering digunakan untuk menghitung umur atau masa kerja).
* **`TO_CHAR()`**: Mengubah format tanggal menjadi teks yang mudah dibaca (misal: 'Monday, Jan 2026').

---

## 3. Numeric & Logical Functions (Transformasi Nilai)

Mengolah angka dan menangani data yang hilang (*missing values*).

* **`COALESCE(val1, val2, ...)`**: Mengambil nilai pertama yang tidak `NULL`. Ini adalah fungsi wajib untuk menangani data kosong.
* **`ROUND(value, decimal_places)`**: Membulatkan angka desimal.
* **`CASE WHEN`**: Logika kondisional (If-Then-Else) untuk membuat pengkategorian baru.

---

## Final Case Study: "E-Commerce Data Cleansing & Segmentation"

Mari kita terapkan semuanya dalam satu skenario nyata. Kita akan mengolah tabel `orders` yang kotor menjadi laporan yang siap dianalisis oleh tim bisnis.

**Problem:**

1. Nama pelanggan berantakan (spasi liar & beda *case*).
2. Ada kolom `discount` yang isinya `NULL` (harus dianggap 0).
3. Kita perlu tahu bulan transaksi untuk laporan *seasonality*.
4. Kita perlu mengelompokkan transaksi menjadi 'Small', 'Medium', dan 'Big' berdasarkan total harga.

**The Solution Query:**

```sql
SELECT 
    -- Merapikan Nama
    UPPER(TRIM(customer_name)) AS clean_name,

    -- Menangani NULL pada diskon & menghitung harga akhir
    -- Tanpa COALESCE, jika diskon NULL, maka final_price akan NULL (Error Logika)
    ROUND(price * (1 - COALESCE(discount, 0)), 2) AS final_price,

    -- Mengambil Nama Bulan saja
    TO_CHAR(order_date, 'Month') AS transaction_month,

    -- Segmentasi Transaksi dengan CASE WHEN
    CASE 
        WHEN (price * quantity) > 1000000 THEN 'Big Transaction'
        WHEN (price * quantity) BETWEEN 500000 AND 1000000 THEN 'Medium Transaction'
        ELSE 'Small Transaction'
    END AS transaction_tier,

    -- Menghitung sudah berapa lama transaksi terjadi (dalam hari)
    EXTRACT(DAY FROM (CURRENT_TIMESTAMP - order_date)) AS days_ago

FROM orders
WHERE status = 'Completed';

```

---

## Mentor's Summary: Kenapa ini Penting?

Seorang *Data Scientist* yang mahir menggunakan fungsi bawaan SQL akan:

1. **Menghemat Waktu**: Data yang ditarik ke Python/Tableau sudah dalam keadaan "bersih".
2. **Efisiensi Memori**: Melakukan kalkulasi di sisi server database jauh lebih cepat daripada memproses jutaan baris di memori lokal (RAM) komputer kita.
3. **Akurasi**: Menghindari kesalahan perhitungan akibat nilai `NULL` dengan menggunakan `COALESCE`.

