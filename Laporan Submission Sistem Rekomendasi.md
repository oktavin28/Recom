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
Dataset bersumber dari [Makeup Insights - Kaggle](https://www.kaggle.com/datasets/zarasarkar/makeup-insights-customer-reviews), yang  terdiri dari dua file:
- **cleaned_makeup_products.csv**: berisi rincian produk makeup (1.373 entri, 35 variabel).
- **cleaned_makeup_reviews.csv**: berisi ulasan produk makeup (314.029 entri, 27 variabel).
 

Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

### Variabel Utama
- `product_link_id`: ID unik produk.
- `product_name`: Nama produk.
- `brand`: Merek produk.
- `category`: Kategori produk.
- `rating`: Rating dari ulasan.

### Insight dari EDA
1. Kategori "face primer" memiliki jumlah produk terbanyak.
2. Merek "Ask A Question" mendominasi jumlah produk.
3. Sebagian besar ulasan memiliki rating antara 4 dan 5.

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

