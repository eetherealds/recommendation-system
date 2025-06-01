# Laporan Proyek Machine Learning - Dearni Lambardo Saragih

## Project Overview
Kulit merupakan organ terluar tubuh manusia yang memiliki peran vital dalam melindungi tubuh dari berbagai faktor eksternal. Dalam era modern ini, perawatan kulit (skincare) telah menjadi bagian penting dari rutinitas kesehatan dan kecantikan bagi banyak orang. Namun, dengan banyaknya produk skincare yang tersedia di pasaran, konsumen sering kali mengalami kesulitan dalam memilih produk yang sesuai dengan jenis dan permasalahan kulit mereka [1](https://jgi.internationaljournallabs.com/index.php/ji/article/view/192).

Menurut penelitian yang dilakukan oleh Magfiroh (2023), sebagian besar konsumen mengalami kebingungan saat memilih produk skincare karena kurangnya pengetahuan tentang kandungan produk dan kesesuaiannya dengan tipe kulit mereka. Hal ini dapat menyebabkan pembelian produk yang tidak tepat, yang berpotensi memperburuk kondisi kulit atau tidak memberikan hasil yang diharapkan [2].

Sistem rekomendasi pemilihan produk skincare hadir sebagai solusi untuk mengatasi permasalahan tersebut. Dengan memanfaatkan teknologi informasi, sistem ini dapat membantu konsumen menemukan produk skincare yang sesuai dengan jenis kulit dan permasalahan kulit yang dialami [1]. Pengembangan sistem rekomendasi ini menjadi penting karena dapat memberikan panduan yang lebih personal dan spesifik bagi pengguna, dibandingkan dengan saran umum yang mungkin tidak sesuai dengan kondisi kulit individu.

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
Paragraf awal bagian ini menjelaskan informasi mengenai jumlah data, kondisi data, dan informasi mengenai data yang digunakan. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Restaurant+%26+consumer+data).

Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- dst

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
