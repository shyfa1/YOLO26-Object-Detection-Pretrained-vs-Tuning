# YOLO26-Object-Detection-Pretrained-vs-Tuning
Perbandingan model YOLO26 pretrained dan tuning menggunakan dataset custom untuk mendeteksi objek landing pad berdasarkan perubahan ukuran gambar (image size).

## 1. Deskripsi Proyek

Proyek ini menggunakan YOLO26 untuk mendeteksi kotak berwarna kuning yang merepresentasikan landing pad. Dataset yang digunakan merupakan dataset custom dengan satu kelas, yaitu square dengan class ID 0.

Eksperimen dilakukan dengan membandingkan model pretrained sebagai baseline dan model tuning yang menggunakan perubahan ukuran gambar input. Hasil kedua eksperimen dianalisis untuk mengetahui perbedaan performa deteksi objek.

## 2. Tujuan

- Melatih model YOLO26 menggunakan dataset custom.
- Membandingkan performa model pretrained dengan model yang telah melalui perubahan konfigurasi.
- Menganalisis hasil training berdasarkan nilai loss dan metrik evaluasi.

## 3. Dataset

Dataset yang digunakan berisi 3.253 gambar kotak kuning yang merepresentasikan landing pad. Setiap objek diberi anotasi dengan satu kelas, yaitu square.

| Informasi | Keterangan |
|---|---|
| Sumber dataset | Dataset custom |
| Objek | Kotak kuning sebagai representasi *landing pad* |
| Jumlah gambar | 3.253 |
| Jumlah kelas | 1 |
| Nama kelas | `square` |
| Class ID | `0` |

Anotasi objek digunakan untuk melatih model agar dapat mengenali dan menentukan lokasi kotak kuning pada gambar. Dataset kemudian digunakan dalam eksperimen pretrained dan tuning untuk membandingkan hasil deteksi dari kedua konfigurasi model.


## 4. Eksperimen

Membandingkan model YOLO26 pretrained sebagai baseline (E1) dan model tuning (E3) yang menggunakan ukuran gambar input berbeda.

| Parameter | E1 Pretrained | E3 Tuning |
|---|---|---|
| Model | `yolo26n.pt` | `yolo26n.pt` |
| Dataset | `dataset_v1/data.yaml` | `dataset_v1/data.yaml` |
| Epoch | 30 | 30 |
| Batch size | 16 | 16 |
| Image size (`imgsz`) | 416 | 640 |
| Pretrained | `true` | `true` |

Perbedaan utama kedua eksperimen terletak pada ukuran gambar input, yaitu 416 pada E1 dan 640 pada E3. Hasil training dan evaluasi kemudian dibandingkan untuk melihat pengaruh perubahan ukuran gambar terhadap performa deteksi objek.


## 5. Analisis Perbandingan E1 Pretrained dan E3 Tuning

### 5.1 Perbandingan Loss

| Metrik | E1 Pretrained | E3 Tuning |
|---|---:|---:|
| Train box loss | 0.34438 | 0.33900 |
| Train classification loss | 0.11092 | 0.11811 |
| Train L1 loss | 0.00411 | 0.00407 |
| Validation box loss | 0.35256 | 0.34786 |
| Validation classification loss | 0.10768 | 0.11606 |
| Validation L1 loss | 0.00539 | 0.00546 |

Pada box loss, E3 memperoleh nilai yang lebih rendah dibandingkan E1, baik pada data training maupun validasi. Validation box loss E3 sebesar 0.34786, sedangkan E1 sebesar 0.35256. Hasil ini menunjukkan bahwa E3 memiliki nilai kesalahan yang lebih rendah pada komponen loss prediksi kotak objek.

Namun, E1 memperoleh classification loss yang lebih rendah pada data training maupun validasi. Pada validation classification loss, E1 memperoleh 0.10768, sedangkan E3 memperoleh 0.11606. Untuk L1 loss, perbedaannya relatif kecil, dengan E1 memiliki validation L1 loss yang sedikit lebih rendah.

Hasil ini menunjukkan bahwa keunggulan E3 pada box loss tidak diikuti oleh penurunan pada seluruh komponen loss. Oleh karena itu, performa kedua eksperimen perlu dianalisis bersama metrik evaluasi lainnya.

### 5.2 Perbandingan Metrik Evaluasi

| Metrik | E1 Pretrained | E3 Tuning |
|---|---:|---:|
| Precision | 99.715% | 99.705% |
| Recall | 100% | 100% |
| mAP50 | 99.389% | 99.372% |
| mAP50-95 | 94.458% | 94.649% |

E1 memperoleh precision sebesar 99.715%, sedikit lebih tinggi dibandingkan E3 sebesar 99.705%. Pada mAP50, E1 juga memperoleh nilai sedikit lebih tinggi, yaitu 99.389%, dibandingkan E3 sebesar 99.372%.

Sebaliknya, E3 memperoleh mAP50-95 sebesar 94.649%, sedangkan E1 memperoleh 94.458%. Selisihnya sebesar 0.191 poin persentase. Metrik mAP50-95 mengevaluasi ketepatan deteksi pada beberapa ambang IoU, sehingga memberikan gambaran performa deteksi dengan kriteria yang lebih beragam.

Kedua eksperimen memperoleh recall sebesar 100% pada data validasi. Artinya, seluruh objek target pada data validasi berhasil ditemukan berdasarkan hasil evaluasi tersebut. Namun, nilai recall yang sama tidak berarti kedua model memiliki performa identik, karena perbedaan masih terlihat pada precision, mAP, dan nilai loss.

Secara keseluruhan, kedua eksperimen menunjukkan hasil validasi yang relatif sebanding. E1 unggul tipis pada precision dan mAP50, sedangkan E3 unggul tipis pada mAP50-95.

### 5.3 Grafik Training

#### E1 Pretrained

![Grafik Training E1 Pretrained](E1_pretrained_results.png)

#### E3 Tuning

![Grafik Training E3 Tuning](E3_tuning_results.png)

### 5.4 Pembahasan Hasil Tuning

Perubahan konfigurasi pada E3 berfokus pada ukuran gambar input dari 416 menjadi 640. Ukuran gambar yang lebih besar dapat mempertahankan lebih banyak detail visual, terutama ketika objek memiliki ukuran relatif kecil pada gambar.

Berdasarkan hasil evaluasi, E3 memperoleh performa yang mendekati E1 dan memiliki mAP50-95 yang sedikit lebih tinggi. Namun, E3 tidak unggul pada seluruh metrik. Hasil ini menunjukkan bahwa perubahan ukuran gambar input dapat memengaruhi performa model secara berbeda pada setiap metrik evaluasi.

## 6. Kesimpulan

Berdasarkan hasil evaluasi pada data validasi, E1 Pretrained dan E3 Tuning menunjukkan performa yang relatif sebanding. E1 memperoleh precision dan mAP50 yang sedikit lebih tinggi, sedangkan E3 memperoleh box loss yang lebih rendah dan mAP50-95 yang sedikit lebih tinggi.

Hasil tersebut menunjukkan bahwa perubahan ukuran gambar input dari 416 menjadi 640 menghasilkan perbedaan performa pada beberapa metrik evaluasi. E3 menunjukkan hasil yang lebih baik pada box loss dan mAP50-95, sedangkan E1 unggul pada precision dan mAP50. Dengan demikian, kedua konfigurasi memiliki keunggulan masing-masing dalam mendeteksi objek *landing pad*.
