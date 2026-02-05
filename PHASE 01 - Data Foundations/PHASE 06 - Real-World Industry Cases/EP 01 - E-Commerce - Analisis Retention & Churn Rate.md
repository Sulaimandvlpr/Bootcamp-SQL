## E-Commerce - Analisis Retention & Churn Rate

Dalam bisnis e-commerce, biaya mendatangkan pelanggan baru jauh lebih mahal daripada mempertahankan pelanggan lama. Oleh karena itu, kemampuan SQL untuk menghitung seberapa setia pelanggan sangatlah dicari.

### 1. Retention Rate (Tingkat Pertahanan)

Retention rate mengukur berapa banyak pelanggan yang kembali belanja di bulan berikutnya setelah transaksi pertama mereka.

**Problem Statement:**
"Dari semua pelanggan yang belanja di bulan Januari 2026, berapa persen yang kembali belanja di bulan Februari 2026?"

**Solusi SQL (Menggunakan CTE & Join):**

```sql
WITH jan_customers AS (
    SELECT DISTINCT customer_id 
    FROM orders 
    WHERE order_date >= '2026-01-01' AND order_date < '2026-02-01'
),
feb_customers AS (
    SELECT DISTINCT customer_id 
    FROM orders 
    WHERE order_date >= '2026-02-01' AND order_date < '2026-03-01'
)
SELECT 
    COUNT(f.customer_id)::FLOAT / COUNT(j.customer_id) * 100 AS retention_rate_pct
FROM jan_customers j
LEFT JOIN feb_customers f ON j.customer_id = f.customer_id;

```

---

### 2. Churn Rate (Tingkat Kepergian)

Churn terjadi ketika pelanggan berhenti menggunakan layanan atau berhenti membeli dalam jangka waktu tertentu.

**Problem Statement:**
"Siapa saja pelanggan yang sudah tidak belanja selama lebih dari 90 hari? (Potensi Churn)."

**Solusi SQL (Window Function & Date Logic):**

```sql
SELECT 
    customer_id,
    MAX(order_date) AS last_purchase_date,
    CURRENT_DATE - MAX(order_date)::DATE AS days_since_last_purchase,
    CASE 
        WHEN CURRENT_DATE - MAX(order_date)::DATE > 90 THEN 'Churned'
        WHEN CURRENT_DATE - MAX(order_date)::DATE > 30 THEN 'At Risk'
        ELSE 'Active'
    END AS status
FROM orders
GROUP BY customer_id
HAVING CURRENT_DATE - MAX(order_date)::DATE > 30;

```

---

## Kasus 2: Marketing - Segmentasi RFM

RFM (*Recency, Frequency, Monetary*) adalah teknik segmentasi pelanggan yang paling populer di dunia marketing untuk menentukan siapa pelanggan "Sultan" kita.

* **Recency**: Kapan terakhir belanja?
* **Frequency**: Berapa kali belanja?
* **Monetary**: Berapa total uang yang dihabiskan?

**Solusi SQL (Advanced Aggregation):**

```sql
WITH rfm_base AS (
    SELECT 
        customer_id,
        CURRENT_DATE - MAX(order_date)::DATE AS recency,
        COUNT(order_id) AS frequency,
        SUM(total_amount) AS monetary
    FROM orders
    GROUP BY customer_id
)
SELECT 
    customer_id,
    recency, frequency, monetary,
    NTILE(5) OVER (ORDER BY recency DESC) AS r_score, -- 5 paling baru
    NTILE(5) OVER (ORDER BY frequency ASC) AS f_score, -- 5 paling sering
    NTILE(5) OVER (ORDER BY monetary ASC) AS m_score  -- 5 paling boros
FROM rfm_base;

```

---

## Sesi Mentor: Bagaimana Menulis Ini di GitHub?

Jangan hanya copy-paste kode. Gunakan struktur **STAR** (Situation, Task, Action, Result) di file README folder kasus ini:

1. **Situation**: Jelaskan konteks bisnis (Misal: Perusahaan ingin mengurangi biaya marketing).
2. **Task**: Apa yang harus kamu hitung? (Misal: Mengidentifikasi segmen pelanggan "VIP").
3. **Action**: Jelaskan kueri SQL yang kamu gunakan (Misal: "Saya menggunakan Window Functions `NTILE` untuk membagi pelanggan ke dalam 5 kelompok...").
4. **Result**: Berikan *Insight* (Misal: "Ditemukan bahwa 20% pelanggan menyumbang 80% total revenue").
