## Database Design & Modeling (Advanced)

Desain database adalah tentang efisiensi penyimpanan dan kecepatan akses. Jika desainmu berantakan, query sekeren apa pun tidak akan bisa menyelamatkan performa aplikasimu. Di bab ini, kita akan belajar membangun **Blueprint Data** yang profesional.

---

## 1. Entitas & Atribut: Anatomi Objek

Sebelum menyentuh koding, kita harus menentukan "siapa" yang akan kita catat datanya.

* **Entitas**: Objek dunia nyata yang unik. (Contoh: `Pelanggan`, `Produk`, `Transaksi`).
* **Atribut**: Properti atau ciri khas yang melekat pada entitas tersebut.
* *Simple Attribute*: Nama, Harga.
* *Composite Attribute*: Alamat (yang bisa dipecah jadi Jalan, Kota, Kode Pos).
* *Derived Attribute*: Atribut yang didapat dari perhitungan atribut lain (Contoh: `Umur` yang didapat dari `Tanggal Lahir`). **Mentor Note:** Di industri, atribut turunan biasanya tidak disimpan permanen agar menghemat memori.



---

## 2. Entity Relationship Diagram (ERD): Memetakan Hubungan

ERD adalah bahasa visual untuk melihat bagaimana data saling "berbicara". Standar industri yang paling sering digunakan adalah **Notasi Crow's Foot**.

### Jenis Kardinalitas (Hubungan):

1. **One-to-One (1:1)**: Jarang terjadi, biasanya untuk alasan keamanan.
* *Case*: Satu `User` memiliki satu `User_Secret_Key`.


2. **One-to-Many (1:N)**: Fondasi utama database relasional.
* *Case*: Satu `Kategori` berisi banyak `Produk`.


3. **Many-to-Many (M:N)**: Hubungan kompleks yang **HARUS** dipecah.
* *Case*: Banyak `Pesanan` berisi banyak `Buku`.
* **Solusi Mentor**: Kamu wajib menggunakan **Junction Table** (Tabel Perantara) untuk memecah M:N menjadi dua hubungan 1:N agar database tidak pusing mencari data.



---

## 3. Integritas Data: Menjaga "Fondasi" Tetap Kokoh

Agar data tidak "sampah", kita harus memberikan aturan main (*Constraints*):

* **Primary Key (PK)**: KTP-nya data. Harus unik, tidak boleh kosong (`NOT NULL`).
* **Foreign Key (FK)**: "Kabel" penghubung. FK di Tabel A harus merujuk ke PK di Tabel B.
* **Nullability**: Apakah kolom boleh kosong? (Penting: Kolom harga atau Nama Pelanggan biasanya `NOT NULL`).
* **Unique Constraint**: Selain PK, data ini tidak boleh kembar (Contoh: Nomor Telepon atau Email).

---

## 4. Normalisasi: Rahasia Database Level Expert

Normalisasi adalah proses mengorganisir tabel untuk meminimalkan redundansi (duplikasi data). Kita akan bedah sampai **3NF**.

### A. 1NF (First Normal Form) - Atomisitas

**Aturan**: Tidak boleh ada kolom yang memiliki lebih dari satu nilai (multivalued).

* *Bad Case*: Kolom `Hobi` isinya "Membaca, Berenang, Lari".
* *Fix*: Pecah sehingga satu baris hanya berisi satu hobi.

### B. 2NF (Second Normal Form) - Ketergantungan Parsial

**Aturan**: Harus sudah 1NF dan semua kolom non-kunci harus bergantung penuh pada *Primary Key*.

* *Bad Case*: Di tabel `Transaksi`, kamu memasukkan `Nama_Produk`. Padahal `Nama_Produk` harusnya ikut ke `ID_Produk`, bukan `ID_Transaksi`.
* *Fix*: Pisahkan data produk ke tabel sendiri.

### C. 3NF (Third Normal Form) - Ketergantungan Transitif

**Aturan**: Harus sudah 2NF dan tidak boleh ada kolom non-kunci yang bergantung pada kolom non-kunci lainnya.

* *Bad Case*: Di tabel `Pelanggan`, ada kolom `ID_Kota` dan `Nama_Kota`. `Nama_Kota` bergantung pada `ID_Kota`, bukan langsung ke `ID_Pelanggan`.
* *Fix*: Buat tabel `Kota` tersendiri.

---

## Sesi Mentor: Contoh Implementasi "Toko Buku Online"

Mari kita terjemahkan teori di atas ke dalam tabel nyata. Perhatikan bagaimana setiap tabel hanya mengurusi "urusannya sendiri" (Terspesialisasi).

### Tabel 1: `pelanggan` (Master Data)

| id_pelanggan (PK) | nama_lengkap | email (Unique) | id_kota (FK) |
| --- | --- | --- | --- |
| 1 | Budi Sudarsono | budi@mail.com | 10 |
| 2 | Ani Wijaya | ani@mail.com | 11 |

### Tabel 2: `kota` (Master Data - Hasil 3NF)

| id_kota (PK) | nama_kota | provinsi |
| --- | --- | --- |
| 10 | Jakarta | DKI Jakarta |
| 11 | Surabaya | Jawa Timur |

### Tabel 3: `buku` (Master Data)

| id_buku (PK) | judul | harga | id_penerbit (FK) |
| --- | --- | --- | --- |
| B-01 | SQL For Life | 150000 | P-99 |
| B-02 | Data Is Oil | 200000 | P-99 |

### Tabel 4: `pesanan` (Header Transaksi)

| id_pesanan (PK) | id_pelanggan (FK) | tgl_transaksi | total_bayar |
| --- | --- | --- | --- |
| TR-001 | 1 | 2026-02-05 | 350000 |

### Tabel 5: `detail_pesanan` (Junction Table - Hasil Pecahan M:N)

*Tabel ini sangat penting untuk mencatat detail barang yang dibeli tanpa merusak struktur tabel utama.*
| id_detail (PK) | id_pesanan (FK) | id_buku (FK) | jumlah | subtotal |
| :--- | :--- | :--- | :--- | :--- |
| 1 | TR-001 | B-01 | 1 | 150000 |
| 2 | TR-001 | B-02 | 1 | 200000 |
