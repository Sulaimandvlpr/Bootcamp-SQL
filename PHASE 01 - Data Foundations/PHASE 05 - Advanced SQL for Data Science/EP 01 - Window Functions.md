## Window Functions (Analytic Functions)

Dalam SQL standar, saat kamu menggunakan `GROUP BY`, baris-baris data akan "menciut" menjadi satu baris ringkasan. **Window Functions** melakukan perhitungan di seluruh set baris tabel yang masih berhubungan dengan baris saat ini, tetapi baris-baris tersebut tetap terpisah.

Ciri khas Window Function adalah adanya klausa **`OVER()`**.

---

## 1. Anatomi Window Function

Struktur dasarnya adalah:
`FUNCTION_NAME() OVER (PARTITION BY column_a ORDER BY column_b)`

* **`PARTITION BY`**: Membagi data menjadi kelompok-kelompok (seperti `GROUP BY`, tapi tidak menciutkan baris).
* **`ORDER BY`**: Menentukan urutan data di dalam "jendela" (*window*) tersebut.

---

## 2. Ranking Functions (Peringkat)

Sangat berguna untuk mencari "Siapa pemenang di setiap kategori?".

* **`ROW_NUMBER()`**: Memberikan nomor urut unik untuk setiap baris.
* **`RANK()`**: Memberikan peringkat. Jika ada nilai kembar, peringkat berikutnya akan "lompat" (1, 2, 2, 4).
* **`DENSE_RANK()`**: Peringkat tanpa lompatan (1, 2, 2, 3).

### 💡 Contoh Kasus: Mencari Top 3 Produk per Kategori

```sql
SELECT 
    product_name, 
    category, 
    revenue,
    DENSE_RANK() OVER (PARTITION BY category ORDER BY revenue DESC) as sales_rank
FROM sales_data;

```

---

## 3. Value Functions (Analisis Tren/Time-Series)

Ini adalah fungsi "ajaib" yang memungkinkan kita mengintip data di baris sebelum atau sesudahnya tanpa menggunakan `SELF JOIN` yang lambat.

* **`LAG()`**: Mengambil nilai dari baris **sebelumnya**. Cocok untuk menghitung pertumbuhan (*growth*).
* **`LEAD()`**: Mengambil nilai dari baris **berikutnya**.
* **`FIRST_VALUE()` / `LAST_VALUE()**`: Mengambil nilai pertama atau terakhir dalam satu jendela.

### Contoh Kasus: Menghitung Pertumbuhan Penjualan Harian

```sql
SELECT 
    order_date, 
    daily_revenue,
    LAG(daily_revenue) OVER (ORDER BY order_date) as prev_day_revenue,
    (daily_revenue - LAG(daily_revenue) OVER (ORDER BY order_date)) as revenue_diff
FROM daily_sales;

```

---

## 4. Aggregate Window Functions (Kumulatif)

Kamu bisa menggunakan fungsi agregasi standar (`SUM`, `AVG`, `COUNT`) sebagai Window Function.

* **Running Total**: Menghitung total yang terus bertambah seiring baris berjalan.
* **Moving Average**: Menghitung rata-rata bergerak (misal: rata-rata 7 hari terakhir) untuk menghaluskan fluktuasi data.

### Contoh Kasus: Running Total Penjualan

```sql
SELECT 
    order_date, 
    revenue,
    SUM(revenue) OVER (ORDER BY order_date) as running_total
FROM sales;

```

---

## Final Case Study: "Performance Analysis Deep Dive"

Mari kita buat kueri yang akan membuat rekruter terpukau. Kita akan menganalisis data transaksi karyawan:

1. Hitung total penjualan tiap karyawan.
2. Berikan peringkat karyawan di departemennya masing-masing.
3. Bandingkan penjualan mereka dengan rata-rata penjualan di departemen tersebut.

```sql
SELECT 
    employee_name,
    department,
    sales_amount,
    -- 1. Memberikan peringkat berdasarkan sales di tiap departemen
    DENSE_RANK() OVER (PARTITION BY department ORDER BY sales_amount DESC) as rank_in_dept,
    
    -- 2. Menghitung rata-rata departemen untuk dibandingkan dengan sales individu
    AVG(sales_amount) OVER (PARTITION BY department) as dept_avg_sales,
    
    -- 3. Menghitung kontribusi kumulatif penjualan di dalam departemen
    SUM(sales_amount) OVER (PARTITION BY department ORDER BY sales_amount DESC) as cumulative_dept_sales
FROM employees_performance;

```

---

## Mentor's Summary: Kenapa Window Function adalah "Senjata Pamungkas"?

1. **Tanpa Menghilangkan Detail**: Kamu bisa melihat `Total Gaji Departemen` tepat di samping `Gaji Individu`.
2. **Performa Tinggi**: Dibandingkan menggunakan *Correlated Subqueries* atau banyak *Self-Join*, Window Functions jauh lebih cepat dieksekusi oleh mesin database.
3. **Analisis Kompleks Jadi Mudah**: Menghitung *Month-over-Month Growth* atau *Year-to-Date Total* hanya butuh 1 baris kode.

---

### Kesimpulan

Window Functions adalah kunci untuk melakukan **Advanced Analytics**. Jika kamu bisa menjelaskan dan mendemonstrasikan kueri di atas dalam sesi wawancara, kamu secara otomatis berada di atas 90% kandidat lainnya.
