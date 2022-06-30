

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

5. Perbaharui nilai centroid baru yang didapatkan dari rata-rata klaster yang bersangkutan

6. Lakukan langkah ke-3 sampai 5, hingga tiap cluster tidak mengalami perubahan anggota lagi.



## Code Clustering K-Means

Adapun code keseluruhan untuk melakukan clustering dengan k-means adalah sebagai berikut.

```python
# ------ Import Library --------
import pandas as pd
import numpy as np
import string
import re
import nltk
import swifter

from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.cluster import KMeans
```

```python
# ------ Load Data --------
data = pd.read_csv("ta-manajemen.csv")
data
```

```python
# ------ Case Folding --------
# gunakan fungsi Series.str.lower() pada Pandas
data['abstrak_lowercase'] = data['abstraksi'].str.lower()
print('Case Folding Result : \n')
print(data['abstrak_lowercase'].head(5))
```

```python
# ------ Tokenizing ---------

def remove_desc_special(abstrak):
    # hapus tab, baris baru, dan irisan belakang
    abstrak = abstrak.replace('\\t'," ").replace('\\n'," ").replace('\\u'," ").replace('\\',"")
    # hapus non ASCII (emotikon, kata cina, .etc)
    abstrak = abstrak.encode('ascii', 'replace').decode('ascii')
    # hapus mention, link, tagar
    abstrak = ' '.join(re.sub("([@#][A-Za-z0-9]+)|(\w+:\/\/\S+)"," ", abstrak).split())
    # hapus URL
    return abstrak.replace("http://", " ").replace("https://", " ")
                
# hapus angka
def remove_number(abstrak):
    return  re.sub(r"\d+", "", abstrak)

# hapus tanda baca, simbol
def remove_punctuation(abstrak):
    return abstrak.translate(str.maketrans("","",string.punctuation))

# hapus spasi di awal & akhir
def remove_whitespace_LT(abstrak):
    return abstrak.strip()

# hapus beberapa spasi putih menjadi spasi tunggal
def remove_whitespace_multiple(abstrak):
    return re.sub('\s+',' ',abstrak)

# hapus char tunggal
def remove_singl_char(abstrak):
    return re.sub(r"\b[a-zA-Z]\b", "", abstrak)

# NLTK word_tokenize 
def word_tokenize_wrapper(abstrak):
    return word_tokenize(abstrak)

data['abstrak_lowercase'] = data['abstrak_lowercase'].apply(remove_desc_special)
data['abstrak_lowercase'] = data['abstrak_lowercase'].apply(remove_number)
data['abstrak_lowercase'] = data['abstrak_lowercase'].apply(remove_punctuation)
data['abstrak_lowercase'] = data['abstrak_lowercase'].apply(remove_whitespace_LT)
data['abstrak_lowercase'] = data['abstrak_lowercase'].apply(remove_whitespace_multiple)
data['abstrak_lowercase'] = data['abstrak_lowercase'].apply(remove_singl_char)
data['abstrak_tokens'] = data['abstrak_lowercase'].apply(word_tokenize_wrapper)

print('Tokenizing Result : \n') 
print(data['abstrak_tokens'].head())
```

```python
# ------ Stopwords Removal ---------

# ambil list stopword indonesia
list_stopwords = stopwords.words('indonesian')

# konversi string stopword ke daftar & tambahkan stopword
list_stopwords = set(list_stopwords)

# hapus stopword yang ada pada list
def stopwords_removal(words):
    return [word for word in words if word not in list_stopwords]

data['abstrak_stopwords'] = data['abstrak_tokens'].apply(stopwords_removal) 

print('Stopwords Result : \n') 
print(data['abstrak_stopwords'].head())
```

```python
# ------ Stemming ---------
# buat stemmer
factory = StemmerFactory()
stemmer = factory.create_stemmer()

def stemmed(term):
    return stemmer.stem(term)

term_dict = {}

for abstrak in data['abstrak_stopwords']:
    for term in abstrak:
        if term not in term_dict:
            term_dict[term] = ' '

#hitung berapa jumlah kata yang didapat
print(len(term_dict))
print("------------------------")

for term in term_dict:
    term_dict[term] = stemmed(term)
    print(term,":" ,term_dict[term])
    
print(term_dict)
print("------------------------")

# terapkan get_stemmed_term() ke atribut deskripsi dalam Dataframe dengan swifter
def get_stemmed_term(desc):
    return [term_dict[term] for term in desc]

data['abstrak_stemming'] = data['abstrak_stopwords'].swifter.apply(get_stemmed_term)
print(data['abstrak_stemming'])
```

```python
# ------ TF-IDF ---------
vectorizer = TfidfVectorizer(stop_words='english')
ta_mj = []
for data in data['abstrak_stemming']:
    content = ''
    for term in data:
        content += term + ' '
    ta_mj.append(content)

vectorizer.fit(ta_mj)
vector = vectorizer.fit_transform(ta_mj)
print(vector)
```

```python
# ------ Clustering K-Means ---------
true_k = 5
model = KMeans(n_clusters=true_k, init='k-means++', max_iter=100, n_init=1)
model.fit(vector)

print("Top terms per cluster:")
order_centroids = model.cluster_centers_.argsort()[:, ::-1]
terms = vectorizer.get_feature_names()
for i in range(true_k):
    print("Cluster %d:" % i),
    for ind in order_centroids[i, :10]:
        print(' %s' % terms[ind]),
    print
```




## 
