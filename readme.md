# Pemodelan dan Simulasi Sistem Antrian Pelayanan Pelanggan pada Supermarket (M/M/5)

Repositori ini berisi implementasi analisis distribusi data observasi dan **Discrete-Event Simulation (DES)** menggunakan **SimPy** untuk memodelkan sistem antrian pelayanan pembayaran di area kasir **Supermarekt**.

## A. Penjelasan Umum Terkait Sistem yang Dimodelkan

Sistem antrian yang dimodelkan dalam proyek ini adalah sistem antrian pembayaran di di kasir sebuah supermarket. Supermarket ini merupakan salah satu pusat perbelanjaan modern yang menyediakan berbagai kebutuhan rumah tangga. Seperti halnya pusat perbelanjaan besar lainnya, salah satu proses penting yang terjadi di dalamnya adalah proses pembayaran di kasir. Oleh karena itu, sistem antrian kasir memegang peran penting dalam menentukan kenyamanan dan efisiensi pelayanan di supermarket.

Sistem antrian dapat diartikan sebagai suatu proses di mana pelanggan datang untuk memperoleh pelayanan, menunggu jika tidak segera dilayani, dan meninggalkan fasilitas pelayanan (sistem) setelah selesai dilayani ([Hanikmah dkk., 2019](https://doi.org/10.26714/jsunimus.7.2.2019.%2525p)). Ilmu yang mempelajari proses dan perilaku antrian dikenal sebagai teori antrian (*queueing theory*). Tujuan utama dari sistem antrian adalah untuk meningkatkan efisiensi, mengurangi waktu tunggu, dan memberikan pelayanan yang lebih optimal kepada pelanggan ([Ibrahim dkk., 2025](https://doi.org/10.62383/algoritma.v3i2.431)).

Pada sistem yang diamati, terdapat **lima kasir aktif (server)** yang masing-masing melayani pelanggan (*customer*) secara independen. Setiap kasir memiliki satu mesin kasir, satu petugas yang bertanggung jawab untuk memproses transaksi, dan satu petugas yang membantu mengatur barang belanjaan pelanggan. Secara umum, sistem antrian dapat digambarkan melalui tiga tahapan utama, yaitu kedatangan pelanggan, proses menunggu (antri), dan pelayanan:
- Pelanggan yang telah berbelanja akan menuju area kasir untuk melakukan pembayaran dan dapat memilih kasir yang ingin dituju, umumnya berdasarkan panjang antrian atau lokasi kasir yang terdekat.
- Proses pelayanan di kasir meliputi proses pemindaian barang belanjaan menggunakan *scanner*, perhitungan total harga, serta proses pembayaran.
- Setelah transaksi selesai, pelanggan meninggalkan area kasir yang menandakan bahwa pelanggan telah keluar dari sistem.

Sistem antrian ini termasuk ke dalam kategori *multi-server queueing system* karena terdapat beberapa server yang bekerja secara bersamaan untuk melayani pelanggan. Dalam teori antrian, sistem ini dimodelkan sebagai **M/M/5**, yang berarti sistem memiliki pola kedatangan acak (*Markovian arrival*), waktu pelayanan acak (*Markovian service time*), dan lima server yang beroperasi paralel. Pada proyek ini, dilakukan observasi langsung di lokasi untuk mencatat data kedatangan pelanggan, waktu mulai pelayanan, dan waktu berakhirnya pelayanan pada pukul **20:15:00 WITA sampai dengan 21:47:17 WITA**.

Tujuan utama dari pemodelan dan simulasi ini adalah untuk menganalisis kinerja sistem antrian kasir supermarket dalam rangka memastikan pelayanan yang efektif dan efisien, memperoleh *insight* serta rekomendasi bagi manajemen dalam meningkatkan efisiensi pelayanan, dan memberikan gambaran realistis mengenai performa operasional guna mengoptimalkan sumber daya yang dimiliki.

## B. Diagram Model dan Penjelasan Model

Berdasarkan karakteristiknya, sistem ini dikategorikan sebagai sistem antrian *multi-server* dengan notasi **M/M/5**, yang berarti:
1. **M (*Markovian Arrival*)**: Waktu antar kedatangan pelanggan bersifat acak dan mengikuti distribusi Poisson ([Hanikmah dkk., 2019](https://doi.org/10.26714/jsunimus.7.2.2019.%2525p)), dengan estimasi rata-rata waktu antar kedatangan **0,533 menit** ($\lambda = 1,875383\text{ pelanggan/menit}$).
2. **M (*Markovian Service*)**: Waktu pelayanan bersifat acak dan mengikuti distribusi Eksponensial, dengan estimasi rata-rata durasi pelayanan **1,864 menit** ($\mu = 0,536339\text{ pelanggan/menit}$ per kasir).
3. **Angka 5**: Menunjukkan jumlah server atau kasir aktif yang melayani pelanggan secara paralel.

Berikut adalah diagram model sistem antrian kasir supermarket:

![Diagram Model] (diagram_model.png)

### Komponen-Komponen Model Sistem Antrian

- **a. Server (Kasir)**: Dalam model sistem antrian ini, setiap kasir berperan sebagai *server* yang bertugas memberikan layanan kepada pelanggan yang datang untuk melakukan pembayaran. Pada observasi proyek ini, terdapat lima kasir aktif yang berfungsi sebagai lima *server* dalam sistem antrian supermarket.
- **b. Pelanggan (Obyek)**: Obyek pada sistem ini adalah pelanggan, di mana setiap pelanggan akan dilayani oleh *server* (kasir). Proses dimulai ketika pelanggan menuju area kasir sambil membawa barang belanjaan, memilih salah satu kasir yang tersedia, dan menunggu giliran untuk dilayani. Ketika tiba gilirannya, pelanggan menyerahkan barang belanjaan kepada kasir untuk diproses, lalu meninggalkan area kasir dan keluar dari sistem setelah seluruh transaksi pembayaran selesai.
- **c. Kedatangan Pelanggan (*Arrival Process*)**: Pelanggan datang ke area kasir secara acak dengan waktu antar kedatangan yang bervariasi. Data kedatangan diperoleh dari hasil observasi langsung di lokasi, di mana setiap waktu kedatangan pelanggan dicatat berdasarkan jam aktual saat pelanggan masuk ke area kasir.
- **d. Antrian (*Queue*)**: Ketika seluruh kasir sedang sibuk, pelanggan harus menunggu di antrian. Berdasarkan hasil observasi yang dilakukan, sistem antrian di supermarket cenderung terpisah, di mana setiap kasir memiliki baris pelanggannya masing-masing. Antrian yang panjang mengindikasikan pelayanan yang kurang optimal karena mengakibatkan pelanggan harus menunggu lebih lama untuk dilayani, mengganggu kenyamanan, serta menurunkan kepuasan dan citra bisnis ([Syaharani dkk., 2024](https://doi.org/10.37090/indstrk.v8i4.1710)).
- **e. Pelayanan oleh Server (*Service Process*)**: Setiap kasir bertindak sebagai *server* yang memberikan pelayanan kepada pelanggan satu per satu. Proses pelayanan mencakup pemindaian barang, perhitungan total harga, serta proses pembayaran (tunai atau non-tunai). Lama waktu pelayanan berbeda-beda tergantung jumlah dan jenis barang belanjaan pelanggan.
- **f. Keberangkatan (*Departure*)**: Setelah pelanggan selesai dilayani, mereka meninggalkan area kasir dan keluar dari sistem. Data waktu keberangkatan ini digunakan untuk menghitung waktu total pelanggan berada di dalam sistem, termasuk waktu tunggu dan waktu pelayanan.

## C. Distribusi Data

Pada saat melakukan observasi sistem, data yang diperoleh mencakup informasi mengenai waktu kedatangan pelanggan ke sistem (`Arrival Time`), waktu pelayanan dimulai (`Service Start Time`), waktu pelayanan diselesaikan (`Service End Time`), dan kasir tempat pelayanan berlangsung (`Server`). Total data hasil observasi mencakup **161 pelanggan** yang terdistribusi pada 5 kasir aktif:
- **Kasir 1 (Server 1)**: Pelanggan 1 – 37 (37 pelanggan)
- **Kasir 2 (Server 2)**: Pelanggan 38 – 68 (31 pelanggan)
- **Kasir 3 (Server 3)**: Pelanggan 69 – 99 (31 pelanggan)
- **Kasir 4 (Server 4)**: Pelanggan 100 – 131 (32 pelanggan)
- **Kasir 5 (Server 5)**: Pelanggan 132 – 161 (30 pelanggan)

Untuk perhitungan distribusi waktu tunggu dan waktu pelayanan pada `Distribusi_Data.ipynb`, digunakan Python beserta library `pandas`, `numpy`, dan `matplotlib` dengan aturan Sturges ($k = \lceil \log_2(N) \rceil$ dan $p = \frac{\text{Range}}{k}$):

| Metrik Distribusi Statistik | Waktu Tunggu (Menit) | Waktu Pelayanan (Menit) |
| :--- | ---: | ---: |
| **Nilai Minimum (`Min`)** | 2,933 | 0,283 |
| **Nilai Maksimum (`Max`)** | 22,900 | 9,567 |
| **Rentang Data (`Range`)** | 19,967 | 9,284 |
| **Banyak Kelas Interval (`Kelas` / $k$)** | 8 | 8 |
| **Panjang Kelas Interval (`Panjang Kelas` / $p$)** | 2,496 | 1,161 |


![Distribusi Data] (distribusi_data.png)


## D. Rata-Rata Waktu Tunggu dan Pelayanan Objek dalam Sistem

Pada sistem antrian pelayanan pelanggan di supermarket, waktu tunggu dan waktu pelayanan merupakan dua metrik utama yang diperlukan untuk mengevaluasi efisiensi sistem antrian dan kinerja pelayanan:
- **Rata-rata waktu tunggu** menunjukkan rata-rata waktu menunggu pelanggan pada antrian sebelum dilayani oleh kasir.
- **Rata-rata waktu pelayanan** menunjukkan rata-rata waktu yang dibutuhkan oleh kasir untuk melayani seorang pelanggan hingga transaksi berhasil dilakukan.

Rumus yang digunakan untuk menghitung rata-rata waktu tunggu dan rata-rata waktu pelayanan adalah sebagai berikut:

$$\text{Rata-rata Waktu Tunggu} = \frac{\text{Total Waktu Tunggu}}{\text{Jumlah Pelanggan}}$$

$$\text{Rata-rata Waktu Pelayanan} = \frac{\text{Total Waktu Pelayanan}}{\text{Jumlah Pelanggan}}$$

Berdasarkan hasil perhitungan menggunakan Python (`pandas`) terhadap **161 pelanggan**:
- **Total Waktu Tunggu**: `1884,53 menit` $\rightarrow$ **Rata-rata Waktu Tunggu** = $\frac{1884,53}{161} =$ **`11,71 menit`** per pelanggan.
- **Total Waktu Pelayanan**: `300,18 menit` $\rightarrow$ **Rata-rata Waktu Pelayanan** = $\frac{300,18}{161} =$ **`1,86 menit`** per pelanggan.

## E. Insight dan Rekomendasi dari Pemodelan yang Telah Dibuat

Simulasi kejadian diskrit (*Discrete-Event Simulation*) dijalankan pada `G1_simulation_supermarket.ipynb` menggunakan `SimPy` untuk membandingkan kondisi nyata (*Real Condition*) dengan tiga skenario eksperimen:

| Experiment | Scenario (Number of Cashier) | Average Utilization (%) | Average Time in System (minutes) | Average Waiting Time (minutes) | Total Work Item Completed |
| :--- | :--- | ---: | ---: | ---: | ---: |
| **Real Condition** | **5 (Default)** | 39,4243 | 4,0023 | 2,1378 | 161 |
| **Vary Cashier** | **4** | 45,9396 | 5,2015 | 3,3234 | 149 |
| **Vary Cashier** | **6** | 32,8536 | 4,0023 | 2,1378 | 161 |
| **High Arrival (Rush Hour)** | **5** | 30,5439 | 5,5768 | 3,6059 | 118 |
| **High Arrival (Rush Hour)** | **6** | 26,2924 | 6,2210 | 4,2022 | 119 |
| **Single Queue** | **5 (All Cashier Shared)** | **69,9969** | **2,1947** | **0,2914** | **266** |

### 1. Kondisi Nyata (Hasil Pengamatan — 5 Kasir)
Hasil simulasi dari data riil menunjukkan bahwa rata-rata tingkat pemanfaatan (*utilization*) kasir berada pada kisaran **39–40%**, dengan rata-rata waktu pelanggan berada dalam sistem sekitar **4 menit**, dan rata-rata waktu tunggu simulasi sekitar **2 menit**. Nilai ini menandakan bahwa arus pelanggan relatif lancar, dan kapasitas pelayanan masih memiliki ruang *idle* sekitar **60%** dari waktu operasional. Artinya, beban kerja kasir belum mencapai titik jenuh. Jumlah 5 kasir pada kondisi normal tergolong memadai dan efisien—tidak terjadi antrian panjang yang signifikan, tetapi juga tidak terlalu banyak waktu *idle* yang berlebihan.

### 2. Eksperimen 1 (Penambahan dan Pengurangan Jumlah Kasir)
Saat jumlah kasir dikurangi menjadi **4**, tingkat pemanfaatan meningkat menjadi **45,94%** (karena beban dibagi ke lebih sedikit server), tetapi waktu tunggu pelanggan bertambah sekitar **55%** (menjadi **3,32 menit**) dan jumlah pelanggan terlayani turun menjadi **149 orang**. Saat kasir ditambah menjadi **6**, antrian memang lebih cepat terurai, tetapi peningkatan efisiensi waktu tunggu tidak signifikan, sementara pemanfaatan per kasir justru turun menjadi hanya **~32%**. Hal ini menunjukkan titik optimal kasir berada di sekitar **5 server**, karena menyeimbangkan antara efisiensi sumber daya dan kenyamanan pelanggan.
- **Rekomendasi**: Tetap gunakan **5 kasir aktif** pada jam normal. Bila ada indikasi antrian mulai meningkat (misalnya *rush hour*), buka 1 kasir tambahan sementara. Jangan menambah kasir permanen karena akan menurunkan efisiensi tenaga kerja tanpa manfaat nyata bagi pelanggan.

### 3. Eksperimen 2 (Skenario Lonjakan Pelanggan — *Rush Hour / High Arrival Rate*)
Ketika laju kedatangan pelanggan meningkat (*rush hour* dengan *burst factor* 1,5x), jumlah pelanggan yang selesai dilayani dalam durasi yang sama turun cukup signifikan, dari **161** menjadi sekitar **118 orang**. Berbeda dengan eksperimen sebelumnya, tingkat pemanfaatan kasir di eksperimen ini tidak naik drastis karena sistem menjadi jenuh di antrian (banyak pelanggan yang menunggu lama sebelum dilayani). Menambah kasir dari 5 ke 6 juga tidak cukup membantu karena pertumbuhan kedatangan pelanggan lebih besar daripada kapasitas tambahan pelayanan (waktu tunggu tetap meningkat di **3,61–4,20 menit**).
- **Rekomendasi**: Untuk mengatasi kondisi *rush hour*, strategi terbaik bukan sekadar menambah kasir, melainkan:
  - Menambah kapasitas pelayanan sementara, misalnya dengan membuka **kasir ekspres** untuk pembelian maksimal 10 item.
  - Mengatur ulang jadwal kedatangan pelanggan, misalnya melalui **promo *non-peak hours***.
  - Mengimplementasikan sistem **antrian tunggal (*single queue*)** agar distribusi pelanggan ke kasir lebih seimbang.
  - Menggunakan prediksi *rush hour* berbasis data historis untuk menentukan kapan perlu membuka kasir tambahan.

### 4. Eksperimen 3 (*Single Queue*)
Dengan penerapan *single queue* (satu antrian terpusat untuk seluruh 5 kasir), rata-rata tingkat pemanfaatan kasir naik menjadi **69,99%**, sementara waktu pelanggan berada di dalam sistem turun menjadi **2,19 menit** dan waktu tunggu hampir mencapai nol (**~0,29 menit** atau ~17,5 detik). Jumlah pelanggan yang dapat dilayani dalam waktu simulasi juga meningkat signifikan hingga **266 pelanggan**, jauh lebih tinggi dibandingkan kondisi nyata dan eksperimen sebelumnya. *Single queue* meratakan beban antar kasir, mengurangi variabilitas waktu tunggu antar pelanggan, dan memanfaatkan sumber daya secara lebih efektif.
- **Rekomendasi**: Menerapkan ***single queue*** sebagai kebijakan antrian utama untuk meningkatkan kecepatan pelayanan dan memangkas waktu tunggu pelanggan.

## Struktur File Repositori

```text
├── G1_simulation_supermarket.ipynb         # Notebook utama simulasi DES (SimPy), animasi MP4, dan 3 skenario eksperimen
├── Distribusi_Data.ipynb                 # Notebook pengolahan waktu tunggu/pelayanan dan visualisasi histogram Sturges
├── HASIL_DATA_PENGAMATAN_DES.csv         # Dataset observasi lapangan (161 pelanggan, 5 kasir, 20:15 - 21:47 WITA)
├── diagram_model.png                     # Gambar 1: Diagram Model Sistem Antrian M/M/5
├── distribusi_data.png                   # Grafik Histogram Distribusi Waktu Tunggu dan Waktu Pelayanan
├── requirements.txt                      # Daftar dependensi library Python
└── README.md                             # Dokumentasi lengkap proyek
```

## Cara Menjalankan Proyek

1. **Instalasi Dependensi**:
   ```bash
   pip install -r requirements.txt
   ```
   *(Catatan: Untuk mengekspor animasi simulasi ke file `.mp4`, pastikan `ffmpeg` dan `imagemagick` telah terinstal).*

2. **Menjalankan Analisis Distribusi Data (`Distribusi_Data.ipynb`)**:
   - Pastikan path file CSV mengarah ke dataset observasi (`HASIL_DATA_PENGAMATAN_DES.csv` atau `data_antrian.csv`).
   - Jalankan seluruh sel untuk menghasilkan `data_antrian_hasil.csv`, statistik interval kelas, serta histogram distribusi waktu tunggu dan pelayanan.

3. **Menjalankan Simulasi & Eksperimen (`G1_simulation_supermarket.ipynb`)**:
   - Pastikan variabel `DATA_CSV` mengarah ke `HASIL_DATA_PENGAMATAN_DES.csv`.
   - Jalankan seluruh sel untuk mengeksekusi simulasi *real-data replay*, membuat video animasi `real_simulation_supermarket_queue.mp4`, dan menjalankan ketiga skenario eksperimen (`DES_experiment_results_all.csv`).

  
- **Daftar Pustaka**:
  - Hanikmah, Y., Sugito, S., & Mustafid, M. (2019). Penerapan Teori Antrean untuk Analisis Kinerja Sistem Pelayanan di Gerbang Tol Gayamsari. *Jurnal Statistika Universitas Muhammadiyah Semarang*, 7(2). [https://doi.org/10.26714/jsunimus.7.2.2019.%2525p](https://doi.org/10.26714/jsunimus.7.2.2019.%2525p)
  - Ibrahim, N., Nasib, S. K., Nuha, A. R., Katili, M. R., Nurwan, N., & Wungguli, D. (2025). Analisis Sistem Antrian dengan Model M/M/C dalam Meningkatkan Efektivitas Kinerja Sistem. *Algoritma: Jurnal Matematika, Ilmu Pengetahuan Alam, Kebumian dan Angkasa*, 3(2), 20–34. [https://doi.org/10.62383/algoritma.v3i2.431](https://doi.org/10.62383/algoritma.v3i2.431)
  - Syaharani, A. D., Saputro, M. R., Noor, F. M., Dellanovia, S., & Shofa, M. J. (2024). Analisis Simulasi Sistem Antrian Pemesanan Makanan di Restoran di Kota Cilegon. *Industrika: Jurnal Ilmiah Teknik Industri*, 8(4), 813–824. [https://doi.org/10.37090/indstrk.v8i4.1710](https://doi.org/10.37090/indstrk.v8i4.1710)

## Tim Penyusun

- **Nadia Indriani Sumardi**
- **Novi Yuliyanti**
- **Saida Muthia Khadijah**
- **Devi Permata Sari Lam**
- **Amanda Tarina Allo Bunga'**