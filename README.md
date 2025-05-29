# Laporan Proyek Sistem Rekomendasi Buku - Ade Ripaldi Nuralim

## Project Overview

### Latar Belakang

Meningkatnya jumlah buku yang tersedia di platform digital membuat pengguna kesulitan menemukan buku yang sesuai dengan preferensi mereka. Oleh karena itu, diperlukan sistem rekomendasi yang dapat membantu pengguna menemukan buku yang relevan berdasarkan minat atau preferensi sebelumnya. Sistem rekomendasi seperti ini banyak digunakan oleh e-commerce, perpustakaan digital, dan aplikasi pembelajaran.

Masalah ini penting untuk diselesaikan karena dapat meningkatkan pengalaman pengguna, mempercepat proses pencarian, dan mendorong peningkatan keterlibatan (engagement). Rekomendasi yang relevan juga dapat berkontribusi pada peningkatan penjualan atau peminjaman buku.

Beberapa pendekatan populer dalam sistem rekomendasi adalah content-based filtering dan collaborative filtering. Proyek ini akan membangun sistem rekomendasi buku dengan dua pendekatan tersebut menggunakan dataset publik.

> Referensi:
> - F. Ricci, L. Rokach, B. Shapira, and P. B. Kantor, *Recommender Systems Handbook*, Springer, 2011. [doi:10.1007/978-0-387-85820-3](https://doi.org/10.1007/978-0-387-85820-3)
> - Xavier Amatriain, "Mining Large Streams of User Data for Personalized Recommendations", Netflix Tech Blog, 2013. [Link](https://netflixtechblog.com/mining-large-streams-of-user-data-for-personalized-recommendations-7a5365310e9e)

## Business Understanding

### Problem Statements
1. Bagaimana merekomendasikan buku berdasarkan judul menggunakan Content-Based Filtering
2. Prediksi rating untuk buku yang belum dibaca pengguna menggunakan collborate

### Goals

1. Menghasilkan sistem rekomendasi berbasis konten buku menggunakan algoritma content-based filtering.
2. Menghasilkan sistem rekomendasi berbasis perilaku pengguna dengan pendekatan collaborative filtering.

### Solution Statements

- **Solution 1**: Menggunakan algoritma content-based filtering dengan TF-IDF dan cosine similarity pada kolom judul dan kategori buku.
- **Solution 2**: Menggunakan algoritma matrix factorization (SVD) dari pustaka Surprise untuk collaborative filtering berdasarkan interaksi pengguna-buku.

## Data Understanding

Dataset ini berisi informasi mengenai berbagai buku yang akan digunakan dalam sistem rekomendasi. Setiap baris mewakili satu buku unik dengan beberapa atribut yang mendeskripsikannya.
## Informasi BOOK Dataset 

| Jenis | Keterangan |
| ------ | ------ |
| Title | _Books Dataset_ |
| Source | [Kaggle](https://www.kaggle.com/datasets/abdallahwagih/books-dataset) |
| Maintainer | [Abdullah Wagih Ibrahim](https://www.kaggle.com/abdallahwagih) |
| License | Other (specified in description) |
| Visibility | Publik |
| Tags | _Literature, Deep Learning, Recommender System, Sentence Similarity_ |
| Usability | 10.00 |

## Informasi BOOK Dataset

![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/info.png)

Jumlah Baris dan Kolom

6810 baris → ada 6810 entri data buku.

12 kolom → Asetiap buku memiliki 12 atribut atau fitur.


## 🔢 Struktur Dataset

| Kolom            | Deskripsi                                                              |
| ---------------- | ---------------------------------------------------------------------- |
| `isbn13`         | Nomor ISBN versi 13 digit dari buku                                    |
| `isbn10`         | Nomor ISBN versi 10 digit dari buku                                    |
| `title`          | Judul utama dari buku                                                  |
| `subtitle`       | Subjudul buku (jika tersedia)                                          |
| `authors`        | Nama penulis atau beberapa penulis, dipisahkan dengan titik koma (`;`) |
| `categories`     | Kategori atau genre buku                                               |
| `thumbnail`      | URL gambar sampul buku (thumbnail)                                     |
| `description`    | Ringkasan atau deskripsi singkat isi buku                              |
| `published_year` | Tahun buku diterbitkan                                                 |
| `average_rating` | Rata-rata rating dari pembaca                                          |
| `num_pages`      | Jumlah halaman pada buku                                               |
| `ratings_count`  | Jumlah total penilaian/rating yang diberikan oleh pembaca              |


### mengecek jumlah nilai kosong (null/NaN) di setiap kolom dari DataFrame df.


```python
df.isnull().sum()
```

## 📊 Missing Data Summary


| Kolom            | Jumlah Null | Keterangan                                   |
|------------------|--------------|-----------------------------------------------|
| `subtitle`       | 4429         | Banyak buku tidak memiliki subjudul.         |
| `authors`        | 72           | Beberapa data buku tidak mencantumkan penulis. |
| `thumbnail`      | 329          | Tidak semua buku memiliki gambar sampul.     |
| `published_year` | 6            | Tahun terbit tidak tersedia untuk sebagian kecil buku. |
| `categories`     | 99           | Ada buku yang tidak dikategorikan secara eksplisit. |
| `description`    | 262          | Deskripsi buku tidak tersedia seluruhnya.    |
| `average_rating` | 43           | Nilai rata-rata rating tidak tercatat.       |
| `num_pages`      | 43           | Jumlah halaman tidak disebutkan.             |
| `ratings_count`  | 43           | Tidak diketahui berapa banyak rating yang diberikan. |



📌 **Alasan:**  
Pengecekan perlu dilakukan sebelum analisis

---

### Cek duplikasi

```python
print("Jumlah duplikat:", df.duplicated().sum())
```

Dari hasil kode di atas jumlah duplikasi pada data ini adalah 0, tidak ada duplikasi.

📌 **Alasan:**  
Cek duplikasi dilakukan agar data yang digunakan bersih dan tidak menyesatkan hasil analisis atau model.

---


## Univariate Exploratory Data Analysis

![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/uv1.png)

1. Distribusi Tahun Terbit Buku
   
Penjelasan:

- Grafik menunjukkan distribusi jumlah buku berdasarkan tahun terbit.

- Terlihat bahwa sebagian besar buku diterbitkan setelah tahun 1980, dan puncaknya terjadi sekitar tahun 2000–2010.

- Sebelum tahun 1950, hanya sedikit buku yang ada dalam dataset.

- Hal ini bisa berarti bahwa dataset lebih banyak berisi buku-buku modern.

  
![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/uv2.png)

2. Distribusi Rating Buku
   
Penjelasan:

- Histogram menunjukkan distribusi average_rating dari buku-buku.

- Rata-rata rating buku berkisar antara 3.5 hingga 4.5.

- Distribusi cenderung normal (lonceng), dengan sedikit outlier di bawah 2 dan di atas 4.8.

- Artinya sebagian besar buku memiliki rating cukup baik, menunjukkan kemungkinan bias ke buku-buku populer atau berkualitas.

  
![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/uv3.png)


3. Top 10 Penulis
   
Penjelasan:

- Grafik horizontal bar chart menampilkan 10 penulis dengan jumlah buku terbanyak di dataset.

- Penulis paling banyak adalah Agatha Christie, diikuti oleh Stephen King dan William Shakespeare.

- Ini menunjukkan bahwa penulis-penulis ini sangat produktif atau karyanya banyak masuk dalam dataset.

Kesimpulan Univariate Analysis:
Univariate analysis fokus pada satu variabel dalam satu waktu, dan dari visualisasi di atas kita bisa menyimpulkan bahwa:

- Dataset didominasi oleh buku-buku yang terbit setelah tahun 1980.

- Rata-rata rating buku cukup tinggi dan terdistribusi normal.

- Beberapa penulis legendaris mendominasi jumlah karya dalam dataset.

## Data Preprocessing

Tahapan _data preparation_ dilakukan untuk membersihkan dan mempersiapkan data sebelum digunakan pada sistem rekomendasi. Teknik yang diterapkan dilakukan secara berurutan sebagai berikut:

### 1. Menghapus Duplikat Berdasarkan Kolom `isbn13`

```python
df = df.drop_duplicates(subset='isbn13')
```

📌 **Alasan:**  
Menghindari data redundan yang bisa menyebabkan bias dalam hasil rekomendasi. Duplikat data dapat mempengaruhi hasil pelatihan model dan evaluasi sistem rekomendasi.

---

### 2. Menghapus Baris Kosong (Missing Values) pada Kolom `title`, `categories`, dan `average_rating`

```python
df = df.dropna(subset=['title', 'categories', 'average_rating'])
```

📌 **Alasan:**  
Ketiga kolom ini merupakan informasi penting untuk sistem rekomendasi berbasis konten. Jika data kosong, maka informasi konten buku tidak dapat direpresentasikan secara optimal.

---

## 📊 Data Preparation

### 1. Reset Index

```python
df.reset_index(drop=True, inplace=True)
```

📌 **Alasan:**
Menghapus indeks lama untuk memastikan urutan baris bersih dan konsisten, terutama setelah proses merge atau filter data.


### 2. Penanganan Missing Values

```python
df['description'] = df['description'].fillna('')
df['authors'] = df['authors'].fillna("Unknown Author")
df['thumbnail'] = df['thumbnail'].fillna("https://via.placeholder.com/150")
df['published_year'] = df['published_year'].fillna(df['published_year'].median())
```

📌 **Alasan:**
- Kolom description diisi dengan string kosong agar tidak mengganggu pemrosesan teks.

- Kolom authors diisi dengan "Unknown Author" sebagai nilai default.

- Kolom thumbnail diisi dengan URL placeholder agar tetap bisa ditampilkan.

- Kolom published_year menggunakan median karena merupakan nilai numerik dan median lebih tahan terhadap outlier.
  
### 3. Standarisasi Kolom categories

```python
df['categories'] = df['categories'].apply(lambda x: x.split(',')[0].strip())
```

📌 **Alasan:**  
Setiap buku bisa memiliki lebih dari satu kategori. Untuk menyederhanakan representasi konten, kita ambil kategori utama (pertama) agar pemodelan lebih fokus dan tidak terlalu kompleks.

### 4. Standarisasi Kolom categories

```python
df['content'] = df['categories'] + " " + df['description']
```

📌 **Alasan:**  
Fitur content dibentuk dengan menggabungkan kategori dan deskripsi untuk menghasilkan representasi teks yang lebih kaya dan relevan dalam pendekatan berbasis konten (content-based filtering).

### 5. Transformasi Teks dengan TF-IDF

```python
from sklearn.feature_extraction.text import TfidfVectorizer

vectorizer = TfidfVectorizer(stop_words='english')
tfidf_matrix = vectorizer.fit_transform(df['content'])
```

📌 **Alasan:**  
TF-IDF digunakan untuk mengubah teks content menjadi vektor numerik yang merepresentasikan pentingnya setiap kata, sekaligus menghilangkan kata-kata umum yang tidak bermakna (stop_words='english').

### 5. Split data dan  Simulasi Data Rating untuk Collaborative Filtering

```python
from surprise.model_selection import train_test_split

# Buat dataset Surprise
reader = Reader(rating_scale=(0, 5))
data = Dataset.load_from_df(df[['isbn13', 'title', 'average_rating']], reader)

trainset, testset = train_test_split(data, test_size=0.2)

```

📌 **Alasan:**  

Pemisahan data (train-test split) sangat penting dalam proses Collaborative Filtering agar model dapat dilatih pada sebagian data (trainset) dan dievaluasi pada data yang belum pernah dilihat sebelumnya (testset). Hal ini memastikan bahwa performa model diukur secara objektif dan tidak mengalami overfitting.

Simulasi ini juga menggunakan rating rata-rata (average_rating) sebagai representasi interaksi pengguna-buku karena data rating eksplisit dari pengguna sesungguhnya tidak tersedia. Strategi ini umum digunakan ketika hanya terdapat metadata buku tanpa histori pengguna.


---

Dengan proses data preparation ini, dataset telah siap untuk digunakan pada tahap modeling sistem rekomendasi berbasis konten.


## Modeling

Tahapan ini membahas model sistem rekomendasi yang dibangun untuk memberikan rekomendasi buku. Dua pendekatan digunakan dalam proyek ini:

### 📘 1. Content-Based Filtering (Rekomendasi Berdasarkan Judul)

Pendekatan ini merekomendasikan buku berdasarkan kemiripan antara deskripsi dan kategori buku menggunakan teknik **TF-IDF** dan **cosine similarity**.
Pendekatan ini memberikan rekomendasi berdasarkan kesamaan isi buku, dengan menggunakan informasi dari kolom categories dan description. Teknik yang digunakan adalah TF-IDF (Term Frequency-Inverse Document Frequency) untuk representasi teks dan Cosine Similarity untuk mengukur kemiripan antar buku.


#### Contoh Kode:
```python
recommend_books_content("Rage of angels")
```

#### Output:

![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/1.png)

Sistem ini bertujuan untuk merekomendasikan buku-buku yang mirip dari segi isi (konten), dalam hal ini berdasarkan gabungan kategori dan deskripsi, bukan dari perilaku pengguna

penjeleasan:

- Interpretasi Hasil:
  - Buku-buku yang direkomendasikan memiliki tema serupa dengan "Rage of Angels":

  - Genre seperti Fiction, Organized Crime, dan Drama Hukum (Legal Drama).

  - Hal ini menunjukkan bahwa sistem berhasil menangkap kemiripan konten, bukan hanya berdasarkan judul, tetapi berdasarkan topik utama yang dijelaskan dalam kategori dan deskripsi.

  - Rata-rata rating buku juga cukup tinggi (≥ 3.6), yang berarti hasil rekomendasinya layak dibaca.


### 👥 2. Collaborative Filtering (Prediksi Rating Buku)

Pendekatan ini menggunakan metode **model-based collaborative filtering** dengan algoritma **SVD (Singular Value Decomposition)** dari pustaka `surprise`.
Pendekatan ini menggunakan algoritma SVD (Singular Value Decomposition) dari pustaka surprise, dan termasuk dalam kategori model-based collaborative filtering. Tujuannya adalah mempelajari pola rating antar pengguna dan buku untuk memprediksi rating buku yang belum pernah diberi rating oleh pengguna tertentu.

#### Langkah-langkah:
- Membuat dataset dengan kolom `isbn13`, `title`, dan `average_rating`.
- Melatih model `SVD` untuk mempelajari pola rating.
- Melakukan prediksi rating buku untuk pengguna fiktif `test_user`.

#### Contoh Kode:
```python
recommend_books_collab()
```

#### Output:

![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/2.png)

Sistem ini bertujuan untuk memprediksi rating buku yang belum dibaca oleh pengguna, berdasarkan pola rating pengguna lain terhadap buku-buku yang sama atau serupa. Sistem ini tidak melihat isi/konten buku, melainkan mengandalkan kesamaan perilaku pengguna.

interpretasi Rekomendasi Collaborative Filtering

Berbasis Statistik Rating Pengguna Lain Sistem merekomendasikan buku kepada 'test_user' meskipun tidak ada data riil pengguna ini, dengan mengandalkan pola rating dari pengguna lain pada buku-buku populer.

Fokus pada Pola, Bukan Isi Buku Model tidak mempertimbangkan genre atau deskripsi buku. Buku dari genre yang berbeda bisa direkomendasikan jika memiliki rating tinggi secara umum.

Buku Populer Cenderung Direkomendasikan Buku-buku dengan average rating tinggi seperti Fiction, Mystery, dan Christian Life muncul karena disukai oleh banyak pengguna, bukan karena kemiripan konten.

---

### 🔍 Kelebihan dan Kekurangan:

| Pendekatan            | Kelebihan                                                                 | Kekurangan                                                                  |
|------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| Content-Based          | Tidak membutuhkan data pengguna lain, cocok untuk pengguna baru           | Terbatas pada informasi buku yang mirip dengan yang pernah dilihat         |
| Collaborative Filtering| Dapat menemukan buku baru berdasarkan pola pengguna lain                  | Memerlukan cukup banyak data interaksi pengguna, sensitif terhadap cold-start |

---


## Evaluation

Untuk mengevaluasi performa model collaborative filtering, digunakan dua metrik evaluasi:

### 📐 Metrik Evaluasi

- **RMSE (Root Mean Squared Error)**: Mengukur rata-rata kuadrat selisih antara rating aktual dan prediksi.
- **MAE (Mean Absolute Error)**: Mengukur rata-rata absolut dari selisih antara rating aktual dan prediksi.

#### Formula:
- RMSE:
  
![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/3.png)


- MAE:
  
![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/4.png)

Keterangan :

![image](https://raw.githubusercontent.com/revaile/sistem-rekomendasi-buku/refs/heads/main/5.png)


#### Hasil Evaluasi:
```python
RMSE: 0.3261
MAE:  0.2355  
```

Nilai RMSE dan MAE yang relatif rendah menunjukkan bahwa model SVD cukup akurat dalam memprediksi rating buku yang disukai pengguna fiktif. Hal ini menandakan bahwa sistem dapat digunakan untuk memberikan rekomendasi yang relevan berdasarkan preferensi pengguna.

---

## ✅ Apakah sudah menjawab setiap *Problem Statement*?

1. Bagaimana merekomendasikan buku berdasarkan judul menggunakan Content-Based Filtering?
2. Bagaimana memprediksi rating untuk buku yang belum dibaca pengguna menggunakan Collaborative Filtering?

Seluruh *Problem Statement* telah dijawab dengan baik:

- Rekomendasi berdasarkan judul diimplementasikan menggunakan TF-IDF dan cosine similarity.
- Prediksi rating dilakukan menggunakan model SVD dari pustaka Surprise.

---

## 🎯 Apakah berhasil mencapai setiap *Goal* yang diharapkan?

- Membangun sistem rekomendasi berbasis konten menggunakan algoritma **Content-Based Filtering**.
- Membangun sistem rekomendasi berbasis perilaku pengguna dengan pendekatan **Collaborative Filtering**.

Kedua tujuan ini berhasil tercapai:

- Sistem content-based berhasil memanfaatkan informasi dari judul, kategori, dan deskripsi buku.
- Sistem collaborative berhasil mempelajari preferensi pengguna berdasarkan interaksi rating dengan akurasi tinggi.

---


## 💡 Apakah setiap *Solution Statement* yang direncanakan berdampak?

- **Content-Based Filtering**  
  Menggunakan algoritma TF-IDF dan cosine similarity pada kolom kategori dan deskripsi buku.  
  → Cocok untuk cold-start problem dan ketika belum banyak interaksi pengguna.

- **Collaborative Filtering**  
  Menggunakan matrix factorization (SVD) dari pustaka `Surprise` berdasarkan interaksi pengguna-buku.  
  → Mampu memberikan rekomendasi personal berbasis pola perilaku pengguna lain.


---

## 📌 Kesimpulan

- Proyek ini berhasil membangun dua sistem rekomendasi yang saling melengkapi.
- **Content-Based Filtering** cocok untuk pengguna baru atau buku baru.
- **Collaborative Filtering** cocok untuk rekomendasi yang dipersonalisasi berdasarkan riwayat rating.
- Kombinasi keduanya dapat digunakan untuk menciptakan pengalaman pencarian buku yang lebih baik dan personal.

---

## 🛠️ Tools & Libraries

- Python
- pandas, numpy
- scikit-learn
- surprise (for SVD)
- TF-IDF Vectorizer & Cosine Similarity

---

## Penutup
Dengan dua pendekatan ini, sistem rekomendasi dapat membantu pengguna menemukan buku yang sesuai berdasarkan konten maupun interaksi pengguna lain, dan meningkatkan pengalaman membaca digital secara keseluruhan.

## 🧠 Author

**Ade Ripaldi Nuralim**  
Informatics Engineering, UIN Sunan Gunung Djati Bandung  
2025
