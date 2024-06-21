# Final Project TKA 

## Kelompok C3 TKA(B)

**Anggota:**
| Nama                     | NRP        |
|--------------------------|------------|
| Diandra Naufal Abror     | 5027231004 |
| Rafael Jonathan Arnldus  | 5027231006 |
| Michael Kenneth Salim    | 5027231008 |
| Elgracito Iryanda Endia  | 5027221057 |
| Muhammad Hildan Adiwena  | 5027221077 |

## I. Introduction
Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan project untuk mendeploy sebuah aplikasi Sentiment Analysis dengan komponen Backend menggunakan python: sentiment-analysis.py dengan spesifikasi sebagai berikut 

### Endpoints:
**1. Analyze Text**
  - **Endpoint:** POST `/analyze`
  - **Description:** This endpoint accepts a text input and returns the sentiment score of the text.
  - **Request:**
    ```json
    {
      "text": "Your text here"
    }
    ```
  - **Response:**
    ```json
    {
      "sentiment": <sentiment_score>
    }
    ```

- **Retrieve History**
  - **Endpoint:** GET `/history`
  - **Description:** Retrieves the history of previously analyzed texts along with their sentiment scores.
  - **Response:**
    ```json
    [
      {
        "text": "Your previous text here",
        "sentiment": <sentiment_score>
      },
      ...
    ]
    ```

Kemudian juga disediakan sebuah Frontend sederhana menggunakan index.html dan styles.css dengan tampilan antarmuka sebagai berikut: 
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/54bfcb8c-8d63-4c59-986a-f73d09bda5cf)

Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?

## II. Rancangan Arsitektur Serta Harganya 
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/2d2a4ab8-4d3e-4ce4-a213-3301154a9706)

### Dengan Spesifikasi Harga : 

| No. | Name         | Specifications                                                                      | Work Load            | Price |
|-----|--------------|-------------------------------------------------------------------------------------|----------------------|-------|
| 1   | worker1      | 1 GB Memory / 1 AMD vCPU / 25 GB Disk NVME SSD / 1TB Transfer / SGP1 - Ubuntu 22.04 (LTS) x64 | Back-End Worker      | $7    |
| 2   | worker2      | 4 GB Memory / 2 Intel vCPUs / 25 GB Disk NVME SSD / 1TB Transfer / SGP1 - Ubuntu 22.04 (LTS) x64 | Back-End Worker   | $32    |
| 3   | mongodb      | 1 GB RAM / 1 vCPU / 15 GB Disk NVME SSD / Primary only / SGP1 - MongoDB             | Database Server      | $15   |
|**Total**     |     |        |        | **$54**|


## III. Langkah-langkah Implementasi 
1. Buat Backend dan Frontend menggunakan Droplet pada DigitalOcean 
2. Buat Database di DigitalOcean 
3. Mengkonfigurasikan Backend dan Frontend dengan konfigurasi yang telah diberikan baik untuk Frontend maupun Backend. 

Untuk Front-end : 
-  Menginstal apache dengan command **Sudo apt-get install apache2**
- Konfigurasikan untuk front-end index.html dan styles.css 
- lalu restart apache2

Untuk Back-end :
- Mengintal python3 dengan command **sudo apt-get install python3** 
- Install semua yang dibutuhkan dalam menjalankan konfigurasi pada back-end 
    - sudo apt-get install python3-pip 
    - pip install flask
    - pip install flask_cors
    - pip install textblob
    - pip install pymongo
- Konfigurasikan sentiment_analysis.py 
4. Buka IP front-end untuk memastikan bahwa web server dapat diakses 
5. Lalu lakukan pengujian menggunakan locust pada komputer lain dan menjalankan locusfile.py dengan command **locust -f locustfile.py --host http://[IP backend yang ada pada DigitalOcean]:5000**

## IV. Hasil Pengujian endpoint 
- Untuk pengujian get/analyze menggunakan webnya 
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/ab420cc3-6b50-4ece-b3cc-a315c0eb823f)
- Untuk pengujian get/history menggunakan thunder client dan web 
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/1a649765-0b54-497c-abdd-bede31641226)
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/1412b1fe-4aea-4d05-8740-d20e565d4d66)

## V. Hasil dan Analisis Pengujian Locust 
### Test Locust Spawn Rate 50
- Ini adalah Grafik untuk Tes Locust 100 User dengan 50 Spawn Rate 
![Locus 50](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/b8995a63-eb95-4129-a6d7-eee120029620)
- Ini adalah hasil pengujian Tes Locust 500 User dengaan 50 Spawn Rate
![Locus 50 (500)](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/82738be1-fb2c-481c-852d-a396d84332c2)
- Ini adalah hasil pengujian Tes Locust dengan membandingkan antara 500 user, 800 user dan 700 user 
![Locus 50 (700)](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/f69f20e9-e5a2-4688-ae8e-1089f46095a4)
Dimana pada pengujian untuk 800 user ada tingkat failure diatas 2% dan untuk yang 700 masih tetap 0% dan untuk 500 memliki tingkat failure 0% juga 

### Test Locust dengan spawn rate 100
- Ini adalah Grafik untuk Tes Locust 100 User dengan 100 Spawn Rate 
![Locus 100](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/d668a8f9-fd93-48c5-8c16-90287a0f3888) 
- Ini adalah Grafik untuk Tes Locust 500 User dan 400 user dengan 100 Spawn Rate 
![Locust 100(400)](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/0aecd082-c300-41b6-b062-d78794720342)
Dimana untuk pengujian untuk 500 user terdapat failure sebanyak 3% dan pada 400 user masih tetap 0% 

### Test Locust dengan spawn rate 200
- Ini adalah Grafik untuk Tes Locust 100 User dengan 200 Spawn Rate 
![Locus 200](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/16177525-9b83-4e35-a46e-2e14224b0e68)
- Ini adalah Grafik untuk Tes Locust 400 dan 500 User dengan 200 Spawn Rate
![Locust 200(400-500)](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/c993c775-4907-4775-8fac-96debe6bf35e)
Dimana untuk pengujian 400 user memiliki failure 0% namun di pengujian 500 user memiliki failure 1% 

### Test Locust dengan spawn rate 500 
- Ini adalah Grafik untuk Tes Locust 100 User dengan 500 Spawn Rate 
![Locus 500](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/fa9748c4-b84a-40a3-ae6b-2ac501a759e9)
- Ini adalah Grafik untuk Tes Locust 400 dan 300 User dengan 500 Spawn Rate 
![Locust500(300)](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/de652363-830b-47d2-aac8-f4ef8f743012)
Dimana untuk pengujian 400 user memiliki tingkat failure 1% sedangkan untuk pengujian 300 user memiliki tingkat  failure 0% 

## VI. Kesimpulan dan Saran 
Kesimpulan dengan menggunakan 2 VM satu untuk front-end dan satu untuk back-end dan disambungkan dengan database maka akan bisa bekerja dengan cukup baik dengan melihat puncak dari RPS(Request per second) yang bisa mencapai 150 an saat test locust dengan user 300 dan dengan spawn rate 500. 

Saran dari kelompok kami adalah saat pengujian locust ada baiknya bila server direstart terlebih dahulu bila sudah melakukan 5 pengujian karena hal ini bisa mempengaruhi hasil dari pengetesan locust dan juga baikknya dari rendah terlebih dahulu pengujiannya baru setelah itu naik secara bertahap 
