# Glosarium Teknis: Data, Database, dan SQL

Dokumen ini berisi kumpulan istilah teknis yang mencakup fundamental hingga konsep tingkat lanjut dalam manajemen database dan analisis data.

---

### 1. Database dan Arsitektur

Fokus pada rumah tempat data disimpan dan bagaimana strukturnya dibangun.

| Istilah | Definisi |
| --- | --- |
| **RDBMS** | *Relational Database Management System*. Sistem perangkat lunak yang mengelola database relasional (seperti PostgreSQL, MySQL, BigQuery). |
| **Schema** | Cetak biru logis yang mendefinisikan tabel, kolom, tipe data, dan hubungan antar tabel dalam database. |
| **Normalisasi** | Proses desain database untuk mengurangi redundansi data dan meningkatkan integritas (1NF, 2NF, 3NF). |
| **Primary Key** | Kolom unik yang berfungsi sebagai identitas tunggal untuk setiap baris dalam tabel. |
| **Foreign Key** | Kolom yang menghubungkan satu tabel ke tabel lain, menciptakan integritas referensial. |
| **Constraint** | Aturan yang diterapkan pada tabel (seperti NOT NULL atau UNIQUE) untuk menjaga validitas data. |
| **ERD** | *Entity Relationship Diagram*. Representasi visual dari entitas dan hubungan antar tabel dalam database. |

---

### 2. Manipulasi dan Operasi SQL

Fokus pada bahasa dan logika yang digunakan untuk mengolah data.

| Istilah | Definisi |
| --- | --- |
| **DDL** | *Data Definition Language*. Perintah untuk mengubah struktur database (CREATE, ALTER, DROP). |
| **DML** | *Data Manipulation Language*. Perintah untuk memanipulasi baris data (INSERT, UPDATE, DELETE). |
| **DQL** | *Data Query Language*. Perintah utama untuk mengambil atau mencari data (SELECT). |
| **CTE** | *Common Table Expression*. Tabel sementara yang didefinisikan dengan klausa `WITH` untuk mempermudah pembacaan kueri. |
| **Join** | Operasi untuk menggabungkan dua tabel atau lebih berdasarkan kolom kunci yang terkait. |
| **Subquery** | Kueri SQL yang bersarang di dalam kueri lain (Nested Query). |
| **Window Function** | Fungsi kalkulasi yang berjalan lintas baris tanpa menghilangkan detail baris individu (menggunakan klausa `OVER`). |

---

### 3. Optimasi dan Performa

Fokus pada efisiensi kueri saat berhadapan dengan dataset skala besar.

| Istilah | Definisi |
| --- | --- |
| **Indexing** | Struktur data yang mempercepat proses pencarian tanpa harus memindai seluruh isi tabel. |
| **Execution Plan** | Langkah-langkah logis yang diambil oleh database untuk mengeksekusi kueri seefisien mungkin. |
| **Sargable** | *Search Argumentable*. Kondisi di mana kueri ditulis sedemikian rupa sehingga database dapat menggunakan indeks secara optimal. |
| **Partitioning** | Pembagian tabel raksasa menjadi bagian-bagian kecil secara fisik untuk mempercepat pemrosesan. |
| **Full Table Scan** | Operasi berat di mana database membaca seluruh baris tabel karena ketiadaan indeks yang relevan. |

---

### 4. Analisis Data dan Metrik Bisnis

Fokus pada transformasi data menjadi wawasan bisnis yang berharga.

| Istilah | Definisi |
| --- | --- |
| **Aggregation** | Proses meringkas data mentah menjadi nilai tunggal (SUM, AVG, COUNT, MIN, MAX). |
| **Granularity** | Tingkat kedetailan data; semakin rendah granulasi, semakin detail data yang tersedia. |
| **Retention Rate** | Persentase pelanggan yang kembali menggunakan layanan dalam periode waktu tertentu. |
| **Churn Rate** | Persentase pelanggan yang berhenti menggunakan layanan dalam periode waktu tertentu. |
| **RFM Analysis** | Segmentasi pelanggan berdasarkan kebaruan belanja (Recency), frekuensi, dan nilai uang (Monetary). |
| **Null** | Penanda dalam SQL yang menunjukkan bahwa data tidak ada atau tidak diketahui (kosong). |
