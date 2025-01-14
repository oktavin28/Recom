# Laporan Proyek Machine Learning - Nur Oktavin Idris
Ini adalah proyek akhir sistem rekomendasi untuk memenuhi submission dicoding. Proyek ini membangun model berbasis content based filtering yang dapat menentukan top-N rekomendasi produk makeup

## Project Overview
Industri kecantikan terus berkembang pesat dengan berbagai macam produk makeup yang tersedia di pasaran, pengguna sering kali kesulitan memilih produk yang sesuai jika produk yang digunakan habis atau masih diproduksi lagi. Sehingga sistem rekomendasi dapat menjadi solusi efektif untuk membantu pelanggan memilih produk yang sesuai dengan preferensi mereka. Proyek ini bertujuan mengembangkan sistem rekomendasi produk makeup dengan memanfaatkan algoritma Content-Based Filtering, di mana rekomendasi diberikan berdasarkan kesamaan fitur antarproduk. Sistem ini dirancang untuk membantu pelanggan menemukan produk yang sesuai dengan kebutuhan mereka berdasarkan kategori dan merek yang relevan, sekaligus meningkatkan pengalaman berbelanja mereka.

**Referensi Dataset:** [Makeup Insights - Kaggle](https://www.kaggle.com/datasets/zarasarkar/makeup-insights-customer-reviews).


## Business Understanding
Pengembangan sistem rekomendasi produk makeup berbasis content-based filtering memiliki potensi besar untuk meningkatkan pengalaman belanja pengguna dan mendukung pertumbuhan platform penjualan produk kecantikan. Sistem ini dapat membantu pengguna menemukan produk makeup yang sesuai dengan kebutuhan dan preferensi mereka dengan lebih cepat dan akurat, seperti jenis kulit, kategori produk, dan fitur produk yang diinginkan. Selain itu, sistem ini dapat meningkatkan engagement pengguna, mendorong loyalitas pelanggan, dan mengoptimalkan konversi penjualan produk. Dengan menyediakan rekomendasi yang dipersonalisasi, brand/merek dapat membangun hubungan yang lebih baik dengan pengguna sekaligus meningkatkan efisiensi operasional.

### Problem Statements

1. Bagaimana sistem dapat memberikan rekomendasi produk makeup yang sesuai dengan preferensi pengguna?
2. Bagaimana sistem memanfaatkan data kategori dan merek untuk menentukan tingkat kesamaan antarproduk?

### Goals

1. Memberikan rekomendasi *top-N* produk yang relevan berdasarkan preferensi pengguna menggunakan teknik Content-Based Filtering
2. Meningkatkan pengalaman pengguna dalam memilih produk makeup.

### Solution Approach
Menggunakan informasi atribut produk untuk merekomendasikan produk yang mirip dengan produk yang telah disukai pengguna.

## Data Understanding
Luxxify Makeup Dataset adalah Kumpulan Data Rias Luxxify yang dirancang untuk mendukung sistem rekomendasi produk rias(Makeup). Ini terdiri dari dua file CSV: satu untuk detail produk (*cleaned_makeup_products.csv*) dan satu lagi untuk ulasan produk (*cleaned_makeup_reviews.csv*). Dataset yang digunakan pada proyek kali ini disediakan secara publik di kaggle dengan nama datasets yaitu: *makeup-insights-customer-reviews* . Dataset ini dapat diunduh di Kaggle : [Makeup Insights - Kaggle](https://www.kaggle.com/datasets/zarasarkar/makeup-insights-customer-reviews).

Pada model kali ini dataset yang digunakan adalah file `cleaned_makeup_products.csv`

- Dataset memiliki 1373 sample dengan 37 fitur.
- Dataset memilik 11 fitur object, 16 fitur int64, dan 8 fitur float64.
- Terdapat Missing value pada fitur `category` sebanyak 22, fitur `num_shades` sebanyak 1309 dan `faceoff_negative` serta `faceoff_positive` sebanyak 129.
- Tidak ada data yang duplikat.


### Variabel Dataset
Kolom dataset memiliki informasi berikut:
- `product_link_id`: ID unik tautan produk.
- `product_link`: tautan produk.
- `category`: Kategori produk.
- `item_id`: id item produk
- `product_name`: Nama produk.
- `brand`: Nama merek produk.
- `price`: Harga produk
- `num_shades`: variasi warna (shades) yang tersedia untuk produk
- `rating`: Rating rata-rata dari pengguna
- `num_reviews`:  Jumlah ulasan yang tersedia untuk produk.
- `description`: Deskripsi produk.
- `pros`: Kelebihan produk berdasarkan ulasan.
- `cons`: Kekurangan produk berdasarkan ulasan.
- `best_uses`: Penggunaan terbaik untuk produk berdasarkan ulasan.
- `describe_yourself`: Deskripsi tentang diri pengguna yang memberikan ulasan.
- `review_star_1`, `review_star_2`, `review_star_3`, `review_star_4`, `review_star_5` : Jumlah ulasan berdasarkan bintang 1-5. 
- `rating_star_1`, `rating_star_2`, `rating_star_3`, `rating_star_4`, `rating_star_5`: Jumlah rating berdasarkan bintang 1-5.
- `rating_count`: Total jumlah rating yang diberikan untuk produk.
- `review_count`: Total jumlah ulasan yang diberikan untuk produk.
- `average_rating`: Rating rata-rata berdasarkan semua ulasan.
- `recommended_ratio`: Rasio rekomendasi pengguna (dari ulasan)
- `native_review_count: Jumlah ulasan tiap pengguna produk.
- `native_sampling_review_count: Jumlah ulasan dari sampel asli.
- `native_community_content_review_count`: Jumlah ulasan komunitas untuk produk.
- `syndicated_review_count`: Jumlah ulasan dari sumber eksternal.
- `faceoff_negative`: Komentar negatif dari ulasan.
- `faceoff_positive`: Komentar positif dari ulasan.
    
    


### Insight dari EDA

![Distribusi Produk perKategori](https://github.com/oktavin28/Recom/blob/main/Distribusi%20produk.png?raw=true)

![Distribusi Produk perMerek](https://github.com/oktavin28/Recom/blob/main/Distribusi%20Merek.png?raw=true)

![Rating Produk](https://github.com/oktavin28/Recom/blob/main/Rating%20Produk.png?raw=true)

![Rating Merek](https://github.com/oktavin28/Recom/blob/main/Rating%20Merek.png?raw=true)

## Data Preparation
### Teknik yang Diterapkan
1. **Mengatasi Missing Values**: Menghapus entri dengan data kosong.
2. **Menghapus Duplikasi**: Menggunakan `drop_duplicates()` pada kolom `product_link_id`.
3. **Konversi Data**: Mengubah kolom menjadi bentuk list untuk proses selanjutnya.

Hasil akhir: Dataset bersih dengan 915 produk unik.

## Modeling
### Menggunakan Content-Based Filtering dengan Pendekatan 
1. **TF-IDF Vectorizer**: Mengubah kategori produk menjadi matriks numerik.
2. **Cosine Similarity**: Menghitung tingkat kesamaan antarproduk.

### Fungsi Rekomendasi
Fungsi `rekomendasi_produk` dirancang untuk memberikan rekomendasi *top-N* produk berdasarkan produk yang dipilih pengguna.

### Hasil
Produk yang mirip dengan produk yang telah disukai pengguna sebelumnya direkomendasikan. Misalnya, pengguna memilih "Bio Stick Foundation", sistem memberikan rekomendasi 5 produk serupa, seperti:
- **Product 1**: Nama produk, kategori, merek.
- **Product 2**: Nama produk, kategori, merek.


## Evaluation
Metrik evaluasi utama adalah tingkat relevansi rekomendasi berdasarkan *Cosine Similarity*. Sistem menunjukkan hasil yang baik dalam mengelompokkan produk serupa.

Metrik Evaluasi
contohnya
Precision@K: Mengukur proporsi rekomendasi yang relevan dalam top-K rekomendasi.

Recall@K: Mengukur proporsi item relevan yang berhasil direkomendasikan.

Hasil Evaluasi
contohnya:
Content-Based Filtering:

Precision@10: 0.75

Recall@10: 0.60

Hasil menunjukkan bahwa pendekatan Content-Based Filtering memiliki kinerja yang baik dalam memberikan rekomendasi yang relevan bagi pengguna.


## Kesimpulan

Sistem rekomendasi produk makeup yang dikembangkan menggunakan Content-Based Filtering berhasil memberikan rekomendasi yang relevan dan bermanfaat bagi pengguna. Pendekatan ini mampu menyajikan rekomendasi spesifik berdasarkan preferensi pengguna.

---

