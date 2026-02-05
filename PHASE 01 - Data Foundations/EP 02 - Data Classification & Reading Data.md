# Data Classification & Reading Data

Data Scientist tidak hanya "melihat" data, tetapi harus mampu "membaca" struktur dan sifatnya. Klasifikasi data menentukan teknik statistik yang digunakan dan tipe data yang akan didefinisikan dalam tabel SQL.

---

## 1. Klasifikasi Data Berdasarkan Sifat (Statistical Type)

Secara statistik, data dibagi menjadi dua kategori besar: Kualitatif dan Kuantitatif.

### A. Data Kualitatif (Categorical)

Data yang mendeskripsikan karakteristik atau label. Tidak dapat dihitung secara matematis (seperti ditambah atau dibagi).

* **Nominal**: Data label tanpa urutan atau tingkatan.
* *Contoh*: Jenis Kelamin (Pria/Wanita), Kategori Produk (Elektronik, Fashion), Metode Pembayaran (OVO, GoPay, Transfer).
* *Case*: Di SQL, kita menggunakan ini untuk pengelompokan (`GROUP BY`).


* **Ordinal**: Data label yang memiliki urutan atau peringkat logis.
* *Contoh*: Tingkat Kepuasan (Puas, Cukup, Tidak Puas), Level Keanggotaan (Bronze, Silver, Gold).
* *Case*: Penting untuk analisis tren peningkatan layanan.



### B. Data Kuantitatif (Numerical)

Data yang berupa angka dan dapat dikenakan operasi matematika.

* **Diskrit**: Data angka bulat yang didapat dari hasil perhitungan (*counting*).
* *Contoh*: Jumlah anak, Jumlah transaksi per hari, Stok barang.
* *Note*: Tidak mungkin ada "1.5 transaksi".


* **Kontinu**: Data angka yang dapat memiliki nilai desimal dan didapat dari hasil pengukuran (*measurement*).
* *Contoh*: Suhu (36.5°C), Harga produk, Berat badan, Pendapatan.



---

## 2. Klasifikasi Data Berdasarkan Struktur (Format Type)

Dalam arsitektur data industri, kita mengenal tiga bentuk struktur penyimpanan:

| Jenis Struktur | Penjelasan | Teknologi Terkait |
| --- | --- | --- |
| **Structured** | Data memiliki skema tetap, rapi dalam baris dan kolom. | **RDBMS (SQL)** seperti PostgreSQL, MySQL. |
| **Semi-Structured** | Tidak memiliki tabel kaku, tapi memiliki tag/label penanda struktur. | **JSON**, XML, NoSQL (MongoDB). |
| **Unstructured** | Data mentah tanpa pola yang terorganisir. | Gambar, Video, Audio, Dokumen PDF mentah. |

---

## 3. Sumber-Sumber Data (Data Sources)

Dari mana data berasal? Di industri, seorang Data Scientist mengambil data dari:

1. **Internal Database**: Database transaksional perusahaan (misal: Data penjualan di MySQL).
2. **External API**: Mengambil data dari pihak ketiga (misal: Data cuaca dari OpenWeatherMap API).
3. **Web Scraping**: Mengekstraksi data dari situs web (misal: Memantau harga kompetitor di marketplace).
4. **IoT & Logs**: Data dari sensor perangkat keras atau catatan aktivitas server.
5. **Open Data**: Dataset publik untuk riset (misal: **Kaggle**, **BPS**, **Google Dataset Search**).

---

## 4. Case Study: Membaca Data Transaksi E-Commerce

Perhatikan baris data berikut dan identifikasi jenisnya:

> "Seorang pelanggan Gold membeli 2 unit smartphone seharga Rp5.500.000 dengan metode kartu kredit."

* **Gold**: Data Kualitatif (Ordinal) - Tingkat keanggotaan.
* **2 unit**: Data Kuantitatif (Diskrit) - Jumlah barang (hasil hitung).
* **Rp5.500.000**: Data Kuantitatif (Kontinu) - Nilai moneter (bisa memiliki desimal).
* **Kartu Kredit**: Data Kualitatif (Nominal) - Label metode pembayaran.
* **Smartphone**: Data Kualitatif (Nominal) - Label kategori.

---

## 5. Mengapa Ini Penting di SQL?

Jika Anda salah mengklasifikasikan data saat membuat tabel:

1. Anda menyimpan angka harga sebagai **String (Teks)**: Anda tidak akan bisa menghitung Total Pendapatan menggunakan fungsi `SUM()`.
2. Anda menyimpan data kategori sebagai **Angka**: Laporan Anda akan membingungkan (misal: Kategori '1' padahal maksudnya 'Elektronik').

---

### Kesimpulan

Kemampuan mengklasifikasikan data adalah langkah awal validasi data (*Data Profiling*). Tanpa pemahaman ini, proses pembersihan data (*Data Cleansing*) akan menjadi sangat sulit.
