# Clustering

## Apa itu Clustering?

Clustering merupakan bagian dari unsupervised learning karena datanya tidak berlabel seperti  pada data classification. Clustering digunakan untuk pengklasteran data atau juga sering  digunakan untuk segmentasi data. Cluster adalah pengelompokan data yang memiliki  kemiripan atribut dalam satu kelompok dan berbeda dengan atribut data di kelompok lain. Data  yang memiliki kemiripan atribut tersebut akan berkumpul dan membentuk satu wilayah yang  sama, lalu data dengan ciri atribut yang berbeda akan bergabung di wilayah lainnya.



## Tipe Model Clustering

- Partition-based Clustering

  Pengklasteran data dengan membentuk bagian-bagian yang  lebih kecil. Model algoritma ini kerap kali digunakan karena efisien dan cukup baik untuk  pengklasteran data dengan jumlah sedang sampai besar.

- Hierarchical Clustering

  Pengelompokan data menjadi pohon-pohon klaster. Model  algoritma ini sangat intuitif, oleh karena itu hanya bisa digunakan untuk data dengan  jumlah sedikit.

- Density-based Clustering

  Pengelompokan data terjadi sesuai dengan kehendak algoritma  ini sendiri. Model algoritma ini berjalan dengan baik walaupun menemukan noise dalam  data.



## Algoritma Clustering

- K-Means
- K-Median
- Fuzzy c-Means
- Agglomerative
- Divisive
- DBSCAN



## K-Means

K-Means adalah salah satu algoritma clustering yang mengklasterkan data ke dalam **k** klaster. Untuk melakukan clustering ini, nilai **k** harus ditentukan di awal. Untuk menentukan nilai **k** tersebut bisa secara bebas atau menggunakan ukuran perbedaan untuk mengelompokkan obyek kita. Perbedaan bisa diartikan sebagai jarak antar data. Jika jarak dua data atau memeiliki titik cukup dekat, maka dua data tersebut mirip. Semakin dekat jaraknya berarti semakin tinggi kemiripannya. Semakin jauh jaraknya berarti semakin tinggi ketidak miripannya.



## Langkah-Langkah melakukan Clustering K-Means

1. Tentukan **k** (nilainya dipilih secara bebas) sebagai jumlah cluster yang ingin dibentuk.

2. Inisialisasi nilai random untuk pusat cluster awal (centroid) sebanyak **k**.

3. Hitung jarak setiap data dengan masing–masing centroid, hingga ditemukan jarak yang paling dekat dari setiap data dengan centroid.

4. Kelompokkan setiap data berdasarkan kedekatannya dengan centroid (jarak terkecil).

5. Perbaharui nilai centroid baru yang didapatkan dari rata-rata klaster yang bersangkutan dengan menggunakan rumus:

   

6. Lakukan langkah ke-3 sampai 5, hingga tiap cluster tidak mengalami perubahan anggota lagi.



## Code Topic Modelling Latent Semantic Analysis (LSA)

Adapun code keseluruhan untuk melakukan topic modelling Latent Semantic Analysis (LSA) adalah sebagai berikut.

```python
# ------ Import Library --------
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.decomposition import TruncatedSVD
```

```python
# ------ TF-IDF --------
vect = TfidfVectorizer(stop_words=list_stopwords,max_features=1000)
vect_text = vect.fit_transform(papers['Abstrak_Lowercase'])
print(vect_text.shape)
print(vect_text)
```

```python
# ------ Singular Value Decomposition (SVD) --------
lsa_model = TruncatedSVD(n_components=10, algorithm='randomized', n_iter=10, random_state=42)
lsa_top=lsa_model.fit_transform(vect_text)
print(lsa_top)
print(lsa_top.shape)
```

```python
# ------ Menampilkan hasil topik --------
l=lsa_top[0]
print("Document 0 :")
for i,topic in enumerate(l):
  print("Topic ",i," : ",topic*100)
```

```python
# ------ Menampilkan nilai komponen tiap topik --------
print(lsa_model.components_.shape)
print(lsa_model.components_)
```

```python
# ------ Menampilkan kata penting tiap topik --------
vocab = vect.get_feature_names_out()

for i, comp in enumerate(lsa_model.components_):
    vocab_comp = zip(vocab, comp)
    sorted_words = sorted(vocab_comp, key= lambda x:x[1], reverse=True)[:10]
    print("Topic "+str(i)+": ")
    for t in sorted_words:
        print(t[0],end=" ")
    print("\n")
```




## Referensi

```{bibliography}

```
