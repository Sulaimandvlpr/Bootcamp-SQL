## Performance Tuning & Optimization

Mengapa optimasi itu penting? Karena waktu tunggu kueri adalah biaya. Di layanan berbasis awan (seperti Google BigQuery atau AWS Redshift), kueri yang tidak efisien berarti tagihan yang lebih mahal dan waktu tunggu yang menghambat keputusan bisnis.

---

## 1. Memahami Indexing: "Daftar Isi" Database

Bayangkan mencari satu kata di buku setebal 1000 halaman tanpa daftar isi. Kamu harus membaca satu per satu (**Full Table Scan**). Indexing bertindak sebagai daftar isi yang memberi tahu database lokasi persis data tersebut.

* **B-Tree Index (Default)**: Paling umum. Cocok untuk pencarian rentang (`BETWEEN`, `>`, `<`) dan pencarian tepat (`=`).
* **Hash Index**: Sangat cepat untuk pencarian tepat (`=`), tapi tidak bisa digunakan untuk rentang.
* **Composite Index**: Index yang mencakup lebih dari satu kolom. Sangat kuat jika kamu sering melakukan filter pada kombinasi kolom tertentu (misal: `WHERE provinsi = 'Jakarta' AND kategori = 'Elektronik'`).

> **Golden Rule**: Jangan meng-index semua kolom! Index mempercepat pembacaan (`SELECT`), tapi memperlambat penulisan (`INSERT`, `UPDATE`) karena database harus memperbarui daftar isinya setiap kali data berubah.

---

## 2. Execution Plan: Membedah Pikiran Database

Sebelum mengeksekusi kueri, database membuat rencana perjalanan. Kita bisa melihat rencana ini menggunakan perintah `EXPLAIN` atau `EXPLAIN ANALYZE`.

```sql
EXPLAIN ANALYZE
SELECT id_pelanggan, SUM(total)
FROM transaksi
WHERE tgl_transaksi > '2026-01-01'
GROUP BY id_pelanggan;

```

**Hal yang harus diperhatikan dalam output:**

* **Seq Scan (Sequential Scan)**: Database membaca seluruh tabel (Buruk jika tabel besar).
* **Index Scan**: Database menggunakan index (Bagus!).
* **Cost**: Angka estimasi sumber daya yang digunakan. Semakin kecil, semakin baik.

---

## 3. Partitioning: Memecah Raksasa

Partitioning adalah teknik membagi tabel raksasa menjadi bagian-bagian kecil yang lebih mudah dikelola secara fisik, namun tetap terlihat sebagai satu tabel secara logis.

* **Range Partitioning**: Berdasarkan rentang (misal: satu partisi per bulan).
* **List Partitioning**: Berdasarkan daftar nilai (misal: satu partisi per wilayah/negara).

**Keuntungan**: Jika kamu hanya mencari data bulan Januari 2026, database hanya akan membaca partisi Januari dan mengabaikan (pruning) sisanya. Ini menghemat pemrosesan data secara drastis.

---

## 4. SQL Best Practices untuk Kecepatan

Selain teknik di atas, cara kamu menulis kode sangat berpengaruh:

* **Hindari `SELECT ***`: Selalu sebutkan kolom yang dibutuhkan. Memanggil data yang tidak perlu adalah pemborosan *bandwidth* dan memori.
* **Sargable Queries (Searchable Arguments)**: Hindari menggunakan fungsi pada kolom di dalam klausa `WHERE`.
*     *Buruk*: `WHERE YEAR(tgl_transaksi) = 2026` (Ini mematikan fungsi index).
*     *Baik*: `WHERE tgl_transaksi >= '2026-01-01' AND tgl_transaksi <= '2026-12-31'`.


* **Gunakan `LIMIT` untuk Eksplorasi**: Jangan tarik jutaan baris hanya untuk melihat contoh data.
* **Gunakan `UNION ALL` daripada `UNION**`: Jika kamu tahu tidak ada duplikat, `UNION ALL` jauh lebih cepat karena database tidak perlu melakukan proses pengecekan duplikat yang berat.

---

## Sesi Mentor: Skenario Optimasi Nyata

**Masalah**: Kueri laporan bulanan di perusahaanmu butuh waktu 5 menit untuk selesai.

**Langkah Diagnosis & Solusi**:

1. Jalankan `EXPLAIN ANALYZE`. Ternyata ditemukan `Seq Scan` pada tabel `sales` yang berisi 50 juta baris.
2. **Tindakan 1**: Tambahkan Index pada kolom `tgl_transaksi`.
3. **Tindakan 2**: Ubah kueri agar tidak menggunakan fungsi pada kolom filter.
4. **Tindakan 3**: Jika data sangat besar, ajukan ke tim Data Engineer untuk melakukan **Partitioning** berdasarkan tahun.

**Hasil**: Kueri turun dari 5 menit menjadi **3 detik**. Kamu baru saja menyelamatkan waktu produktif seluruh tim analis.

---

## Mengapa ini Penting untuk Portofolio?

Di GitHub, sertakan folder bernama `optimization/`. Isi dengan contoh kueri "Sebelum" dan "Sesudah", lalu lampirkan hasil `EXPLAIN ANALYZE`-nya. Ini menunjukkan bahwa kamu bukan sekadar "coders", tapi seorang **Engineer** yang peduli pada efisiensi sistem.

---

### Kesimpulan Mentor

Optimasi adalah seni menyeimbangkan antara kecepatan baca dan kecepatan tulis. Tidak ada solusi satu untuk semua, tapi dengan memahami Indexing dan Execution Plan, kamu punya senter untuk melihat kegelapan di dalam mesin database.
