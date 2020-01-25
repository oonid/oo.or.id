---
title: "Membuat Kode Algoritma Perceptron"
date: 2018-12-02T23:46:00+08:00
publishDate: 2018-12-02T23:46:00+08:00
image: "/images/2018/pytorchudacityscholar.png"
url: "/content/membuat-kode-algoritma-perceptron"
tags: ["Udacity", "Machine Learning"]
draft: false
---

{{< figure src="/images/2018/pytorchudacityscholar.webp" title="Membuat Kode Algoritma Perceptron" alt="Membuat Kode Algoritma Perceptron" position="center" >}}


> Tulisan ini adalah catatan belajar PyTorch Udacity Scholar nd188. Semua gambar milik kelas tersebut.

Ini adalah halaman tambahan dari halaman [Pengenalan Neural Networks](/content/pengenalan-neural-networks) yang membahas bagian membuat kode untuk algoritma perceptron. **PERINGATAN:** Di halaman ini terdapat jawaban dari quiz, sehingga <u>tutup kembali halaman</u> ini jika tidak ingin melanjutkan.

_Pseudocode_ yang akan digunakan dalam pembuatan perceptron menggunakan trik di atas adalah:

> 1. Start with random weights: $w\_1, ..., w\_n, b$
>
> 2. For every misclassified point $(x\_1, ..., x\_n)$:
>
>    2.1. If prediction = 0:
>
>       - For i = 1 ... n
>         - Change $w\_i + \alpha x\_i$
>       - Change $b$ to $b + \alpha$
>
>    2.2. If prediction = 1:
>
>       - For i = 1 ... n
>         - Change $w\_i - \alpha x\_i$
>       - Change $b$ to $b - \alpha$

Dengan catatan:

* `prediction = 0` artinya seharusnya positif tapi salah klasifikasi di area negatif, sehingga digeser dengan menambahkan $\alpha$.
* `prediction = 1` artinya seharusnya negatif tapi salah klasifikasi di area positif, sehingga digeser dengan mengurangkan $\alpha$.
* $\alpha$ adalah _learning rate_

### Kode Awal yang Sudah Disediakan

Ini adalah kode awal yang perlu dilengkapi **perceptron.py**. Dengan dataset ada di bagian bawah halaman ini.

```python
import numpy as np
# Setting the random seed, feel free to change it and see different solutions.
np.random.seed(42)

def stepFunction(t):
    return 1 if t >= 0 else 0

def prediction(X, W, b):
    return stepFunction((np.matmul(X,W)+b)[0])

# The function should receive as inputs the data X, the labels y,
# the weights W (as an array), and the bias b,
# update the weights and bias W, b, according to the perceptron algorithm,
# and return W and b.
def perceptronStep(X, y, W, b, learn_rate = 0.01):
    # TODO: Fill in the code below to implement the perceptron trick.
    return W, b
    
# This function runs the perceptron algorithm repeatedly on the dataset,
# and returns a few of the boundary lines obtained in the iterations,
# for plotting purposes.
# Feel free to play with the learning rate and the num_epochs.
def trainPerceptronAlgorithm(X, y, learn_rate = 0.01, num_epochs = 25):
    x_min, x_max = min(X.T[0]), max(X.T[0])
    y_min, y_max = min(X.T[1]), max(X.T[1])
    W = np.array(np.random.rand(2,1))
    b = np.random.rand(1)[0] + x_max
    boundary_lines = []  # These are the solution lines that get plotted
    for i in range(num_epochs):  # In each epoch, we apply the perceptron step.
        W, b = perceptronStep(X, y, W, b, learn_rate)
        boundary_lines.append((-W[0]/W[1], -b/W[1]))
    return boundary_lines

```



### Pembahasan dan Pembuatan Kode

Kode program di sistem Udacity di awal akan memanggil fungsi `trainPerceptronAlgorithm()`, dengan input sbb:

* `X` adalah masukan atau input data latih dari dataset di bagian bawah halaman ini. Hanya 2 kolom pertama, yang digunakan sebagai pasangan `(x, y)` untuk plot di koordinat kartesian.
* `y` adalah label atau output data latih dari dataset di bagian bawah halaman ini. Kolom ke-3 (terakhir) yang digunakan sebagai hasil klasifikasi sesuai masukan yang ada di `X`.
* `X.T` dalam notasi NumPy artinya `X transpose`.
  *  `X.T[0]` artinya yang tadinya merupakan kolom pertama, yakni berisi data semua nilai `x` yang diplot di koordinat kartesian, diubah jadi baris pertama, sebuah vector. Dicari nilai min dan max untuk kebutuhan plot data.
  *  `X.T[1]` artinya yang tadinya kolom kedua, yang berisi data semua nilai `y` yang diplot di koordinat kartesian, diubah menjadi baris kedua, sebuah vector. Dicari nilai min dan max untuk kebutuhan plot data.
* `W` adalah sebuah matrix `2 x 1` (2 baris, 1 kolom), yang diinisialisasi dengan nilai random dari NumPy.
* `b` adalah bias, merupakan sebuah skalar, yang diinisialisasi dengan nilai random dari NumPy, kemudian ditambahkan `x_max` pada saat inisialisasi agar hasil plot keluarannya lebih fokus atau rapat (ini hanya asumsi saja, karena sudah dicoba tanpa menambahkan `x_max` bedanya adalah proses plot nya jaraknya lebih renggang). 
* (proses `perceptronStep()`)
* untuk menampilkan sejumlah garis hasil penggeseran, datanya disimpan di `boundary_lines`. Data _weights_ `W` dan _bias_ `b` yang didapatkan dari `perceptronStep()` ditambahkan sebagai `(-W[0]/W[1], -b/W[1])`.
  * `W[0]` dan `W[1]` karena _weights_ adalah sebuah matrix `2 x 1`. `W[0]` adalah bobot untuk `x` kartesian, `W[1]` adalah bobot untuk `y` kartesian.
  * Kemudian menggunakan nilai negatif dalam `-W[0]/W[1]` dan `-b/W[1]` diprediksi karena mekanisme plotting yang digunakan di kelas membutuhkan gradien negatif tersebut.

```python
def perceptronStep(X, y, W, b, learn_rate = 0.01):
    for i in range(0, X.shape[0]):
        y_hat = prediction(X[i], W, b)
        if y_hat - y[i] == -1:  # misclasified y_hat == 0
            for _w, _x in zip(W, X[i]):  # W.shape == X[i].shape
                _w += learn_rate * _x
            b += learn_rate
        elif y_hat - y[i] == 1:  # misclasifief y_hat == 1
            for _w, _x in zip(W, X[i]):
                _w -= learn_rate * _x
            b -= learn_rate
        else: pass  # i'm good
    return W, b
```

Catatan:

* hati-hati untuk tidak memanggil `prediction(X, W, b)` karena fungsi tersebut tetap mengembalikan prediksi. `X` adalah kumpulan berisi titik-titik yang akan diprediksi, sehingga proses prediksi seharusnya dengan iterasi terhadap `X`, untuk setiap titik. dalam kode di atas dituliskan sebagai `prediction(X[i], W, b)`.

* pada kode di atas digunakan fungsi `zip(W, X[i])`, dimana sebetulnya akan melakukan iterasi untuk setiap element dari `W` dan dari `X[i]` asalkan keduanya punya ukuran `.shape` yang sama. di jawaban yang disediakan Udacity, tidak menggunakan iterasi, langsung diakses indexnya, untuk `W[0] dengan X[i][0]` dan `W1 dengan X[i][1]`.

* ada opsi lain yang kodenya mengecek **misclasified** terlebih dahulu, sehingga tidak perlu melakukan operasi `y_hat - y[i]`, contoh kodenya seperti ini.

  ```python
  y_hat = prediction(X[i], W, b)
  if y_hat != y[i]:  # misclasified, get closer
      if y_hat == 0:
          pass # process with + learn_rate
      else:  # y_hat == 1
          pass # process with - learn_rate
  else:  # i'm good
      pass # do nothing
  ```

  

*  menarik untuk menghitung total **misclasified** sejak awal _epoch_, sehingga menjadi patokan, apakah total _misclasified_ tersebut menurut dalam iterasi atau tidak. ada kemungkinan, tidak perlu jumlah _epoch_ terlalu banyak karena sebetulnya total _misclasified_-nya pun sudah tidak turun lagi. nantinya ini akan menjadi semacam fungsi error, tapi nanti dijelaskan lebih lanjut bahwa fungsi itu seharusnya bukan bilang bulat (_discrete_), tapi pecahan (_continuous_).

### Dataset

Ini adalah dataset **data.csv** yang digunakan dalam `trainPerceptronAlgorithm()`:

```csv
0.78051,-0.063669,1
0.28774,0.29139,1
0.40714,0.17878,1
0.2923,0.4217,1
0.50922,0.35256,1
0.27785,0.10802,1
0.27527,0.33223,1
0.43999,0.31245,1
0.33557,0.42984,1
0.23448,0.24986,1
0.0084492,0.13658,1
0.12419,0.33595,1
0.25644,0.42624,1
0.4591,0.40426,1
0.44547,0.45117,1
0.42218,0.20118,1
0.49563,0.21445,1
0.30848,0.24306,1
0.39707,0.44438,1
0.32945,0.39217,1
0.40739,0.40271,1
0.3106,0.50702,1
0.49638,0.45384,1
0.10073,0.32053,1
0.69907,0.37307,1
0.29767,0.69648,1
0.15099,0.57341,1
0.16427,0.27759,1
0.33259,0.055964,1
0.53741,0.28637,1
0.19503,0.36879,1
0.40278,0.035148,1
0.21296,0.55169,1
0.48447,0.56991,1
0.25476,0.34596,1
0.21726,0.28641,1
0.67078,0.46538,1
0.3815,0.4622,1
0.53838,0.32774,1
0.4849,0.26071,1
0.37095,0.38809,1
0.54527,0.63911,1
0.32149,0.12007,1
0.42216,0.61666,1
0.10194,0.060408,1
0.15254,0.2168,1
0.45558,0.43769,1
0.28488,0.52142,1
0.27633,0.21264,1
0.39748,0.31902,1
0.5533,1,0
0.44274,0.59205,0
0.85176,0.6612,0
0.60436,0.86605,0
0.68243,0.48301,0
1,0.76815,0
0.72989,0.8107,0
0.67377,0.77975,0
0.78761,0.58177,0
0.71442,0.7668,0
0.49379,0.54226,0
0.78974,0.74233,0
0.67905,0.60921,0
0.6642,0.72519,0
0.79396,0.56789,0
0.70758,0.76022,0
0.59421,0.61857,0
0.49364,0.56224,0
0.77707,0.35025,0
0.79785,0.76921,0
0.70876,0.96764,0
0.69176,0.60865,0
0.66408,0.92075,0
0.65973,0.66666,0
0.64574,0.56845,0
0.89639,0.7085,0
0.85476,0.63167,0
0.62091,0.80424,0
0.79057,0.56108,0
0.58935,0.71582,0
0.56846,0.7406,0
0.65912,0.71548,0
0.70938,0.74041,0
0.59154,0.62927,0
0.45829,0.4641,0
0.79982,0.74847,0
0.60974,0.54757,0
0.68127,0.86985,0
0.76694,0.64736,0
0.69048,0.83058,0
0.68122,0.96541,0
0.73229,0.64245,0
0.76145,0.60138,0
0.58985,0.86955,0
0.73145,0.74516,0
0.77029,0.7014,0
0.73156,0.71782,0
0.44556,0.57991,0
0.85275,0.85987,0
0.51912,0.62359,0

```

