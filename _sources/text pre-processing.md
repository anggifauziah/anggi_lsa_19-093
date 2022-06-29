# Text Pre-Processing

## Apa itu Text Pre-Processing?

Text pre-processing adalah suatu teknik untuk memproses informasi teks menjadi bentuk teks yang lebih terstruktur dan mudah untuk dilakukan proses pengolahan selanjutnya. Tahap tekxt pre-processing merupakan bagian yang paling penting dalam pengolahan data, hal ini dikarenakan data yang didapatkan tidak semuanya berupa data yang sudah sempurna, oleh karena itu dilakukan text pre-processing.

Banyak sekali proses dari text pre-processing, tetapi tidak pernah ada aturan mutlak yang mengharuskan untuk menggunakan semua proses tersebut. Text pre-processing dapat digunakan sesuai dengan kebutuhan pada data yang dimiliki.



## Macam-macam proses Text Pre-processing

1. Case Folding

   Proses yang digunakan untuk mengubah atau menyamakan format dari data. Seringkali yang dipakai untuk menyamakan format data pada case folding yaitu lowercase (format huruf kecil).

2. Remove symbols, number, punctuation

   Proses yang digunakan untuk menghapus simbol, angka, dan juga tanda baca yang tidak akan berpengaruh pada proses pengolahan data. Menghapus simbol tersebut dapat membantu proses pengolahan data menjadi lebih cepat karena data tidak lagi kotor dengan simbol.

3. Tokenizing

   Proses yang digunakan untuk membuat suatu kalimat menjadi potongan-potongan kata yang biasa disebut token kata. Hal ini berguna untuk pengolahan data selanjutnya, karena kata sudah dipecah menjadi satuan kata sendiri.

4. Stopword Removal

   Proses yang digunakan untuk membuang kata yang tidak mempunyai makna atau tidak penting. Maksud dari tidak mempunyai makna yaitu seperti pada kata aku, kamu, dia, dan lain-lain.

5. Stemming

   Proses yang digunakan untuk mengubah kata menjadi kata dasar yang baku. Kata yang dihasilkan pada proses stemming ini haruslah berupa kata dasar yang sesuai dengan ejaan pada Kamus Besar Bahasa Indonesia (KBBI).



## Langkah-langkah melakukan Text Pre-processing

Pada pembahasan kali ini text pre-processing yang dilakukan hanya terfokus pada **case folding, remove symbols, dan stopword removal**. Adapun langkah-langkah untuk melakukan text pre-processing pada data yang sudah dimiliki sebelumnya yaitu data dari website https://pta.trunojoyo.ac.id/ adalah sebagai berikut:

1. Install Library

   Hal yang pertama harus dilakukan adalah install library yang digunakan untuk proses text pre-processing. Berikut list library yang akan digunakan, untuk menginstall library yaitu dengan membuka command prompt lalu mengetikkan perintah yang ada di bawah ini.

   - pandas

     ```
     pip install pandas
     ```

   - numpy

     ```
     pip install numpy
     ```

   - string

     ```
     pip install strings
     ```

   - re

     ```
     pip install regex
     ```

   - nltk

     ```
     pip install nltk
     ```

2. Buka Jupyter notebook

   Jupyter notebook digunakan untuk memudahkan dalam melakukan text pre-processing data. Kemudian buat file baru pada jupyter notebook dengan nama "Text Pre-Processing".

3. Import Library

   Import library yang sudah diinstal dan digunakan untuk proses text pre-processing. Berikut code untuk import library.

   ```python
   # ------ Import Library --------
   import pandas as pd 
   import numpy as np
   import string 
   import re
   
   from nltk.corpus import stopwords
   ```

   Keterangan :

   Pandas : digunakan untuk membuat tabel, mengubah dimensi data, mengecek data, dan lain sebagainya

   Numpy : digunakan untuk melakukan operasi vektor dan matriks dengan mengolah array dan array multidimensi

   String : digunakan untuk menghasilkan string yang diformat

   Re : digunakan untuk mengubah objek yang cocok dengan objek yang lain. Caranya adalah dengan menggunakan metode sub()

   Nltk : digunakan untuk membantu saat bekerja dengan teks (tokenizing, stopword, stemming)

4. Load Data

   Setelah membuat file baru kemudia selanjutnya yang dilakukan adalah load data.

   Load data yang sudah dimiliki dari website https://pta.trunojoyo.ac.id/ dengan bentuk format excel maupun csv. Data tersebut di load dengan menggunakan library pandas. Berikut code untuk load data.

   ```python
   # ------ Load Data --------
   papers = pd.read_csv("result.csv")
   papers
   ```

   Keterangan :

   papers : nama variabel untuk menampilkan dataframe

   pd : nama alias dari library pandas

   read_csv : function untuk read data dengan format csv

   result.csv : nama dari file data csv yang akan di load

5. Case Folding

   Kemudian dilakukan case folding dengan code sebagai berikut.

   ```python
   # ------ Case Folding --------
   papers['Abstrak_Lowercase'] = papers['Abstrak'].str.lower()
   print('Case Folding Result : \n')
   print(papers['Abstrak_Lowercase'].head(5))
   ```

   Keterangan : 

   str.lower() : merupakan function untuk menyamakan teks menjadi lowercase (huruf kecil) semua

   head() : merupakan function untuk melihat 5 data teratas

6. Remove symbols, number, punctuation

   Penghapusan simbol, angka, dan tanda baca yang tidak berguna dalam proses text pre-processing dengan code sebagai berikut

   ```python
   # ------ Remove symbols, number, punctuation ---------
   
   def remove_abstract_special(abstract):
       # hapus tab, baris baru, dan irisan belakang
       abstract = abstract.replace('\\t'," ").replace('\\n'," ").replace('\\u'," ").replace('\\',"")
       # hapus non ASCII (emotikon, kata cina, .etc)
       abstract = abstract.encode('ascii', 'replace').decode('ascii')
       # hapus mention, link, tagar
       abstract = ' '.join(re.sub("([@#][A-Za-z0-9]+)|(\w+:\/\/\S+)"," ", abstract).split())
       # hapus URL
       return abstract.replace("http://", " ").replace("https://", " ")
                   
   # hapus angka
   def remove_number(abstract):
       return  re.sub(r"\d+", "", abstract)
   
   # hapus tanda baca, simbol
   def remove_punctuation(abstract):
       return abstract.translate(str.maketrans("","",string.punctuation))
   
   # hapus spasi di awal & akhir
   def remove_whitespace_LT(abstract):
       return abstract.strip()
   
   # hapus beberapa spasi putih menjadi spasi tunggal
   def remove_whitespace_multiple(abstract):
       return re.sub('\s+',' ',abstract)
   
   # hapus char tunggal
   def remove_singl_char(abstract):
       return re.sub(r"\b[a-zA-Z]\b", "", abstract)
       
   papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_abstract_special)
   papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_number)
   papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_punctuation)
   papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_whitespace_LT)
   papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_whitespace_multiple)
   papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_singl_char)
   
   print(papers['Abstrak_Lowercase'].head())
   ```

   Keterangan :

   replace() : digunakan untuk mengganti bentuk dari yang sudah ada dengan bentuk yang baru

   re.sub() : digunakan untuk mengubah objek yang cocok dengan objek yang lain

   string.punctuation : digunakan untuk menghapus tanda baca diubah dengan spasi atau string kosong

   apply() : digunakan untuk menerapkan function ke dalam objek yang lain

7. Stopword Removal

   Penghapusan kata yang tidak mempunyai makna dengan code sebagai berikut

   ```python
   # ------ Stopwords Removal ---------
   
   # ambil list stopword indonesia
   list_stopwords = stopwords.words('indonesian')
   list_stopwords.extend(["aam","absolute","abstract","abstrakxd","adm","ahp","ai","aid","akanxd","akhirxd","alert","algorithm","alpha","alternative","ambroxol","analysis","analytic","analytical","and","angkaangka","angular","anp","apl","aplikasixd","application","architecture","artifical","as","asesoris","attribute","automatic","average","babnyajika","background","bahanbahan","baikxd","balanced","base","based","basic","bc","beasiwa","benarbenar","benedict","beratxd","berbedabeda","berturutturut","bifurcation","binaryzation","bisoprolol","bkd","block","blue","bold","bolditalic","bpp","bps","browsing","bsc","business","by","canny","caps","cbir","cefixime","center","centroid","chain","chaining","chainning","character","cipherteks","class","classfier","classification","classifier","close","cluster","clustering","coding","combat","commerce","component","compute","computer","confix","content","contex","context","corepoint","corpus","cosine","criteria","criteriaxd","crm","crossing","customer","cut","cycle","dalamxd","darixd","database","datadata","dataxd","daviesbouldin","decision","decomposition","defuzzyfikasi","dekripsi","denga","denganxd","depanxd","depth","design","development","dibatasixd","dicarixd","difference","diprosesxd","direction","disegmentasi","disk","disperindag","distance","distemming","dkpp","dlda","dominant","download","dperoleh","dr","drop","dsebut","eclipse","ecommerce","ecommercexd","economic","edge","edm","education","egovernment","eigenxd","ekuitas","electre","electreii","electronic","emulator","engghibunten","engine","engineering","enginexd","english","enhanced","enjeiyeh","enterprise","environment","eoq","epoch","epoh","error","eucledian","euclidean","exploiting","exponential","express","fahp","fanp","feature","fighting","filter","filtering","fine","fingerprint","fingerprintbitmaps","finite","firewall","first","fisik","fmeasure","fmop","font","foreign","forward","framework","free","frustasi","fsm","function","fuzzy","fvc","galis","game","games","garisgaris","gateway","gaussian","geometry","gizixd","gldm","glrm","gr","gradient","gradients","gray","grayscaling","grcitra","ground","growth","ha","haar","had","handwriting","harris","hash","hh","hidden","hierarchy","high","hijauxd","hl","hog","ht","idb","ii","ij","iksass","image","indahmulya","indicator","indicators","infoinfo","inginxd","inixd","inktech","interface","interprise","intervace","intervensi","interview","intraseluler","intrusion","invariant","inventori","ips","iptables","italic","jaringanjaringan","jarixd","java","jejaring","jiwaxd","jst","kabupatenkabupaten","kaganga","kallista","karakterkarakter","karapanxd","kec","kerapan","kerjasamaxd","kesejahteraanya","key","keypoint","keyword","keywords","kg","kit","kkm","kluster","kmeans","kohonen","kokop","komulatif","konang","konekasi","kpi","kriteriakriteria","kriteriaxd","ksom","kub","kuisioner","kuisoner","lainlain","lainxd","langkahlangkah","language","languange","latent","layer","lda","learning","least","length","lerning","leveling","lh","life","light","linier","link","listening","ll","load","log","logic","low","lsa","lsasom","lt","lunakxd","lunturnya","lvq","lyapunov","machine","madistrindo","maduraindonesia","maduraxd","mail","making","malan","mamdani","management","manager","mandiriauto","map","mape","maps","mapserver","martodirdjo","masingmasing","masking","matching","matrix","maze","mazexd","mcdm","mdf","mean","melakukanxd","membatu","memilikixd","mengenkripsi","menggunakanxd","message","metadata","method","mg","middleware","minimnya","minutea","minutiae","modung","momentum","monitoring","morfologi","mosaic","mosaikpanoramik","moving","mpc","mse","multiatribute","multimedia","multiobjective","multiple","naive","nave","nbc","negaranegara","network","neural","ngram","node","nomor","non","npc","number","numberxd","nya","obatobatan","objective","obyek","obyektif","of","offline","ofr","oldinary","ols","omax","ontologi","ontology","open","optical","optimized","optimiztion","optimum","ordered","organizing","oriented","orl","output","owl","panoramic","panoramik","panoteng","parameterparameter","parsing","part","particle","pasienxd","pattern","pca","pe","pejualan","pelajarsantri","pelevelan","pemvalidasian","penjadwaln","perankingan","perankingannya","percentage","perconbaan","performance","periodeperiode","permasalaha","perusahaanxd","pihakpihak","pihakxd","pixel","pixels","plainteks","plasmodium","plastec","platform","playable","player","pmg","podhek","point","pose","ppa","prakandidat","precision","preference","presentase","preshion","prevention","prim","principal","prinsipnya","print","prism","probabilitasmetode","process","processing","produksipada","produktivitas","profitabilitas","programing","programming","programprogram","project","prosentase","prosesnya","pso","pt","ptxd","quantity","quantization","query","rangkebbhan","rank","ranks","raskin","ratarata","rate","rater","rating","ratus","rbfn","rbfnn","rbfnnxd","rc","rdf","reading","real","realistisxd","realitas","reality","realtime","recall","recognition","rekomndasi","relative","release","resource","resources","responden","retrieval","reuse","ridge","riilxd","rill","riwayatxd","rehabilitasi","roughness","rts","run","saaty","salafiyah","roughness","sales","sasaranxd","satunya","satunya","scale","scm","scorecard","scoring","screen","sdk","sdkxd","sdlc","sdm","search","second","security","segmentasinya","seharihari","sekuensial","self","semantic","sencitivity","seolaholah","separation","seringkali","server","service","ses","seseorangxd","shop","shortest","sift","sikannya","similar","similaritas","similarity","simple","simtak","single","singular","sistemxd","skenarioskenario","sky","sma","smarter","smartphone","smoothing","smooting","smp","sms","snort","software","solusinya","solusinyaxd","solution","som","sort","source","spare","spasial","spci","speaking","specificity","speech","spk","square","stakeholder","state","statistik","statusxd","stemmer","stemming","stockpile","strategi","strategy","straw","stripping","style","sub","subkriteria","subset","subsistem","subtropics","subyektivitas","sumenep","suplier","supplier","supply","swarm","syafiiyah","system","tab","tamansepanjang","technique","telekomunikasi","terater","terhadapxd","termination","terpisahpisah","tersebutxd","tertentuumumnya","test","testes","testing","thinning","thomas","threshold","tiaptiap","time","tinggixd","titiktitik","tnpk","to","toba","toefl","toeflxd","togaf","tool","tools","tooltool","topsis","traffic","tragah","training","transform","treshold","truth","tsai","tujuansetelah","tulangan","tuneup","two","ujicoba","userxd","utnuk","validitas","value","vector","velocity","vii","virtual","vision","vr","wachid","waktuxd","waterfall","watershed","wavelet","web","website","webxd","wide","window","winnowing","world","www","xd","xna","yakersuda","yangxd",'baiknya','berkali','kali','kurangnya','mata','olah','sekurang','setidak','tama','tidaknya'])
   list_stopwords = set(list_stopwords)
   ```

   Keterangan :

   list_stopwords : merupakan nama variabel untuk list stopword

   stopwords.words('indonesian') : digunakan untuk memanggil function stopword bahasa indonesia
   extend() : digunakan untuk menambahkan list stopword yang masih dirasa kurang dari library yang digunakan

   

## Code Text Pre-processing

Adapun code keseluruhan untuk melakukan text pre-processing adalah sebagai berikut.

```python
# ------ Import Library --------
import pandas as pd 
import numpy as np
import string 
import re

from nltk.corpus import stopwords
```

```python
# ------ Load Data --------
papers = pd.read_csv("result.csv")
papers
```

```python
# ------ Case Folding --------
papers['Abstrak_Lowercase'] = papers['Abstrak'].str.lower()
print('Case Folding Result : \n')
print(papers['Abstrak_Lowercase'].head(5))
```

```python
# ------ Remove symbols, number, punctuation ---------

def remove_abstract_special(abstract):
    # hapus tab, baris baru, dan irisan belakang
    abstract = abstract.replace('\\t'," ").replace('\\n'," ").replace('\\u'," ").replace('\\',"")
    # hapus non ASCII (emotikon, kata cina, .etc)
    abstract = abstract.encode('ascii', 'replace').decode('ascii')
    # hapus mention, link, tagar
    abstract = ' '.join(re.sub("([@#][A-Za-z0-9]+)|(\w+:\/\/\S+)"," ", abstract).split())
    # hapus URL
    return abstract.replace("http://", " ").replace("https://", " ")
                
# hapus angka
def remove_number(abstract):
    return  re.sub(r"\d+", "", abstract)

# hapus tanda baca, simbol
def remove_punctuation(abstract):
    return abstract.translate(str.maketrans("","",string.punctuation))

# hapus spasi di awal & akhir
def remove_whitespace_LT(abstract):
    return abstract.strip()

# hapus beberapa spasi putih menjadi spasi tunggal
def remove_whitespace_multiple(abstract):
    return re.sub('\s+',' ',abstract)

# hapus char tunggal
def remove_singl_char(abstract):
    return re.sub(r"\b[a-zA-Z]\b", "", abstract)
    
papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_abstract_special)
papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_number)
papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_punctuation)
papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_whitespace_LT)
papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_whitespace_multiple)
papers['Abstrak_Lowercase'] = papers['Abstrak_Lowercase'].apply(remove_singl_char)

print(papers['Abstrak_Lowercase'].head())
```

```python
# ------ Stopwords Removal ---------

# ambil list stopword indonesia
list_stopwords = stopwords.words('indonesian')
list_stopwords.extend(["aam","absolute","abstract","abstrakxd","adm","ahp","ai","aid","akanxd","akhirxd","alert","algorithm","alpha","alternative","ambroxol","analysis","analytic","analytical","and","angkaangka","angular","anp","apl","aplikasixd","application","architecture","artifical","as","asesoris","attribute","automatic","average","babnyajika","background","bahanbahan","baikxd","balanced","base","based","basic","bc","beasiwa","benarbenar","benedict","beratxd","berbedabeda","berturutturut","bifurcation","binaryzation","bisoprolol","bkd","block","blue","bold","bolditalic","bpp","bps","browsing","bsc","business","by","canny","caps","cbir","cefixime","center","centroid","chain","chaining","chainning","character","cipherteks","class","classfier","classification","classifier","close","cluster","clustering","coding","combat","commerce","component","compute","computer","confix","content","contex","context","corepoint","corpus","cosine","criteria","criteriaxd","crm","crossing","customer","cut","cycle","dalamxd","darixd","database","datadata","dataxd","daviesbouldin","decision","decomposition","defuzzyfikasi","dekripsi","denga","denganxd","depanxd","depth","design","development","dibatasixd","dicarixd","difference","diprosesxd","direction","disegmentasi","disk","disperindag","distance","distemming","dkpp","dlda","dominant","download","dperoleh","dr","drop","dsebut","eclipse","ecommerce","ecommercexd","economic","edge","edm","education","egovernment","eigenxd","ekuitas","electre","electreii","electronic","emulator","engghibunten","engine","engineering","enginexd","english","enhanced","enjeiyeh","enterprise","environment","eoq","epoch","epoh","error","eucledian","euclidean","exploiting","exponential","express","fahp","fanp","feature","fighting","filter","filtering","fine","fingerprint","fingerprintbitmaps","finite","firewall","first","fisik","fmeasure","fmop","font","foreign","forward","framework","free","frustasi","fsm","function","fuzzy","fvc","galis","game","games","garisgaris","gateway","gaussian","geometry","gizixd","gldm","glrm","gr","gradient","gradients","gray","grayscaling","grcitra","ground","growth","ha","haar","had","handwriting","harris","hash","hh","hidden","hierarchy","high","hijauxd","hl","hog","ht","idb","ii","ij","iksass","image","indahmulya","indicator","indicators","infoinfo","inginxd","inixd","inktech","interface","interprise","intervace","intervensi","interview","intraseluler","intrusion","invariant","inventori","ips","iptables","italic","jaringanjaringan","jarixd","java","jejaring","jiwaxd","jst","kabupatenkabupaten","kaganga","kallista","karakterkarakter","karapanxd","kec","kerapan","kerjasamaxd","kesejahteraanya","key","keypoint","keyword","keywords","kg","kit","kkm","kluster","kmeans","kohonen","kokop","komulatif","konang","konekasi","kpi","kriteriakriteria","kriteriaxd","ksom","kub","kuisioner","kuisoner","lainlain","lainxd","langkahlangkah","language","languange","latent","layer","lda","learning","least","length","lerning","leveling","lh","life","light","linier","link","listening","ll","load","log","logic","low","lsa","lsasom","lt","lunakxd","lunturnya","lvq","lyapunov","machine","madistrindo","maduraindonesia","maduraxd","mail","making","malan","mamdani","management","manager","mandiriauto","map","mape","maps","mapserver","martodirdjo","masingmasing","masking","matching","matrix","maze","mazexd","mcdm","mdf","mean","melakukanxd","membatu","memilikixd","mengenkripsi","menggunakanxd","message","metadata","method","mg","middleware","minimnya","minutea","minutiae","modung","momentum","monitoring","morfologi","mosaic","mosaikpanoramik","moving","mpc","mse","multiatribute","multimedia","multiobjective","multiple","naive","nave","nbc","negaranegara","network","neural","ngram","node","nomor","non","npc","number","numberxd","nya","obatobatan","objective","obyek","obyektif","of","offline","ofr","oldinary","ols","omax","ontologi","ontology","open","optical","optimized","optimiztion","optimum","ordered","organizing","oriented","orl","output","owl","panoramic","panoramik","panoteng","parameterparameter","parsing","part","particle","pasienxd","pattern","pca","pe","pejualan","pelajarsantri","pelevelan","pemvalidasian","penjadwaln","perankingan","perankingannya","percentage","perconbaan","performance","periodeperiode","permasalaha","perusahaanxd","pihakpihak","pihakxd","pixel","pixels","plainteks","plasmodium","plastec","platform","playable","player","pmg","podhek","point","pose","ppa","prakandidat","precision","preference","presentase","preshion","prevention","prim","principal","prinsipnya","print","prism","probabilitasmetode","process","processing","produksipada","produktivitas","profitabilitas","programing","programming","programprogram","project","prosentase","prosesnya","pso","pt","ptxd","quantity","quantization","query","rangkebbhan","rank","ranks","raskin","ratarata","rate","rater","rating","ratus","rbfn","rbfnn","rbfnnxd","rc","rdf","reading","real","realistisxd","realitas","reality","realtime","recall","recognition","rekomndasi","relative","release","resource","resources","responden","retrieval","reuse","ridge","riilxd","rill","riwayatxd","rehabilitasi","roughness","rts","run","saaty","salafiyah","roughness","sales","sasaranxd","satunya","satunya","scale","scm","scorecard","scoring","screen","sdk","sdkxd","sdlc","sdm","search","second","security","segmentasinya","seharihari","sekuensial","self","semantic","sencitivity","seolaholah","separation","seringkali","server","service","ses","seseorangxd","shop","shortest","sift","sikannya","similar","similaritas","similarity","simple","simtak","single","singular","sistemxd","skenarioskenario","sky","sma","smarter","smartphone","smoothing","smooting","smp","sms","snort","software","solusinya","solusinyaxd","solution","som","sort","source","spare","spasial","spci","speaking","specificity","speech","spk","square","stakeholder","state","statistik","statusxd","stemmer","stemming","stockpile","strategi","strategy","straw","stripping","style","sub","subkriteria","subset","subsistem","subtropics","subyektivitas","sumenep","suplier","supplier","supply","swarm","syafiiyah","system","tab","tamansepanjang","technique","telekomunikasi","terater","terhadapxd","termination","terpisahpisah","tersebutxd","tertentuumumnya","test","testes","testing","thinning","thomas","threshold","tiaptiap","time","tinggixd","titiktitik","tnpk","to","toba","toefl","toeflxd","togaf","tool","tools","tooltool","topsis","traffic","tragah","training","transform","treshold","truth","tsai","tujuansetelah","tulangan","tuneup","two","ujicoba","userxd","utnuk","validitas","value","vector","velocity","vii","virtual","vision","vr","wachid","waktuxd","waterfall","watershed","wavelet","web","website","webxd","wide","window","winnowing","world","www","xd","xna","yakersuda","yangxd",'baiknya','berkali','kali','kurangnya','mata','olah','sekurang','setidak','tama','tidaknya'])
list_stopwords = set(list_stopwords)
```



## Referensi

```{bibliography}

```
