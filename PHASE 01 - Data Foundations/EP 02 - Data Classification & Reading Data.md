# Data Classification & Reading Data

Sebagai seorang *Data Scientist*, kamu tidak hanya "melihat" data, tetapi harus mampu "membaca" struktur dan sifatnya. Kemampuan ini adalah syarat mutlak sebelum kamu merancang tabel di SQL. Mengapa? Karena komputer tidak tahu apakah sebuah angka mewakili harga (yang bisa dijumlahkan) atau mewakili kode pos (yang tidak boleh dijumlahkan).

---

## 1. Klasifikasi Berdasarkan Sifat (Statistical Perspectives)

Dalam statistika industri, kita membagi data menjadi dua kelompok besar. Mari kita bedah satu per satu layaknya seorang mentor.

### A. Data Kualitatif (Categorical)

Data ini mendeskripsikan "kualitas" atau "karakteristik" suatu objek. Data ini berupa label dan **tidak bisa dikenakan operasi matematika** (kamu tidak bisa menghitung rata-rata dari "Warna Mata").

* **Nominal (Tanpa Urutan)**: Data yang hanya berfungsi sebagai label pembeda. Tidak ada tingkatan di dalamnya.
* *Contoh*: Jenis Kelamin, Nama Kota, Metode Pembayaran (Cash, QRIS, Debet).
* *Mentor Note*: Ingat, Nominal berasal dari kata "Name". Hanya nama, tidak ada yang lebih tinggi atau rendah.


* **Ordinal (Dengan Urutan)**: Data kategori yang memiliki tingkatan atau urutan logis.
* *Contoh*: Tingkat Pendidikan (SD, SMP, SMA), Level Member (Bronze, Gold, Platinum).
* *Mentor Note*: Meskipun ini bukan angka, urutannya sangat penting untuk analisis progres atau loyalitas.



### B. Data Kuantitatif (Numerical)

Data yang berupa angka dan **bisa dihitung secara matematis**.

* **Diskrit (Hasil Menghitung)**: Angkanya pasti bulat dan tidak bisa dipecah menjadi desimal.
* *Contoh*: Jumlah anak, jumlah stok barang di gudang, jumlah kursi di kelas.
* *Mentor Note*: Kamu tidak mungkin punya 2,5 anak, kan? Jika datanya pasti bulat, itu adalah Diskrit.


* **Kontinu (Hasil Mengukur)**: Angka yang bisa memiliki nilai desimal tak terhingga dalam rentang tertentu.
* *Contoh*: Suhu badan (36,5°C), Berat badan (60,75 kg), Jarak tempuh (10,2 km).
* *Mentor Note*: Kontinu biasanya berasal dari alat ukur (timbangan, penggaris, termometer).



---

## 2. Klasifikasi Berdasarkan Struktur (Technical Perspectives)

Di industri, data dikirim dalam berbagai format. Kamu harus mengenali "wadahnya":

1. **Structured Data (Terstruktur)**: Data yang sudah rapi dalam kolom dan baris. Inilah yang kita olah di **SQL**. Contoh: Tabel Excel, Database MySQL.
2. **Semi-Structured Data**: Tidak memiliki tabel kaku, tapi punya penanda (tag). Contoh: **JSON** (yang sering digunakan di API) dan XML.
3. **Unstructured Data**: Data mentah yang tidak punya pola tetap. Contoh: Video TikTok, rekaman suara, foto produk, atau file PDF.

---

## 3. Sesi Mentor: Studi Kasus "Deep Dive" E-Commerce

Mari kita bedah satu baris transaksi nyata di dunia kerja. Perhatikan kalimat ini:

> **"Pelanggan level Platinum membeli 3 unit laptop seharga Rp15.450.000,50 menggunakan kartu kredit."**

Sebagai pengolah data, begini cara kamu membacanya:

* **Platinum**: Ini adalah **Kualitatif Ordinal**. Mengapa? Karena Platinum adalah level member. Ada urutan jelas: Platinum lebih tinggi dari Gold. Di database, kita butuh ini untuk memberikan diskon khusus member prioritas.
* **3 unit**: Ini adalah **Kuantitatif Diskrit**. Mengapa? Karena laptop dihitung per satuan. Di database, kolom ini akan bertipe `INTEGER`. Kamu bisa menjumlahkannya dengan fungsi `SUM()` untuk melihat total stok yang keluar.
* **Rp15.450.000,50**: Ini adalah **Kuantitatif Kontinu**. Mengapa? Karena ada nilai desimal (50 perak). Meskipun terlihat bulat di aplikasi, nilai uang secara sifat bisa pecah (misal setelah kena pajak atau diskon persenan). Di SQL, kita simpan dengan tipe `DECIMAL` atau `FLOAT`.
* **Kartu Kredit**: Ini adalah **Kualitatif Nominal**. Mengapa? Ini hanya label metode bayar. Tidak ada urutan. Kita butuh ini untuk menganalisis bank mana yang paling banyak bekerja sama dengan perusahaan kita.
* **Laptop**: Ini adalah **Kualitatif Nominal**. Label kategori produk. Gunanya untuk pengelompokan laporan penjualan per kategori (`GROUP BY category`).

---

## 4. Mengapa Kamu Harus Paham Ini? (Penerapan SQL)

Kesalahan klasifikasi di awal akan membuat *query* SQL-mu berantakan:

1. Jika **Harga** kamu simpan sebagai **Nominal (Teks)**, kamu tidak akan pernah bisa menghitung rata-rata pendapatan. SQL akan menganggapnya sebagai "tulisan", bukan "angka".
2. Jika **Level Member** kamu simpan tanpa aturan **Ordinal**, kamu akan kesulitan membuat laporan performa member dari yang paling rendah ke yang paling tinggi.

---

### Kesimpulan Mentor

Data adalah bahan baku. Jika kamu salah mengenali bahan bakunya, hasil "masakan" (analisis) kamu akan salah rasa. Selalu tanyakan: *"Apakah angka ini bisa saya bagi?"* atau *"Apakah label ini punya urutan?"* sebelum kamu membuat tabel di database.
