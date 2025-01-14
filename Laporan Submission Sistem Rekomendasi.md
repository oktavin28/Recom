# Laporan Proyek Machine Learning - Nur Oktavin Idris
Ini adalah proyek akhir sistem rekomendasi untuk memenuhi submission dicoding. Proyek ini membangun model berbasis content based filtering yang dapat menentukan top-N rekomendasi produk makeup

## Project Overview

![produk makeup](https://github.com/oktavin28/Recom/blob/main/foto%20makeup.jpg?raw=true)

[sumber gambar](https://asset.kompas.com/crops/ivcmMnUgnJA8nkHy5e-hYjH9eiM=/0x272:1500x1272/1200x800/data/photo/2024/05/26/665366304d237.jpg)

Industri kecantikan di Indonesia terus berkembang pesat, dengan pertumbuhan signifikan dalam beberapa tahun terakhir. Menurut data, jumlah perusahaan kosmetik meningkat dari 819 pada tahun 2021 menjadi 1.039 pada akhir 2023[[1]](https://www.goodnewsfromindonesia.id/2024/08/22/industri-kosmetik-lokal-kian-meroket-pertumbuhan-tembus-angka-48-persen). Pertumbuhan ini didorong oleh inovasi produk lokal, tren dinamis, dan peningkatan konsumsi masyarakat[[2]](https://wartaekonomi.co.id/read554079/tahan-banting-industri-kecantikan-indonesia-makin-potensial). Namun, dengan banyaknya produk makeup yang tersedia di pasaran, konsumen sering kali kesulitan memilih produk yang sesuai, terutama jika produk yang biasa mereka gunakan habis atau tidak lagi diproduksi. Untuk mengatasi tantangan ini, sistem rekomendasi berbasis content-based filtering telah diterapkan dalam berbagai penelitian untuk membantu konsumen menemukan produk yang sesuai dengan preferensi mereka. Seperti, penelitian lainnya yang mengembangkan sistem rekomendasi untuk produk Emina Cosmetics menggunakan metode ini[[3]](https://e-journal.stmiklombok.ac.id/index.php/misi/article/view/250). Sistem ini memberikan rekomendasi berdasarkan kesamaan fitur antarproduk, seperti deskripsi dan kategori, sehingga membantu pelanggan menemukan produk yang sesuai dengan kebutuhan mereka, sekaligus meningkatkan pengalaman berbelanja.



## Business Understanding
Pengembangan sistem rekomendasi produk makeup berbasis content-based filtering memiliki potensi besar untuk meningkatkan pengalaman belanja pengguna dan mendukung pertumbuhan platform penjualan produk kecantikan. Sistem ini dapat membantu pengguna menemukan produk makeup yang sesuai dengan kebutuhan dan preferensi mereka dengan lebih cepat dan akurat, seperti jenis kulit, kategori produk, dan fitur produk yang diinginkan. Selain itu, sistem ini dapat meningkatkan engagement pengguna, mendorong loyalitas pelanggan, dan mengoptimalkan konversi penjualan produk. Dengan menyediakan rekomendasi yang dipersonalisasi, brand/merek dapat membangun hubungan yang lebih baik dengan pengguna sekaligus meningkatkan efisiensi operasional[[4]](https://journal.lppmunindra.ac.id/index.php/STRING/article/view/24066).

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
    
    
### EDA

![Distribusi Produk perKategori](https://github.com/oktavin28/Recom/blob/main/Distribusi%20produk.png?raw=true)

Berdasarkan grafik yang ditampilkan, dapat disimpulkan bahwa distribusi produk makeup menunjukkan bahwa kategori *Face Primer* memiliki jumlah produk terbanyak dibandingkan dengan kategori lainnya, diikuti oleh *Blush*, *Setting Spray & Powder*, dan *Foundation*. Sebaliknya, kategori dengan jumlah produk paling sedikit adalah *BB & CC Creams*. Hal ini mengindikasikan bahwa *Face Primer* merupakan salah satu kategori yang paling banyak dikembangkan atau dipasarkan oleh produsen dalam dataset ini.

![Distribusi Produk perMerek](https://github.com/oktavin28/Recom/blob/main/Distribusi%20Merek.png?raw=true)

Berdasarkan grafik distribusi produk tiap merek, terlihat bahwa merek *Ask A Question* memiliki jumlah produk yang jauh lebih banyak dibandingkan dengan merek lainnya, sedangkan merek-merek seperti *NARS*, *Smashbox*, dan *Milani* memiliki jumlah produk yang jauh lebih sedikit. Hal ini menunjukkan dominasi satu merek tertentu di dataset, sementara distribusi produk untuk merek lainnya relatif merata dengan jumlah yang lebih kecil. 

![Rating Produk](https://github.com/oktavin28/Recom/blob/main/Rating%20Produk.png?raw=true)

Berdasarkan grafik distribusi rating produk, terlihat bahwa produk *Shape Tape Full Coverage Concealer* memiliki jumlah total rating tertinggi dibandingkan produk lainnya. Sebagian besar rating untuk produk-produk yang ditampilkan cenderung didominasi oleh rating tinggi, terutama Rating 4 dan Rating 5, yang menunjukkan kepuasan pengguna secara umum terhadap produk-produk tersebut. Namun, ada juga kontribusi kecil dari Rating 1 dan Rating 2 di semua produk, yang menunjukkan beberapa ketidakpuasan pelanggan. Produk-produk dengan rating tertinggi menunjukkan potensi penerimaan pasar yang baik, sementara yang lain memiliki peluang untuk peningkatan kualitas.

![Rating Merek](https://github.com/oktavin28/Recom/blob/main/Rating%20Merek.png?raw=true)

Berdasarkan grafik distribusi rating merek, terlihat bahwa merek *IT Cosmetics* memiliki jumlah rating tertinggi dibandingkan merek-merek lainnya, dengan dominasi Rating 5 dan Rating 4 yang menunjukkan tingkat kepuasan pelanggan yang tinggi terhadap produk mereka. Merek *Too Faced* juga memiliki jumlah rating yang signifikan, namun terlihat lebih beragam dengan kontribusi yang cukup besar dari rating yang lebih rendah seperti Rating 1 dan Rating 2. Merek-merek lainnya, seperti *Wet n Wild*, *Lancôme*, dan *Benefit Cosmetics*, memiliki jumlah rating yang lebih sedikit namun tetap didominasi oleh Rating 4 dan Rating 5, menunjukkan kecenderungan positif dalam kepuasan pelanggan secara keseluruhan. Hal ini mencerminkan bahwa sebagian besar merek di grafik ini memiliki persepsi yang baik di pasar, terutama pada produk-produk unggulan mereka.

- Identifikasi Missing Values

Menghitung jumlah Missing Values di setiap kolom. Pada langkah ini, menggunakan fungsi `df.isnull().sum()` untuk menghitung jumlah missing values di setiap kolom dalam dataset. Analisis ini membantu mengidentifikasi kolom mana yang memerlukan penanganan, seperti imputasi atau penghapusan baris/kolom. Langkah ini penting untuk memastikan dataset siap digunakan untuk analisis dan pemodelan machine learning. Output menunjukkan fitur `category` memiliki 5 missing value, `brand` memiliki 1 missing value, `pros` sebanyak 36, `cons` sebanyak 38, dan `best_uses` sebanyak 41.


## Data Preparation

- Memilih Kolom yang Relevan
  
Dataset biasanya memiliki banyak kolom, tetapi tidak semuanya relevan untuk analisis atau pemodelan. Untuhk itu pada model ini, hanya memilih kolom yang relevan dari file detail produk untuk membantu mengurangi kompleksitas data dan meningkatkan efisiensi analisis lebih lanjut pada sistem rekomendasi. Kolom yang dipilih mencakup `product_link_id` sebagai ID unik produk, `product_name` untuk nama produk, `category` untuk kategori produk, dan `brand` untuk merek. Selain itu, kolom `price` menunjukkan harga produk, sementara `description` memberikan deskripsi produk. Kolom seperti `pros`, `cons`, dan `best_uses` digunakan untuk menganalisis umpan balik pengguna tentang kelebihan, kekurangan, serta penggunaan terbaik produk. Terakhir, kolom `average_rating` memberikan gambaran umum tentang kualitas produk berdasarkan rata-rata rating dari pengguna. 

- Menangani Missing Values

Untuk mengatasi missing value digunakan fungsi dropna() untuk membersihkan missing value dari dataset subset `products_relevant`. Fungsi ini akan menghapus semua baris yang memiliki nilai kosong (missing values) di salah satu kolom yang dipilih. Hasilnya adalah DataFrame baru bernama `product_clean`, yang hanya berisi data lengkap tanpa nilai kosong. Meskipun metode ini efektif untuk memastikan integritas data, penggunaan `dropna()` dapat menyebabkan hilangnya banyak data jika terdapat banyak missing value dalam dataset, terutama pada kolom yang jarang terisi. Oleh karena itu, metode ini cocok digunakan jika data yang hilang relatif sedikit atau jika data yang lengkap lebih penting untuk analisis dibandingkan mempertahankan jumlah data yang besar.

- TF-IDF Vectorizer

Pada tahap ini, data diproses untuk menghasilkan representasi fitur yang relevan, di mana teknik seperti vektorisasi teks dengan TF-IDF digunakan untuk mengubah deskripsi atau kategori produk menjadi vektor numerik yang dapat digunakan dalam perhitungan kesamaan (similarity). Proses ini dimulai dengan menghitung nilai IDF untuk kategori produk menggunakan metode fit pada data kategori, diikuti dengan transformasi data menjadi matriks TF-IDF menggunakan fit_transform. Matriks hasil transformasi berukuran (35, 10), di mana 35 adalah jumlah data produk dan 10 adalah jumlah kategori. Matriks ini diubah menjadi bentuk dense dengan fungsi todense(), lalu divisualisasikan sebagian untuk memperlihatkan hubungan antara produk dan kategori, seperti contoh *Blush-Mallow Soft Blusher* yang masuk dalam kategori *blush*. Representasi fitur ini memungkinkan sistem untuk memahami hubungan antar produk, yang menjadi dasar untuk menghitung derajat kesamaan antar produk menggunakan metode cosine similarity guna menghasilkan rekomendasi produk berdasarkan kemiripan kategori dan nama produk.


## Modeling
Pada proyek ini menggunakan Model Cosine Similarity yang akan mempelajari kesamaan antar data dalam fitur yang ada.

### Cosine similarity
Cosine similarity adalah metode untuk mengukur seberapa mirip dua vektor dalam ruang multidimensi. Ini adalah pengukuran kosinus sudut antara dua vektor yang dimensi dan magnitudonya direpresentasikan sebagai titik dalam ruang. Nilai similaritas kosinus berkisar antara -1 hingga 1, di mana nilai 1 menunjukkan kedua vektor sepenuhnya sejajar (100% mirip), 0 menunjukkan vektor tegak lurus (tidak ada keterkaitan), dan -1 menunjukkan kedua vektor sepenuhnya berlawanan arah (100% tidak mirip). Metode ini sering digunakan dalam pemrosesan teks dan pengelompokan data untuk menentukan tingkat kesamaan antara dokumen atau fitur dalam dataset[[5]](https://medium.com/geekculture/cosine-similarity-and-cosine-distance-48eed889a5c4).

Cosine Similarity dituliskan dalam rumus:

![Rumus CosineSimilarity](https://github.com/oktavin28/Recom/blob/main/rumus%20cosine%20similarity.png?raw=true)

dimana:
   - (A·B)menyatakan produk titik dari vektor A dan B.
   - ||A|| mewakili norma Euclidean (magnitudo) dari vektor A.
   - ||B|| mewakili norma Euclidean (magnitudo) dari vektor B.

Selanjutya dilakukan pengujian untuk mendapatkan rekomendasi menggunakan kode berikut:

`rekomendasi_produk('Bio Stick Foundation')`

![hasil rekomendasi](https://github.com/oktavin28/Recom/blob/main/Hasil%20Rekomendasi.png?raw=true)

Berdasarkan tabel di atas untuk hasil pengujian model Content-Based Filtering, sistem berhasil menyajikan rekomendasi top 5 produk yang mirip dengan Bio Stick Foundation, semuanya termasuk dalam kategori foundation dari berbagai merek. Hal ini menunjukkan bahwa sistem mampu mengidentifikasi produk serupa berdasarkan kemiripan fitur kategori. Pendekatan ini memungkinkan pengguna yang menyukai Bio Stick Foundation untuk menemukan alternatif produk lain yang sesuai dengan preferensi mereka dalam kategori yang sama, memberikan pengalaman rekomendasi yang relevan dan bervariasi.

Kelebihan Cosine Similarity:

- Kompleksitas yang rendah, membuatnya efisien dalam perhitungan.
- Cocok digunakan pada dataset dengan dimensi yang besar karena tidak terpengaruh oleh jumlah dimensi.

Kekurangan Cosine Similarity:

- Hanya memperhitungkan arah dari vektor, tanpa memperhitungkan magnitudo (besarnya).
- Perbedaan dalam magnitudo vektor tidak sepenuhnya diperhitungkan, yang berarti nilai-nilai yang sangat berbeda dapat dianggap mirip jika arah vektornya sama.


## Evaluation
Metrik evaluasi digunakan untuk menilai seberapa baik performa suatu model. Dalam konteks ini,  metrik evaluasi yang digunakan untuk mengukur kinerja model yaitu Davies Bouldin Score. Metrik ini bertujuan untuk memberikan gambaran tentang seberapa baik model bekerja dalam melakukan tugas tertentu, seperti klasifikasi atau klastering data.

### Davies Bouldin Score
Davies Bouldin Score (DB) adalah metrik evaluasi kinerja pengelompokan yang mengukur rata-rata kesamaan setiap cluster dengan cluster yang paling mirip dengan membandingkan jarak dalam cluster terhadap jarak antar cluster. Dengan skor minimum nol, semakin rendah nilai DB, semakin baik kinerja pengelompokannya, menunjukkan cluster yang lebih dekat satu sama lain dan kurang tersebar. DB tidak memerlukan pengetahuan apriori tentang label kebenaran dasar, namun memiliki formulasi yang lebih sederhana, memberikan cara efisien untuk mengevaluasi pengelompokan tanpa memerlukan pengetahuan tambahan tentang struktur data[[6]](https://ieeexplore.ieee.org/document/4766909)

Rumus Davies-Bouldin Score (DB) adalah:

![rumus db score](https://github.com/oktavin28/Recom/blob/main/Rumus%20DBscore.png?raw=true)

Di mana:

- ( k ) adalah jumlah cluster.
- ( R_i ) adalah radius dalam cluster ke-i.
- ( d(c_i, c_j) ) adalah jarak antara pusat cluster ke-i (( c_i )) dan pusat cluster ke-j (( c_j )).

Davies-Bouldin didefinisikan sebagai rata-rata dari nilai-nilai R, di mana setiap nilai R adalah rasio antara jumlah dari radius dalam cluster (dalam pengertian jarak, misalnya Euclidean distance) dan jarak antara pusat cluster, dengan pusat-pusat yang lain. Rasio ini digunakan untuk mengevaluasi kemiripan setiap cluster dengan cluster lain.

diperoleh hasil score berikut:

![dbscore](https://github.com/oktavin28/Recom/blob/main/hasil%20DB%20score.png?raw=true)

Hasil evaluasi Davies-Bouldin Score sebesar 0.13 menunjukkan bahwa hasil clustering dengan algoritma KMeans (menggunakan 5 cluster) memiliki kualitas yang sangat baik. Nilai Davies-Bouldin Score berkisar antara 0 hingga tak hingga, di mana nilai yang lebih rendah menunjukkan cluster yang lebih kompak (intra-cluster similarity tinggi) dan lebih terpisah satu sama lain (inter-cluster dissimilarity tinggi). Dengan skor mendekati 0, model clustering ini dapat diinterpretasikan bahwa pembagian data ke dalam 5 cluster berhasil memisahkan grup dengan baik dan memberikan hasil yang optimal dalam mengelompokkan produk berdasarkan fitur yang digunakan.

## Kesimpulan

Sistem rekomendasi produk makeup yang dikembangkan menggunakan Content-Based Filtering berhasil memberikan rekomendasi yang relevan dan bermanfaat bagi pengguna. Pendekatan ini mampu menyajikan rekomendasi spesifik berdasarkan preferensi pengguna.


## Referensi

1. M. F. Mileneo. (2024). "Industri Kosmetik Lokal Kian Meroket, Pertumbuhan Tembus Angka 48 Persen". Good News From Indonesia. [Available](https://www.goodnewsfromindonesia.id/2024/08/22/industri-kosmetik-lokal-kian-meroket-pertumbuhan-tembus-angka-48-persen).
2. Warta Ekonomi. (2024). "Tahan Banting, Industri Kecantikan Indonesia Makin Potensial". [Available](https://wartaekonomi.co.id/read554079/tahan-banting-industri-kecantikan-indonesia-makin-potensial).
3. F. B. A. Larasati and H. Februariyanti. (2021). "Sistem Rekomendasi Product Emina Cosmetics Dengan Menggunakan Metode Content-Based Filtering". Jurnal Manajemen Informatika & Sistem Informasi, vol. 4, no. 1, pp. 45–54. [Available](https://e-journal.stmiklombok.ac.id/index.php/misi/article/view/250).
4. A. Sulami, V. Atina, and N. Nurmalitasari. (2024). "Penerapan Metode Content Based Filtering dalam Sistem Rekomendasi Pemilihan Produk Skincare". STRING (Satuan Tulisan Riset dan Inovasi Teknologi), vol. 9, no. 2, pp. 172–181. [Available](https://journal.lppmunindra.ac.id/index.php/STRING/article/view/24066).
5. Sindhu Seelam. (2021). "Machine Learning Fundamentals: Cosine Similarity and Cosine Distance". Published in Geek Culture. [Available](https://medium.com/geekculture/cosine-similarity-and-cosine-distance-48eed889a5c4).
6. Davies, David L. Bouldin, Donald W. (1979). A Cluster Separation Measure. IEEE Transactions on Pattern Analysis and Machine Intelligence. PAMI-1 (2): 224-227. [Available](https://ieeexplore.ieee.org/document/4766909).

---

