## DDL (Data Definition Language) - Membangun Struktur

DDL adalah bahasa arsitek. Dalam dunia Data Science, meskipun kamu lebih sering menarik data (`SELECT`), memahami DDL adalah wajib agar kamu tahu batasan (*constraints*) dari data yang kamu olah. Jika kamu tahu sebuah kolom dibuat dengan aturan `UNIQUE`, kamu tidak akan heran kenapa tidak ada data duplikat di sana.

---

## 1. Perintah `CREATE`: Membangun dari Nol

Perintah `CREATE` digunakan untuk membuat objek baru di database. Kita akan fokus pada dua hal: **Database** dan **Table**.

### A. Membuat Database

```sql
CREATE DATABASE bootcamp_db;

```

*Mentor Note:* Biasanya setelah membuat database, kamu perlu memberikan instruksi `USE bootcamp_db;` (di MySQL) agar sistem tahu kamu ingin bekerja di dalam gudang tersebut.

### B. Membuat Tabel (The Real Work)

Mari kita praktekkan desain **Tabel Students** yang sudah kita buat di Bab 2.2. Perhatikan detail tipe data dan *constraints*-nya:

```sql
CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,        -- SERIAL: Angka urut otomatis (1, 2, 3...)
    first_name VARCHAR(50) NOT NULL,      -- VARCHAR: Teks dengan batas karakter
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,   -- UNIQUE: Tidak boleh ada email kembar
    date_of_birth DATE,                   -- DATE: Format YYYY-MM-DD
    gpa DECIMAL(3, 2) CHECK (gpa <= 4.0), -- DECIMAL(TotalDigit, DigitKoma)
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- Mencatat waktu input otomatis
);

```

---

## 2. Memahami Tipe Data (Data Types)

Sebagai mentor, saya ingin menekankan: **Pemilihan tipe data yang salah adalah awal dari bencana.**

| Tipe Data | Kegunaan | Alasan Expert |
| --- | --- | --- |
| `INT` / `BIGINT` | Angka Bulat | Gunakan `BIGINT` untuk data yang sangat besar (seperti ID transaksi global). |
| `VARCHAR(n)` | Teks Fleksibel | Lebih hemat memori dibanding `CHAR` karena hanya memakan ruang sesuai panjang teks. |
| `DECIMAL` / `NUMERIC` | Angka Desimal Presisi | **WAJIB** untuk uang/keuangan. Jangan gunakan `FLOAT` untuk uang karena masalah pembulatan. |
| `BOOLEAN` | True / False | Cocok untuk status (misal: `is_active`). |
| `TEXT` | Teks Sangat Panjang | Digunakan untuk ulasan produk atau deskripsi panjang. |

---

## 3. Perintah `ALTER`: Renovasi Struktur

Di industri, bisnis selalu berubah. Mungkin tiba-tiba manajer meminta kita mencatat nomor telepon siswa. Kita tidak perlu menghapus tabel dan membuatnya lagi (karena data di dalamnya akan hilang). Kita gunakan `ALTER`.

### A. Menambah Kolom Baru

```sql
ALTER TABLE students ADD COLUMN phone_number VARCHAR(15);

```

### B. Mengubah Tipe Data Kolom

Misal, kita ingin memperpanjang batas karakter nama belakang:

```sql
ALTER TABLE students ALTER COLUMN last_name TYPE VARCHAR(100);

```

### C. Menghapus Kolom

*Hati-hati: Perintah ini akan menghapus seluruh data di dalam kolom tersebut secara permanen!*

```sql
ALTER TABLE students DROP COLUMN gpa;

```

---

## 4. Perintah `DROP` vs `TRUNCATE`

Ini adalah pertanyaan favorit saat interview kerja. Keduanya terdengar mirip, tapi fungsinya sangat berbeda.

### A. `DROP TABLE` (Hancurkan Gedung)

Menghapus tabel beserta seluruh strukturnya dari database. Tabel tersebut hilang dari daftar.

```sql
DROP TABLE students;

```

### B. `TRUNCATE TABLE` (Kosongkan Isi)

Menghapus seluruh **isi data** di dalam tabel, tetapi **strukturnya tetap ada**. Ini jauh lebih cepat daripada menghapus baris satu per satu.

```sql
TRUNCATE TABLE students;

```

---

## 5. Hubungan Antar Tabel (Foreign Key DDL)

Mari kita buat tabel kedua, yaitu `enrollments`, yang menghubungkan siswa dengan kursus menggunakan **Foreign Key**. Inilah inti dari *Relational Database*.

```sql
CREATE TABLE enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INT,
    course_name VARCHAR(100),
    enrollment_date DATE,
    
    -- Jembatan ke tabel students
    CONSTRAINT fk_student 
        FOREIGN KEY (student_id) 
        REFERENCES students(student_id)
        ON DELETE CASCADE -- Jika siswa dihapus, data pendaftarannya otomatis ikut terhapus
);

```

---

## 👨‍🏫 Sesi Mentor: Mengapa DDL Penting untuk Data Science?

Mungkin kamu berpikir, *"Saya kan mau jadi Analyst, kenapa harus pusing bikin tabel?"*

1. **Data Cleaning**: Saat kamu tahu kolom `date_of_birth` bertipe `DATE`, kamu tidak perlu repot mengubah teks menjadi tanggal di Python nanti.
2. **Performance**: Database yang dibuat dengan tipe data yang tepat dan memiliki **Index** (akan dibahas di level Expert) akan membuat kueri kamu 100x lebih cepat.
3. **Data Integrity**: *Constraints* (seperti `CHECK` atau `NOT NULL`) memastikan data "sampah" tidak masuk ke sistem. Ingat prinsip: *Garbage In, Garbage Out.*

---

### Kesimpulan Mentor

DDL adalah cara kita memberikan aturan main kepada komputer. Tanpa DDL yang kuat, data akan menjadi kacau dan tidak konsisten. Kuasai perintah-perintah ini untuk memastikan kamu punya kontrol penuh atas "Gudang Data" kamu.
