# Laporan Proyek Machine Learning - Muhamad Dani

## Project Overview

Perusahaan Adidas, membutuhkan sistem rekomendasi produk untuk *platform* *e-commerce* mereka. Kondisi *platform* masih baru dan penggunannya masih sedikit, sehingga untuk mengatasi kondisi *cold start* tersebut sistem rekomendasi yang sesuai yaitu *content-based filtering*. Sistem rekomendasi yang dibuat akan merokemendasikan produk Adidas berdasarkan kategori .

Guo, Lei, et al melakukan penelitian tentang *Streaming Session-based Recommendation*  untuk *fashion e-commerce* menggunakan metode *Matrix Factorization (MF)* [[1]](https://drive.google.com/file/d/19fGCgjWe_3BWY8Ra5ojWFWWadTrQMlQj/view). Sejalan dengan hal tersebut, solusi yang ditawarkan pada kasus ini adalah sistem rekomendasi (*content based filtering*) dengan menggunakan metode *Cosine Similarity* dan *Euclidean Similarity* dengan metrik yang digunakan metrik *precision*.

## Business Understanding
### Problem Statements

Bagaimana cara membuat sistem rekomendasi dengan metode terbaik untuk rekomendasi produk Adidas berdasarkan kategori atau subkategorinya?

### Goals

Mengetahui cara membuat sistem rekomendasi dengan metode terbaik untuk rekomendasi produk Adidas berdasarkan kategori.

### Solution Statements

Menawarkan solusi sistem rekomendasi dengan jenis *content based filtering*  yang menghasilkan rekomendasi produk Adidas berdasarkan kategori. Untuk mendapatkan solusi terbaik, akan digunakan dua model (metode) yang berbeda yaitu *Cosine Similarity* dan *Euclidean Similarity*. Selain itu, untuk mengukur kinerja model akan digunakan metrik *Precision* dan lama waktu komputasi setiap modelnya.

## Data Understanding

Tabel 1. Informasi Dataset


| | Keterangan |
|---|---|
| Sumber | [Kaggle - Adidas Product Dataset](https://www.kaggle.com/datasets/thedevastator/adidas-fashion-retail-products-dataset-9300-prod) |
| Jumlah Data | 845 |
| *Usability* | 10.00 |
| Lisensi | *Unknown* |
| *Rating* | *None* |
| Jenis dan Ukuran Berkas | csv (1.45 MB) |

### Deskripsi Variabel

Berdasarkan informasi dari sumber dataset, variabel-variabel pada dataset adalah sebagai berikut:

- url           : URL halaman produk di situs web sumber. 
- name          : Nama produk
- sku	        : SKU atau pengidentifikasi unik untuk produk.
- selling_price	: Harga jual dalam bentuk USD atau Euros.
- original_price: Harga Original dalam bentuk USD atua Euros
- currency      : Jenis mata uang untuk harga jual dan harga asli.
- availability  : Ketersediaan produk.
- color         : Warna produk.
- category      : Kategori produk.
- source_website: Situs sumber dari mana data dikumpulkan.
- breadcrumbs   : Subkategori produk.
- description   : Penjelasan singkat tentang produk yang disediakan oleh Adidas.
- brand         : Merek produk.
- images        : Beberapa gambar produk disediakan oleh Adidas.
- country       : Negara asal/tujuan produk.
- language      : Bahasa yang digunakan untuk menampilkan halaman produk di situs web sumber.
- average_rating: Peringkat pelanggan rata-rata dari 5 bintang.
- reviews_count : Jumlah ulasan pelanggan untuk produk.
- crawled_at    : Tanggal dan waktu saat data dikumpulkan.

Berikut contoh lima data teratas pada dataset:

Tabel 2. Tampilan Lima Teratas pada Dataset

|level\_0|index|url|name|sku|selling\_price|original\_price|currency|availability|color|category|source|source\_website|breadcrumbs|description|brand|images|country|language|average\_rating|reviews\_count|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|0|https://www\.adidas\.com/us/beach-shorts/FJ5089\.html|Beach Shorts|FJ5089|40|NaN|USD|InStock|Black|Clothing|adidas United States|https://www\.adidas\.com|Women/Clothing|Splashing in the surf\. Making memories with your friends\. Beach days are the best days\. These shorts are made of stretchy woven fabric\. An elastic waistband that features the adidas logo brings a sporty look to your day at the beach\.|adidas|https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/adce4c860bd841e0a853aafd00b362d7\_9366/Beach\_Shorts\_Black\_FJ5089\_21\_model\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/cad3233f6c2445e1a7fdaafd00b371ac\_9366/Beach\_Shorts\_Black\_FJ5089\_22\_model\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/66223c9a595e40ef9c7caafd00b3809b\_9366/Beach\_Shorts\_Black\_FJ5089\_23\_hover\_model\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/85706d099dfc4092a373aafd00b38e36\_9366/Beach\_Shorts\_Black\_FJ5089\_25\_model\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/139affb83f16488cb899aafd00b3e2b9\_9366/Beach\_Shorts\_Black\_FJ5089\_01\_laydown\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/c235a4db563143b18db4aafd00b3ef38\_9366/Beach\_Shorts\_Black\_FJ5089\_02\_laydown\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/3020a9362134409a9995aafd00b399d2\_9366/Beach\_Shorts\_Black\_FJ5089\_25\_outfit\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/9a04933e87f24464b7b8aafd00b3a9aa\_9366/Beach\_Shorts\_Black\_FJ5089\_41\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/2e48f8751aa54d9f9719aafd00b3b971\_9366/Beach\_Shorts\_Black\_FJ5089\_42\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/5afa70cd99ce4e00abd9aafd00b3c915\_9366/Beach\_Shorts\_Black\_FJ5089\_43\_detail\.jpg|USA|en|4\.5|35|
|1|1|https://www\.adidas\.com/us/five-ten-kestrel-lace-mountain-bike-shoes/BC0770\.html|Five Ten Kestrel Lace Mountain Bike Shoes|BC0770|150|NaN|USD|InStock|Grey|Shoes|adidas United States|https://www\.adidas\.com|Women/Shoes|Lace up and get after it\. The Five Ten Kestrel Lace Mountain Bike Shoes offer efficient pedal power with low-profile style\. The wide platform is compatible with all clipless pedals and offers high-friction grip on and off the bike\. You'll find the find comfort and versatility for extended trail rides and afterwork hot laps alike\.|adidas|https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/2b04943c525e4909a7a5a9fa0116153d\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_01\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/64ef0f437ce249fe980da9fa01164284\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_02\_standard\_hover\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/12746d2167c348b2a583a9fa01169669\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_03\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/91b253099ece4b6c8b5fa9fa0116a5a1\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_04\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/a2b39ff910204553af50a9fa0116b3a0\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_05\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/9294030e3be54f83854aa9fa01162719\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_06\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/f2cc280368794b2c9a9ca9fa0116c094\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_41\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/39426096737d4102b7ada9fa0116cceb\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_42\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/9f384bc43cf84ce9845ca9fa0116d8b3\_9366/Five\_Ten\_Kestrel\_Lace\_Mountain\_Bike\_Shoes\_Grey\_BC0770\_43\_detail\.jpg|USA|en|4\.8|4|
|2|2|https://www\.adidas\.com/us/mexico-away-jersey/GC7946\.html|Mexico Away Jersey|GC7946|70|NaN|USD|InStock|White|Clothing|adidas United States|https://www\.adidas\.com|Kids/Clothing|Clean and crisp, this adidas Mexico Away Jersey for juniors displays a design that began life in an artist's studio\. Soft, quick-drying fabric keeps you comfortable\. Standing out on the chest, a team badge lets fans show pride in their soccer team\.|adidas|https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/4b12d5462aec410faee9ab1000feb34f\_9366/Mexico\_Away\_Jersey\_White\_GC7946\_01\_laydown\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/be2b4748ccd04e88b0e7ab1000fec081\_9366/Mexico\_Away\_Jersey\_White\_GC7946\_02\_laydown\_hover\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/e538f94d0fd742868052ab1000fecdbb\_9366/Mexico\_Away\_Jersey\_White\_GC7946\_41\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/7f1492f2985841dbb59cab1000fedc44\_9366/Mexico\_Away\_Jersey\_White\_GC7946\_42\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/d3485a6cd3fd42c19083ab1000fee858\_9366/Mexico\_Away\_Jersey\_White\_GC7946\_43\_detail\.jpg|USA|en|4\.9|42|
|3|3|https://www\.adidas\.com/us/five-ten-hiangle-pro-competition-climbing-shoes/FV4744\.html|Five Ten Hiangle Pro Competition Climbing Shoes|FV4744|160|NaN|USD|InStock|Black|Shoes|adidas United States|https://www\.adidas\.com|Five Ten/Shoes|The Hiangle Pro takes on the classic shape of the original Hiangle with the addition of a seamless outsole wrapping around the toes, allowing for maximum rubber contact when tackling the most challenging boulder problems\.|adidas|https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/a50bf634157248c99dbcac02007a8d9f\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_01\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/3c802934504e4b7fb3a3ac02007a9c73\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_02\_standard\_hover\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/862a51ddd0df402e94e7ac02007aa392\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_03\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/a7f417bf88b44b07a792ac02007aab1d\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_04\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/18556e5bfa6d4438b363ac02007ab2a4\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_05\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/a78cc40417304c9ba71eac02007a9527\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_06\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/3bb67ff00e764e0ba5e8ac020080e925\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_09\_standard\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/cfbc398cd1fe4e6eb283ac02007aba39\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_41\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/2a09443d57c344719997ac02007ac15c\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_42\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/9f8beb8acfcb4d40a540ac02007ac719\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_43\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/2cef972779b941fa877eac8d011dca97\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_HM1\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/9bbdc255ddd74d7cbe45ac8d011dcc07\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_HM2\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/8d704616e0fa45a3ac06ac8d011dca28\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_HM3\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/c6221d2435b8455eaa7aac8d011dcc7a\_9366/Five\_Ten\_Hiangle\_Pro\_Competition\_Climbing\_Shoes\_Black\_FV4744\_HM4\.jpg|USA|en|3\.7|7|
|4|4|https://www\.adidas\.com/us/mesh-broken-stripe-polo-shirt/GM0239\.html|Mesh Broken-Stripe Polo Shirt|GM0239|65|NaN|USD|InStock|Blue|Clothing|adidas United States|https://www\.adidas\.com|Men/Clothing|Step up to the tee relaxed\. This adidas golf polo shirt lets you focus on accurate drives and putts\. Four-way stretch matches the body's natural movements\. Breathable fabric keeps you comfortable spring, summer and fall\. Recycled materials are one step towards reducing plastic waste\.|adidas|https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/0ca38931b0cb4d14a3daac440068e9bd\_9366/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_21\_model\.jpg~https://assets\.adidas\.com/videos/w\_600,f\_auto,q\_auto/1bbe25dd08504d208e00ac67008260b1\_d98c/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_video\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/88c92628bf8045dc8976ac4400689326\_9366/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_23\_hover\_model\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/c000838ecb3a481a8d53ac44006a5c86\_9366/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_25\_model\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/217f93c3862e45648316ac5d011fe2b8\_9366/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_01\_laydown\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/7978441ffc7d4852bc2fac44006cd80e\_9366/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_41\_detail\.jpg~https://assets\.adidas\.com/images/w\_600,f\_auto,q\_auto/1e31163790704db59eefac44006c056c\_9366/Mesh\_Broken-Stripe\_Polo\_Shirt\_Blue\_GM0239\_42\_detail\.jpg|USA|en|4\.7|11|

### Menentukan Fitur yang Akan Digunakan

Fitur yang akan digunakan adalah `nama`, `category` dan `breadcrumbs`. Pada kasus ini kita lebih mengutamakan penggunaan `breadcrumbs` daripada `category` karena memiliki nilai yang lebih variatif.

Tabel 3. Jumlah Banyak *Data Point*

| Fitur | Jumlah |
|---|:---:|
| Produk Adidas | 431 |
| Kategori | 3 |
| Sub Kategori | 22 |

Tabel 4. Tampilan Lima Teratas pada Dataset setelah Pemilihan Fitur

|index|sku|name|breadcrumbs|
|---|---|---|---|
|0|FJ5089|Beach Shorts|Women/Clothing|
|1|BC0770|Five Ten Kestrel Lace Mountain Bike Shoes|Women/Shoes|
|2|GC7946|Mexico Away Jersey|Kids/Clothing|
|3|FV4744|Five Ten Hiangle Pro Competition Climbing Shoes|Five Ten/Shoes|
|4|GM0239|Mesh Broken-Stripe Polo Shirt|Men/Clothing|

## Data Preparation
### Menangani Missing Value

Untuk mendeteksi *missing value* digunakan fungsi isnull().sum() dan untuk mendeteksi nilai NAN digunakan isna().sum().

Tabel 5. Hasil Deteksi *Missing Value*

| Fitur | Jumlah *Missing Value* |
|---|:---:|
| sku   |   0 |
| name  |   0 |
| breadcrumbs     |   0 |

Tabel 6. Hasil Deteksi Nilai NAN

| Fitur | Jumlah Nilai NAN |
|---|:---:|
| sku   |   0 |
| name  |   0 |
| breadcrumbs     |   0 |

Dari Tabel 5. dan Tabel 6. terlihat bahwa setiap fitur tidak memiliki Missing Value (NULL) maupun Nilai NAN sehingga dapat dilanjutkan ke tahapan selanjutnya yaitu menghilangkan data duplikat.

### Menghilangkan data Duplikat

Pada tahap ini, akan dilakukan penghapusan data yang duplikat berdasarkan nama produk. Tujuannya adalah agar sistem rekomendasi yang dibuat dapat menghasilkan rekomendasi yang berbeda tanpa ada nama produk yang muncul lebih dari satu kali. Berikut perbandingan banyak data sebelum dan sesudah penghapusan data duplikat.

Tabel 7. Perbandingan Banyak Data Sebelum dan Sesudah Penghapusan Data Duplikat

| Banyak Data (*Before*) | Banyak Data (*After*) |
|:---:|:---:|
| 845 | 431 |

## Modelling

Pada tahap ini, untuk sistem rekomendasi yang dibuat akan menggunakan model dengan metode Cosine Similarity dan Euclidean Similarity. Namun sebelum itu akan dilakukan perubahan tipe data dari kategorikal menjadi data numerik menggunakan metode `TF-IDF Vectorizer`.

### TF-IDF Vectorizer

Pada tahap ini akan dilakukan ekstraksi fitur sekaligus menyeleksi fitur mana saja yang sering muncul dan jarang muncul. Berikut daftar fitur hasil `TF-IDF Vectorizer`.

Tabel 8. Daftar Fitur Hasil `TF-IDF Vectorizer`

| Nama Fitur |
|---|
| 'accessories' |
| 'clothing' |
| 'essentials' |
| 'five' |
| 'kids' |
| 'men' |
| 'originals' |
| 'running' |
| 'shoes' |
| 'soccer' |
| 'sportswear' |
| 'swim' |
| 'ten' |
| 'training' |
| 'women' |
| 'jackets' |

Kemudian, dilakukan *one-hot-encoding* pada setiap produk terhadap fitur-fitur yang dihasilkan oleh `TF-IDF Vectorizer`. Berikut matrik TF-IDF yang dihasilkan.

Tabel 9. Matrik TF-IDF dengan 10 Sampel pada Kolom dan 5 Sampel pada Baris

|name|accessories|soccer|five|running|women|training|essentials|swim|originals|ten|
|---|---|---|---|---|---|---|---|---|---|---|
|Court Rallye Slip Shoes|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.9086068503703572|0\.0|
|Adilette Slides|0\.0|0\.0|0\.0|0\.0|0\.7463816566645045|0\.0|0\.0|0\.0|0\.0|0\.0|
|Racer TR21 Wide Shoes|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Climacool Vento Shoes|0\.0|0\.0|0\.0|0\.960282774140845|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Essentials Logo Hoodie \(Gender Neutral\)|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.9607642954712873|0\.0|0\.0|0\.0|


### Cosine Similarity

Rumus *Cosine Similarity* didefinisikan sebagai berikut [[2]](http://www.snet.tu-berlin.de/fileadmin/fg220/courses/SS11/snet-project/recommender-systems_asanov.pdf):
$$\text{S}_{\text{Cosinus}}(\vec{x},\vec{y})=\cos(\theta)=\frac{\vec{x}\cdot\vec{y}}{\|\vec{x}\|\|\vec{y}\|}$$
Dengan:
* $\theta$ merupakan sudut antara vektor $\vec{x}$ dan $\vec{y}$.
* $\|\vec{x}\|$ dan $\|\vec{y}\|$ berturut-turut adalah besar atau panjang vektor $\vec{x}$ dan $\vec{y}$. 

Catatan: 
$$\|\vec{x}\|=\sqrt{\sum_{i=1}^n {x_{i}^2}}$$
dimana $x_{i}$ merupakan elemen vektor $\vec{x}$ posisi ke $i$.

Kelebihan metode ini adalah tidak bergantung pada besarnya vektor. Contoh vektor $\vec{x}=[2, 0, 4]$ dengan vektor $\vec{y}=[1, 0, 2]$ yang memiliki arah yang sama namun berbeda besarannya (akibat berbeda nilai pada fiturnya). Jika vektor $\vec{x}$ dan $\vec{y}$ dihitung tingkat kemiripan atau relevansi dengan metode ini maka nilainya $1$ (kemiripan penuh). Namun kelebihan ini dapat menjadi kekurangan jika pada kasus tertentu, makna frekuensi kemunculan fitur menjadi penting. Sedangkan pada kasus ini, *Cosine Similarity* aman digunakan karena sudah dilakukan tahap *one-hot-encoding* pada matrik tfidf. Sehingga frekuensi tiap kategori pada produk mempunyai bobot yang sama yaitu 0 (tidak ada) atau 1 (ada).

Untuk implementasinya menggunakan fungsi `cosine_similarity()` dari *library* sklearn dengan lama waktu komputasinya sebagai berikut.
```
Execution Time Cosine Similarity (Seconds) : 0.019988536834716797
```

Dengan hasil matrik *Cosine Similarity* nya sebagai berikut.

Tabel 10. Matrik *Cosine Similarity* dengan 5 Sampel pada Kolom dan 10 Sampel pada Baris

|name|adidas SPRT Logo Tee|Superstar Shoes|Continental 80 Stripes Shoes|Ultimate365 3-Stripes Polo Shirt|EQ21 Run Shoes|
|---|---|---|---|---|---|
|Techfit Tights|0\.35484306292935686|0\.0|0\.0|0\.35484306292935686|0\.0|
|Ownthegame Shoes|0\.5725404475889482|0\.2624339518853438|0\.4181815521225731|0\.5725404475889482|1\.0|
|4D Fusio Shoes|0\.0|1\.0000000000000002|0\.27795530835488785|0\.0|0\.2624339518853438|
|FortaRun Lace Running Shoes|0\.0|0\.19856948266164054|0\.31641521177831505|0\.0|0\.2987462083566319|
|ZX 360 Shoes|0\.0|0\.19856948266164054|0\.31641521177831505|0\.0|0\.2987462083566319|
|Trefoil Backpack|0\.0|0\.6730736529330555|0\.0|0\.0|0\.0|
|NMD\_R1 Spectoo Shoes|0\.0|0\.27795530835488785|1\.0|0\.0|0\.4181815521225731|
|X9000L4 Shoes|0\.5725404475889482|0\.2624339518853438|0\.4181815521225731|0\.5725404475889482|1\.0|
|Postmove Shoes|0\.5725404475889482|0\.2624339518853438|0\.4181815521225731|0\.5725404475889482|1\.0|
|Cloudfoam Pure 2\.0  Shoes|0\.0|0\.27795530835488785|1\.0|0\.0|0\.4181815521225731|

### Euclidean Similarity

*Euclidean* Distances didefinisikan sebagai berikut:

$$ \text{D}_{\text{Euclidean}}(\vec{x},\vec{y})=\sqrt{\sum^{n}_{i=1}(x_{i}-y_{i})^2} $$

Dengan $x_{i}$ dan $y_{i}$ berturut-turut adalah elemen ke $i$ pada vektor $\vec{x}$ dan $\vec{y}$.

Dalam buku berjudul `Programming Collective Intelligence` karya Tobby Segaran (2007) mendefinisikan persamaan *Euclidean Similarity* sebagai berikut [[3]](https://www.oreilly.com/library/view/programming-collective-intelligence/9780596529321/):

$$ \text{S}_{\text{Euclidean}}(\vec{x},\vec{y})=\frac{1}{1+\text{D}_{\text{Euclidean}}(\vec{x}, \vec{y})} $$

Kelebihan Euclidean adalah dapat memperoleh nilai perbedaan antara dua vektor yang sama arahnya namun beda besarannya. Sedangkan kekurangan algoritma ini adalah fitur dengan frekuensi kemunculan paling banyak akan mendominasi fitur lain dalam hasil komputasi jarak euclideannya.

Untuk implementasinya menggunakan fungsi `euclidean_distances()` dari *library* sklearn dengan lama waktu komputasinya sebagai berikut.

```
Execution Time Euclidean Similarity (Seconds) : 0.029959440231323242
```

Dengan hasil matrik *Euclidean Similarity* nya sebagai berikut.

Tabel 11. Matrik *Euclidean Similarity* dengan 5 Sampel pada Kolom dan 10 Sampel pada Baris

|name|Runner Tee|ZX 1K Boost Shoes|Logo Play Tee|AEROREADY Designed 2 Move Feelready Sport Long Sleeve Tee|ZX 2K Boost Pure Shoes|
|---|---|---|---|---|---|
|4ATHLTS Backpack|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Originals Sunglasses OR0032|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|adidas x Zoe Saldana Feelbrilliant AEROREADY 3/4 Printed Sport Tights|0\.495667351195943|0\.4142135623730951|1\.0|0\.495667351195943|0\.5060730963343802|
|Bryony Shoes|0\.4142135623730951|0\.4609866387789766|0\.5060730963343802|0\.4142135623730951|1\.0|
|Real Madrid Tee|0\.4681823648429434|0\.585408932084946|0\.47182756475351967|0\.4681823648429434|0\.4142135623730951|
|QT Racer 2\.0 Shoes|0\.4142135623730951|0\.4609866387789766|0\.5060730963343802|0\.4142135623730951|1\.0|
|Court Rallye Slip Shoes|0\.4142135623730951|0\.4412981879858138|0\.4142135623730951|0\.4142135623730951|0\.4541939287373496|
|Nizza Platform Shoes|0\.4142135623730951|0\.4609866387789766|0\.5060730963343802|0\.4142135623730951|1\.0|
|Terrex Trailmaker GORE-TEX Hiking Shoes|0\.4142135623730951|0\.4609866387789766|0\.5060730963343802|0\.4142135623730951|1\.0|
|Postmove Shoes|0\.4142135623730951|0\.4609866387789766|0\.5060730963343802|0\.4142135623730951|1\.0|
### Mendapatkan Rekomendasi

Pada tahap ini akan dilakukan pengujian hasil top-10 rekomendasi produk Adidas. Kemudian untuk sampel yang dipilih adalah sebagai berikut.

Tabel 12. Sampel Produk Adidas yang Digunakan 

|index|sku|name|breadcrumbs|
|---|---|---|---|
|761|GR4259|Real Madrid Tee|Kids/Clothing|

#### Rekomendasi dengan Cosine Similarity

Tabel 13. Top-10 Rekomendasi dengan Cosine Similarity

|index|name|sku|breadcrumbs|
|---|---|---|---|
|0|Graphic Tee and Shorts Set|EX3625|Kids/Clothing|
|1|Camo-Print SST Top|H20311|Kids/Clothing|
|2|Camo-Print Hoodie|H20312|Kids/Clothing|
|3|Marimekko Techfit Primegreen AEROREADY Training Pocket Tights|GV2052|Kids/Clothing|
|4|Techfit Tights|EY1067|Kids/Clothing|
|5|Techfit Tights|EY1068|Kids/Clothing|
|6|Techfit Tights|EY0319|Kids/Clothing|
|7|Techfit Tights|EY1066|Kids/Clothing|
|8|Techfit Tights|EY1067|Kids/Clothing|
|9|Techfit Tights|EY1068|Kids/Clothing|

Pada Tabel 13. di atas, terlihat bahwa untuk 10 hasil rekomendasi dengan *Cosine Similarity* mempunyai kategori yang sama dengan sampel yaitu `Kids/Clothing` sehingga top-10 rekomendasi di atas relevan dengan sampel.

#### Rekomendasi dengan Euclidean Similarity

Tabel 14. Top-10 Rekomendasi dengan Euclidean Similarity

|index|name|sku|breadcrumbs|
|---|---|---|---|
|0|Graphic Tee and Shorts Set|EX3625|Kids/Clothing|
|1|Camo-Print SST Top|H20311|Kids/Clothing|
|2|Camo-Print Hoodie|H20312|Kids/Clothing|
|3|Marimekko Techfit Primegreen AEROREADY Training Pocket Tights|GV2052|Kids/Clothing|
|4|Techfit Tights|EY1067|Kids/Clothing|
|5|Techfit Tights|EY1068|Kids/Clothing|
|6|Techfit Tights|EY0319|Kids/Clothing|
|7|Techfit Tights|EY1066|Kids/Clothing|
|8|Techfit Tights|EY1067|Kids/Clothing|
|9|Techfit Tights|EY1068|Kids/Clothing|

Pada Tabel 14. di atas, terlihat bahwa untuk 10 hasil rekomendasi dengan *Euclidean Similarity* mempunyai kategori yang sama dengan sampel yaitu `Kids/Clothing` sehingga top-10 rekomendasi di atas relevan dengan sampel.

## Evaluasi

$$\text{Recommender system precision (P)} = \frac{\text{\#of our recommendation that relevant}}{\text{\#of item we recommend}}\times 100% $$

Dari hasil rekomendasi di atas, dapat diketahui bahwa `Real Madrid Tee` termasuk ke dalam kategori `Kids/Clothing`. Dari 10 produk yang direkomendasikan, berikut nilai _precision_ pada model _cosine similarity_ dan _euclidean distance_.

Tabel 15. Komparasi Metrik *Precision*

|Model | Sesuai | Tidak Sesuai |Total| Precision |
|---|---|---|---|---|
|_Cosine Similarity_|10|0|10|100%|
|_Euclidean Similarity_|10|0|10|100%|
 
Pada tabel di atas, terlihat bahwa model *Cosine Similiarity* dan *Euclidean Distance* memiliki nilai _precision_ yang sama pada top-10 rekomendasi di atas.

Selain dari nilai _precision_, lama komputasi setiap metode juga perlu dipertimbangkan. Berikut perbandingannya:

Tabel 16. Komparasi Waktu Komputasi

|index|Cosine Similarity|Euclidean Similarity|
|---|---|---|
|Time \(Seconds\)|0\.019988536834716797|0\.029959440231323242|

Berdasarkan output di atas, waktu komputasi pada metode Cosine Similarity (0.03885 detik) lebih cepat dibandingkan Euclidean Similarity (0.11328 detik).

Jadi dapat disimpulkan bahwa model terbaik untuk sistem rekomendasi produk Adidas adalah model dengan metode *Cosine Similarity*.

## Daftar Referensi
[1] AGuo, Lei, et al. "Streaming session-based recommendation." Proceedings of the 25th ACM SIGKDD international conference on knowledge discovery & data mining. 2019. Tersedia: [tautan](https://drive.google.com/file/d/19fGCgjWe_3BWY8Ra5ojWFWWadTrQMlQj/view).  
[2] Melville, Prem, and Vikas Sindhwani. "Recommender systems." Encyclopedia of machine learning 1 (2010): 829-838. Tersedia: [tautan](http://www.snet.tu-berlin.de/fileadmin/fg220/courses/SS11/snet-project/recommender-systems_asanov.pdf).  
[3] Segaran, Toby. "Programming Collective Intelligence". O'Reilly Media, Inc. 2007. Tersedia: [O'Reilly Media](https://www.oreilly.com/library/view/programming-collective-intelligence/9780596529321/).