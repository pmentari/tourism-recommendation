# Laporan Proyek Machine Learning - Putri Mentari

## Project Overview

Sektor pariwisata merupakan salah satu penggerak utama perekonomian Indonesia. Berdasarkan data Badan Pusat Statistik (BPS), kontribusi pariwisata terhadap Produk Domestik Bruto (PDB) Indonesia mencapai 4,11% pada tahun 2022, menunjukkan peran strategis sektor ini dalam pemulihan ekonomi nasional pasca pandemi (BPS, 2023). Di tengah banyaknya destinasi wisata yang tersedia, wisatawan kerap menghadapi tantangan dalam memilih tujuan yang sesuai dengan preferensi mereka. Oleh karena itu, dibutuhkan solusi digital yang mampu memberikan rekomendasi destinasi wisata secara personal dan efisien.

Penelitian oleh Ricci et al. (2022) menunjukkan bahwa sistem rekomendasi dalam konteks pariwisata dapat meningkatkan pengalaman pengguna, mendorong eksplorasi destinasi yang belum populer, serta mengurangi kelelahan dalam pengambilan keputusan. Hal serupa juga disampaikan oleh Kzaz et al. (2018), yang menekankan bahwa sistem ini efektif dalam mengatasi informasi berlebih (information overload) dan dalam menyediakan pengalaman wisata yang lebih personal. Dengan demikian, proyek ini tidak hanya mendukung transformasi digital pariwisata Indonesia, tetapi juga membantu menciptakan pemerataan kunjungan ke berbagai destinasi lokal.

## Business Understanding

### Problem Statements

- Bagaimana sistem rekomendasi dapat membantu wisatawan untuk menemukan drama yang sesuai dengan preferensinya?

### Goals

- Memberikan rekomendasi tempat wisata sesuai dengan preferensi wisatawan.


## Data Understanding
Dataset yang digunakan dalam proyek ini adalah dataset Indonesia Tourism Destination. Dataset ini diperoleh dari https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination

Terdapat 4 file dataset yang terdapat dalam folder, tetapi dalam proyek ini hanya akan menggunakan 2 file dataset yaitu mengenai destinasi wisata dan rating tempat wisata yang ada di 5 kota besar di Indonesia.

### Dataset Destinasi Wisata

Dataset tourism_with_id berisi 437 baris dan 13 kolom data.

Variabel-variabel pada dataset ini adalah sebagai berikut:
1. Place_Id = ID untuk destinasi wisata
2. Place_Name = nama destinasi wisata
3. Description = Informasi singkat tentang destinasi wisata
4. Category = kategori destinasi wisata
5. City = lokasi destinasi wisata
6. Price = biaya tiket masuk ke destinasi wisata
7. Rating = rating destinasi wisata
8. Time-Minutes = lama waktu yang ditempuh dari pusat kota
9. Coordinate = lokasi geografis destinasi wisata
10. Lat = garis lintang destinasi wisata secara geografis
11. Long = garis bujur destinasi wisata secara geografis
12. Unnamed: 11 = informasi untuk kolom ini tidak diketahui
13. Unnamed" 12 = informasi untuk kolom ini tidak diketahui

Terdapat 2 kolom yang memiliki missing value, yaitu 232 missing values pada kolom Time_Minutes dan 437 missing values di kolom Unnamed: 11.

Hasil dari statistik deskriptif di atas menunjukkan biaya tiket masuk yang diperlukan berkisar dari Rp0 sampai Rp900.000. Sementara itu, beragam destinasi wisata dapat diakses dari pusat kota dalam 10 menit sampai 6 jam.

Tidak ada duplikasi data yang ditemukan dalam dataset ini.

Terdapat distribusi data yang tidak normal pada kolom numerik dalam dataset ini. Mayoritas destinasi wisata memiliki harga tiket masuk di bawah Rp50.000 dengan rating di antara 4 sampai 5. Selain itu, Mayoritas destinasi wisata dapat ditempuh kurang dari 100 menit dari pusat kota.

Destinasi wisata yang ada dalam dataset ini berada di 5 kota besar di Indonesia, yaitu Yogyakarta, Bandung, Jakarta, Semarang, dan Surabaya. Daerah Yogyakarta dan Bandung memiliki jumlah destinasi wisata paling banyak masing0masing 126 dan 124 destinasi. Surabaya memiliki destinasi wisata paling rendah yaitu 46 destinasi.

### Dataset Rating Destinasi Wisata

Dalam dataset ini menginformasikan rating dari pengunjung yang diberikan untuk destinasi wisata. Terdapat total 10.000 rating yang diberikan oleh pengunjung.

Variabel-variabel pada dataset ini adalah sebagai berikut:
1. User_Id = ID untuk pengunjung
2. Place_Id = ID untuk destinasi wisata
3. Place_Ratings = rating dari masing-masing pengunjung untuk destinasi wisata 

Tidak ada missing values yang terdeteksi dalam dataset ini, tetapi memiliki 79 data yang terduplikasi.

Diketahui terdapat total 10.000 rating yang diberikan oleh 300 pengunjung. Rating yang diberikan oleh pengunjung cukup beragam dari 1 sampai 5 atas pengalamannya saat mengunjungi destinasi wisata.

## Data Preparation

Pada tahap ini, hal yang pertama dilakukan adalah mengisi missing value dengan median pada kolom Time_Minutes di dataset destinasi wisata. Pengisian dengan median dipilih karena tidak dipengaruhi oleh nilai ekstrim sehingga tahan terhadap outlier. Selain itu, median juga lebih representatif untuk kolom yang memiliki distribusi tidak normal (skewed). Data duplikat pada file dataset rating dibersihkan dengan cara dihapus.

Selanjutnya dilakukan penggabungan kedua dataset dengan menggunakan fungsi merge dengan jenis merge left. Data hasil merge kemudian dibersihkan kembali data kolom-kolom yang tidak diperlukan untuk pengerjaan proyek. Hasilnya diperoleh 9921 baris data dan 9 kolom yang dapat digunakan untuk membuat sistem rekomendasi.

### Preparation for Content-based Filtering
Sebagai persiapan untuk membuat sistem rekomendasi dengan metode content-based filtering, dilakukan vektorisasi untuk menemukan representasi fitur untuk setiap kategori destinasi. Dari 437 destinasi, teridentifikasi terdapat 6 kategori destinasi wisata. 

Vektorisasi pada kolom Category dilakukan dengan fungsi tfidfvectorizer(). Selanjutnya vektor tf-idf dibuat dalam bentuk matriks dengan fungsi todende(). Langkah ini akan menunjukkan korelasi antara kategori dengan destinasi wisata. 


## Modeling

Untuk menghasilkan rekomendasi tempat wisata, dilakukan perhitungan derajat kesamaan antar destinasi berdasarkan konten (setelah vektorisasi) dengan teknik cosine similarity. Cosine similarity dipilih karena efisien dalam komputasinya. Jika nilai cosine similarity antara 2 destinasi adalah 1, maka kedua destinasi tersebut memiliki kemiripan/kesamaan, dan jika nilai cosine similaritynya adalah 0 berarti kedua destinasi wisata tidak memiliki kesamaan.

Rumus cosine similarity adalah sebagai berikut
Cosine Similarity = Σ (Aᵢ × Bᵢ) / (√Σ Aᵢ² × √Σ Bᵢ²)

Keterangan:
Aᵢ, Bᵢ = elemen ke-i dari vektor A dan B
Σ = penjumlahan seluruh elemen
√ = akar kuadrat

Setelah menghitung cosine similarity, saya mencoba menampilkan rekomendasi destinasi. Pada tahapan ini, diambil indeks dari item dengan skor similarity terbesar (fungsi closest). Selanjutnya, agar nama tempat yang dicari similaritynya tidak muncul dalam hasil rekomendasi, maka tempat tersebut dihapus dari daftar "closest". Hasilnya memberikan 20 destinasi yang direkomendasikan yang sama dengan Taman Menteng sebagai berikut.
1. Waterpark Kenjeran Surabaya
2. Tugu Pal Putih Jogja
3. Embung Tambakboyo
4. Atlantis Water Adventure
5. Taman Kunang-Kunang
6. Taman Ekspresi dan Perpustakaan
7. Wisata Agro Edukatif Istana Susu Cibugary
8. Bumi Perkemahan Cibubur
9. Atlantis Land Surabaya
10. Taman Tabanas
11. Waterbook PIK
12. Hutan Kota Srengseng
13. Taman Pintar Yogyakarta
14. Panama Park 825
15. Dusun Bambu
16. Hutan Pinus Pengger
17. Ocean Ecopark
18. Taman Situ Lembang
20. Tektona Waterpark

Cosine similarity memiliki sejumlah kelebihan, di antaranya adalah efisiensinya yang tinggi dalam menghitung kemiripan antar item berbasis teks seperti kategori wisata, tidak terpengaruh oleh skala nilai, serta sangat cocok digunakan bersama representasi TF-IDF. Teknik ini juga sederhana dan cepat, menjadikannya populer dalam sistem rekomendasi berbasis konten. Namun, cosine similarity juga memiliki keterbatasan, seperti hanya mempertimbangkan arah vektor tanpa memahami konteks makna, tidak menangkap hubungan semantik antar kata, dan sangat bergantung pada kualitas representasi data awal.

Selanjutnya dalam proyek ini juga mencoba memberikan rekomendasi kepada pengunjung sesuai dengan destinasi favoritnya. Sebelum memulai prediksi, terlebih dahulu membuat ground truth yang berisi data destinasi wisata yang diberi rating >= 3 oleh pengunjung. Data ground truth kemudian dibagi menjadi data train dan data test.

Model dengan predicted yang dibangun berisi hasil rekomendasi top 20 destinasi wisata untuk setiap user berdasarkan destinasi wisata yang mereka sukai. Sebagai contoh, terdapat 20 rekomendasi destinasi wisata berdasarkan destinasi favorit dari User ID 120.


## Evaluation
Terdapat beberapa metrik untuk mengevaluasi model content-based filtering.

1. Precision@k, yang dapat menjelaskan seberapa banyak dari rekomendasi yang diberikan adlaah relevan. Metrik ini dapat mengukur ketepatan sistem. Precision@k dapat dihitung debagai berikut:
Precision@k = (Jumlah item relevan di top-k) / k

2. Recall@k, mengukur proporsi item yang relevan yang berhasil direkomendasikan dari semua item yang memungkinkan. 
Recall@k = (Jumlah item relevan di top-k) / (Jumlah total item relevan di ground truth)

3. F1@k, adalah rata-rata anatar precision dan recall yang menjunjukkan keseimbangan antaravakurasi dan cakupan.
F1@k = 2 * (Precision@k * Recall@k) / (Precision@k + Recall@k)

4. MAP@k (Mean Average Precision), mengukur urutan relevansi rekomendasi.
MAP@k = (1 / jumlah user) * Σ (Average Precision per user)

5. NDCS@k (Normalized Discounted Cumulatice Gain), mengukur kualitas urutan rekomendasi, di man auntuk item yang berada di posisi atas dinilai lebih bernilai. Rumus menghitungnya adalah sebagai berikut:
**NDCG@k = DCG@k / IDCG@**
dengan
DCG@k = Σ (relevansi_i / log2(i + 1))  untuk i = 1 hingga k
IDCG@k = DCG dengan urutan ideal (semua item relevan di posisi atas)


Berikut adalah hasil evaluasi dari model yang dibangun:
- Precision@20:  0.0057
- Recall@20:     0.0378
- F1@20:         0.0099
- MAP@20:        0.0052
- NDCG@20:       0.0357

Hasil evaluasi tersebut menunjukkan bahwa model yang dibangun belum optimal dalam memberikan rekomendasi destinasi wisata kepada pengunjung. 
- Precision@20: 0.0057, berarti sistem ini dapat memberikan banyak rekomendasi destinasi tetapi hanya 0,57% yang relevan dan disukai oleh user.
- Recall@20: 0.0378, berarti sistem rekomendasi hanya mencakup sebagian sebagian kecil item yang relevan, di mana dari semua tempat uang seharusny direkomendasikan, hanya 3,78% di anataranya yang berhasil ditemukan.
- F1@20: 0.0099, menunjukkan sistem yang tidak seimbang anatara ketepatan dan cakupannya.
- MAP@20: 0.0052, berarti bahwa item yang relevan jarang uncul di urutan awal sehingga tidak terlihat oleh user.
- NDCG@20: 0.0357, menunjukkan urutan rekomendasi yang belum baik karena item relevan banyak muncul di posisi bawah.

Proyek ini sudah bisa memberikan rekomendasi destinasi wisata kepada pengunjung, tetapi dari beragam item yang direkomendasikan, item yang relevan dengan preferensi pengunjung masih jarang diberikan/diterima oleh pengunjung.


## Referensi

- Badan Pusat Statistik. (2023). Statistik Wisatawan Nusantara 2022. https://www.bps.go.id
- Ricci, F., Rokach, L., & Shapira, B. (2022). Recommender Systems in Tourism. Springer.
- Kzaz, L., El Asri, B., & Hafiddi, H. (2018). Tourism Recommender Systems: An Overview of Recommendation Approaches. Procedia Computer Science, 127, 104–113.



**---Ini adalah bagian akhir laporan---**