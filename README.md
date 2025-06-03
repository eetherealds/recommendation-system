# Laporan Proyek Machine Learning - Dearni Lambardo Saragih

## Project Overview
Kulit merupakan organ terluar tubuh manusia yang memiliki peran vital dalam melindungi tubuh dari berbagai faktor eksternal. Dalam era modern ini, perawatan kulit (skincare) telah menjadi bagian penting dari rutinitas kesehatan dan kecantikan bagi banyak orang. Namun, dengan banyaknya produk skincare yang tersedia di pasaran, konsumen sering kali mengalami kesulitan dalam memilih produk yang sesuai dengan jenis dan permasalahan kulit mereka [[1]](https://jgi.internationaljournallabs.com/index.php/ji/article/view/192).

Menurut penelitian yang dilakukan oleh Magfiroh (2023), sebagian besar konsumen mengalami kebingungan saat memilih produk skincare karena kurangnya pengetahuan tentang kandungan produk dan kesesuaiannya dengan tipe kulit mereka. Hal ini dapat menyebabkan pembelian produk yang tidak tepat, yang berpotensi memperburuk kondisi kulit atau tidak memberikan hasil yang diharapkan [[2]](http://etheses.uin-malang.ac.id/71365/).

Sistem rekomendasi pemilihan produk skincare hadir sebagai solusi untuk mengatasi permasalahan tersebut. Dengan memanfaatkan teknologi informasi, sistem ini dapat membantu konsumen menemukan produk skincare yang sesuai dengan jenis kulit dan permasalahan kulit yang dialami. 

Pengembangan sistem rekomendasi ini menjadi penting karena dapat memberikan panduan yang lebih personal dan spesifik bagi pengguna, dibandingkan dengan saran umum yang mungkin tidak sesuai dengan kondisi kulit individu. Dalam pengembangan sistem rekomendasi, penting untuk memperhatikan aspek keamanan data pengguna dan keakuratan rekomendasi. Validasi sistem oleh pakar di bidang dermatologi atau kecantikan dapat meningkatkan kredibilitas dan keandalan sistem [[3]](https://jurnal.aksaraglobal.co.id/index.php/jkppk/article/view/483).

## Business Understanding
### Problem Statements
- Bagaimana distribusi produk tiap brand untuk jenis kulit tertentu?
- Bagaimana bahan tertentu mempengaruhi kecocokan produk untuk jenis kulit tertentu?
- Bagaimana hubungan antara jumlah ingredients dan rank produk?

### Goals

- Memahami brand mana yang paling banyak menyediakan produk untuk jenis kulit tertentu, sehingga dapat membantu perusahaan dalam menentukan strategi pemasaran dan pengembangan produk untuk segmen kulit yang spesifik.

- Mengidentifikasi bahan-bahan yang paling efektif atau populer digunakan dalam produk untuk tipe kulit tertentu, sehingga dapat menjadi referensi dalam formulasi dan inovasi produk baru yang sesuai kebutuhan konsumen.

- Menganalisis apakah terdapat korelasi antara jumlah ingredients yang digunakan pada suatu produk kosmetik dengan peringkat (rank) produk tersebut.

### Solution statements
- Membangun sistem rekomendasi content-based filtering menggunakan teknik cosine similarity untuk menyajikan film yang relevan.
- Menerapkan pendekatan collaborative filtering dengan memanfaatkan algoritma deep learning agar dapat menangkap pola preferensi pengguna secara lebih mendalam.

## Data Understanding
Dataset yang digunakan merupakan kumpulan data produk kosmetik dari platform Sephora yang tersedia secara publik di [Kaggle](https://www.kaggle.com/datasets/kingabzpro/cosmetics-datasets/data). Dataset ini berisi 1472 baris dan 11 kolom yang merepresentasikan berbagai atribut produk kosmetik, seperti Label, Brand, Name, Price, Rank, Ingredients, serta kecocokan produk untuk tipe kulit tertentu (Combination, Dry, Normal, Oily, Sensitive). Pada awalnya dataset tidak memiliki user ID, lalu saya menambahkan user ID untuk di collaborative-based filtering nanti.

### Data Loading
#### Variabel pada Cosmetics dataset
Dataset ini memiliki 12 variabel dengan keterangan sebagai berikut.
Variabel    | Keterangan
------------|-----------
Label       |	Kategori/jenis produk kosmetik
Brand       | Merek produk
Name        | Nama produk
Price       | Harga produk
Rank        | Peringkat/popularitas produk
Ingredients |	Daftar bahan-bahan pada produk
Combination |	Cocok untuk kulit kombinasi (1=ya, 0=tidak)
Dry         | Cocok untuk kulit kering (1=ya, 0=tidak)
Normal      |	Cocok untuk kulit normal (1=ya, 0=tidak)
Oily        | Cocok untuk kulit berminyak (1=ya, 0=tidak)
Sensitive   | Cocok untuk kulit sensitif (1=ya, 0=tidak)
user_id     | Nomor unik pembeli


#### Data Info

 No | Column      | Non-Null Count |  Dtype 
----|-------------|----------------|---------  
 0  | Label       |  1472 non-null |  object 
 1  | Brand       |  1472 non-null |  object 
 2  | Name        |  1472 non-null |  object 
 3  | Price       |  1472 non-null |  int64  
 4  | Rank        |  1472 non-null |  float64
 5  | Ingredients |  1472 non-null |  object 
 6  | Combination |  1472 non-null |  int64  
 7  | Dry         |  1472 non-null |  int64  
 8  | Normal      |  1472 non-null |  int64  
 9  | Oily        |  1472 non-null |  int64  
 10 | Sensitive   |  1472 non-null |  int64
 11 | user_id     |  1472 non-null |  object 

Berdasarkan tabel di atas, dapat diketahui bahwa tidak terdapat nilai null pada setiap kolom dalam dataset yang digunakan. Dataset ini terdiri dari 1472 entri produk dan memiliki total 11 fitur.
- Terdapat 4 fitur dengan tipe categorical atau object, yaitu: `Label`, `Brand`, `Name`, `Ingredients`, `user_id`.
- Terdapat 1 fitur dengan tipe float64, yaitu: `Rank`
- Terdapat 6 fitur dengan tipe int64, yaitu: `Price`, `Combination`, `Dry`, `Normal`, `Oily`, `Sensitive`.

#### Statistik Deskriptif

  Ket | Price	      | Rank	       |Combination |	Dry         |	Normal     	| Oily              |	Sensitive 
------|-------------|-------------|------------|------------ |-------------|-------------------|----------
count	| 1472.000000 | 1472.000000 |	1472.00000	| 1472.000000	| 1472.000000 |	1472.000000      	| 1472.000000
mean  | 55.584239	  | 4.153261	   | 0.65625	   | 0.614130    | 	0.652174   |	0.607337	         | 0.513587
std	  | 45.014429   | 	0.633918  	| 0.47512    | 	0.486965  	| 0.476442   	| 0.488509	         | 0.499985
min  	| 3.000000	   | 0.000000	   | 0.00000	   | 0.000000    |	0.000000	   | 0.000000	         | 0.000000
25%	  | 30.000000	  | 4.000000   	| 0.00000    |	0.000000	   | 0.000000    |	0.000000	         | 0.000000
50%   |	42.500000   |	4.300000    | 	1.00000  	|1.000000	    | 1.000000	   | 1.000000         	| 1.000000
75%   |	68.000000   |	4.500000	   | 1.00000	   | 1.000000	   | 1.000000	   | 1.000000         	| 1.000000
max   |	370.000000 	| 5.000000	   | 1.00000	   | 1.000000    |	1.000000	   | 1.000000	         | 1.000000

Berdasarkan tabel di atas keterangan statistik deskriptif nya sebagai berikut.

- count : Jumlah data (1472 untuk semua kolom)
- mean : Rata-rata nilai variabel
- std : Standar deviasi (penyebaran nilai)
- min : Nilai minimum
- 25% : Kuartil pertama (batas 25% data terbawah)
- 50% : Median (50% data di bawah nilai ini)
- 75% : Kuartil ketiga (75% data di bawah nilai ini)
- max : Nilai maksimum
  
### Data Cleaning
Variabel | 0
---------|---
Label    | 0
Brand    | 0
Name     | 0
Price    | 0
Rank     | 0
Ingredients	| 0
Combination | 0
Dry         | 0
Normal      | 0
Oily        | 0
Sensitive   | 0
user_id     | 0

Tidak terdapat missing values, jadi tidak dilakukan cleaning pada data.

### Exploratory Data Analysis (EDA)
#### Product Distribution for Specific Skin Types

![image](https://github.com/user-attachments/assets/ecd1dd52-412c-4574-b77a-b4d338d70d93)

![image](https://github.com/user-attachments/assets/03858c19-7ef7-4216-86db-7ef3afbd34ad)

![image](https://github.com/user-attachments/assets/9f0ff770-293b-443b-a0f6-9b02ceeb88dc)

![image](https://github.com/user-attachments/assets/14c2e834-ba87-49f0-8407-f6877882aebe)

![image](https://github.com/user-attachments/assets/ea57b91b-a44f-419f-a172-00eb4d9aaa93)


Berdasarkan grafik pada gambar di atas, penulis mendapat kesimpulan yaitu:

Top 5 Product Brands     | Penjelasan
-------------------------|-----------
Combination              | menampilkan lima brand teratas dengan jumlah produk terbanyak yang direkomendasikan untuk tipe kulit kombinasi. DR. JART+ menempati posisi teratas, yang berarti brand ini paling banyak menawarkan produk untuk kulit kombinasi dibandingkan brand lain dalam dataset.
Dry                      | menampilkan lima brand teratas untuk tipe kulit kering. DR. JART+ juga menjadi brand dengan produk terbanyak untuk kulit kering.
Normal                   | menampilkan lima brand teratas untuk tipe kulit normal. DR. JART+ merupakan brand dengan jumlah produk terbanyak untuk kulit normal.
Oily                     | menampilkan lima brand teratas untuk tipe kulit berminyak. KIEHL'S SINCE 1851 berada di posisi teratas, menandakan brand ini memiliki produk terbanyak untuk kulit berminyak.
Sensitive                | menampilkan lima brand teratas untuk tipe kulit sensitif. DR. JART+ berada di posisi teratas, menandakan brand ini memiliki produk terbanyak untuk kulit sensitif.

#### Ingredients Affect

![image](https://github.com/user-attachments/assets/a2b22c0b-ac90-4688-bb62-0d1de8a986e6)

![image](https://github.com/user-attachments/assets/c6609182-809f-453d-86da-ec61f8185744)

![image](https://github.com/user-attachments/assets/38cda003-766f-414d-bf37-40dba765a7ac)

![image](https://github.com/user-attachments/assets/9c8556de-ed0f-4f33-ae53-21aaf65a1c44)

![image](https://github.com/user-attachments/assets/05409623-ca6d-411b-850c-a432038baaa3)

Berdasarkan grafik pada gambar di atas, penulis mendapat kesimpulan yaitu:
Top 10 Most Frequent Ingredients | Penjelasan
---------------------------------|-----------
Combination                      | Water dan glycerin adalah bahan yang paling banyak ditemukan, diikuti phenoxyethanol dan butylene glycol. Bahan lain seperti disodium edta, caprylyl glycol, ethylhexylglycerin, sodium hyaluronate, xanthan gum, dan dimethicone juga sering digunakan.
Dry                              | Bahan yang paling sering ditemukan adalah water, glycerin, phenoxyethanol, dan butylene glycol. Ini adalah bahan pelembap dan pelarut yang umum di produk kulit kering.
Normal                           | Urutan bahan mirip dengan kulit kering, didominasi oleh water dan glycerin, lalu phenoxyethanol, butylene glycol, dan disodium edta.
Oily                             | Bahan utama tetap water dan glycerin. Phenoxyethanol dan butylene glycol juga sering muncul, produk untuk kulit berminyak juga banyak menggunakan bahan dasar yang serupa, namun jumlahnya sedikit lebih rendah dibanding kulit normal/kering.
Sensitive                        | Water dan glycerin tetap dominan, diikuti butylene glycol dan phenoxyethanol. Pilihan bahan cenderung aman dan sering digunakan untuk kulit sensitif.

**Kesimpulan:**

Water dan glycerin adalah bahan yang sangat umum dan hampir selalu ada di berbagai produk perawatan kulit, apapun jenis kulitnya. Pola bahan untuk setiap jenis kulit hampir identik, yang berarti sebagian besar produk menggunakan komposisi dasar yang sama, tetapi mungkin berbeda dalam konsentrasi atau kombinasi bahan lainnya sesuai dengan jenis kulitnya.

#### Relationship Between the Ingredients Quantity and Product Rank

![image](https://github.com/user-attachments/assets/47237a32-7a76-404e-8e48-ac184ee9f56e)

**Penjelasan:** 

Gambar scatterplot di atas menunjukkan hubungan antara jumlah bahan (Ingredients Quantity) dan ranking pada produk skincare. Sebagai hasil dari visualisasi tersebut, jelas bahwa sebagian besar produk memiliki jumlah bahan kurang dari 60, dan produk tersebar di berbagai nilai, terutama di rentang nilai yang lebih tinggi (mendekati 5). Tidak ada contoh yang jelas yang menunjukkan bahwa penggunaan bahan yang lebih besar atau lebih sedikit akan berdampak signifikan pada kualitas produk. Jumlah titik yang tersebar secara acak menunjukkan bahwa tidak ada korelasi yang signifikan antara peringkat produk dan jumlah bahan dalamnya. Dengan kata lain, produk yang memiliki banyak bahan atau sedikit bahan dapat memiliki ranking yang baik atau buruk. Hal ini didukung juga oleh hasil korelasi yang sangat lemah antara kedua variabel tersebut.

#### Ingredients Average Price

![image](https://github.com/user-attachments/assets/9ccf08fe-1a0c-4002-9cd5-2794489e0d58)

**Penjelasan:**
- Produk kategori Cheap rata-rata mengandung bahan lebih sedikit (~26 bahan).
- Produk kategori Medium rata-rata mengandung lebih banyak bahan (~32 bahan).
- Produk kategori Expensive memiliki rata-rata jumlah bahan terbanyak (~35 bahan).

Semakin mahal harga produk, semakin banyak pula rata-rata bahan yang digunakan dalam produk tersebut.

## Data Preparation
Proses persiapan data untuk content-based filtering dan collaborative filtering dilakukan secara terpisah dan disesuaikan dengan kebutuhan masing-masing karena ada perbedaan mendasar antara keduanya.

### 1. Content-Based Filtering
Pada tahap ini, data dipersiapkan untuk membangun sistem rekomendasi berbasis konten (content-based filtering) pada produk skincare. Fokus utama adalah pada empat kolom, yaitu:
- `Brand`           : Nama merek produk
- `Name`            : Nama produk
- `Brand_Product`   : Gabungan dari Brand, Name, dan Label produk
- `Ingredient_Skin` : Daftar label, ingredients produk, diakhiri dengan tipe kulit yang sesuai

Seluruh kolom dikonversi ke dalam list, lalu digabungkan kembali dalam sebuah DataFrame baru bernama `content_based_data`. Kolom `Ingredient_Skin` selanjutnya diolah menggunakan TfidfVectorizer untuk mengubah data teks menjadi representasi numerik, sehingga bisa digunakan untuk perhitungan kemiripan antar produk (cosine similarity).

Hasil vektorisasi TF-IDF ini disusun dalam DataFrame, di mana:
- **Baris** : Brand_Product (identitas unik produk)
- **Kolom** : fitur hasil ekstraksi dari kombinasi ingredients dan skin type
  
![image](https://github.com/user-attachments/assets/cdaade94-02b1-40ab-ad3c-cff0e90c6446)

**note:** karena kolom sangat banyak jadi tidak bisa dimuat semua kolomnya.

![image](https://github.com/user-attachments/assets/8b5aa48d-391f-489e-8e35-72d326cb68bc)

### 2.  Collaborative - based Filtering
Pada tahap ini, data dipersiapkan untuk pembuatan sistem rekomendasi collaborative filtering pada produk skincare. 
- `Brand`           : Nama merek produk
- `Name`            : Nama produk
- `Rank`            : Peringkat/popularitas produk
- `user_id`         : Nomor unik pembeli
- `Brand_Product`   : Gabungan dari Brand, Name, dan Label produk
- `Ingredient_Skin` : Daftar label, ingredients produk, diakhiri dengan tipe kulit yang sesuai

Selanjutnya, dilakukan encoding terhadap kolom `user_id` dan `Brand_Product` menjadi bentuk numerik menggunakan dictionary mapping, sehingga data dapat diterima oleh model machine learning untuk collaborative filtering. Kolom Rank yang merepresentasikan popularitas atau rating produk kemudian dinormalisasi ke rentang 0–1 agar seragam dan sesuai untuk pemodelan. 

Setelah itu, data diacak untuk memastikan distribusi yang merata saat proses training. Fitur input berupa pasangan angka hasil encoding (user dan name). Selanjutnya, fitur input yang digunakan adalah pasangan user dan produk dalam bentuk angka (`user`, `name`), sementara targetnya adalah `Rank` yang telah dinormalisasi ke rentang 0 hingga 1. Normalisasi ini penting agar model dapat beradaptasi dengan perbedaan skala dan menghasilkan prediksi yang lebih stabil.

No   | user	| name	| Rank
-----|------|------|-----
852	 | 852 	| 852	 | 3.8
184	 | 184	 | 184	 | 4.7
1261	| 1261	| 1261 |	4.0
67	  | 67	  | 67	  | 4.0
220	 | 220  |	220  |	4.5
...	 | ...	 | ...	 | ...
1130	| 1130	| 1130	| 4.0
1294	| 1294	| 1294	| 5.0
860	 | 860	 | 860	 | 4.4
1459	| 1459	| 1459 | 4.7
1126 |	1126	| 1126 | 4.2


Terakhir, data dibagi menjadi dua bagian, yaitu 80% untuk training dan 20% untuk validasi, sehingga performa model dapat dievaluasi secara objektif.

## Modeling & Result

### 1. Content-Based Filtering
Pada tahap ini, dilakukan pemodelan sistem rekomendasi content-based filtering untuk produk skincare dengan memanfaatkan kemiripan antar produk berdasarkan fitur konten yang telah diolah sebelumnya.

Langkah pertama adalah menghitung nilai cosine similarity antar seluruh produk menggunakan matriks hasil transformasi TF-IDF (`tfidf_matrix`). Cosine similarity digunakan untuk mengukur tingkat kemiripan antar produk berdasarkan kombinasi ingredients dan tipe kulit yang sesuai. Proses ini menghasilkan matriks kemiripan (similarity matrix) di mana setiap nilai merepresentasikan tingkat kedekatan antara dua produk.

Setelah diperoleh matriks cosine similarity, hasilnya disimpan ke dalam sebuah DataFrame (`cosine_sim_df`) dengan baris dan kolom berupa nama produk (`Brand_Product`). Dengan format ini, setiap produk dapat dibandingkan kemiripannya dengan produk lain secara langsung.

Untuk menghasilkan rekomendasi, digunakan sebuah fungsi yang mengambil produk acuan, mencari nilai cosine similarity tertinggi terhadap produk lain, lalu mengembalikan sejumlah produk yang paling mirip sebagai hasil rekomendasi. Fungsi ini memastikan produk yang direkomendasikan memiliki karakteristik bahan dan kecocokan tipe kulit yang serupa dengan produk referensi, sehingga relevan bagi pengguna. Dengan pendekatan ini, sistem content-based filtering mampu memberikan rekomendasi produk skincare yang personal dan sesuai kebutuhan, berdasarkan kemiripan konten antar produk.

Sebagai contoh, jika pengguna mencari produk `CLINIQUE - Pep-Start 2-in-1 Exfoliating Cleanser (Cleanser)`, fungsi rekomendasi akan mengembalikan 10 produk skincare yang paling mirip berdasarkan skor cosine similarity tertinggi. Berikut ilustrasi format hasil top-10 rekomendasi yang dapat diterima pengguna:

![image](https://github.com/user-attachments/assets/56c19664-c41b-43ca-8497-bf66e1cdadb7)

Hasil rekomendasi berdasarkan label, ingredient, dan skin type berhasil memberikan rekomendasi sebanyak 8 dan yang tidak sesuai rekomendasi 2.


### 2.  Collaborative-Based Filtering
Pada tahap ini, dilakukan model sistem rekomendasi produk skincare dibangun menggunakan pendekatan collaborative filtering berbasis neural network dengan framework TensorFlow dan Keras.Model ini dibuat dalam bentuk class `RecommenderNet` yang merupakan turunan dari `tf.keras.Model`. Di dalamnya, terdapat embedding layer untuk user (`product_embedding`) dan produk (`name_embedding`), masing-masing dengan regularisasi dan ukuran embedding yang bisa diatur. Selain itu, terdapat juga embedding bias untuk user dan produk. Embedding user dan produk kemudian digabung (concatenate), dilewatkan ke dense layer dengan 128 neuron dan aktivasi ReLU, diikuti dropout untuk mengurangi risiko overfitting, dan diteruskan ke dense layer output berukuran 1 (linear) untuk menghasilkan skor prediksi.

Model diinisialisasi dengan jumlah user dan produk hasil encoding, serta embedding size yang diinginkan. Proses kompilasi model menggunakan fungsi loss `BinaryCrossentropy`, optimizer Adam dengan learning rate 0.0005, dan metrik evaluasi `RootMeanSquaredError`. Untuk mencegah overfitting, digunakan callback `EarlyStopping` yang akan menghentikan training jika nilai RMSE validasi tidak membaik selama 5 epoch berturut-turut. Proses training dilakukan dengan batch size 32, maksimal 100 epoch, serta memanfaatkan data validasi 20% dari dataset.

Arsitektur model terdiri dari embedding layer untuk `user_id` dan `Brand_Product`, masing-masing dengan bias, yang kemudian hasil embeddingnya digabungkan dan diproses melalui beberapa dense layer hingga menghasilkan skor prediksi. Model ini dioptimasi menggunakan loss function Mean Squared Error dan optimizer Adam dengan learning rate kecil, serta menggunakan EarlyStopping untuk mencegah overfitting saat pelatihan.

Setelah training, performa model divisualisasikan dengan membuat grafik learning curve RMSE pada data training dan validasi di setiap epoch, sehingga dapat dipantau apakah model mengalami overfitting atau underfitting. Berdasarkan grafik, model menunjukkan penurunan error yang signifikan di awal, kemudian stabil, menandakan proses training berjalan baik.

Untuk implementasi sistem rekomendasi, disediakan fungsi `recommend_products_based_on_name`. Fungsi ini menerima nama produk sebagai input, lalu meng-encode produk tersebut dan membuat kombinasi input dengan seluruh user. Model akan memprediksi skor popularitas untuk tiap kombinasi, kemudian memilih 10 user dengan skor tertinggi. Informasi produk yang direkomendasikan ditampilkan lengkap dengan nama, label, ingredients & skin type, serta rating dari dataset hasil filter.

![image](https://github.com/user-attachments/assets/66e837b9-c53b-4165-a7e4-a0184e4817a5)


## Evaluation
### 1. Content-Based Filtering
Model content-based filtering pada kode yang digunakan bekerja dengan membandingkan fitur-fitur dari produk skincare. Fitur utama yang digunakan adalah gabungan nama brand, nama produk, label (kategori produk), ingredients, dan jenis kulit yang sesuai. Untuk menghasilkan rekomendasi, sistem mencari produk lain yang memiliki kemiripan fitur dengan produk yang dipilih pengguna. Kemiripan ini biasanya dihitung menggunakan teknik representasi vektor dan similarity measurement seperti cosine similarity. Produk-produk dengan nilai similarity tertinggi akan direkomendasikan sebagai produk yang mirip.

Precision@K : mengukur proporsi item relevan yang terdapat pada K rekomendasi teratas.

![image](https://github.com/user-attachments/assets/a5c59a28-988d-4276-a8b9-21a297e9cdec)

**Precision@10** = 8 / 10 = 0.8 atau 80%

Nilai Precision@10 sebesar 80% menunjukkan bahwa sebagian besar produk yang direkomendasikan oleh sistem memang relevan terhadap kebutuhan pengguna berdasarkan label, ingredients, dan skin type. Artinya, sistem content-based filtering sudah cukup efektif dalam memberikan rekomendasi yang akurat, walaupun masih ada ruang perbaikan untuk meningkatkan relevansi rekomendasi pada aspek yang belum sesuai.

### 2. Collaborative-Based Filtering
Model collaborative filtering dibangun menggunakan deep learning dengan embedding. Model menerima input berupa pasangan user dan produk hasil encoding, kemudian mempelajari pola interaksi antara user dan produk melalui kolom 'Rank' sebagai nilai rating/popularitas. Setelah proses training, model dapat memprediksi skor rekomendasi produk untuk setiap user berdasarkan pola kolektif dari seluruh data interaksi.
  
Evaluasi collaborative filtering dilakukan dengan membandingkan prediksi model terhadap nilai aktual pada data validasi menggunakan metrik Root Mean Squared Error (RMSE). RMSE yang lebih rendah menandakan prediksi model semakin mendekati nilai sebenarnya.

![image](https://github.com/user-attachments/assets/2b8e9109-dc14-4497-b8ef-7df37198f665)


![image](https://github.com/user-attachments/assets/ef8a0e58-13b0-44a1-8b56-95d72d4d53cc)


Grafik di atas menunjukkan bahwa nilai root mean squared error (RMSE) untuk data training dan test turun dengan cepat di awal epoch, yang berarti model belajar dengan baik dari data. Setelah sekitar epoch ke-7, penurunan RMSE melambat dan hasilnya menjadi lebih stabil, terutama pada data test yang nilainya berada di kisaran 0.28–0.34. Hal ini menunjukkan model mampu memprediksi dengan cukup baik dan tidak mengalami overfitting.

## Kesimpulan 
**1. Bagaimana distribusi produk tiap brand untuk jenis kulit tertentu?**
Berdasarkan analisis distribusi produk, ditemukan bahwa brand DR. JART+ secara konsisten mendominasi jumlah produk skincare yang ditujukan untuk berbagai jenis kulit seperti kombinasi, kering, normal, dan sensitif. Sedangkan untuk kulit berminyak, KIEHL'S SINCE 1851 menjadi brand dengan produk terbanyak. Temuan ini menunjukkan adanya spesialisasi dan fokus brand tertentu pada segmen kulit tertentu.

**2. Bagaimana bahan tertentu mempengaruhi kecocokan produk untuk jenis kulit tertentu?**
Hasil analisis ingredients menunjukkan bahwa bahan dasar seperti water dan glycerin hampir selalu ditemukan dalam produk skincare untuk semua jenis kulit. Selain itu, bahan seperti phenoxyethanol dan butylene glycol juga sering muncul di berbagai produk. Komposisi bahan-bahan utama cenderung serupa antar tipe kulit, namun produk untuk kulit sensitif biasanya menghindari bahan yang bersifat iritatif. Hal ini menunjukkan bahwa meskipun bahan dasar sama, pemilihan bahan tambahan yang lebih spesifik tetap berpengaruh terhadap kecocokan produk untuk jenis kulit tertentu.

**3. Bagaimana hubungan antara jumlah ingredients dan rank produk?**
Analisis hubungan antara jumlah ingredients dan rank produk memperlihatkan bahwa tidak ada korelasi langsung yang signifikan antara banyaknya jumlah bahan dengan tingkat popularitas atau peringkat (rank) produk. Produk skincare dengan rank tinggi bisa saja memiliki jumlah bahan yang sedikit atau banyak. Hal ini menegaskan bahwa kualitas dan efektivitas bahan lebih penting daripada kuantitas bahan dalam menentukan reputasi dan popularitas produk di pasaran. 

## Referensi
[1] "Sistem Rekomendasi Pemilihan Produk Skincare," Jurnal Global Ilmiah, vol. 5, no. 2, pp. 1-10, 2023. [Online]. Available: https://jgi.internationaljournallabs.com/index.php/ji/article/view/192

[2] D. N. Magfiroh, "Sistem Rekomendasi Pemilihan Skincare Personal Berdasarkan Analisis Tipe Kulit Wajah Menggunakan Metode Item Based Collaborative Filtering," UIN Malang, 2023. [Online]. Available: http://etheses.uin-malang.ac.id/71365/

[3] "Jurnal Kesehatan dan Pengembangan Produk Kesehatan," Jurnal Aksara Global, vol. 3, no. 1, pp. 45-58, 2023. [Online]. Available: https://jurnal.aksaraglobal.co.id/index.php/jkppk/article/view/483
