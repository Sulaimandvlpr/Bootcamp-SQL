# The Essence of Data

Selamat datang di langkah pertama! Sebelum kita menulis satu baris kode `SELECT` pun, kamu harus memahami apa itu **Data**. Banyak orang langsung melompat ke alat (*tools*), padahal rahasia seorang *Data Scientist* hebat bukan pada seberapa jago dia koding, tapi seberapa dalam dia memahami karakteristik datanya.

---

## 1. Apa itu Data? (Bukan Sekadar Angka)

Sebagai mentor, saya sering menyebut data sebagai **Representasi Digital dari Fakta Mentah (*Raw Facts*)**.

Bayangkan kamu sedang berdiri di depan kasir kopi.

* **Fakta Mentah:** Ada seseorang membeli kopi pukul 08:01, harganya Rp25.000, jenisnya Latte.
* **Data:** Catatan digital di sistem kasir yang berisi: `2026-02-05 08:01:00, 25000, 'Latte'`.

**Ingat ini:** Data adalah catatan masa lalu. Tanpa pengolahan, data hanyalah tumpukan "sampah digital". Tugas kita adalah mengubah sampah ini menjadi emas (informasi).

---

## 2. Bagaimana Data Tercipta? (The Digital Footprint)

Data tidak muncul begitu saja dari langit. Di industri modern, data tercipta melalui empat jalur utama yang harus kamu pahami asal-usulnya:

1. **User-Generated Data (Transaksi & Interaksi):**
Setiap kali kamu *checkout* di Shopee atau *like* foto di Instagram, kamu sedang membuat data. Ini adalah sumber data paling umum yang akan kamu olah di SQL.
2. **System/Log Data (Jejak Mesin):**
Server mencatat setiap kali ada orang membuka *website*. Jika *website* lambat, datanya ada di sini.
3. **Sensor/IoT Data (Dunia Fisik ke Digital):**
GPS di motor ojek online, sensor suhu di pabrik, atau detak jantung di *smartwatch*-mu.
4. **External/Third-Party Data:**
Data yang kita ambil dari luar perusahaan, misalnya melalui API cuaca atau data tren pasar dari Google.

---

## 3. Filosofi DIKW: Bagaimana Data Menjadi Duit?

Mengapa perusahaan rela membayar mahal seorang ahli data? Jawabannya ada di **Piramida DIKW**. Mari kita bedah dengan contoh kedai kopi:

* **Data (Fakta Mentah):** Angka penjualan harian: `50, 60, 45, 100`. (Masih bingung ini angka apa?)
* **Information (Konteks):** "Penjualan kita mencapai puncaknya di angka 100 cup pada hari Sabtu." (Sudah mulai jelas).
* **Knowledge (Pola):** "Setiap Sabtu sore saat hujan, penjualan kopi susu meningkat tajam." (Kita menemukan pola/tren).
* **Wisdom (Keputusan):** "Kalau begitu, setiap Sabtu sore kita harus stok susu dua kali lipat dan buat promo 'Bundling Hujan'." (Inilah yang menghasilkan keuntungan).

---

## 4. Analitik Data di Industri (Analytics Maturity)

Di dunia kerja nanti, atasanmu akan bertanya dengan empat level tingkat kesulitan. SQL adalah alat utama untuk menjawab keempatnya:

| Level Analitik | Pertanyaan Utama | Contoh Kasus (E-Commerce) |
| --- | --- | --- |
| **Descriptive** | *Apa yang terjadi?* | Berapa total transaksi bulan Januari? |
| **Diagnostic** | *Kenapa itu terjadi?* | Kenapa penjualan turun 20% di minggu ketiga? |
| **Predictive** | *Apa yang akan terjadi?* | Berapa perkiraan stok barang untuk Lebaran nanti? |
| **Prescriptive** | *Apa yang harus dilakukan?* | Promo apa yang paling efektif agar pelanggan tidak kabur? |

---

## 5. Sesi Mentor: Mengapa Data Scientist Butuh SQL?

Mungkin kamu bertanya, *"Mentor, kenapa nggak pakai Excel saja?"*

Bayangkan perusahaan seperti Gojek yang punya triliunan baris data transaksi.

* **Excel** akan *crash* (hang) jika memuat lebih dari 1 juta baris.
* **SQL** didesain untuk berbicara langsung dengan "gudang data" (*Database*). SQL bisa mencari satu transaksi di antara satu miliar data hanya dalam hitungan detik.

Inilah alasan kenapa SQL adalah *skill* nomor satu yang wajib dikuasai sebelum belajar Python atau Machine Learning. Kamu harus bisa mengambil datanya dulu, baru bisa mengolahnya.

---

### Kesimpulan Mentor

Data adalah bahan baku. Jika kamu ingin menjadi "Koki Data" (Data Scientist), kamu harus tahu dulu apakah bahan bakumu segar, dari mana asalnya, dan mau dimasak jadi apa (Wisdom). Di bab selanjutnya, kita akan belajar cara membedakan jenis-jenis bahan baku tersebut agar tidak salah bumbu.
