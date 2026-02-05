# The Essence of Data: Fondasi Dasar Data Science

Sebelum melangkah ke teknis penulisan *query*, seorang praktisi data harus memahami entitas yang mereka olah. Bab ini membahas filosofi data, mekanisme terciptanya data, hingga bagaimana data memberikan nilai strategis bagi bisnis.

---

## 1. Apa itu Data? (Definition)

Secara teknis, **Data** adalah representasi digital dari fakta mentah (*raw facts*) yang belum diolah. Data merupakan catatan atas kumpulan kejadian, transaksi, atau observasi.

* **Fakta Mentah**: Angka "25000", kata "Kopi Susu", tanggal "2024-05-20".
* **Informasi**: Hasil pengolahan data yang memiliki konteks. "Penjualan 1 cup Kopi Susu seharga Rp25.000 pada tanggal 20 Mei 2024."

Dalam industri, data dianggap sebagai **"The New Oil"**, namun ia hanya akan berharga jika berhasil diekstraksi dan dimurnikan menjadi sebuah wawasan (*insight*).

---

## 2. Bagaimana Data Tercipta? (The Digital Footprint)

Di era modern, data tercipta hampir setiap detik melalui berbagai aktivitas manusia dan mesin. Memahami asal-usul data membantu *Data Scientist* dalam melakukan *debugging* dan validasi kualitas data.

### Mekanisme Penciptaan Data:

* **Transaksi E-commerce**: Setiap klik, barang yang dimasukkan ke keranjang, hingga proses pembayaran terekam dalam tabel basis data.
* **Log Sistem**: Server mencatat setiap alamat IP yang mengakses situs web, durasi kunjungan, dan *error* yang terjadi.
* **Sensor IoT (Internet of Things)**: Mesin pabrik, *smartwatch*, atau sensor cuaca mengirimkan aliran data numerik secara terus-menerus (*streaming*).
* **Interaksi Media Sosial**: *Like*, *share*, komentar, dan unggahan video menghasilkan data tidak terstruktur dalam skala masif.

---

## 3. Value of Data: Hierarki DIKW

Data tidak langsung menjadi keputusan. Ia melewati proses transformasi yang dikenal sebagai piramida **DIKW (Data, Information, Knowledge, Wisdom)**.

| Level | Penjelasan | Contoh Kasus (Retail) |
| --- | --- | --- |
| **Data** | Fakta mentah tanpa konteks. | Angka penjualan: 10, 15, 8, 20. |
| **Information** | Data yang sudah diolah/diberi konteks. | Rata-rata penjualan harian adalah 13 unit. |
| **Knowledge** | Pola atau tren yang ditemukan dari informasi. | Penjualan meningkat 50% setiap hari Jumat sore. |
| **Wisdom** | Kebijakan/keputusan strategis berbasis pola. | Memberikan promo "Happy Hour" di hari Jumat untuk memaksimalkan profit. |

---

## 4. Implementasi: Analitik Data dalam Bisnis

Dalam industri, data digunakan untuk menjawab empat jenis pertanyaan fundamental melalui **Analytics Maturity Model**:

### A. Descriptive Analytics (Apa yang telah terjadi?)

Fokus pada rekapitulasi data historis.

* *Contoh*: Laporan total pendapatan bulan lalu atau jumlah pengguna aktif harian (DAU).

### B. Diagnostic Analytics (Mengapa hal itu terjadi?)

Mencari hubungan sebab-akibat dengan melakukan *drill-down* pada data.

* *Contoh*: Mengapa penjualan turun di wilayah Jakarta? (Apakah karena stok habis atau kompetitor baru?).

### C. Predictive Analytics (Apa yang mungkin terjadi di masa depan?)

Menggunakan data historis untuk memprediksi tren masa depan melalui permodelan statistik.

* *Contoh*: Estimasi jumlah stok barang yang harus disediakan untuk bulan depan agar tidak terjadi *out-of-stock*.

### D. Prescriptive Analytics (Apa yang harus kita lakukan?)

Memberikan rekomendasi tindakan berdasarkan prediksi.

* *Contoh*: Sistem secara otomatis memberikan diskon khusus kepada pelanggan yang terdeteksi akan berhenti berlangganan (*churn*).

---

## Studi Kasus: Alur Data di Perusahaan Ride-Hailing

Bayangkan sebuah aplikasi seperti Grab atau Gojek:

1. **Data Tercipta**: Saat Anda membuka aplikasi, GPS mengirim koordinat (Data).
2. **Informasi**: Sistem menghitung jarak antara Anda dan pengemudi terdekat (Information).
3. **Knowledge**: Sistem mengetahui bahwa pada jam 08.00 pagi, permintaan di area Anda selalu tinggi (Knowledge).
4. **Wisdom**: Sistem menerapkan *Surge Pricing* (harga naik) untuk menarik lebih banyak pengemudi ke area tersebut agar permintaan terpenuhi (Wisdom/Decision).

---

### Kesimpulan

Data adalah aset diam. Peran Anda sebagai pengguna SQL adalah "menghidupkan" data tersebut agar dapat bercerita dan membantu perusahaan mengambil langkah yang tepat.
