## DQL (Data Query Language) - Fundamental

DQL adalah subset SQL yang paling sering digunakan. Fokus utamanya hanya satu perintah: `SELECT`. Namun, di balik satu perintah ini, terdapat kekuatan besar untuk menyaring, mengurutkan, dan meringkas jutaan baris data menjadi informasi yang bermakna.

---

## 1. Struktur Dasar `SELECT`

Kueri SQL selalu mengikuti urutan penulisan tertentu. Mentor menyebutnya sebagai "Kalimat Bertanya" kepada Database.

```sql
SELECT column_1, column_2 
FROM table_name;

```

* **`SELECT *`**: Mengambil semua kolom (Gunakan ini hanya untuk eksplorasi awal, jangan digunakan di aplikasi produksi karena boros memori).
* **`SELECT DISTINCT`**: Menghapus duplikat dan hanya menampilkan nilai unik.

---

## 2. Filtering: Menemukan Jarum dalam Jerami

Di dunia nyata, kita tidak butuh semua data. Kita butuh data yang memenuhi kriteria tertentu menggunakan klausa `WHERE`.

### A. Operator Perbandingan & Logika

* **Basic**: `=`, `<>`, `>`, `<`, `>=`, `<=`
* **`AND`, `OR`, `NOT**`: Menggabungkan beberapa kondisi.

### B. Operator Spesial untuk Data Scientist

* **`IN`**: Mencari nilai dalam sebuah daftar.
* *Contoh*: `WHERE kota IN ('Jakarta', 'Bandung', 'Surabaya')`


* **`BETWEEN`**: Mencari dalam rentang nilai (termasuk nilai batas).
* *Contoh*: `WHERE gaji BETWEEN 5000000 AND 10000000`


* **`LIKE`**: Pencarian pola teks (*wildcards*).
* `%`: Mewakili banyak karakter.
* `_`: Mewakili satu karakter.
* *Contoh*: `WHERE nama LIKE 'A%'` (Nama berawalan huruf A).


* **`IS NULL`**: Mencari data yang kosong/tidak terisi.

---

## 3. Ordering & Limiting: Merapikan Output

Setelah difilter, data harus disajikan secara teratur.

* **`ORDER BY`**: Mengurutkan data berdasarkan kolom tertentu.
* `ASC` (Default): Kecil ke besar.
* `DESC`: Besar ke kecil (Sering digunakan untuk melihat "Top 10" atau "Terbaru").


* **`LIMIT`**: Membatasi jumlah baris yang muncul.
* Sangat penting untuk efisiensi saat bekerja dengan miliaran data.


* **`OFFSET`**: Melewati sejumlah baris (Biasanya digunakan untuk fitur *pagination* di aplikasi).

---

## 4. Aggregations: Kekuatan Statistika Dasar

Inilah tempat di mana data berubah menjadi angka ringkasan. Fungsi agregasi melakukan perhitungan pada sekumpulan nilai dan menghasilkan satu nilai tunggal.

| Fungsi | Deskripsi | Kasus Penggunaan |
| --- | --- | --- |
| **`COUNT()`** | Menghitung jumlah baris | Mengetahui jumlah total pelanggan. |
| **`SUM()`** | Menghitung total nilai | Menghitung total pendapatan bulanan. |
| **`AVG()`** | Menghitung rata-rata | Mencari rata-rata nilai ujian siswa. |
| **`MIN()`** | Menemukan nilai terkecil | Mencari harga produk termurah. |
| **`MAX()`** | Menemukan nilai terbesar | Mencari skor transaksi tertinggi. |

---

## 5. Grouping Data: `GROUP BY` & `HAVING`

Agregasi menjadi 10x lebih kuat jika digabungkan dengan pengelompokan.

### `GROUP BY`

Digunakan untuk mengelompokkan baris yang memiliki nilai yang sama ke dalam baris ringkasan.

* *Contoh*: "Tampilkan total penjualan **per kategori** produk."

### `HAVING`

Ini adalah "WHERE" untuk hasil agregasi. Kamu tidak bisa menggunakan `WHERE` untuk memfilter hasil `SUM` atau `COUNT`.

* *Contoh*: "Tampilkan kategori produk yang total penjualannya **lebih dari 100 juta**."

---

## Sesi Mentor: Studi Kasus Analisis Penjualan

Mari kita gabungkan semua ilmu di atas ke dalam satu kueri "Sakti" untuk menjawab tantangan bisnis:

**Pertanyaan Bisnis:**
*"Tampilkan 3 kategori buku yang paling laris (total stok terjual > 50), urutkan dari yang paling banyak terjual, dan hitung rata-rata harganya!"*

```sql
SELECT 
    kategori, 
    SUM(stok_terjual) AS total_laku, 
    AVG(harga) AS rata_rata_harga
FROM buku
WHERE status = 'Tersedia'        -- 1. Filter data mentah
GROUP BY kategori                -- 2. Kelompokkan
HAVING SUM(stok_terjual) > 50    -- 3. Filter hasil agregasi
ORDER BY total_laku DESC         -- 4. Urutkan dari yang terlaris
LIMIT 3;                         -- 5. Ambil Top 3

```

---

## Tips Expert: Urutan Eksekusi SQL (Logika Mesin)

Secara tertulis kita memulai dengan `SELECT`, tapi mesin database mengeksekusinya dengan urutan berbeda. Memahami ini akan menyelamatkanmu dari *error*:

1. **`FROM`** (Cari tabelnya)
2. **`WHERE`** (Filter baris mentah)
3. **`GROUP BY`** (Kelompokkan)
4. **`HAVING`** (Filter kelompok)
5. **`SELECT`** (Pilih kolom/hitung agregasi)
6. **`ORDER BY`** (Urutkan hasil akhir)
7. **`LIMIT`** (Potong jumlah baris)

---

### Kesimpulan Mentor

DQL Fundamental adalah pondasi dari semua laporan *dashboard* di dunia. Jika kamu menguasai `GROUP BY` dan fungsi agregasi, kamu sudah bisa menyelesaikan 70% masalah pelaporan di perusahaan mana pun.
