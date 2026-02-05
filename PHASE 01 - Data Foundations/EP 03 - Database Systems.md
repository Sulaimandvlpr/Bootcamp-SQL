# Database Systems: Mengenal "Gudang Data" Modern

Halo! Mentor di sini. Pernahkah kamu bertanya-tanya, *"Kenapa perusahaan besar tidak menyimpan data transaksinya di Excel saja?"* atau *"Apa bedanya MySQL dengan PostgreSQL yang sering dimention orang-orang?"*

Di bab ini, kita akan membedah infrastruktur tempat data dikelola.

---

## 1. Database vs Spreadsheet (Excel/Google Sheets)

Banyak pemula menganggap Database hanyalah "Excel yang lebih rumit". Padahal, perbedaannya seperti sepeda dengan pesawat kargo. Meskipun keduanya bisa memindahkan barang, kapasitas dan mekanismenya jauh berbeda.

| Fitur | Spreadsheet (Excel) | Database (SQL) |
| --- | --- | --- |
| **Kapasitas** | Terbatas (Max ~1 Juta baris). Mulai melambat di angka 100rb baris. | Mampu menampung milyaran baris data tanpa penurunan performa signifikan. |
| **Integritas Data** | Lemah. Kamu bisa menulis "Tujuh" di kolom yang seharusnya berisi angka "7". | Sangat Ketat. Aturan (Constraints) memastikan data yang masuk harus sesuai format. |
| **Keamanan** | Siapa pun yang memiliki file bisa mengubah semua data. | Granular. Kamu bisa mengatur siapa yang boleh melihat, tapi tidak boleh mengubah. |
| **Hubungan** | Sulit menghubungkan antar file (VLOOKUP sering rusak). | Dirancang khusus untuk menghubungkan triliunan data melalui relasi (Relation). |

**Kesimpulan Mentor:** Gunakan Spreadsheet untuk analisis kecil dan cepat. Gunakan Database untuk penyimpanan data yang permanen, besar, dan krusial.

---

## 2. Mengenal DBMS vs RDBMS

Saat kamu belajar SQL, kamu akan sering mendengar istilah ini. Jangan sampai tertukar!

* **DBMS (Database Management System):** Software yang digunakan untuk mengelola database secara umum.
* **RDBMS (Relational Database Management System):** Jenis DBMS yang menyimpan data dalam bentuk **Tabel** yang saling berhubungan (Relasi). Inilah yang menggunakan bahasa **SQL**.

**Analogi Mentor:**

* **Database** adalah *Gudang*.
* **Data** adalah *Barang di dalam gudang*.
* **DBMS/RDBMS** adalah *Sistem Manajemen Gudang* (software pengatur stok, keamanan, dan lokasi barang).
* **SQL** adalah *Bahasa/Instruksi* yang kamu gunakan untuk bicara kepada petugas gudang.

---

## 3. Populer RDBMS Engines di Industri

Di dunia kerja, kamu akan menemui berbagai merk database. Meskipun merknya beda, bahasanya tetap sama: **SQL**.

1. **PostgreSQL:** "Si Paling Lengkap". Sangat populer di kalangan *Data Engineer* dan *Scientist* karena fitur analisisnya yang canggih dan gratis (*open-source*).
2. **MySQL:** "Si Paling Populer". Banyak digunakan di aplikasi web (seperti Facebook atau WordPress) karena kecepatannya.
3. **Google BigQuery / Snowflake:** "Si Raksasa Cloud". Digunakan untuk data berukuran Terabyte hingga Petabyte. Sangat umum di perusahaan teknologi besar.
4. **SQLite:** "Si Kecil Cabe Rawit". Database ringan yang biasanya tertanam di dalam aplikasi HP-mu.

---

## 4. Relational Model: Kekuatan di Balik Tabel

Mengapa disebut **Relational**? Karena kekuatan utamanya bukan pada tabel tunggal, melainkan pada **bagaimana tabel-tabel tersebut saling bicara**.

Bayangkan sebuah E-commerce:

* Ada tabel **Users** (Isinya Nama, Email).
* Ada tabel **Orders** (Isinya Tanggal, Total Harga).

Daripada menulis nama user berulang-ulang di setiap pesanan (yang bikin boros memori), kita hanya perlu menghubungkan tabel *Orders* ke tabel *Users* menggunakan sebuah **ID**. Inilah yang disebut **Relasi**.

---

## 5. Sesi Mentor: Mengapa Kita Butuh Abstraksi?

Salah satu keunggulan Database adalah **Data Abstraction**. Artinya, kamu tidak perlu tahu di mana file database itu disimpan di harddisk atau bagaimana cara kerja biner-nya.

Kamu cukup bilang:

> *"SQL, tolong ambilkan nama pelanggan yang belanja di atas 1 juta hari ini."*

Dan RDBMS akan melakukan kerja kerasnya di belakang layar. Inilah yang membuat SQL menjadi bahasa yang sangat manusiawi dan mudah dipelajari untuk analisis data.

---

### Kesimpulan Mentor

Memahami "Rumah Data" memberimu perspektif tentang batasan sistem. Sebagai *Data Scientist*, kamu harus tahu database apa yang digunakan perusahaanmu agar kamu bisa menulis kueri yang paling efisien untuk mesin tersebut.
