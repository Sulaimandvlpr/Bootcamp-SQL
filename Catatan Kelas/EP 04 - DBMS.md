# Catatan: DBMS (Database Management System)

## Definisi DBMS
**DBMS (Database Management System)** adalah software yang digunakan untuk membuat, mengelola, menyimpan, dan mengakses database secara terstruktur sehingga data lebih aman, konsisten, dan mudah digunakan.

**Intinya:**  
DBMS adalah penghubung antara **user / aplikasi** dengan **database**.

### Contoh DBMS populer:
- MySQL  
- PostgreSQL  
- Oracle  
- Microsoft SQL Server  
- MongoDB  

---

# Client vs Server DBMS

## Client-Based DBMS
DBMS yang berjalan langsung di komputer pengguna (lokal). Biasanya dipakai untuk aplikasi kecil atau single user.

### Ciri-ciri:
- Tidak membutuhkan server terpisah  
- Setup lebih mudah  
- Performa cukup untuk skala kecil  

**Contoh:**  
- SQLite  
- Microsoft Access  

**Bayangan kasus:**  
Aplikasi kasir kecil di toko → data tersimpan langsung di komputer kasir.

---

## Server-Based DBMS
Database dijalankan di server khusus dan bisa diakses banyak user secara bersamaan melalui jaringan.

### Ciri-ciri:
- Mendukung multi-user  
- Lebih aman  
- Cocok untuk aplikasi besar  
- Bisa menangani traffic tinggi  

**Contoh:**  
- MySQL  
- PostgreSQL  
- Oracle  

**Bayangan kasus:**  
Tokopedia / Shopee → jutaan user mengakses database yang sama secara bersamaan.

---

# Kapan Menggunakan RDBMS vs NoSQL?

## RDBMS (Relational Database)
Database berbentuk tabel yang memiliki relasi antar data.

### Gunakan RDBMS jika:
- Data terstruktur (jelas kolom dan tipe datanya)  
- Membutuhkan transaksi akurat (contoh: pembayaran)  
- Konsistensi sangat penting  
- Relasi antar data kompleks  

**Contoh kasus:**  
- Sistem perbankan  
- Payroll karyawan  
- Sistem akademik kampus  

**Contoh DBMS:**  
MySQL, PostgreSQL, SQL Server

---

## NoSQL (Not Only SQL)
Database fleksibel tanpa schema tetap.

### Gunakan NoSQL jika:
- Data berubah-ubah  
- Tidak terstruktur  
- Membutuhkan scalability tinggi  
- Banyak data real-time  

**Contoh kasus:**  
- Media sosial  
- Chat apps  
- Big Data  
- IoT  

**Contoh DBMS:**  
MongoDB, Cassandra, Firebase

---

# Perbandingan Cepat

| RDBMS | NoSQL |
|------|--------|
| Struktur tabel | Fleksibel |
| Schema tetap | Schema bebas |
| Konsistensi tinggi | Lebih scalable |
| Cocok untuk transaksi | Cocok untuk big data |

---

# Analogi Biar Kebayang

**RDBMS = Excel super rapi**  
Semua data harus sesuai kolom.

**NoSQL = Folder berisi berbagai file**  
Bisa menyimpan apa saja tanpa format kaku.

---

Kalimat simpel untuk diingat:

**"Kalau datanya rapi → RDBMS.  
Kalau datanya fleksibel dan besar → NoSQL."**
