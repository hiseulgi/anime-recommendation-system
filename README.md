# Laporan Proyek Machine Learning - Muhammad Bagus Adi Prayoga

## Project Overview

Industri anime mengalami pertumbuhan pesat dalam beberapa tahun terakhir, didorong oleh popularitas platform streaming dan konten yang berlimpah. Dengan banyaknya judul anime yang tersedia, penonton sering kali kesulitan menemukan konten baru yang sesuai dengan selera mereka [1]. Di sinilah sistem rekomendasi anime berperan, memberikan saran personal berdasarkan preferensi pengguna dan karakteristik konten.

Ada beberapa alasan mengapa sistem rekomendasi sangat penting bagi industri anime:
- **Menavigasi Konten yang Beragam**: Pertumbuhan anime yang eksponensial dapat membuat penonton kewalahan dan kesulitan menemukan konten yang sesuai dengan minat mereka. Sistem rekomendasi membantu pengguna menemukan konten yang relevan dengan minat mereka.
- **Meningkatkan Kepuasan Pengguna**: Rekomendasi yang dipersonalisasi membantu pengguna menemukan konten yang mereka sukai, meningkatkan kepuasan dan keterlibatan dengan platform.
- **Mempertahankan Pengguna**: Sistem rekomendasi membantu platform mempertahankan pengguna dengan terus menyediakan konten sesuai preferensi mereka, mengurangi kemungkinan berpindah ke platform lain.

Penelitian sebelumnya pada sistem rekomendasi anime telah mengeksplorasi berbagai pendekatan dan teknik untuk meningkatkan akurasi dan efektivitas rekomendasi. Beberapa bidang penelitian utama yang telah dipelajari meliputi:
- ***Collaborative Filtering***: Pendekatan ini melibatkan analisis perilaku pengguna yang serupa untuk memberikan rekomendasi. Pendekatan ini terbukti efektif dalam memberikan rekomendasi berdasarkan kesamaan minat di antara para pengguna [2].
- ***Content-Based Filtering***: Metode ini berfokus pada karakteristik konten itu sendiri, seperti genre, sutradara, atau pengisi suara, untuk memberikan rekomendasi. Metode ini memungkinkan rekomendasi yang dipersonalisasi berdasarkan preferensi spesifik pengguna [2].

Singkatnya, pertumbuhan industri anime dan banyaknya konten yang tersedia mengharuskan penggunaan sistem rekomendasi untuk membantu penonton menemukan dan menikmati konten yang sesuai dengan preferensi mereka. Penelitian yang ada telah mengeksplorasi berbagai pendekatan dan teknik untuk meningkatkan efektivitas sistem ini, dengan fokus pada *collaborative filtering* dan *content-based filtering*.


## Business Understanding

### Problem Statements

Berdasarkan pemahaman atas *project overview* yang telah diuraikan sebelumnya, berikut adalah *problem statements* yang teridentifikasi:
1. Bagaimana membangun sistem rekomendasi anime yang efektif untuk membantu pengguna menemukan konten yang sesuai dengan informasi anime yang tersedia?
2. Bagaimana membangun sistem rekomendasi anime yang dapat memberikan rekomendasi berdasarkan kesamaan minat di antara pengguna?

### Goals

Berdasarkan *problem statements* yang telah diidentifikasi sebelumnya, berikut adalah beberapa *goals* dari proyek ini:
1. Mengembangkan sistem rekomendasi anime berdasarkan kesamaan informasi anime yang tersedia.
2. Mengembangkan sistem rekomendasi anime berdasarkan kesamaan penilaian anime yang diberikan oleh pengguna.

### Solution statements

Berdasarkan *goals* di atas, maka diperoleh beberapa *solution statement* untuk menyelesaikan masalah tersebut, yaitu:
1. Membuat sistem rekomendasi dengan teknik *Content-Based Filtering* berdasarkan informasi anime.
2. Membuat sistem rekomendasi dengan teknik *Collaborative Filtering* berdasarkan penilaian anime yang diberikan oleh pengguna.


## Data Understanding

Datasets yang digunakan dalam proyek ini adalah datasets yang berasal dari data preferensi pengguna terhadap anime dan data informasi anime dari situs web MyAnimeList (Sumber datasets: [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database)). Dataset ini terdiri dari dua tabel yaitu `anime.csv` dan `rating.csv`.

Berikut adalah fitur-fitur pada tabel `anime.csv`:
- ***anime_id***: ID unik untuk setiap anime pada situs web MyAnimeList.
- ***name***: Judul anime.
- ***genre***: Genre anime yang dipisahkan oleh koma.
- ***type***: Type anime, seperti TV, Movie, OVA, dll.
- ***episodes***: Jumlah episode anime.
- ***rating***: Rata-rata rating anime (skala 1-10).
- ***members***: Jumlah anggota yang tergabung dalam grup anime.

Kemudian, berikut adalah fitur-fitur pada tabel `rating.csv`:
- ***user_id***: ID unik untuk pengguna.
- ***anime_id***: ID unik untuk setiap anime pada situs web MyAnimeList.
- ***rating***: Rating yang diberikan oleh pengguna pada anime tertentu dalam skala 1-10 (-1 jika pengguna tidak memberikan rating).

### Descriptive Statistics

#### Tabel `anime.csv`

Pada tabel `anime.csv`, terdapat 12.064 baris data dan 7 kolom. Berikut adalah deskriptif statistik untuk tabel ini:

Tabel 1. Deskriptif statistik tabel `anime.csv`
|          |   count |       mean |         std |   min |     25% |      50% |      75% |             max |
|:---------|--------:|-----------:|------------:|------:|--------:|---------:|---------:|----------------:|
| anime_id |   12064 | 13704.5    | 11260.4     |  1    | 3409.25 | 10004    | 23863.5  | 34519           |
| rating   |   12064 |     6.4739 |     1.02675 |  1.67 |    5.88 |     6.57 |     7.18 |    10           |
| members  |   12064 | 18279.5    | 55275.8     | 12    |  221    |  1539    |  9485.5  |     1.01392e+06 |

Tabel 2. Statistik deskriptif untuk kolom kategorikal pada tabel `anime.csv`
|          |   count |   unique | top                     |   freq |
|:---------|--------:|---------:|:------------------------|-------:|
| name     |   12064 |    12062 | Shi Wan Ge Leng Xiaohua |      2 |
| genre    |   12064 |     3230 | Hentai                  |    816 |
| type     |   12064 |        6 | TV                      |   3671 |
| episodes |   12064 |      187 | 1                       |   5612 |

Dari tabel di atas, dapat dilihat bahwa kolom episodes seharusnya adalah kolom numerik, namun memiliki tipe data string. Sehingga, perlu pengecekan lebih lanjut terhadap kolom tersebut. Selain itu juga dilakukan pengecekan *missing values* dan *duplicate values* pada tabel `anime.csv`. Berikut adalah hasil pengecekan tersebut:
- Terdapat *missing value* pada kolom rating, sehingga perlu dilakukan *missing value handling*
    - Dalam hal ini langsung dilakukan penghapusan data, karena data yang memiliki nilai tersebut sangat sedikit
- Tidak ada data duplikat pada tabel ini

#### Tabel `rating.csv`

Pada tabel `rating.csv`, terdapat 7.813.730 baris data dan 3 kolom. Berikut adalah deskriptif statistik untuk tabel ini:

|          |       count |        mean |        std |   min |   25% |   50% |   75% |   max |
|:---------|------------:|------------:|-----------:|------:|------:|------:|------:|------:|
| user_id  | 7.81373e+06 | 36728       | 20998      |     1 | 18974 | 36791 | 54757 | 73516 |
| anime_id | 7.81373e+06 |  8909.07    |  8883.95   |     1 |  1240 |  6213 | 14093 | 34519 |
| rating   | 7.81373e+06 |     6.14403 |     3.7278 |    -1 |     6 |     7 |     9 |    10 |

Dari tabel di atas, terlihat bahwa kolom rating memiliki nilai minimum -1, yang seharusnya nilai minimal dari rating adalah 1. Sehingga, perlu dilakukan pengecekan lebih lanjut terhadap kolom tersebut. Selain itu juga dilakukan pengecekan *missing values* dan *duplicate values* pada tabel `rating.csv`. Berikut adalah hasil pengecekan tersebut:
- Tidak ada *missing value* pada tabel ini
- Terdapat data duplikat pada tabel ini, sehingga data tersebut perlu dihapus

### Exploratory Data Analysis (EDA)

xxx

Gambar 1. Distribusi rata-rata rating dan user rating

xxx

Gambar 2. Distribusi kategori anime berdasarkan type

xxx

Gambar 3. Wordcloud genre anime

Berikut adalah beberapa informasi yang dapat diambil dari hasil EDA:
- Fitur rating (rata-rata rating pada tabel `anime.csv`) memiliki distribusi normal.
- Terdapat outlier pada fitur user rating (tabel `rating.csv`) dimana terdapat data rating yang bernilai -1, sehinnga perlu dilakukan pembersihan pada data tersebut.
- Terdapat nilai `Unknown` pada jumlah episode anime, sehingga perlu dilakukan dilakukan handling data.
- Action, Comedy, dan Romance adalah genre anime yang paling banyak ditemui.


## Data Preparation

### Table `anime.csv`

Berikut adalah tahapan data preparation yang dilakukan pada tabel `anime.csv`:
1. Penghapusan Data Tidak Valid:
   1. Data dengan missing value dihapus karena dapat menyebabkan bias dalam model.
   2. Data dengan duplicate value dihapus karena redundan dan dapat memengaruhi performa model.
   3. Nilai Unknown pada kolom episodes dihapus karena tidak valid dan dapat mengganggu proses modeling.
2. Normalisasi Data:
   1. Nilai pada kolom genre dibersihkan dari tanda koma dan diubah menjadi huruf kecil untuk konsistensi dan memudahkan pengolahan data.
   2. Kolom type diubah menjadi huruf kecil untuk konsistensi.
3. Encoding Data: Kolom episodes, rating, dan members dienkode untuk mengubah nilai numerik menjadi kategori baru. Hal ini membantu model mempelajari hubungan antar kategori dan meningkatkan akurasi prediksi.
4. Penggabungan Kolom: Kolom genre, type, hasil encode episodes, rating, dan members digabungkan menjadi satu kolom baru untuk mempermudah proses modeling.
5. Penataan Data: Kolom anime_id di-reindex untuk menjadikannya sebagai index utama pada tabel. Hal ini mempermudah akses data, meningkatkan performa query, dan memastikan keunikan setiap baris data.

### Table `rating.csv`

Berikut adalah tahapan data preparation yang dilakukan pada tabel `rating.csv`:
1. Penghapusan Data Tidak Valid:
   1. Data dengan nilai rating -1 dihapus karena tidak valid dan dapat mengganggu proses modeling.
   2. Data dengan duplicate value dihapus karena redundan dan dapat memengaruhi performa model.
2. Limitasi Data: Data dibatasi hanya untuk pengguna yang telah melakukan minimal 500 rating. Hal ini dilakukan untuk memastikan keakuratan rekomendasi dan mengurangi komputasi yang berlebihan. Pengguna dengan rating minimal 500 diasumsikan memiliki preferensi yang lebih jelas dan datanya lebih reliable untuk membangun model.
3. Merge Data: Tabel rating.csv di-merge dengan tabel anime.csv berdasarkan kolom anime_id untuk menggabungkan informasi rating dan informasi anime menjadi satu tabel. Penggabungan ini bertujuan agar informasi anime dapat dilihat bersamaan dengan rating yang diberikan oleh pengguna.
4. Penataan Data dengan Pivot Table: Tabel hasil merge diubah menjadi pivot table dengan index anime_name dan kolom user_id. Hal ini memudahkan proses modeling dan memastikan keunikan setiap baris data. Pivot table meringkas data rating untuk setiap kombinasi anime_name dan user_id, yang menjadi input model untuk menghasilkan rekomendasi.


## Modeling

### Content-Based Filtering

TODO: gambar ilustrasi content-based filtering dengan tambahan ilustrasi anime

Content-based filtering merupakan teknik yang digunakan dalam sistem rekomendasi untuk menganalisis karakteristik atau atribut item (dalam hal ini, anime) dan memberikan rekomendasi berdasarkan kesamaan karakteristik tersebut. Pendekatan ini sangat berguna ketika data pengguna yang tersedia terbatas, atau ketika terdapat item baru yang dimasukkan ke dalam sistem.

Salah satu metode yang umum digunakan dalam content-based filtering untuk sistem rekomendasi anime adalah kombinasi dari TF-IDF (Term Frequency-Inverse Document Frequency) dan cosine similarity.

#### TF-IDF (Term Frequency-Inverse Document Frequency)

TF-IDF adalah sebuah ukuran statistik yang mengevaluasi pentingnya sebuah kata atau *term* dalam sebuah dokumen atau korpus. Ukuran ini mempertimbangkan frekuensi *term* dalam dokumen tertentu (Term Frequency, TF) dan kebalikan dari jumlah dokumen yang mengandung *term* tersebut (Inverse Document Frequency, IDF).

Dalam konteks sistem rekomendasi anime di proyek ini, "dokumen" dapat berupa gabungan dari beberapa variabel, seperti genre, type, episodes, rating, dan members yang terkait dengan setiap judul anime. Skor TF-IDF dihitung untuk setiap *term* dalam deskripsi anime, sehingga sistem dapat mengidentifikasi *term* yang paling relevan dan diskriminatif untuk anime tertentu.

Berikut adalah rumus untuk menghitung skor TF-IDF:

$$ \text{TF-IDF}(t, d, D) = \text{TF}(t, d) \times \text{IDF}(t, D) $$

Dimana:
- $\text{TF}(t, d)$ adalah frekuensi kemunculan term $t$ dalam dokumen $d$.
- $\text{IDF}(t, D)$ adalah kebalikan dari jumlah dokumen dalam korpus $D$ yang mengandung term $t$.

Kemudian rumus untuk menghitung skor IDF adalah sebagai berikut:

$$ \text{IDF}(t, D) = \log\left(\frac{N}{DF(t, D)}\right) $$

Dimana:
- $N$ adalah jumlah total dokumen dalam korpus $D$.
- $DF(t, D)$ adalah jumlah dokumen dalam korpus $D$ yang mengandung term $t$.

#### Cosine Similarity

Cosine similarity adalah metrik yang digunakan untuk mengukur kemiripan antara dua vektor yang bukan nol. Dalam konteks pemfilteran berbasis konten, setiap anime dapat direpresentasikan sebagai sebuah vektor, di mana setiap dimensi berhubungan dengan sebuah *term* (misalnya, genre, nama karakter, kata kunci plot) dan nilai dalam dimensi tersebut adalah nilai TF-IDF untuk *term* tersebut.

Untuk merekomendasikan judul anime kepada pengguna, sistem menghitung kemiripan kosinus antara vektor yang mewakili preferensi pengguna (berdasarkan anime yang telah mereka tonton atau nilai) dan vektor yang mewakili setiap anime dalam database. Nilai kemiripan kosinus berkisar dari 0 (sama sekali tidak berbeda) hingga 1 (vektor yang identik).

Sistem rekomendasi kemudian mengurutkan judul-judul anime berdasarkan nilai kemiripan kosinus dengan vektor preferensi pengguna. Anime dengan nilai kemiripan kosinus yang lebih tinggi dianggap lebih relevan dan direkomendasikan kepada pengguna.

Berikut adalah rumus untuk menghitung cosine similarity antara dua vektor $A$ dan $B$:

$$ \text{Cosine Similarity} = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|}

= \frac{\sum_{i=1}^{n} A_i B_i}{\sqrt{\sum_{i=1}^{n} A_i^2} \sqrt{\sum_{i=1}^{n} B_i^2}} $$

Dimana:
- $\mathbf{A} \cdot \mathbf{B}$ adalah hasil perkalian titik antara vektor $A$ dan $B$.
- $\|\mathbf{A}\|$ dan $\|\mathbf{B}\|$ adalah norma dari vektor $A$ dan $B$.

#### Kelebihan dan Kekurangan Content-Based Filtering

Kelebihan dari content-based filtering adalah:
- Penanganan item baru: Pemfilteran berbasis konten dapat secara efektif menangani item baru, karena tidak bergantung pada data pengguna atau teknik pemfilteran kolaboratif. Hal ini memungkinkannya untuk memberikan rekomendasi untuk judul anime baru yang mungkin belum pernah dinilai atau ditonton oleh pengguna.
- Mengurangi masalah cold-start: Karena pemfilteran berbasis konten tidak bergantung pada data pengguna, pemfilteran ini dapat memberikan rekomendasi meskipun data pengguna yang tersedia terbatas, sehingga mengurangi masalah cold-start.
- Explainability: Pemfilteran berbasis konten dapat memberikan penjelasan untuk rekomendasinya, karena didasarkan pada fitur eksplisit dari konten. Hal ini dapat membantu pengguna memahami mengapa rekomendasi tertentu dibuat dan meningkatkan kepercayaan mereka pada sistem.

Sedangkan, kekurangan dari content-based filtering adalah:
- Kurangnya unsur kebetulan: Karena penyaringan berbasis konten bergantung pada fitur eksplisit dari konten, penyaringan ini mungkin tidak dapat menangkap rekomendasi yang tidak terduga atau tidak disengaja yang dapat ditemukan melalui teknik penyaringan kolaboratif.
- Skalabilitas terbatas: Pemfilteran berbasis konten dapat menjadi mahal secara komputasi ketika berurusan dengan kumpulan data yang besar, karena memerlukan pemrosesan dan analisis fitur setiap item.
- Penanganan preferensi pengguna yang terbatas: Pemfilteran berbasis konten mungkin tidak dapat secara efektif menangkap nuansa dalam preferensi pengguna, seperti perubahan preferensi dari waktu ke waktu atau pengaruh faktor sosial.

#### Implementasi Content-Based Filtering

TODO: next

### Collaborative Filtering

#### XXXXXx

#### Kelebihan dan Kekurangan Collaborative Filtering

#### Implementasi Collaborative Filtering



## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.

## References

[1] S. K. Raghuwanshi dan R. K. Pateriya, “Collaborative Filtering Techniques in Recommendation Systems,” dalam Data, Engineering and Applications, R. K. Shukla, J. Agrawal, S. Sharma, dan G. Singh Tomer, Ed., Singapore: Springer Singapore, 2019, hlm. 11–21. doi: 10.1007/978-981-13-6347-4_2.
[2] H. D. Putri dan M. Faisal, “Analyzing the Effectiveness of Collaborative Filtering and Content-Based Filtering Methods in Anime Recommendation Systems,” JKKI, vol. 7, no. 2, hlm. 124–133, Nov 2023, doi: 10.31603/komtika.v7i2.9219.

