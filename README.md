# Final Project Teknologi Komputasi Awan 

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
Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah kemampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan _project_ untuk men-_deploy_ sebuah aplikasi **Sentiment Analysis** dengan komponen Backend menggunakan python: sentiment-analysis.py dengan spesifikasi sebagai berikut 

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

Kemudian anda diminta untuk mendesain arsitektur _cloud_ yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$), konfigurasi _cloud_ terbaik seperti apa yang bisa dibuat?

## II. Rancangan Arsitektur Serta Harganya 
![image](https://github.com/RafaelJonathanA/Sisop-FP-2024-MH-IT26/assets/150375098/ce60838f-7a99-4433-a38c-4b7e9eb25d81)

### Dengan Spesifikasi Harga : 

| No. | Name         | Specifications                                                                      | Work Load            | Price |
|-----|--------------|-------------------------------------------------------------------------------------|----------------------|-------|
| 1   | worker1      | 1 GB Memory / 1 AMD vCPU / 25 GB Disk NVME SSD / 1TB Transfer / SGP1 - Ubuntu 22.04 (LTS) x64 | Back-End Worker + Front-End Worker     | $7    |
| 2   | worker2      |  1 GB Memory / 1 AMD vCPU / 25 GB Disk NVME SSD / 1TB Transfer / SGP1 - Ubuntu 22.04 (LTS) x64 | Back-End Worker      | $7    |
| 3   | worker3     |  1 GB Memory / 1 AMD vCPU / 25 GB Disk NVME SSD / 1TB Transfer / SGP1 - Ubuntu 22.04 (LTS) x64 | Back-End Worker      | $7    |
| 4   | loadbalancer     | 2 node / 20K Simultaneous Connection / 20K Request per Second / 50 SSL Connection per Second | Database Server      | $24   |
| 5   | mongodb      | 1 GB RAM / 1 vCPU / 15 GB Disk NVME SSD / Primary only / SGP1 - MongoDB             | Database Server      | $15   |
|**Total**     |     |        |        | **$60**|


## III. Langkah-langkah Implementasi 
1. Buat Backend dan Frontend menggunakan Droplet pada DigitalOcean, kita membuat sebanyak 3 droplets 
2. Buat Database di DigitalOcean 
3. Mengkonfigurasikan Backend dan Frontend dengan konfigurasi yang telah diberikan baik untuk Frontend maupun Backend. 
4. Lakukan konfigurasi loadbalancer pada DigitalOcean 

Untuk Front-end : 
- Menginstal apache dengan command **Sudo apt-get install apache2**
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
5. Buka IP front-end untuk memastikan bahwa web server dapat diakses 
6. Lalu lakukan pengujian menggunakan locust pada komputer lain dan menjalankan locusfile.py dengan command **locust -f locustfile.py --host http://[IP LoadBalancer DigitalOcean]**

## IV. Hasil Pengujian endpoint 
- Untuk pengujian get/analyze menggunakan webnya 
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/ab420cc3-6b50-4ece-b3cc-a315c0eb823f)
- Untuk pengujian get/history menggunakan thunder client dan web 
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/1a649765-0b54-497c-abdd-bede31641226)
![image](https://github.com/RafaelJonathanA/FP-TKA-C3/assets/150375098/1412b1fe-4aea-4d05-8740-d20e565d4d66)

## V. Hasil dan Analisis Pengujian Locust 
### Maksimal RPS 
Dengan **number off user** : 1500, **Ramp UP** : 200 dan **Waktu** : 15 Sekon 

![image](https://github.com/RafaelJonathanA/Sisop-FP-2024-MH-IT26/assets/150375098/707ea657-ffb2-42de-bedb-06a89d5ccc21)

Untuk maksimal RPS ada di angka 368 sedangkan rata-ratanya ada di angka 247.8 sehingga bisa dihitung dengan cara 247/200 X 30 = 37 

Seperti yang kita lihat pada grafik bahwa terjadi lonjakan response time pada akhir pengujian yang menandakan bahwa arsitektur mualai kesulitan menangani 1500 user dan RPS juganya mulai menurun yang tadinya pada angka 280 menjadi 240 namun hal ini menjadi lebih baik dibandingkan pada arsitektur sebelumnya yang menggunakan 1 VM untuk front-end dan 1 VM untuk back-end dan satu database yang maksimum RPSnya hanya 50 saja dan rata-ratanya hanya 40 sehingga dengan menambahkan load balancer RPS menjadi lebih baik 

### Mencari Peak of Currency 
#### 50 Spawn Rate (60s)
![image](https://github.com/RafaelJonathanA/Sisop-FP-2024-MH-IT26/assets/150375098/e52e0cb0-4afd-46e8-9ce8-7b9d3d5e5f94)

Bila kita lihat terjadi fail saat user mencapai 2500 sehingga bisa disimpulkan peak currency untuk 50 spawn rate adalah 2500

#### 100 Spawn Rate (60s)
![image](https://github.com/RafaelJonathanA/Sisop-FP-2024-MH-IT26/assets/150375098/013f1331-f177-4837-b1bc-896f959f4eb1)

Bila kita lihat tidak terjadi fail pada percobaan ini dengan peak of currencynya adalah 1800 namun pada percobaan sebelumnya di angka 1900 mengalami fail 1% sehingga dapat disimpulkan peak od curencynya adalah 1800 

#### 200 Spawn Rate (60s)
![image](https://github.com/RafaelJonathanA/Sisop-FP-2024-MH-IT26/assets/150375098/59c8c69d-c2d9-454c-8700-ef875f4a4a78)

Bila kita lihat tidak terjadi fail pada percobaan ini dengan peak currencynya adalah 1800 namun pada percobaan sebelumnya di angka 2000 mengalami fail 3% sehingga dapat disimpulkan peak of currencynya adalah 1800 

#### 500 Spawn Rate (60s)
![image](https://github.com/RafaelJonathanA/Sisop-FP-2024-MH-IT26/assets/150375098/d3d12096-6484-4ecd-b46b-d2459647c2c4)

Bila kita lihat tidak terjadi fail pada percobaan ini dengan peak currencynya adalah 1700 namun pada percobaan sebelumnya di angka 1800 mengalami fail 6% sehingga dapat disimpulkan peak of currencynya adalah 1700 

## VI. Kesimpulan dan Saran 
Kesimpulan dengan menggunakan 3 VM satu untuk front-end dan dua untuk back-end dan disambungkan dengan loadbalancer lalu disambungkan kembali dengan database maka akan bisa bekerja dengan cukup baik dengan melihat puncak dari RPS(Request per second) yang bisa mencapai 360 an saat test locust dengan user 1500 dan dengan spawn rate 200. 

Dari revisi kemarin yang dilakukan ada beberapa hal penting yaitu dengan menggunakan load balancer maka akan memungkinkan web untuk menanggung beban yang lebih banyak, dibandingkan hanya dengan menggunakan 1 VM dengan spesifikasi tinggi sebagai back-end lebih baik menggunakan 3 VM dengan spesifikasi rendah sehingga masing-masing back-end dapat membagi tugas menjadi lebih baik karena memang _core_nya lebih banyak 

Saran dari kelompok kami adalah saat pengujian locust ada baiknya bila **server direstart terlebih dahulu** bila sudah melakukan 5 pengujian karena hal ini bisa mempengaruhi hasil dari pengetesan locust dan juga baiknya dari rendah terlebih dahulu pengujiannya baru setelah itu naik secara bertahap.
