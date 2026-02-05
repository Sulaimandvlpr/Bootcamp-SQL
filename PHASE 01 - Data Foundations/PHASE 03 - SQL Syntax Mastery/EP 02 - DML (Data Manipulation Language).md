## DML (Data Manipulation Language) - Manajemen Konten

DML adalah sekumpulan perintah SQL yang digunakan untuk memanipulasi data di dalam tabel. Sebagai seorang praktisi data, kamu akan sering berinteraksi dengan DML ketika melakukan proses *Data Ingestion* (memasukkan data) atau *Data Cleaning* (membersihkan data yang salah).

---

## 1. Operasi CRUD Dasar

Dalam pengembangan perangkat lunak dan manajemen data, kita mengenal istilah **CRUD** (*Create, Read, Update, Delete*). Di SQL, operasi ini direpresentasikan oleh perintah DML.

> **Catatan Mentor:** Meskipun `SELECT` sering dianggap bagian dari DQL (Data Query Language), dalam konteks praktis ia sering dikelompokkan dalam operasi manipulasi data karena perannya dalam membaca konten.

---

## 2. Perintah `INSERT`: Menambahkan Data Baru

Perintah ini digunakan untuk memasukkan baris (*rows*) baru ke dalam tabel.

### A. Insert Tunggal (Single Row)

Cara paling dasar untuk memasukkan satu baris data:

```sql
INSERT INTO students (first_name, last_name, email, date_of_birth, gpa)
VALUES ('Budi', 'Sudarsono', 'budi.s@email.com', '2001-05-15', 3.75);

```

### B. Insert Banyak Data (Bulk Insert)

Di industri, kita jarang memasukkan data satu per satu. Kita biasanya memasukkan banyak data sekaligus untuk efisiensi:

```sql
INSERT INTO students (first_name, last_name, email, date_of_birth, gpa)
VALUES 
    ('Siti', 'Aminah', 'siti.a@email.com', '2002-11-20', 3.80),
    ('Andi', 'Wijaya', 'andi.w@email.com', '2001-02-10', 3.45),
    ('Rina', 'Pratiwi', 'rina.p@email.com', '2003-07-25', 3.90);

```

---

## 3. Perintah `UPDATE`: Mengubah Data yang Ada

Digunakan untuk memodifikasi baris data yang sudah tersimpan.

**PERINGATAN KERAS DARI MENTOR:** Jangan pernah lupa menggunakan klausa `WHERE` saat melakukan `UPDATE`. Jika kamu lupa, **SEMUA** baris di tabel tersebut akan berubah!

### A. Update Spesifik

Misal, Andi Wijaya baru saja memperbaiki nilai dan GPA-nya naik:

```sql
UPDATE students
SET gpa = 3.60
WHERE student_id = 3; -- Menggunakan Primary Key sebagai penanda unik

```

### B. Update Berdasarkan Kondisi

Menaikkan GPA semua mahasiswa yang saat ini di bawah 3.5 sebesar 0.1 (mungkin karena kebijakan bonus):

```sql
UPDATE students
SET gpa = gpa + 0.1
WHERE gpa < 3.5;

```

---

## 4. Perintah `DELETE`: Menghapus Data

Digunakan untuk membuang baris data yang tidak lagi diperlukan atau salah input.

**PERINGATAN KERAS DARI MENTOR:** Sama seperti `UPDATE`, gunakan selalu `WHERE`. Tanpa `WHERE`, tabelmu akan kosong melompong (walaupun strukturnya tetap ada).

### A. Menghapus Baris Tertentu

```sql
DELETE FROM students
WHERE email = 'siti.a@email.com';

```

### B. Perbedaan `DELETE` vs `TRUNCATE` (Review Bab 3.1)

* **`DELETE`**: Menghapus baris demi baris, bisa difilter pakai `WHERE`, mencatat log aktivitas (lebih lambat tapi aman).
* **`TRUNCATE`**: Menghapus seluruh isi tabel secara instan, tidak bisa pakai `WHERE`, tidak mencatat log baris (sangat cepat).

---

## 5. Upsert: `INSERT ... ON CONFLICT` (Expert Insight)

Dalam *Data Pipeline* nyata, sering kali kita menghadapi situasi: "Kalau datanya belum ada, masukkan (Insert). Kalau sudah ada, perbarui (Update)." Ini disebut **UPSERT**.

Di PostgreSQL, kita menggunakan:

```sql
INSERT INTO students (student_id, first_name, last_name, email, gpa)
VALUES (1, 'Budi', 'Sudarsono', 'budi.s@email.com', 3.95)
ON CONFLICT (student_id) 
DO UPDATE SET gpa = EXCLUDED.gpa;

```

*Mentor Note: Ini sangat berguna saat kamu melakukan sinkronisasi data harian agar tidak terjadi error "Duplicate Primary Key".*

---

## Sesi Mentor: Studi Kasus Kasir Toko

Bayangkan kamu adalah admin di Toko Buku Online yang kita desain di Fase 2. Berikut alur kerja DML-mu:

1. **Stok Datang:** Kamu memasukkan buku baru.
```sql
INSERT INTO buku (id_buku, judul, harga, stok) 
VALUES ('B-05', 'Mastering SQL', 150000, 100);

```


2. **Buku Terjual:** Kamu harus mengurangi stok.
```sql
UPDATE buku 
SET stok = stok - 1 
WHERE id_buku = 'B-05';

```


3. **Salah Input:** Kamu memasukkan pelanggan fiktif dan ingin menghapusnya.
```sql
DELETE FROM pelanggan WHERE id_pelanggan = 'C-999';

```



---

## Best Practices DML untuk Data Science

1. **Gunakan Transaksi:** Jika kamu melakukan banyak perubahan sekaligus, gunakan `BEGIN TRANSACTION` dan `COMMIT`. Jika ada yang salah di tengah jalan, kamu bisa `ROLLBACK`.
2. **Cek Sebelum Eksekusi:** Sebelum menjalankan `DELETE` atau `UPDATE`, jalankan dulu kueri tersebut sebagai `SELECT` untuk memastikan baris yang terkena dampak sudah benar.
* *Contoh:* Ketik `SELECT * FROM students WHERE gpa < 3.0` dulu sebelum mengetik `DELETE FROM students WHERE gpa < 3.0`.


3. **Paham Constraints:** Jika kamu gagal `INSERT`, cek apakah ada kolom yang `NOT NULL` atau `UNIQUE` yang kamu langgar.

---

### Kesimpulan Mentor

DML adalah tentang bagaimana kita menjaga agar data tetap relevan dan akurat. Seorang *Data Scientist* yang mahir DML akan sangat terbantu dalam fase *Data Wrangling*. Tanpa penguasaan DML, kamu akan kesulitan memperbaiki data yang kotor langsung dari sumbernya.
