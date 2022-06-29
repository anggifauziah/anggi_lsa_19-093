# Crawling Data

## Apa itu Crawling data?

Crawling data adalah suatu teknik untuk pengumpulan data atau informasi yang terstruktur dari sebuah website tertentu dengan melakukan ekstraksi informasi yang ada dalam website tersebut. Pada penerapannya, crawling data ini bisa dilakukan secara manual maupun otomatis dengan menggunakan program crawling yang sudah ada.

Penggunaan program crawling data ini dapat membantu pengambilan data secara otomatis, cepat, dan tepat. Tetapi perlu diketahui bahwa untuk mendapatkan data tersebut, sebelumnya harus mengetahui hal-hal berikut:

- **URL** dari website yang akan diambil informasi datanya
- Element **HTML** atau **XML** dari website
- Nama **id** atau **class** pada element **HTML** atau **XML**

Setelah mengetahui tiga poin yang disebutkan di atas, maka dapat dimasukkan dalam program crawling untuk mengambil data sesuai dengan **id** atau **class** dari element di atas.



## Langkah-langkah melakukan Crawling Data

Pada pembahasan kali ini crawling data dilakukan menggunakan program python dengan library Scrapy. Adapun langkah-langkah untuk melakukan crawling data pada sebuah website adalah sebagai berikut:

1. Install Scrapy

   Langkah pertama yaitu install library Scrapy terlebih dahulu. Caranya dengan membuka command prompt kemudian ketik perintah berikut.

   ```python
   pip install scrapy
   ```

2. Project Scrapy

   Kemudian buka lokasi tempat untuk menyimpan folder dari project scrapy pada code editor.

   Disini saya menggunakan Visual Studio Code, kemudian buka terminal pada Visual Studio Code dan ketik perintah berikut.

   ```python
   scrapy startproject projectname
   ```

   Keterangan :

   projectname : nama dari project atau folder baru yang dibuat

   Contoh :

   ```python
   scrapy startproject crawling
   ```

3. Start first spider

   Setelah membuat project baru maka selanjutnya adalah membuat file spider untuk mengetikkan program python dari crawling data. Pada terminal ketik perintah berikut.

   ```
   cd projectname
   ```

   ```
   scrapy genspider example example.com
   ```

   Keterangan :

   projectname : nama dari project atau folder yang dibuat

   example : nama dari file baru yang dibuat

   example.com : url dari website yang akan di crawling datanya

   Contoh :

   ```python
   cd crawling
   ```

   ```python
   scrapy genspider crawling-data https://pta.trunojoyo.ac.id/
   ```

4. Buka file spider

   File spider yang telah dibuat pada langkah sebelumnya yaitu terdapat pada folder spiders dengan spesifik lokasi seperti berikut.

   ```
   crawling/crawling/spiders/crawling-data.py
   ```

5. Inspect Element HTML atau XML

   Buka web browser kemudian kunjungi website yang akan di crawling datanya. Pada kali ini website yang digunakan adalah https://pta.trunojoyo.ac.id/.

   Kemudian lakukan inspect element pada website tersebut dengan cara

   - Klik kanan
   - Pilih **Inspect** pada bagian bawah sendiri

   Setelah itu bisa diklik pada element mana yang akan diambil informasinya. Pada kesempatan kali ini yang akan diambil informasinya pada website https://pta.trunojoyo.ac.id/ adalah bagian dari Judul, Penulis, Dosen Pembimbing 1, Dosen Pembimbing 2, dan Abstrak berbahasa Indonesia berdasarkan program studi Teknik Informatika.

   - Inspect element button

     Inspect element pada button "Selengkapnya", karena keseluruhan Abstrak terdapat pada halaman tersebut. Berikut hasil inspect element dari button "Selengkapnya".

     ```html
     a.gray.button::attr(href)
     ```

   - Inspect element  informasi lainnya

     Inspect element dari Judul, Penulis, Dosen Pembimbing 1, Dosen Pembimbing 2, dan Abstrak berbahasa Indonesia. Berikut hasil inspect element yang didapatkan.

     ```html
     div a.title::text
     div div:nth_child(2) span::text
     div div:nth_child(3) span::text
     div div:nth_child(4) span::text
     div div:nth_child(2) p::text
     ```

     Urutan hasil inspect element di atas yaitu

     - Judul
     - Penulis
     - Dosen Pembimbing 1
     - Dosen Pembimbing 2
     - Abstrak Indonesia

6. Masukkan dalam file spider

   Setelah semua inspect element sudah selesai maka selanjutnya hanya menggabungkan dengan code dari file spider yang sudah dibuat sebelumnya. Berikut hasil akhir code untuk crawling data.

   ```python
   # ------ Import Library --------
   import scrapy
   
   
   # ------ Crawling Data --------
   class CrawlingDataSpider(scrapy.Spider):
       name = 'crawling-data'
       allowed_domains = ['pta.trunojoyo.ac.id']
       start_urls = ['https://pta.trunojoyo.ac.id/c_search/byprod/10/'+str(x)+" " for x in range(2,15)]
   
       def parse(self, response):
           for link in response.css('a.gray.button::attr(href)') :
               yield response.follow(link.get(),callback=self.parse_categories)
               
       def parse_categories(self, response):
           products = response.css('div#content_journal ul li')
           for product in products:
               yield {
                   'Judul' : product.css('div a.title::text').get().strip(),
                   'Penulis' : product.css('div div:nth_child(2) span::text').get().strip(),
                   'Dosen 1' : product.css('div div:nth_child(3) span::text').get().strip(),
                   'Dosen 2' : product.css('div div:nth_child(4) span::text').get().strip(),
                   'Abstrak' : product.css('div div:nth_child(2) p::text').get().strip()
               }
   ```

7. Melihat data crawling

   Untuk melihat data yang berhasil di crawling yaitu dengan mengetikkan perintah berikut pada terminal.

   ```python
   scrapy crawl example
   ```

   Keterangan :

   example : nama dari file spider yang dibuat

   Contoh :

   ```python
   scrapy crawl crawling-data
   ```

8. Menyimpan data crawling

   Untuk dapat menyimpan data crawling yang sudah didapatkan, bisa dengan mengetikkan perintah berikut pada terminal.

   ```python
   scrapy crawl example -O name.csv
   ```

   Keterangan :

   example : nama dari file spider yang dibuat

   name.csv : file csv untuk menampung hasil dari data crawling

   Contoh :

   ```python
   scrapy crawl crawling-data -O result.csv
   ```



## Code Crawling Data

Adapun code keseluruhan untuk crawling data menggunakan program python dengan library Scrapy adalah sebagai berikut.

```python
import scrapy


class CrawlingDataSpider(scrapy.Spider):
    name = 'crawling-data'
    allowed_domains = ['pta.trunojoyo.ac.id']
    start_urls = ['https://pta.trunojoyo.ac.id/c_search/byprod/10/'+str(x)+" " for x in range(2,15)]

    def parse(self, response):
        for link in response.css('a.gray.button::attr(href)') :
            yield response.follow(link.get(),callback=self.parse_categories)
            
    def parse_categories(self, response):
        products = response.css('div#content_journal ul li')
        for product in products:
            yield {
                'Judul' : product.css('div a.title::text').get().strip(),
                'Penulis' : product.css('div div:nth_child(2) span::text').get().strip(),
                'Dosen 1' : product.css('div div:nth_child(3) span::text').get().strip(),
                'Dosen 2' : product.css('div div:nth_child(4) span::text').get().strip(),
                'Abstrak' : product.css('div div:nth_child(2) p::text').get().strip()
            }
```



## Referensi

```{bibliography}

```
