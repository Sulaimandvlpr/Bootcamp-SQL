## Joining Tables - Menghubungkan Ekosistem Data

Dalam database relasional, data dipecah-pecah (Normalisasi) untuk efisiensi. **JOIN** adalah mekanisme untuk menyatukan kembali data tersebut saat kita membutuhkannya untuk analisis.

---

## 1. Visualisasi Logika JOIN (Venn Diagram)

Memahami JOIN paling mudah dilakukan dengan membayangkan dua lingkaran (tabel) yang saling tumpang tindih.

### A. INNER JOIN (Si Paling Eksklusif)

Hanya mengambil baris data yang **memiliki kecocokan di kedua tabel**.

* **Kasus**: "Tampilkan daftar pelanggan yang **pernah** melakukan transaksi." (Pelanggan yang belum pernah belanja tidak akan muncul).

### B. LEFT (OUTER) JOIN (Si Paling Populer)

Mengambil **semua data dari tabel kiri**, dan data yang cocok dari tabel kanan. Jika tidak ada kecocokan di tabel kanan, hasilnya akan berisi `NULL`.

* **Kasus**: "Tampilkan **semua** pelanggan dan jumlah belanja mereka." (Pelanggan yang belum belanja tetap muncul dengan jumlah belanja `NULL`/kosong). **Ini sangat penting untuk mendeteksi pelanggan yang tidak aktif.**

### C. RIGHT (OUTER) JOIN

Kebalikan dari LEFT JOIN. Mengambil semua data dari tabel kanan. Jarang digunakan karena biasanya kita hanya menukar posisi tabel kiri dan kanan untuk tetap menggunakan LEFT JOIN agar kueri lebih mudah dibaca.

### D. FULL OUTER JOIN

Mengambil **semua data** dari kedua tabel, baik yang cocok maupun yang tidak. Sangat berguna untuk audit data guna menemukan rekaman yang "yatim piatu" di kedua sisi.

### E. SELF JOIN

Menggabungkan sebuah tabel dengan dirinya sendiri.

* **Kasus**: Tabel karyawan yang memiliki kolom `manager_id`. Kita melakukan JOIN ke tabel itu sendiri untuk mengetahui siapa nama manajer dari setiap karyawan.

---

## 2. Struktur Sintaks Profesional

Gunakan *alias* tabel agar kueri kamu bersih dan tidak membingungkan mesin.

```sql
SELECT 
    p.nama_lengkap, 
    t.tgl_transaksi, 
    t.total_harga
FROM pelanggan AS p
INNER JOIN transaksi AS t 
    ON p.id_pelanggan = t.id_pelanggan;

```

---

## 3. Strategi JOIN pada Dataset Besar (Industry Standard)

Saat bekerja di perusahaan dengan data harian jutaan baris, asal melakukan JOIN bisa membuat database *down* atau kueri berjalan berjam-jam. Berikut strateginya:

1. **Filter Dulu, JOIN Kemudian**: Jika hanya butuh data tahun 2024, filter tanggalnya sebelum melakukan JOIN ke tabel besar lainnya.
2. **Perhatikan Index**: Pastikan kolom yang digunakan untuk menghubungkan (`ON table_a.id = table_b.id`) memiliki **Index**. Tanpa Index, database melakukan *Full Table Scan* yang sangat lambat.
3. **Hati-hati dengan Many-to-Many**: Jika kamu melakukan JOIN pada kolom yang tidak unik di kedua sisi, kamu akan menghasilkan **Cartesian Product** (baris data meledak jumlahnya secara tidak terkendali).

---

## Sesi Mentor: Studi Kasus "Detecting Churn"

Bayangkan manajer pemasaran bertanya: *"Berikan saya daftar pelanggan yang sudah mendaftar tapi belum pernah beli buku apapun, kita akan kirimkan voucher diskon."*

Sebagai *Data Scientist*, kamu tidak bisa pakai `INNER JOIN`. Kamu butuh `LEFT JOIN` dan filter `IS NULL`.

```sql
SELECT 
    p.id_pelanggan, 
    p.nama, 
    p.email
FROM pelanggan AS p
LEFT JOIN pesanan AS o 
    ON p.id_pelanggan = o.id_pelanggan
WHERE o.id_pesanan IS NULL; -- Mencari pelanggan yang tidak punya record di tabel pesanan

```

---

## Perbedaan JOIN dan UNION (Sering Salah Paham)

Mentor sering melihat pemula bingung antara JOIN dan UNION.

* **JOIN**: Menggabungkan tabel secara **Horizontal** (menambah kolom ke samping). Menghubungkan pelanggan dengan alamatnya.
* **UNION**: Menggabungkan tabel secara **Vertikal** (menambah baris ke bawah). Menggabungkan daftar pelanggan dari cabang Jakarta dan cabang Surabaya menjadi satu daftar panjang.

---

### Kesimpulan Mentor

Kemampuan JOIN adalah pemisah antara pengguna SQL amatir dan profesional. Jangan hanya dihafalkan kuerinya, tapi pahami **aliran datanya**. Gunakan `LEFT JOIN` sebagai *default* jika kamu ragu apakah semua data memiliki pasangan, karena data yang hilang jauh lebih berbahaya daripada data yang berisi `NULL`.
