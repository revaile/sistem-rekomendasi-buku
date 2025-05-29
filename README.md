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

| isbn13        | isbn10     | Title          | Subtitle | Authors                          | Categories                    | Published Year | Avg Rating | Pages | Ratings Count |
| ------------- | ---------- | -------------- | -------- | -------------------------------- | ----------------------------- | -------------- | ---------- | ----- | ------------- |
| 9780002005883 | 0002005883 | Gilead         | –        | Marilynne Robinson               | Fiction                       | 2004           | 3.85       | 247   | 361           |
| 9780002261982 | 0002261987 | Spider's Web   | A Novel  | Charles Osborne; Agatha Christie | Detective and mystery stories | 2000           | 3.83       | 241   | 5164          |
| 9780006163831 | 0006163831 | The One Tree   | –        | Stephen R. Donaldson             | American fiction              | 1982           | 3.97       | 479   | 172           |
| 9780006178736 | 0006178731 | Rage of Angels | –        | Sidney Sheldon                   | Fiction                       | 1993           | 3.93       | 512   | 29532         |
| 9780006280897 | 0006280897 | The Four Loves | –        | Clive Staples Lewis              | Christian life                | 2002           | 4.15       | 170   | 33684         |

Jumlah Baris dan Kolom

6671 baris → ada 6671 entri data buku.

13 kolom → Asetiap buku memiliki 13 atribut atau fitur.


## 🔢 Struktur Dataset

| Kolom            | Deskripsi |
|------------------|-----------|
| `isbn13`         | Kode ISBN-13 sebagai identifikasi unik buku (13 digit). |
| `isbn10`         | Kode ISBN-10 sebagai versi lama identifikasi buku (10 digit). |
| `title`          | Judul utama buku. |
| `subtitle`       | Subjudul buku (jika tersedia). |
| `authors`        | Nama penulis buku. |
| `categories`     | Kategori atau genre buku (misalnya: Fiction, Juvenile Fiction, dll). |
| `thumbnail`      | URL gambar sampul buku dari Google Books API. |
| `description`    | Ringkasan isi buku. |
| `published_year` | Tahun terbit buku. Rentang: 1853–2019. |
| `average_rating` | Rata-rata rating pembaca terhadap buku (skala 0–5). |


## Univariate Exploratory Data Analysis

Statik umum dataset :

- **Jumlah buku**: 6.398 entri
- **Kolom dengan nilai hilang (`null`)**:
  - `subtitle`: 65% missing
  - `categories`: 54% missing
  - `description`: 4% missing
  - `thumbnail`: 5% missing
 
  **EDA**
  
![image](https://github.com/user-attachments/assets/ef45f82c-324c-466e-a7d9-3d357c4e6157)

insight :

- Distribusi Tahun Terbit Buku: Mayoritas buku yang dianalisis diterbitkan setelah tahun 1990-an, dengan puncak yang sangat signifikan di awal tahun 2000-an. Ada sedikit buku yang diterbitkan sebelum tahun 1950, menunjukkan fokus data pada publikasi yang lebih modern.

![image](https://github.com/user-attachments/assets/5650d351-e651-4b2d-8940-479050351c97)

insight:

- Distribusi Rating Buku: Sebagian besar buku memiliki rating rata-rata antara 3.5 dan 4.5, dengan puncak distribusi di sekitar 3.9 hingga 4.0. Ini menunjukkan bahwa buku-buku dalam dataset cenderung memiliki rating yang cukup tinggi.

![image](https://github.com/user-attachments/assets/9cba54f5-49b1-4a0b-b00b-8d8a7e35e5d0)

- Top 10 Penulis: Agatha Christie, Stephen King, dan William Shakespeare adalah tiga penulis teratas dengan jumlah karya terbanyak dalam dataset. Ini menunjukkan popularitas atau kelimpahan karya dari penulis-penulis tersebut dalam koleksi data ini.

## 📊 Data Preparation

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

### 3. Menyederhanakan Kolom `categories` dengan Mengambil Kategori Pertama

```python
df['categories'] = df['categories'].apply(lambda x: x.split(',')[0].strip())
```

📌 **Alasan:**  
Setiap buku bisa memiliki lebih dari satu kategori. Untuk menyederhanakan representasi konten, kita ambil kategori utama (pertama) agar pemodelan lebih fokus dan tidak terlalu kompleks.

---

### 4. Membuat Fitur Gabungan Konten Buku dari `title` dan `categories`

```python
df['content'] = df['title'] + ' ' + df['categories']
```

📌 **Alasan:**  
Gabungan antara judul dan kategori buku dianggap sebagai representasi konten yang bisa dianalisis oleh mesin. Ini sangat berguna dalam pendekatan content-based filtering.

---

### 5. Konversi Fitur `content` Menjadi Representasi Numerik Menggunakan TF-IDF

```python
from sklearn.feature_extraction.text import TfidfVectorizer

tfidf = TfidfVectorizer()
tfidf_matrix = tfidf.fit_transform(df['content'])
```

📌 **Alasan:**  
Algoritma machine learning tidak bisa memproses data teks secara langsung. Oleh karena itu, teks perlu diubah menjadi vektor numerik. TF-IDF memberi bobot berdasarkan frekuensi kata yang relevan, sehingga fitur teks menjadi lebih bermakna.

---

### 6. Menghitung Kemiripan Antar Buku Menggunakan Cosine Similarity

```python
from sklearn.metrics.pairwise import cosine_similarity

cosine_sim = cosine_similarity(tfidf_matrix)
```

📌 **Alasan:**  
Cosine similarity digunakan untuk mengukur tingkat kemiripan antar buku berdasarkan vektor TF-IDF-nya. Semakin tinggi nilai cosine, semakin mirip isi konten dua buku tersebut.

---

Dengan proses data preparation ini, dataset telah siap untuk digunakan pada tahap modeling sistem rekomendasi berbasis konten.


## Modeling

Tahapan ini membahas model sistem rekomendasi yang dibangun untuk memberikan rekomendasi buku. Dua pendekatan digunakan dalam proyek ini:

### 📘 1. Content-Based Filtering (Rekomendasi Berdasarkan Judul)

Pendekatan ini merekomendasikan buku berdasarkan kemiripan antara deskripsi dan kategori buku menggunakan teknik **TF-IDF** dan **cosine similarity**.

#### Langkah-langkah:
- Menggabungkan kolom `categories` dan `description` sebagai fitur teks.
- Melakukan transformasi teks menggunakan `TfidfVectorizer`.
- Menghitung kemiripan antar buku dengan `cosine_similarity`.

#### Contoh Kode:
```python
recommend_books_content("Rage of angels")
```

#### Output:

![image](https://github.com/user-attachments/assets/900d1b30-1722-45fa-afdf-54baa861b42a)

### 👥 2. Collaborative Filtering (Prediksi Rating Buku)

Pendekatan ini menggunakan metode **model-based collaborative filtering** dengan algoritma **SVD (Singular Value Decomposition)** dari pustaka `surprise`.

#### Langkah-langkah:
- Membuat dataset dengan kolom `isbn13`, `title`, dan `average_rating`.
- Melatih model `SVD` untuk mempelajari pola rating.
- Melakukan prediksi rating buku untuk pengguna fiktif `test_user`.

#### Contoh Kode:
```python
recommend_books_collab()
```

#### Output:

![image](https://github.com/user-attachments/assets/642428a7-9905-4776-9bdf-ce50fa342fa3)

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
- 
![image](https://github.com/user-attachments/assets/568b0534-9e1b-4f00-a7cd-74f8ea6dae8d)


- MAE:
- 
![image](https://github.com/user-attachments/assets/657e1fca-fe04-4f39-8a61-1ebfce27eb8d)


#### Hasil Evaluasi:
```python
RMSE: 0.3367
MAE:  0.2272
```

Nilai RMSE dan MAE yang relatif rendah menunjukkan bahwa model SVD cukup akurat dalam memprediksi rating buku yang disukai pengguna fiktif. Hal ini menandakan bahwa sistem dapat digunakan untuk memberikan rekomendasi yang relevan berdasarkan preferensi pengguna.

---

## Penutup
Dengan dua pendekatan ini, sistem rekomendasi dapat membantu pengguna menemukan buku yang sesuai berdasarkan konten maupun interaksi pengguna lain, dan meningkatkan pengalaman membaca digital secara keseluruhan.
