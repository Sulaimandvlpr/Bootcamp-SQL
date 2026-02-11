# Joining Tables — Menghubungkan Ekosistem Data

Dalam database relasional, data dipecah menjadi beberapa tabel (normalisasi) untuk:

* Menghindari duplikasi data
* Menjaga konsistensi
* Meningkatkan efisiensi storage

Namun untuk analisis, data tersebut harus digabungkan kembali.
Itulah fungsi utama dari **JOIN**.

---

# 0. Setup Contoh Data

Kita gunakan dua tabel sederhana.

## Tabel `customers`

| id | name  | city     |
| -- | ----- | -------- |
| 1  | Andi  | Jakarta  |
| 2  | Budi  | Bandung  |
| 3  | Citra | Surabaya |
| 4  | Deni  | Medan    |

## Tabel `orders`

| id | customer_id | order_date | amount |
| -- | ----------- | ---------- | ------ |
| 1  | 1           | 2024-01-10 | 500000 |
| 2  | 1           | 2024-02-15 | 300000 |
| 3  | 2           | 2024-03-01 | 250000 |
| 4  | 5           | 2024-04-01 | 100000 |

Perhatikan:

* Customer 3 dan 4 belum pernah order
* Ada order dengan `customer_id = 5` (tidak ada di tabel customers)

Ini akan membantu memahami setiap jenis JOIN.

---

# 1. INNER JOIN

## Konsep

Mengambil hanya data yang memiliki pasangan di kedua tabel.

Artinya:

* Jika tidak ada kecocokan → tidak muncul

## Query

```sql
SELECT 
    c.id,
    c.name,
    o.order_date,
    o.amount
FROM customers c
INNER JOIN orders o
    ON c.id = o.customer_id;
```

## Output

| id | name | order_date | amount |
| -- | ---- | ---------- | ------ |
| 1  | Andi | 2024-01-10 | 500000 |
| 1  | Andi | 2024-02-15 | 300000 |
| 2  | Budi | 2024-03-01 | 250000 |

Penjelasan:

* Citra dan Deni tidak muncul (tidak punya order)
* Order dengan customer_id 5 tidak muncul (tidak punya pasangan di customers)

INNER JOIN = irisan (intersection)

---

# 2. LEFT JOIN

## Konsep

Ambil semua data dari tabel kiri, meskipun tidak ada pasangan di tabel kanan.

Jika tidak ada pasangan → kolom tabel kanan menjadi NULL.

## Query

```sql
SELECT 
    c.id,
    c.name,
    o.order_date,
    o.amount
FROM customers c
LEFT JOIN orders o
    ON c.id = o.customer_id;
```

## Output

| id | name  | order_date | amount |
| -- | ----- | ---------- | ------ |
| 1  | Andi  | 2024-01-10 | 500000 |
| 1  | Andi  | 2024-02-15 | 300000 |
| 2  | Budi  | 2024-03-01 | 250000 |
| 3  | Citra | NULL       | NULL   |
| 4  | Deni  | NULL       | NULL   |

Penjelasan:

* Semua customer muncul
* Yang tidak punya order → NULL

Ini sangat penting untuk:

* Analisis churn
* Analisis user tidak aktif
* Data completeness check

---

# 3. RIGHT JOIN

Kebalikan dari LEFT JOIN.

Ambil semua data dari tabel kanan.

## Query

```sql
SELECT 
    c.id,
    c.name,
    o.order_date,
    o.amount
FROM customers c
RIGHT JOIN orders o
    ON c.id = o.customer_id;
```

## Output

| id   | name | order_date | amount |
| ---- | ---- | ---------- | ------ |
| 1    | Andi | 2024-01-10 | 500000 |
| 1    | Andi | 2024-02-15 | 300000 |
| 2    | Budi | 2024-03-01 | 250000 |
| NULL | NULL | 2024-04-01 | 100000 |

Penjelasan:

* Order dengan customer_id 5 tetap muncul
* Karena tidak ada customer yang cocok → kolom customer NULL

Di industri, RIGHT JOIN jarang dipakai.
Biasanya cukup tukar posisi tabel dan gunakan LEFT JOIN.

---

# 4. FULL OUTER JOIN

Mengambil semua data dari kedua tabel.

## Query

```sql
SELECT 
    c.id,
    c.name,
    o.order_date,
    o.amount
FROM customers c
FULL OUTER JOIN orders o
    ON c.id = o.customer_id;
```

## Output

| id   | name  | order_date | amount |
| ---- | ----- | ---------- | ------ |
| 1    | Andi  | 2024-01-10 | 500000 |
| 1    | Andi  | 2024-02-15 | 300000 |
| 2    | Budi  | 2024-03-01 | 250000 |
| 3    | Citra | NULL       | NULL   |
| 4    | Deni  | NULL       | NULL   |
| NULL | NULL  | 2024-04-01 | 100000 |

Digunakan untuk:

* Data reconciliation
* Audit data mismatch
* Sinkronisasi sistem

---

# 5. SELF JOIN

Menggabungkan tabel dengan dirinya sendiri.

Contoh:

## Tabel `employees`

| id | name | manager_id |
| -- | ---- | ---------- |
| 1  | Rina | NULL       |
| 2  | Tono | 1          |
| 3  | Sari | 1          |

## Query

```sql
SELECT 
    e.name AS employee_name,
    m.name AS manager_name
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.id;
```

## Output

| employee_name | manager_name |
| ------------- | ------------ |
| Rina          | NULL         |
| Tono          | Rina         |
| Sari          | Rina         |

Ini digunakan untuk:

* Hierarki organisasi
* Struktur kategori bertingkat
* Relationship parent-child

---

# 6. JOIN + Aggregation (Level Analyst)

Misalnya ingin total belanja per customer.

```sql
SELECT 
    c.name,
    SUM(o.amount) AS total_spent
FROM customers c
LEFT JOIN orders o
    ON c.id = o.customer_id
GROUP BY c.name;
```

Output:

| name  | total_spent |
| ----- | ----------- |
| Andi  | 800000      |
| Budi  | 250000      |
| Citra | NULL        |
| Deni  | NULL        |

Biasanya ditambahkan:

```sql
COALESCE(SUM(o.amount), 0) AS total_spent
```

Agar NULL menjadi 0.

---

# 7. JOIN vs UNION

## JOIN

Menggabungkan secara horizontal (menambah kolom).

Contoh:

customer + orders → jadi satu baris lengkap

---

## UNION

Menggabungkan secara vertikal (menambah baris).

Contoh:

```sql
SELECT name FROM customers_jakarta
UNION
SELECT name FROM customers_bandung;
```

Output:

Daftar panjang gabungan dua cabang.

---

# 8. Industry-Level Warning: Cartesian Explosion

Jika kamu JOIN dua tabel tanpa kondisi:

```sql
SELECT *
FROM customers
JOIN orders;
```

Maka akan terjadi:

Cartesian Product
Semua baris customers dikalikan semua baris orders.

Jika:

* customers = 1 juta
* orders = 10 juta

Hasil bisa 10 triliun baris.

Database bisa crash.

Karena itu selalu gunakan ON condition yang benar.

---

# 9. Mental Model Profesional

Sebelum menulis JOIN, tanyakan:

1. Apa tabel utama?
2. Apakah semua data harus muncul?
3. Apakah relationship 1-to-1, 1-to-many, atau many-to-many?
4. Apakah ada risiko duplikasi?

JOIN bukan sekadar sintaks.
JOIN adalah desain aliran data.

---

# Kesimpulan

INNER JOIN
→ Ketika hanya ingin data yang valid dan memiliki pasangan.

LEFT JOIN
→ Default paling aman untuk analisis.

RIGHT JOIN
→ Jarang dipakai, bisa diganti dengan LEFT JOIN.

FULL OUTER JOIN
→ Audit dan rekonsiliasi data.

SELF JOIN
→ Hierarki dan relasi internal.

Kemampuan memahami JOIN secara konseptual adalah fondasi penting untuk menjadi Data Analyst, Data Engineer, maupun Data Scientist.
