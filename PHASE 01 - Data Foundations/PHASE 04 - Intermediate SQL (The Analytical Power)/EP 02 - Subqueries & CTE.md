## Subqueries & CTE (Common Table Expressions)

Selamat datang di level di mana kamu mulai berpikir seperti seorang *Data Architect*. Di bab ini, kita akan belajar cara memecah masalah kompleks menjadi potongan-potongan logika yang mudah dikelola.

---

## 1. Subqueries: Kueri di Dalam Kueri

Subquery adalah kueri `SELECT` yang bersarang di dalam perintah SQL lain (`SELECT`, `INSERT`, `UPDATE`, atau `DELETE`).

### A. Scalar Subquery (Satu Nilai)

Menghasilkan tepat satu baris dan satu kolom. Biasanya digunakan sebagai filter.

* **Kasus**: "Tampilkan semua produk yang harganya **di atas rata-rata**."

```sql
SELECT nama_produk, harga
FROM produk
WHERE harga > (SELECT AVG(harga) FROM produk); -- Subquery mencari rata-rata dulu

```

### B. Columnar/Multiple-Row Subquery

Menghasilkan satu kolom dengan banyak baris. Biasanya dipasangkan dengan operator `IN`.

* **Kasus**: "Tampilkan pelanggan yang pernah membeli produk kategori 'Elektronik'."

```sql
SELECT nama_pelanggan 
FROM pelanggan
WHERE id_pelanggan IN (
    SELECT id_pelanggan 
    FROM transaksi 
    WHERE kategori = 'Elektronik'
);

```

### C. Correlated Subquery (Tingkat Lanjut)

Subquery yang bergantung pada data dari kueri luar. Ini seperti melakukan "looping" di SQL.

* **Mentor Note:** Berhati-hatilah dengan performanya. Correlated subquery sering kali lambat pada dataset besar karena dieksekusi berulang kali untuk setiap baris di kueri utama.

---

## 2. CTE (Common Table Expressions): Standar Profesional

Jika Subquery adalah "cara cepat", maka **CTE** adalah "cara profesional". CTE menggunakan klausa `WITH` untuk membuat tabel sementara yang hanya hidup selama kueri tersebut dijalankan.

### Mengapa Harus Pakai CTE?

1. **Readability**: Kamu membaca kueri dari atas ke bawah, bukan dari dalam ke luar (seperti subquery).
2. **Reusable**: Kamu bisa memanggil CTE yang sama berkali-kali dalam satu kueri.
3. **Recursive**: Hanya CTE yang bisa melakukan kueri rekursif (penting untuk data struktur organisasi/hierarki).

### Contoh Implementasi CTE:

Mari kita pecah masalah ini: "Hitung total belanja per pelanggan, lalu cari rata-rata dari total belanja tersebut."

```sql
WITH total_belanja_pelanggan AS (
    -- Langkah 1: Hitung total per pelanggan
    SELECT 
        id_pelanggan, 
        SUM(total_harga) AS total
    FROM transaksi
    GROUP BY id_pelanggan
)
-- Langkah 2: Ambil rata-rata dari "tabel sementara" di atas
SELECT AVG(total) AS rata_rata_belanja_toko
FROM total_belanja_pelanggan;

```

---

## 3. Strategi: Kapan Pakai Subquery vs CTE vs JOIN?

Sebagai mentor, saya sering melihat pertanyaan ini. Berikut adalah *rule of thumb* di industri:

| Situasi | Pilihan Utama | Alasan |
| --- | --- | --- |
| Filter sederhana berdasarkan satu nilai | **Subquery** | Cepat dan *to-the-point*. |
| Menghubungkan data dari tabel berbeda | **JOIN** | Mesin database sangat teroptimasi untuk JOIN. |
| Logika bertingkat (Aggregasi di atas Aggregasi) | **CTE** | Jauh lebih mudah dibaca dan di-*debug* oleh tim lain. |
| Hierarki Data (Induk -> Anak) | **Recursive CTE** | Satu-satunya cara efisien di SQL. |

---

## Sesi Mentor: Studi Kasus "Top Spender Analysis"

Bayangkan manajer bertanya: *"Tampilkan nama pelanggan dan total belanja mereka, tapi hanya untuk pelanggan yang total belanjanya lebih tinggi dari rata-rata total belanja seluruh pelanggan."*

Tanpa CTE, kueri ini akan sangat memusingkan. Dengan CTE, kita buat alurnya jelas:

```sql
WITH sales_summary AS (
    SELECT id_pelanggan, SUM(amount) as total_spent
    FROM orders
    GROUP BY id_pelanggan
),
average_sales AS (
    SELECT AVG(total_spent) as avg_spent
    FROM sales_summary
)
SELECT 
    p.nama, 
    s.total_spent
FROM pelanggan p
JOIN sales_summary s ON p.id_pelanggan = s.id_pelanggan
WHERE s.total_spent > (SELECT avg_spent FROM average_sales);

```

---

## Best Practices untuk Portofolio GitHub-mu

1. **Gunakan Nama CTE yang Deskriptif**: Jangan beri nama `cte1`, `cte2`. Gunakan `monthly_revenue`, `active_users`, dsb.
2. **Indentasi adalah Koentji**: SQL yang rapi menunjukkan profesionalisme. Pastikan klausa `WITH` dan `SELECT` utama sejajar.
3. **Komentari Logika**: Berikan komentar singkat di atas setiap blok CTE untuk menjelaskan apa yang sedang dihitung.

---

### Kesimpulan Mentor

Subquery dan CTE adalah alat untuk menyederhanakan pikiranmu. Jangan mencoba menulis satu kueri raksasa sekaligus. Pecah masalahmu menjadi blok-blok kecil menggunakan CTE, dan kamu akan melihat betapa SQL menjadi sangat kuat untuk analisis data tingkat tinggi.
