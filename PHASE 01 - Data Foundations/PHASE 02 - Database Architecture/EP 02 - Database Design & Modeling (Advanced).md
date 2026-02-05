## Database Design & Modeling (Advanced)

Selamat datang di inti dari arsitektur data! Jika database adalah sebuah gedung pencakar langit, maka **Data Modeling** adalah cetak biru (*blueprint*) arsitekturalnya. Kesalahan satu sentimeter di cetak biru ini bisa menyebabkan gedung runtuh saat menampung beban jutaan data di masa depan.

---

## 1. Entitas, Atribut, dan Identifikasi Objek

Sebelum kita masuk ke teknis, kita harus melakukan abstraksi terhadap dunia nyata.

### A. Entitas (Objek Utama)

Entitas adalah segala sesuatu yang memiliki eksistensi independen dan datanya ingin kita rekam.

* **Strong Entity**: Bisa berdiri sendiri (contoh: `Pelanggan`, `Buku`).
* **Weak Entity**: Keberadaannya bergantung pada entitas lain (contoh: `Detail_Pesanan` tidak mungkin ada tanpa adanya `Pesanan`).

### B. Atribut (Karakteristik)

Setiap entitas memiliki karakteristik unik:

1. **Simple Attribute**: Tidak bisa dipecah lagi (contoh: `Harga`).
2. **Composite Attribute**: Bisa dipecah menjadi bagian-bagian kecil (contoh: `Alamat` terdiri dari `Jalan`, `Kota`, `Provinsi`, `Kode_Pos`). **Mentor Note:** Di SQL, selalu simpan atribut komposit dalam kolom terpisah agar mudah difilter (misal: mencari pelanggan hanya di kota "Jakarta").
3. **Derived Attribute**: Nilainya dihasilkan dari atribut lain (contoh: `Total_Harga` didapat dari `Qty * Harga_Satuan`). **Best Practice:** Jangan simpan atribut turunan di database permanen; hitunglah saat melakukan *query* agar data selalu akurat (sinkron).

---

## 2. Entity Relationship Diagram (ERD) & Kardinalitas

ERD adalah visualisasi hubungan antar entitas. Kita menggunakan notasi **Crow's Foot** karena ini adalah standar industri paling populer.

### Memahami Kardinalitas (Hubungan Bisnis):

* **One-to-One (1:1)**: Jarang, biasanya untuk data sensitif. Satu `User` memiliki satu `Informasi_Gaji`.
* **One-to-Many (1:N)**: Jantung dari database. Satu `Penulis` menghasilkan banyak `Buku`. Satu `Kategori` membawahi banyak `Produk`.
* **Many-to-Many (M:N)**: Masalah yang harus dipecah. Banyak `Siswa` mengambil banyak `Kursus`.
* **Solusi Expert:** M:N tidak bisa diimplementasikan langsung di tabel. Kita harus memecahnya menggunakan **Associative Entity** atau **Junction Table** (tabel perantara) sehingga menjadi dua hubungan 1:N.



---

## 3. Integritas Data & Key Constraints

Inilah yang membedakan Database dengan Excel. Database memiliki "polisi" yang menjaga data agar tidak rusak.

1. **Primary Key (PK)**: Identitas unik permanen. Tidak boleh `NULL` dan tidak boleh berubah.
2. **Foreign Key (FK)**: Referensi yang menghubungkan satu tabel ke tabel lain. FK memastikan **Referential Integrity**—kamu tidak bisa menghapus data `Kategori` jika masih ada `Produk` yang terhubung dengannya.
3. **Unique Constraint**: Memastikan data seperti email atau nomor telepon tidak duplikat, meskipun bukan Primary Key.
4. **Check Constraint**: Aturan logika pada kolom. Contoh: `Harga >= 0` atau `Rating BETWEEN 1 AND 5`.

---

## 4. Proses Normalisasi: Dari Berantakan ke Elegan

Normalisasi adalah teknik membedah tabel besar menjadi tabel-tabel kecil yang saling berhubungan untuk menghindari **Anomali Data** (duplikasi atau kehilangan data secara tidak sengaja).

### Tahap 1: First Normal Form (1NF)

* **Aturan**: Satu sel hanya boleh berisi satu nilai (**Atomic Value**). Tidak boleh ada grup berulang (misal: kolom `Hobi` berisi "Basket, Musik").
* **Tindakan**: Pecah data tersebut menjadi baris-baris terpisah.

### Tahap 2: Second Normal Form (2NF)

* **Aturan**: Harus sudah 1NF dan semua kolom non-kunci harus bergantung penuh pada **Primary Key** secara utuh.
* **Tindakan**: Jika ada kolom yang hanya bergantung pada sebagian kunci (biasanya pada *composite key*), pindahkan ke tabel baru.

### Tahap 3: Third Normal Form (3NF)

* **Aturan**: Harus sudah 2NF dan tidak boleh ada ketergantungan antar kolom non-kunci (**Transitive Dependency**).
* **Contoh**: Di tabel `Pesanan`, ada kolom `ID_Pelanggan` dan `Nama_Pelanggan`. Padahal `Nama_Pelanggan` itu tergantung pada `ID_Pelanggan`, bukan pada `ID_Pesanan`.
* **Tindakan**: Pindahkan `Nama_Pelanggan` ke tabel `Pelanggan`.

---

## Sesi Mentor: Studi Kasus Perancangan Database "Bootcamp SQL"

Mari kita buat struktur database yang akan kamu gunakan di repositori ini. Kita akan merancang database untuk sistem **Manajemen Bootcamp**.

### Tabel 1: `students` (Master Data)

| student_id (PK) | first_name | last_name | email (Unique) | date_of_birth |
| --- | --- | --- | --- | --- |
| 1 | John | Doe | john@mail.com | 1995-01-01 |

### Tabel 2: `courses` (Master Data)

| course_id (PK) | course_name | duration_weeks | price |
| --- | --- | --- | --- |
| SQL01 | SQL Mastery | 4 | 500000 |
| DS01 | Data Science | 12 | 2000000 |

### Tabel 3: `enrollments` (Junction Table untuk M:N)

*Satu siswa bisa ikut banyak kursus, satu kursus diikuti banyak siswa.*
| enrollment_id (PK) | student_id (FK) | course_id (FK) | enrollment_date | status |
| :--- | :--- | :--- | :--- | :--- |
| 101 | 1 | SQL01 | 2026-02-01 | Active |
| 102 | 1 | DS01 | 2026-02-05 | Active |

---

## Mengapa Desain ini Disebut "Expert"?

1. **Skalabilitas**: Jika John Doe mengubah alamat emailnya, kita cukup mengubah **1 baris** di tabel `students`. Perubahan ini otomatis terbaca di ribuan data `enrollments`.
2. **Keamanan**: Kita bisa membatasi akses. Staf administrasi hanya bisa melihat tabel `enrollments`, sementara tim keuangan hanya bisa melihat tabel `courses` (untuk harga).
3. **Analisis Canggih**: Dengan struktur ini, kamu bisa menjawab pertanyaan kompleks seperti: *"Berapa rata-rata umur siswa yang mengambil kursus dengan harga di atas 1 juta?"* menggunakan SQL **JOIN**.

