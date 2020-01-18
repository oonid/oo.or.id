---
title: "Pengenalan Neural Networks"
date: 2018-12-01T23:46:00+08:00
publishDate: 2018-12-01T23:46:00+08:00
image: "/images/2018/pytorchudacityscholar.png"
url: "/content/pengenalan-neural-networks"
tags: ["Udacity", "Machine Learning"]
draft: false
---

{{< figure src="/images/2018/pytorchudacityscholar.webp" title="Pengenalan Neural Networks" alt="Pengenalan Neural Networks" position="center" >}}


> Tulisan ini adalah catatan belajar PyTorch Udacity Scholar nd188. Semua gambar milik kelas tersebut.

Neural Networks adalah inti dari Deep Learning, dimana sebetulnya berusaha menirukan bagaimana otak bekerja, dengan syaraf-syaraf yang mengirimkan informasi satu sama lain.

Deep Neural Networks adalah Neural Networks dengan banyak layer.

Membayangkan cara kerja Neural Network yang sederhana adalah membayangkan memisahkan kumpulan dua objek merah dan biru dengan sebuah garis. Dalam hal itu, kumpulan dua objek merah dan biru adalah data yang ada. Neural Networks akan mencari garis dengan posisi terbaik untuk memisahkan kumpulan kedua objek tersebut. Deep Neural Network mampu memisahkan dua kumpulan objek meskipun bentuknya kompleks, tidak semudah seperti garis lurus.

### Contoh Kasus Klasifikasi

Membuat klasifikasi untuk menentukan apakah seseorang diterima di sebuah kampus berdasarkan 2 informasi, hasil ujiannya dan nilainya di sekolah? dimana dua informasi ini akan digambarkan di koordinat kartesian 2 dimensi.

Kemudian melakukan hal yang dilakukan oleh semua algoritma Machine Learning, yaitu melihat sebaran dari data sebelumnya. Sehingga muncul kumpulan 2 objek, yakni kumpulan yang diterima di kampus dan kumpulan yang ditolak.

Dari kedua kumpulan objek tersebut, dibuat **pemisahannya dengan jelas** misalnya dengan sebuah garis, sehingga kita dapat melakukan **klasifikasi** dengan baik. Jka berada di kumpulan objek yang diterima, maka titik (orang) tersebut diterima di kampus. Jika sebuah titik dicek berada di kumpulan objek yang ditolak, maka titik (orang) tersebut akan ditolak di kampus.

Garis yang digunakan dalam pemisahan tersebut adalah **model** kita dalam menentukan penerimaan di kampus.

{{< figure src="/images/2018/nn001.webp" title="Klasifikasi Penerimaan di Kampus" alt="Klasifikasi Penerimaan di Kampus" position="center" >}}

### Persamaan Linear (_Linear Equation_)

Dalam matematika, persamaan linear (_linear equation_) untuk garis tersebut digambarkan sebagai persamaan dari 2 informasi: 

* **test** yaitu hasil ujiannya
* **grades** yaitu nilainya di sekolah

Dengan melakukan **prediksi** jika suatu nilai dimasukkan dalam persamaan tersebut hasilnya (_score_) lebih besar atau sama dengan 0 maka akan **diterima** di kampus, sebaliknya akan **ditolak**. Berikut adalah gambaran dari **model** berupa persamaan linear tersebut.

{{< figure src="/images/2018/nn002.webp" title="Menentukan Model Klasifikasi dengan Persamaan Linear" alt="Menentukan Model Klasifikasi dengan Persamaan Linear" position="center" >}}

Secara lebih umum, persamaan tersebut dapat dituliskan sebagai berikut:

> w~1~x~1~ + w~2~x~2~ + b = 0

Atau dalam notasi vector dapat dituliskan sebagai:

> Wx + b = 0
>
> W = (w~1~, w~2~)
>
> x = (x~1~, x~2~)

Dengan referensi **x** sebagai _input_, **W** sebagai _weight_, dan **b** sebagai _bias_.

Untuk titik (orang) **x~1~, x~2~** akan diprediksi _label_ sebagai **y**, dituliskan sebagai **y = label: 0 or 1**. Dimana jika diterima di kampus maka **label = 1** dan jika ditolak maka **label = 0**.

Prediksi tersebut dituliskan sebagai $\hat{y}$ (_y hat_).

> $\hat{y}$ = 1 if Wx + b >= 0
>
> $\hat{y}$ = 0 if Wx + b < 0

{{< figure src="/images/2018/nn003.webp" title="Model secara umum untuk Klasifikasi 2 objek" alt="Model secara umum untuk Klasifikasi 2 objek" position="center" >}}

Selanjutnya kita akan mencoba menambahkan satu kolom informasi lagi dengan **rank** untuk peringkat di kelas, selain dua sebelumnya yaitu **test** dan **grades**. Sekarang total menjadi 3 informasi, sehingga digambarkan sebagai koordinat 3-dimensi.

Jika sebelumnya, saat digambarkan dalam koordinat **2 dimensi**, pemisah antar kedua objek adalah **1 dimensi**, yaitu sebuah garis, maka dalam koordinat **3 dimensi**, pemisah antar kedua objek adalah **2 dimensi**, yaitu sebuah bidang. Gambarnya sebagai berikut.

{{< figure src="/images/2018/nn004.webp" title="Klasifikasi 2 objek dalam 3 jenis informasi" alt="Klasifikasi 2 objek dalam 3 jenis informasi" position="center" >}}

Melihat hal tersebut, jadi sebetulnya jika digambarkan dalam **n dimensi** maka pemisah antar kedua objek adalah **(n-1) dimensi**. :thinking: (ya, susah digambarkan, dibayangkan saja..)

### Perceptron

Perceptron adalah komponen pembangun (_building block_) dalam Neural Networks.

Dalam persamaan linear sebelumnya, dalam contoh kasus klasifikasi penentuan masuk (seleksi) kampus.

> 2x~1~ + x~2~ - 18 = 0

Artinya suatu **score** ditentukan dengan **2 * Test + Grades - 18**.

Dengan **prediksi** jika **score >= 0** maka diterima masuk kampus dan **score < 0** akan ditolak.

Sebagai contoh untuk **Test = 7** dan **Grades = 6** maka **score**-nya adalah **2**, karena **lebih besar dari 0** maka diterima di kampus.

Dalam gagasan tentang **Perceptron**, notasi perceptron dapat digambarkan sebagai berikut.

{{< figure src="/images/2018/nn005.webp" title="Notasi Perceptron untuk Klasifikasi Seleksi Kampus" alt="Notasi Perceptron untuk Klasifikasi Seleksi Kampus" position="center" >}}

Secara lengkap, notikasi persamaan linear dalam klasifikasi seleksi kampus di atas, digambarkan sebagai berikut. Dimana angka **2** (dari Test) dan angka **1** (dari Grades) akan menjadi label masing-masing untuk nilai dari **x~1~** dan **x~2~**, lihat pada dua panah di sebelah kiri. Sedangkan angka **-18** (dari bias) akan menjadi label untuk node yang di tengah.

{{< figure src="/images/2018/nn006.webp" title="Notasi Perceptron jenis 1" alt="Notasi Perceptron jenis 1" position="center" >}}

Gambaran lain dari perceptron yang lebih sering digunakan adalah memisahkan **bias sebagai input** seperti pada gambar berikut, sehingga node yang di tengah akan memroses perkalian dari semua input.

{{< figure src="/images/2018/nn007.webp" title="Notasi Perceptron jenis 2" alt="Notasi Perceptron jenis 2" position="center" >}}

Sehingga lengkap dengan proses prediksi, akan membandingkan hasil kalkulasinya apakah lebih besar atau sama dengan 0, sehingga mengembalikan 1 artinya diterima masuk kampus.

{{< figure src="/images/2018/nn008.webp" title="Notasi Perceptron jenis 2 untuk Klasifikasi Seleksi Kampus" alt="Notasi Perceptron jenis 2 untuk Klasifikasi Seleksi Kampus" position="center" >}}

Dalam kondisi secara umum, maka digambarkan bahwa ada **n input**, dengan nilai **x~1~, x~2~, ... x~n~** dan setiap nilai itu punya bobot **w~1~, w~2~, ... w~n~** serta satu nilai bias **b**. 

{{< figure src="/images/2018/nn009.webp" title="Notasi Perceptron untuk n input, n weight, 1 bias" alt="Notasi Perceptron untuk n input, n weight, 1 bias" position="center" >}}

Dalam notasi perceptron tersebut, sebetulnya proses prediksi tersebut secara implisit menggunakan sebuah fungsi yang disebut **step function**, dimana fungsi tersebut akan **menghasilkan 1** jika nilai masukannya sama dengan atau lebih besar dari 0, sebaliknya akan **menghasilkan 0** jika nilai masukannya lebih kecil dari 0.

Sehingga dituliskan ulang, notasi perceptron tersebut akan merupakan gabungan dua buah node atau fungsi.

{{< figure src="/images/2018/nn010.webp" title="Notasi Perceptron sebagai dua buah node atau fungsi" alt="Notasi Perceptron sebagai dua buah node atau fungsi" position="center" >}}

Fungsi yang pertama sebagai _linear function_ (_summation_ ditandai dengan $\sum$) dan fungsi yang kedua adalah _step function_ dimana ke depan akan digunakan beberapa macam _step function_ (ditandai dengan _step drawing_).

{{< figure src="/images/2018/nn011.webp" title="Notasi Perceptron sebagai dua buah node atau fungsi" alt="Notasi Perceptron sebagai dua buah node atau fungsi" position="center" >}}

### Membandingkan Artificial Neural Networks dengan Biological Neural Networks

Di gambar ini, sebelah kiri adalah Artificial Neural Networks, atau sering disingkat hanya Neural Networks yang baru saja dipelajari sebelumnya. Dan di sisi kanan adalah Biological Neural Networks adalah sistem syaraf yang ada di otak manusia, yang ditirukan oleh implementasi Neural Networks.

{{< figure src="/images/2018/nn012.webp" title="Artificial Neural Network dibandingkan dengan Biological Neural Networks" alt="Artificial Neural Network dibandingkan dengan Biological Neural Networks" position="centre" >}}







**TBD**