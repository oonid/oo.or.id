---
title: "Pengenalan Neural Networks"
date: 2018-12-01T23:46:00+08:00
publishDate: 2018-12-01T23:46:00+08:00
image: "/images/2018/pytorchudacityscholar.png"
url: "/content/pengenalan-neural-networks"
tags: ["Udacity", "Machine Learning"]
aliases: ["/intronn"]
math: true
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

> $w\_1x\_1 + w\_2x\_2 + b = 0$

Atau dalam notasi vector dapat dituliskan sebagai:

> $Wx + b = 0$

> $W = (w\_1, w\_2)$

> $x = (x\_1, x\_2)$

Dengan referensi **x** sebagai _input_, **W** sebagai _weight_, dan **b** sebagai _bias_.

Untuk titik (orang) $x\_1, x\_2$ akan diprediksi _label_ sebagai $y$, dituliskan sebagai **y = label: 0 or 1**. Dimana jika diterima di kampus maka **label = 1** dan jika ditolak maka **label = 0**.

Prediksi tersebut dituliskan sebagai $\hat{y}$ (_y hat_).

> $\hat{y}$ = 1 if $Wx + b >= 0$

> $\hat{y}$ = 0 if $Wx + b < 0$

{{< figure src="/images/2018/nn003.webp" title="Model secara umum untuk Klasifikasi 2 objek" alt="Model secara umum untuk Klasifikasi 2 objek" position="center" >}}

Selanjutnya kita akan mencoba menambahkan satu kolom informasi lagi dengan **rank** untuk peringkat di kelas, selain dua sebelumnya yaitu **test** dan **grades**. Sekarang total menjadi 3 informasi, sehingga digambarkan sebagai koordinat 3-dimensi.

Jika sebelumnya, saat digambarkan dalam koordinat **2 dimensi**, pemisah antar kedua objek adalah **1 dimensi**, yaitu sebuah garis, maka dalam koordinat **3 dimensi**, pemisah antar kedua objek adalah **2 dimensi**, yaitu sebuah bidang. Gambarnya sebagai berikut.

{{< figure src="/images/2018/nn004.webp" title="Klasifikasi 2 objek dalam 3 jenis informasi" alt="Klasifikasi 2 objek dalam 3 jenis informasi" position="center" >}}

Melihat hal tersebut, jadi sebetulnya jika digambarkan dalam **n dimensi** maka pemisah antar kedua objek adalah **(n-1) dimensi**. :thinking: (ya, susah digambarkan, dibayangkan saja..)

### Perceptron

Perceptron adalah komponen pembangun (_building block_) dalam Neural Networks.

Dalam persamaan linear sebelumnya, dalam contoh kasus klasifikasi penentuan masuk (seleksi) kampus.

> $2x\_1 + x\_2 - 18 = 0$

Artinya suatu **score** ditentukan dengan **2 * Test + Grades - 18**.

Dengan **prediksi** jika **score >= 0** maka diterima masuk kampus dan **score < 0** akan ditolak.

Sebagai contoh untuk **Test = 7** dan **Grades = 6** maka **score**-nya adalah **2**, karena **lebih besar dari 0** maka diterima di kampus.

Dalam gagasan tentang **Perceptron**, notasi perceptron dapat digambarkan sebagai berikut.

{{< figure src="/images/2018/nn005.webp" title="Notasi Perceptron untuk Klasifikasi Seleksi Kampus" alt="Notasi Perceptron untuk Klasifikasi Seleksi Kampus" position="center" >}}

Secara lengkap, notikasi persamaan linear dalam klasifikasi seleksi kampus di atas, digambarkan sebagai berikut. Dimana angka **2** (dari Test) dan angka **1** (dari Grades) akan menjadi label masing-masing untuk nilai dari $x\_1$ dan $x\_2$, lihat pada dua panah di sebelah kiri. Sedangkan angka **-18** (dari bias) akan menjadi label untuk node yang di tengah.

{{< figure src="/images/2018/nn006.webp" title="Notasi Perceptron jenis 1" alt="Notasi Perceptron jenis 1" position="center" >}}

Gambaran lain dari perceptron yang lebih sering digunakan adalah memisahkan **bias sebagai input** seperti pada gambar berikut, sehingga node yang di tengah akan memroses perkalian dari semua input.

{{< figure src="/images/2018/nn007.webp" title="Notasi Perceptron jenis 2" alt="Notasi Perceptron jenis 2" position="center" >}}

Sehingga lengkap dengan proses prediksi, akan membandingkan hasil kalkulasinya apakah lebih besar atau sama dengan 0, sehingga mengembalikan 1 artinya diterima masuk kampus.

{{< figure src="/images/2018/nn008.webp" title="Notasi Perceptron jenis 2 untuk Klasifikasi Seleksi Kampus" alt="Notasi Perceptron jenis 2 untuk Klasifikasi Seleksi Kampus" position="center" >}}

Dalam kondisi secara umum, maka digambarkan bahwa ada **n input**, dengan nilai $x\_1, x\_2, ... x\_n$ dan setiap nilai itu punya bobot $w\_1, w\_2, ... w\_n$ serta satu nilai bias **b**.

{{< figure src="/images/2018/nn009.webp" title="Notasi Perceptron untuk n input, n weight, 1 bias" alt="Notasi Perceptron untuk n input, n weight, 1 bias" position="center" >}}

Dalam notasi perceptron tersebut, sebetulnya proses prediksi tersebut secara implisit menggunakan sebuah fungsi yang disebut **step function**, dimana fungsi tersebut akan **menghasilkan 1** jika nilai masukannya sama dengan atau lebih besar dari 0, sebaliknya akan **menghasilkan 0** jika nilai masukannya lebih kecil dari 0.

Sehingga dituliskan ulang, notasi perceptron tersebut akan merupakan gabungan dua buah node atau fungsi.

{{< figure src="/images/2018/nn010.webp" title="Notasi Perceptron sebagai dua buah node atau fungsi" alt="Notasi Perceptron sebagai dua buah node atau fungsi" position="center" >}}

Fungsi yang pertama sebagai _linear function_ (_summation_ ditandai dengan $\sum$) dan fungsi yang kedua adalah _step function_ dimana ke depan akan digunakan beberapa macam _step function_ (ditandai dengan _step drawing_).

{{< figure src="/images/2018/nn011.webp" title="Notasi Perceptron sebagai dua buah node atau fungsi" alt="Notasi Perceptron sebagai dua buah node atau fungsi" position="center" >}}

### Membandingkan Artificial Neural Networks dengan Biological Neural Networks

Di gambar ini, sebelah kiri adalah Artificial Neural Networks, atau sering disingkat hanya Neural Networks, yang baru saja dipelajari sebelumnya. Dan di sisi kanan adalah Biological Neural Networks adalah sistem syaraf yang ada di otak manusia, yang ditirukan oleh implementasi Neural Networks.

{{< figure src="/images/2018/nn012.webp" title="Artificial Neural Network dibandingkan dengan Biological Neural Networks" alt="Artificial Neural Network dibandingkan dengan Biological Neural Networks" position="centre" >}}

### Menggunakan Perceptron sebagai Operator Logika (AND, OR, NOT)

Setelah mengetahui gagasan tentang perceptron, kali ini akan dicoba untuk menggunakannya di operasi yang sederhana, yaitu operasi logika (AND, OR, NOT). Atau ada juga yang menyebutkan dengan operasi boolean, karena yang dioperasikan hanya **0** dan **1** (seperti halnya False dan True).

### Menggabungkan Sejumlah Operasi pada Perceptron

Menggunakan perceptron **AND**, perceptron **OR**, dan perceptron **NOT** yang digabungkan menjadi operasi yang sama dengan **XOR** (_exclusive OR_).

#### Perceptron AND

Operator logika AND, akan **menghasilkan 1** (True) jika kedua input bernilai 1, selain itu akan **menghasilkan 0** (False).

{{< figure src="/images/2018/nn014.webp" title="Tabel Nilai untuk Perceptron AND" alt="Tabel Nilai untuk Perceptron AND" position="center" >}}

Dengan persamaan linear yang telah dibahas sebelumnya $w\_1x\_1 + w\_2x\_2 + b = 0$ kita akan melakukan implementasi dengan program python sederhana untuk mencari berapa nilai **weight1**, **weight2**, dan **bias** untuk implementasi dari perceptron **AND**.

```python
import pandas as pd

weight1 = 0.0  # TODO
weight2 = 0.0  # TODO
bias = 0.0     # TODO

# DON'T CHANGE ANYTHING BELOW
# Inputs and outputs
test_inputs = [(0, 0), (0, 1), (1, 0), (1, 1)]  # possible input for logic operations
correct_outputs = [False, False, False, True]  # correct output of operator AND
outputs = []

# Generate and check output. iterate for every element of test_inputs and correct_outputs
for test_input, correct_output in zip(test_inputs, correct_outputs):
    # y = w1 x1 + w2 x2 + bias
    linear_combination = weight1 * test_input[0] + weight2 * test_input[1] + bias
    output = int(linear_combination >= 0)
    is_correct_string = 'Yes' if output == correct_output else 'No'
    outputs.append([test_input[0], test_input[1], linear_combination, output, is_correct_string])

# Print output using pandas DataFrame .to_string()
num_wrong = len([output[4] for output in outputs if output[4] == 'No'])
output_frame = pd.DataFrame(outputs, columns=['Input 1', '  Input 2', '  Linear Combination', '  Activation Output', '  Is Correct'])
if not num_wrong:
    print('Nice!  You got it all correct.\n')
else:
    print('You got {} wrong.  Keep trying!\n'.format(num_wrong))
print(output_frame.to_string(index=False))

```

#### Perceptron OR

Operator logika OR, akan **menghasilkan 0** (False) jika kedua input bernilai 0, selain itu akan **menghasilkan 1** (True).

{{< figure src="/images/2018/nn015.webp" title="Tabel Nilai untuk Perceptron OR" alt="Tabel Nilai untuk Perceptron OR" position="center" >}}

#### Perceptron NOT

Operator logina NOT, akan **menghasilkan 0** (False) jika **satu input** bernilai 1, selain itu akan **menghasilkan 1** (True) jika **satu input** bernilai 0.

Perhatikan bahwa perceptron NOT ini hanya beroperasi pada satu masukan, sehingga jika perceptron memiliki dua masukan, pastikan yang mana **salah satu dari dua itu** yang sebetulnya dioperasikan.

```python
import pandas as pd

weight1 = 0.0  # TODO
weight2 = 0.0  # TODO
bias = 0.0     # TODO


# DON'T CHANGE ANYTHING BELOW
# Inputs and outputs
test_inputs = [(0, 0), (0, 1), (1, 0), (1, 1)]  # possible input for logic operations
# hints: the expected output related to the second value of input (tuple)
correct_outputs = [True, False, True, False]  # correct (expected) output of operator NOT
outputs = []

# Generate and check output. iterate for every element of test_inputs and correct_outputs
for test_input, correct_output in zip(test_inputs, correct_outputs):
    # y = w1 x1 + w2 x2 + bias
    linear_combination = weight1 * test_input[0] + weight2 * test_input[1] + bias
    output = int(linear_combination >= 0)
    is_correct_string = 'Yes' if output == correct_output else 'No'
    outputs.append([test_input[0], test_input[1], linear_combination, output, is_correct_string])

# Print output
num_wrong = len([output[4] for output in outputs if output[4] == 'No'])
output_frame = pd.DataFrame(outputs, columns=['Input 1', '  Input 2', '  Linear Combination', '  Activation Output', '  Is Correct'])
if not num_wrong:
    print('Nice!  You got it all correct.\n')
else:
    print('You got {} wrong.  Keep trying!\n'.format(num_wrong))
print(output_frame.to_string(index=False))
```

### Membangun (_Multi-layer_) Perceptron XOR dari Perceptron AND, OR, dan NOT

Operator logika XOR, akan **menghasilkan 1** (True) jika salah satu input bernilai 1 (eksklusif), selain itu akan **menghasilkan 0** (False).

{{< figure src="/images/2018/nn016.webp" title="Tabel Nilai untuk Perceptron XOR" alt="Tabel Nilai untuk Perceptron XOR" position="center" >}}

Sebetulnya operator logika XOR ini dapat dibangun dari sejumlah operator AND, OR, dan NOT. Salah satu referensinya di wiki XOR [^1] [^2].

Mengutip dari stackoverflow [^3]:

> So, XOR is just like OR, except it's false if A and B are true.
>
> So, (A OR B) AND (NOT (A AND B)), which is (A OR B) AND (A NAND B)

#### Multi-layer Perceptron XOR

Bisa digunakan untuk menjawab pertanyaan tentang **multi-layer perceptron** yang pertama dibuat:

{{< figure src="/images/2018/nn017.webp" title="Perceptron XOR dengan AND, OR, NOT" alt="Perceptron XOR dengan AND, OR, NOT" position="center" >}}

### Trik dari Perceptron

Di bagian sebelumnya kita membuat perceptron XOR dengan menggunakan perceptron AND, OR, dan NOT.

Tapi pada kenyataannya, di kondisi lapangan, kita nggak bisa membangun perceptron ini sendiri. Idenya adalah memberikan perceptron itu nilai hasil yang diinginkan, kemudian perceptron itu membangun dirinya sendiri.

Ada trik yang dapat digunakan untuk mencapai tujuan tersebut.. :raised_hands:

Kita kembali pada contoh kondisi klasifikasi diterima di kampus, dengan pertanyaan: bagaimana mencari persamaan yang paling sesuai untuk garis yang memisahkan dua kelompok data tersebut?

{{< figure src="/images/2018/nn018.webp" title="Bagaimana mencari persamaan garis yang memisahkan dua kelompok data?" alt="Bagaimana mencari persamaan garis yang memisahkan dua kelompok data?" position="center" >}}

Dengan tujuan untuk memisahkan dua kelompok data, kita akan menyederhanakan kondisinya, dengan hanya 3 titik biru dan 3 titik merah. Kemudian memulai memisahkan dengan **random** di awalnya untuk menghasilkan sebuah garis acak yang akan mencoba memisahkan kelompok biru dan merah.

{{< figure src="/images/2018/nn019.webp" title="Memulai memisahkan data dengan garis acak" alt="Memulai memisahkan data dengan garis acak" position="center" >}}

Hasilnya ada <u>4 titik sudah sesuai</u> dengan pengelompokannya dan <u>2 titik yang masih belum sesuai</u>. Kita akan fokus pada dua titik ini, dimana <u>1 titik merah masih berada di area biru</u> dan <u>1 titik biru masih berada di area merah</u>.

Di titik yang masih salah klasifikasi, **triknya** adalah menggeser garis pemisah ini mendekati (hingga sedikit melewati) titik tersebut, sehingga titik tersebut berpindah klasifikasi ke area yang seharusnya.

Secara matematika, bagaimana menggeser sebuah garis mendekati suatu titik?

Misalnya ada sebuah garis pemisah dengan persamaan: $3x\_1 + 4x\_2 -10 = 0$ dalam gambar berikut memisahkan area merah dimana persamaannya bernilai negatif (<0) dan area biru dimana persamaannya bernilai positif (>0).

{{< figure src="/images/2018/nn020.webp" title="Persamaan 3x1 + 4x2 - 10 membagi dua area" alt="Persamaan 3x1 + 4x2 - 10 membagi dua area" position="center" >}}

<u>Kemudian ada satu titik</u>, misalnya titik **merah (4, 5)** salah klasifikasi berada di area **biru** (area positif). 

{{< figure src="/images/2018/nn021.webp" title="Titik merah (4, 5) salah klasifikasi di area biru" alt="Titik merah (4, 5) salah klasifikasi di area biru" position="center" >}}

Implementasi secara matematika, kita gunakan parameter dari persamaan $3x\_1 + 4x\_2 -10 = 0$ yaitu **3, 4,** dan **-10**. Kemudian parameter dari titik (4, 5) yaitu **4, 5,** dan **1** (angka 1 ini ditambahkan sebagai unit bias).

Untuk menggeser garis dengan persamaan tersebut ke titik, karena titik ini berada **di area positif** maka **kedua parameternya dikurangkan**.

```
 3     4     -10
 4     5      1
________________ - (dikurangi)
-1    -1     -11
```

Hasil pengurangan tersebut, adalah <u>persamaan garis yang baru</u> yaitu $-x\_1 - x\_2 -11 = 0$. Jika digambarkan, garis ini akan langsung <u>bergeser secara drastis</u> mendekati titik (4, 5) dan <u>membuat titik tersebut berada di area yang benar</u>. Sedangkan kita tidak mau perubahan drastis tersebut membuat sejumlah titik lain yang sebelumnya sudah berada di area yang benar menjadi berubah di area yang salah. Kita ingin melakukan perubahan dengan sedikit demi sedikit (_small steps_) mengarah ke titik tersebut. Di sinilah kita membutuhkan **learning rate**.

#### _Learning Rate_

_Learning Rate_ adalah sebuah angka kecil misalnya 0.1 atau 0.01 yang akan digunakan untuk memroses perubahan sedikit demi sedikit (_small steps_) dalam hal penyesuaian persamaan linear.

Jika _learning rate_ ini digunakan, maka kita akan mengalikan **learning rate 0.1** ini ke parameter dari titik (4, 5), sehingga menghasilkan: **0.4, 0.5, 0.1** (angka yang terakhir dari unit bias). Kembali dilakukan operasi **pengurangan**, karena titik tersebut salah di **area positif**.

```
 3     4     -10
0.4   0.5    0.1
_________________ - (dikurangi)
2.6   3.5   -10.1
```

Sehingga persamaan garis yang baru adalah $2.6x\_1 + 3.5\_2 -10.1 = 0$. Dan garis ini mulai mendekat ke arah titik (4, 5), sesuai yang diharapkan.

Untuk melengkapi contoh, <u>kemudian ada satu titik</u> lagi, misalnya titik **biru (1, 1)** salah klasifikasi berada di area **merah** (area negatif).

Karena titik (1, 1) salah klasifikasi di area merah (**area negatif**), maka kita akan melakukan operasi **penambahan**. Dengan mengalikan **learning rate 0.1** ini ke parameter dari titik (1, 1) menghasilkan **0.1, 0.1, 0.1** (angka yang terakhir dari unit bias). 
```
 3     4     -10
0.1   0.1    0.1
_________________ + (ditambahkan)
3.1   4.1   -9.9
```
Sehingga persamaan garis yang baru adalah $3.1x\_1 + 4.1x\_2 -9.9 = 0$. Dan garis ini mulai mendekat ke arah titik (1, 1), sesuai yang diharapkan.

Misalnya kita menggunakan _learning rate_ bernilai **0.01** artinya akan membutuhkan **100 steps** untuk membuat persamaan garis tersebut menjadi benar dalam klasifikasi titik, misalnya (1, 1) di area biru (area positif).

### Algoritma untuk Perceptron

_Pseudocode_ yang akan digunakan dalam pembuatan perceptron menggunakan trik di atas adalah:

> 1. Start with random weights: $w\_1, ..., w\_n, b$
>
> 2. For every misclassified point $(x\_1, ..., x\_n)$:
>
>    2.1. If prediction = 0:
>       - For i = 1 ... n
>          - Change $w\_i + \alpha x\_i$
>       - Change $b$ to $b + \alpha$
>
>    2.2. If prediction = 1:
>       - For i = 1 ... n
>          - Change $w\_i - \alpha x\_i$
>       - Change $b$ to $b - \alpha$

Dengan catatan:

* `prediction = 0` artinya seharusnya positif tapi salah klasifikasi di area negatif, sehingga digeser dengan menambahkan $\alpha$.
* `prediction = 1` artinya seharusnya negatif tapi salah klasifikasi di area positif, sehingga digeser dengan mengurangkan $\alpha$.
* $\alpha$ adalah _learning rate_

### Membuat Kode (_coding_) untuk Algoritma Perceptron

Karena pembahasannya panjang, hanya terkait untuk kode ini, maka dibuatkan halaman khusus [Membuat Kode Algoritma Perceptron](/content/membuat-kode-algoritma-perceptron), karena ada jawaban dari quiz sehingga pertimbangkan dalam membuka halaman tsb.

### Algoritma Perceptron yang Non-Linear

Setelah sebelumnya membahas perceptron yang memisahkan dua kelompok data dengan sebuah garis (persamaan linear), ke depan akan lebih kompleks dengan pemisahan dua kelompok data dengan persamaan yang tidak lagi linear, misalnya kuadratik, atau persamaan lingkaran, dls.

Sebelum mulai membahas ke persamaan yang lebih kompleks, kita akan membahas beberapa hal pendukungnya terlebih dahulu.

### Fungsi Error

Fungsi error adalah sebuah fungsi yang akan menginformasikan sejauh apa nilai (kesalahan, error) dari prediksi yang kita lakukan, biasanya dibanding dengan label atau data latih. Di kelas Udacity, fungsi error (_error function_) didefinisikan sebagai sebuah fungsi yang memberikan informasi sejauh apa kita dari solusi.

Fungsi error ini selanjutnya akan membantu kita dalam proses mencari solusi dari permasalahan (implementasi Neural Networks).

{{< figure src="/images/2018/nn023.webp" title="Fungsi Error menuruni Gunung Errorest" alt="Fungsi Error menuruni Gunung Errorest" position="center" >}}

Fungsi error tidak boleh bernilai bilangan bulat (_discrete_), tetapi harus bernilai pecahan (_continuous_). Dengan langkah (_steps_) yang kecil, maka akan sulit mencari apakah fungsi error sudah mulai mengecil atau belum. Misalnya:

> untuk langkah kecil 0.2, maka nilai 1 ditambah/dikurangi 0.2, dalam bilangan bulat, masih bernilai 1.

Fungsi error harus bisa diturunkan secara matematika (_differentiable_), akan dibahas lebih lanjut nanti.

Lalu bagaimana gagasan fungsi error ini bisa digunakan untuk memecahkan masalah sebelumnya, mencari sebuah garis yang dapat memisahkan dua kelompok data?

Caranya dilakukan dalam dua langkah:

1. Memberikan nilai **penalty** dari setiap titik, dimana nilai penalty akan besar apabila titik (salah klasifikasi sehingga) berada area yang salah, serta nilai penalty bernilai kecil (hampir mendekati 0) apabila titik sudah sesuai (klasifikasi) berada di areanya.
2. Menjumlahkan nilai **penalty** dari semua titik, kemudian mencari persamaan garis (misalnya dengan menggeser-gesernya), hingga dapat mengurangi total nilai error (hasil penjumlahan nilai penalty). Tampak pada gambar berikut bahwa ada 2 titik yang ukurannya besar, menggambarkan bahwa kedua titik tersebut masih salah dalam klasifikasi.

{{< figure src="/images/2018/nn024.webp" title="Fungsi Error dengan menjumlahkan nilai penalty" alt="Fungsi Error dengan menjumlahkan nilai penalty" position="center" >}}

Dengan menggeser-geserkan garis untuk memisahkan dua kelompok data, dengan cara mencari total nilai error yang paling kecil, akhirnya didapatkan gambaran sebagai berikut.

{{< figure src="/images/2018/nn025.webp" title="Fungsi Error dengan total nilai penalty kecil" alt="Fungsi Error dengan total nilai penalty kecil" position="center" >}}

Selanjutnya, bagaimana mendefinisikan fungsi error ini, kemudian bagaimana penggunaannya dalam metode _gradient descent_? akan dijelaskan berikutnya.

### Fungsi Sigmoid untuk Fungsi Aktivasi

Jika fungsi error dipilih untuk menggunakan nilai pecahan (_continuous_), maka demikian juga dengan nilai prediksi dapat dipilih untuk menggunakan nilai pecahan (_continuous_) juga. Di prediksi _discrete_ kita menggunakan label **ya** atau **tidak**, **diterima** atau **ditolak**, dls. Di prediksi _continuous_, kita menggunakan prosentase kemungkinan (_probabilty_), misalnya **80% kemungkinan diterima**, dls.

{{< figure src="/images/2018/nn026.webp" title="Prediksi discrete dan prediksi continuous" alt="Prediksi discrete dan prediksi continuous" position="center" >}}

Dalam contoh di atas, sebelumnya prediksi antara diterima di kampus atau ditolak, diubah menjadi prediksi _continues_ berarti menjadi berapa prosentase kemungkinan diterima. Dalam gambar berikut jika biru adalah diterima maka titik biru 0.85 artinya 85% kemungkinan diterima, sedangkan titik merah 0.2 artinya 20% kemungkinan diterima.

{{< figure src="/images/2018/nn027.webp" title="Prediksi discrete dan prediksi continuous penerimaan di kampus" alt="Prediksi discrete dan prediksi continuous penerimaan di kampus" position="center" >}}

Pengubahan prediksi yang sebelumnya _discrete_ menggunakan _step function_, menjadi prediksi _continuos_ menggunakan **sigmoid function** sebagai **fungsi aktivasi** (_activation function_). Fungsi _sigmoid_ ini akan mengubah nilai masukan positif yang besar menghasilkan nilai **mendekati 1**, nilai masukan negatif yang besar menghasilkan nilai **mendekati 0**, dan nilai masukan mendekati 0 akan menghasilkan nilai **mendekati 0.5**.

{{< figure src="/images/2018/nn028.webp" title="Step function dan Sigmoid function" alt="Step function dan Sigmoid function" position="center" >}}

Persamaan dari fungsi _sigmoid_ ini adalah:

| $\sigma(x) = \frac{1}{1 + e^-x}$

Berikut adalah gambaran saat persamaan $Wx + b$ dimasukkan pada fungsi _sigmoid_.

* Saat nilai dari $Wx + b$ positif yang besar, keluaran dari fungsi _sigmoid_ adalah mendekati 1
* Saat nilai dari $Wx + b$ negatif yang besar, keluaran dari fungsi _sigmoid_ adalah mendekati 0
* Dan di garis utamanya, dimana $Wx + b$ bernilai 0, keluaran dari fungsi sigmoid adalah 0.5

{{< figure src="/images/2018/nn029.webp" title="Fungsi sigmoid untuk Wx + b" alt="Fungsi sigmoid untuk Wx + b" position="center" >}}

Setelah memahami fungsi _sigmoid_ sebagai fungsi aktivasi (_activation function_), saatnya menerapkannya ke Perceptron, sehingga keluarannya berada pada nilai _continues_ antara 0 dengan 1.

{{< figure src="/images/2018/nn030.webp" title="Perceptron dengan Sigmoid sebagai fungsi aktivasi" alt="Perceptron dengan Sigmoid sebagai fungsi aktivasi" position="center" >}}

### Klasifikasi Multi-class

Jika sebelumnya jika hanya membahas klasifikasi dua _class_ (_binary classification_), misalnya diterima atau ditolak masuk kampus. Berikut akan kita bahas klasifikasi lebih dari dua _class_, misalnya menentukan apakah binatang bebek (_duck_), berang-berang (beaver), atau anjing laut (_walrus_).

{{< figure src="/images/2018/nn031.webp" title="Klasifikasi 3 class dengan fungsi Softmax" alt="Klasifikasi 3 class dengan fungsi Softmax" position="center" >}}

### Fungsi Softmax

Untuk dapat melakukan prediksi dengan nilai _continues_ seperti halnya fungsi _Sigmoid_, untuk klasifikasi 3 _class_ atau lebih, menggunakan fungsi **Softmax**. _Probability_ dari class $i$, dengan _score_ dari fungsi linear bernilai $z\_1, ..., z\_n$.

> $P(i) = \frac{e^z\_i}{e^z\_1 + ... + e^z\_n}$

Fungsi _eksponential_ atau $e$ adalah sebuah fungsi yang selalu memberikan nilai positif untuk semua masukan, baik itu masukannya positif ataupun negatif, besar ataupun kecil.

_Pseudocode_ fungsi Softmax menggunakan NumPy (`import numpy as np`):

* `np.exp()` seperti yang sudah diprediksi ini adalah fungsi untuk _exponential_, tapi fungsi ini menerima input bukan hanya skalar, bahkan juga bisa sebuah NumPy array. Jadi fungsi _exponential_-nya di-_broadcast_ ke seluruh element array, masing-masing dilakukan operasi exponential. Di bawah ini fungsi `np.array()` akan membentuk NumPy array dari sebuah List Python.

  ```python
  print(np.exp(np.array([2, 1, 0])))  # [7.3890561  2.71828183 1.        ]
  ```

* `np.sum` juga sesuai dengan namanya akan melakukan _summation_ atau penambahan, termasuk fungsi yang menerima input berupa NumPy array, jadi seluruh elemennya akan dijumlahkan. Sebagai contoh, jika NumPy array hasil yang dicetak di atas dijumlahkan, hasilnya adalah:

  ```python
  print(np.sum(np.exp(np.array([2, 1, 0]))))  # 11.107337927389695
  ```

* Untuk membagi, kita bisa menggunakan operator pembagi biasa ( `/`), atau menggunakan fungsi `np.divide()`, yang bisa melakukan operasi _broadcast_ untuk membagi suatu NumPy array dengan suatu skalar, jadi seluruh elemennya akan dibagi dengan skalar tersebut.

Tinggal menggabungkan tiga fungsi tersebut, untuk membuat fungsi Softmax menggunakan NumPy.


Yang seru, pertanyaan di salah satu materi video, apakah Softmax untuk `n = 2` bernilai sama dengan Sigmoid? Untuk kasus khusus dimana `n = 2`, fungsi Softmax sama dengan fungsi Sigmoid (atau di beberapa referensi disebut fungsi Logistic). Salah satu yang menjelaskan penurunan rumusnya di situs <u>deeplearning stanford</u> [^4], di situ diberikan sebutan bahwa _Softmax regression_ itu adalah **multinomial Logistic regression**, sehingga dalam kasus _binary_ akan sama dengan _Logistic regression_ atau Sigmoid. Nanti akan dibahas selanjutnya mengenai _Logistic regression_.

### One-Hot Encoding

Pada contoh kasus yang digunakan sebelumnya, datanya tersedia dalam bentuk angka, misalnya hasil ujian. Tapi kadang data masukan yang dimiliki tidak berbentuk angka, misalnya berbentuk kategori, untuk itu digunakan proses **One-Hot Encoding** yang akan mendefinisikan setiap jenis kategori sebagai satu kolom data, kemudian nilainya hanya 1 atau 0 sesuai dengan kategorinya.

{{< figure src="/images/2018/nn032.webp" title="One-Hot Encoding duck, beaver, walrus" alt="One-Hot Encoding duck, beaver, walrus" position="center" >}}

Pada gambar di atas, adalah One-Hot Encoding untuk 3 _class_: duck, beaver, walrus. Salah satu yang penting di sini adalah tidak ada ketergantungan antara masing-masing class, sehingga tidak menggunakan nilai urut, misalnya duck 0, beaver 1, walrus 2.

### _Maximum Likelihood_

Masih membahas seputar pemilihan model yang memisahkan dua kelompok data sebelumnya. Mana _model_ yang paling baik dalam memisahkan dua kelompok data?

Dalam membahas tentang statistik, probabilitas misalnya dari dua buah _model_, mana yang lebih akurat? _Model_ yang terbaik adalah _model_ yang memberikan probabilitas paling besar untuk seluruh _label_ (data latih) yang ada di kedua kelompok tersebut. Hal ini disebut dengan **maximum likelihood**.

> Metode _maximum likelihood_ dengan memilih model yang memberikan nilai probabilitas tertinggi untuk _label_ yang ada. Demikian sehingga memaksimalkan probabilitas, kita bisa memilih model terbaik.

Kembali membahas prediksi dari setiap titik dari kedua kelompok adalah $\hat{y} = \sigma(Wx + b)$ sehingga probabilitas dari sebuah titik adalah biru (_blue_) ditentukan dengan:

> $P(blue) = \sigma(Wx + b)$

Sehingga probabilitas dari seluruh data dari kedua kelompok adalah `dot product` dari setiap probabilitas titik yang ada, atau dengan kata lain, perkalian dari semua probabilitas titik yang ada.

{{< figure src="/images/2018/nn033.webp" title="Maximum Likelihood sebagai pencarian probabilitas terbesar dari suatu model" alt="Maximum Likelihood sebagai pencarian probabilitas terbesar dari suatu model" position="center" >}}

Apakah memaksimalkan probabilitas dari suatu model sama artinya dengan meminimalkan fungsi error? Kita akan lihat dalam pembahasan berikut.

Kembali membahas sebelumnya yang menghitung probabilitas dari seluruh data dengan melakukan perkalian (_product_) dari setiap probabilitas titik. Sayangnya perkalian (misalnya untuk ribuan data) adalah proses yang membutuhkan sumber daya (_resource_) besar dimana keseluruhan datanya adalah pecahan (antara 0 dan 1), hasilnya akan pecahan yang sangat kecil, untuk itu dibutuhkan operator lain yang lebih hemat sumber daya.

Fungsi $log$ akan mengubah perkalian (_products_) menjadi penjumlahan (_sum_). Berikut adalah sifat dari logaritma (basis bilangan 10) yang digunakan:

> $log(A * B) = log(A) + log(B)$

Dengan menggunakan basis bilangan $e$, operator logartima yang digunakan adalah logaritma natural $ln()$.

> $ln(A * B) = ln(A) + ln(B)$

{{< figure src="/images/2018/Logarithm_plots.png" title="Plot Logaritma" alt="Plot Logaritma" position="center" >}}

Plot Logaritma tersebut dari artikel wiki [^5]. Perhatikan bahwa untuk nilai **pecahan antara 0 dan 1**, hasil dari logaritmanya adalah **negatif**.

### Cross Entropy

_Cross Entropy_ adalah sebuah konsep yang menjumlahkan nilai negatif dari logaritma propabilitas untuk setiap data. 

{{< figure src="/images/2018/nn034.webp" title="Cross Entropy sebagai penjumlahan nilai negatif dari logaritma probabilitas" alt="Cross Entropy sebagai penjumlahan nilai negatif dari logaritma probabilitas" position="center" >}}

**Model yang baik** akan memiliki nilai **cross entropy yang kecil**, sedangkan model yang buruk akan memiliki nilai _cross entropy_ yang besar. Sebetulnya ini terkait dengan perhitungan probabilitas sebelumnya, model yang baik akan memiliki probabilitas yang besar, dimana hasil operasi negatif dari logaritma nilai yang besar (_cross entropy_) adalah nilai yang kecil. 

{{< figure src="/images/2018/nn035.webp" title="Cross Entropy untuk model yang baik dan buruk" alt="Cross Entropy untuk model yang baik dan buruk" position="center" >}}

Bahkan _cross entropy_ ini bisa diberlakukan untuk setiap titik, jadi bukan hanya penjumlahannya saja. Terlihat pada gambar berikut ini bahwa suatu titik yang salah dikasifikasikan (_misclassified_), akan memiliki nilai _cross entropy_ yang besar. Hal ini **mengingatkan kembali pada fungsi error**, dimana suatu titik yang salah diklasifikasikan akan memiliki nilai error yang besar, sebaliknya titik yang diklasifikasikan dengan benar akan memiliki nilai error yang kecil.

{{< figure src="/images/2018/nn036.webp" title="Cross Entropy untuk titik yang diklasifikasikan benar dan salah" alt="Cross Entropy untuk titik yang diklasifikasikan benar dan salah" position="center" >}}

Kembali membahas mengenai **mencari model yang terbaik**, sehingga saat ini tujuan (caranya) sudah berubah, dari memaksimalnya probabilitas, menjadi **meminimalkan nilai cross entropy**. Sebagai hasil keterhubungan antara probabilitas dan fungsi error.

{{< figure src="/images/2018/nn037.webp" title="Model yang terbaik ditentukan dengan nilai cross entropy yang minimum" alt="Model yang terbaik ditentukan dengan nilai cross entropy yang minimum" position="center" >}}

Konsep _cross entropy_ ini sangat popular di berbagai bidang, salah satunya Machine Learning. Formulanya adalah

> Cross-Entropy = $-\sum_{i=1}^m y\_iln(p\_i) + (1 - y\_i)ln(1 - p\_i)$

Untuk dalam kasus di gambar berikut adalah:

* $i$ akan bernilai 1 sampai $m$ yaitu 3 (jumlah banyaknya _event_ dalam hal ini jumlah pintu)
* $p\_1$ adalah **probabilitas untuk ada hadiah** di pintu ke-1, sebaliknya $(1 - p\_1)$ adalah **probabilitas untuk tidak ada hadiah** di pintu ke-1. begitu juga untuk $p\_2$ dan $(1 - p\_2)$ serta $p\_3$ dan $(1 - p\_3)$.
* $y_1$ akan bernilai **1 jika ada hadiah** di pintu ke-1 atau bernilai **0 tidak tidak ada hadiah** di pintu ke-1. begitu juga untuk $y\_2$ serta $y\_3$.

{{< figure src="/images/2018/nn038.webp" title="Contoh kasus untuk formula Cross-Entropy" alt="Contoh kasus untuk formula Cross-Entropy" position="center" >}}

Sebagai contoh, melakukan kalkulasi _cross-entropy_ untuk pasangan:

* `cross_entropy((1, 1, 0), (0.8, 0.7, 0.1))` menghasilkan 0.69.

  di sini (1, 1, 0) adalah nilai dari ($y\_1$, $y\_2$, $y\_3$).

  di sini (0.8, 0.7, 0.1) adalah nilai dari ($p\_1$, $p\_2$, $p\_3$).
  
  menghasilkan nilai _cross-entropy_ yang kecil, karena (1, 1, 0) itu mirip (_similar_) dengan vector (0.8, 0.7, 0.1).
  
  artinya pengaturan hadiah dengan kumpulan nilai (1, 1, 0) **kemungkinan besar terjadi** berdasarkan nilai propabilitas dengan kumpulan nilai (0.8, 0.7, 0.1).

* `cross_entropy((0, 0, 1), (0.8, 0.7, 0.1))` menghasilkan 5.12.

  di sini (0, 0, 1) adalah nilai dari ($y\_1$, $y\_2$, $y\_3$)).
  di sini (0.8, 0.7, 0.1) adalah nilai dari ($p\_1$, $p\_2$, $p\_3$).

  menghasilkan nilai _cross-entropy_ yang besar, karena (0, 0, 1) itu tidak mirip dengan vector (0.8, 0.7, 0.1).

  artinya pengaturan hadiah dengan kumpulan nilai (1, 1, 0) **kemungkinan besar tidak terjadi** berdasarkan nilai probabilitas dengan kumpulan nilai (0.8, 0.7, 0.1).

{{< figure src="/images/2018/nn039.webp" title="Cross-Entropy menginformasikan kemiripan 2 vector" alt="Cross-Entropy menginformasikan kemiripan 2 vector" position="center" >}}

Seperti apa kode dari formula `cross_entropy(Y, P)` dimana `Y` adalah `List` kategori dan `P` adalah `List` probabilitas?

ternyata di quiz ini udacity menggunakan salah satu contoh dengan data berikut; `Y` bernilai `[1, 0, 1, 1]` dan `P` bernilai  `[0.4, 0.6, 0.1, 0.5]`.

* kita bisa menggunakan fungsi `zip()` untuk sekaligus mengiterasi `Y` dan `P`. Atau, pilihan yang lain, kita menggunakan kemampuan _broadcast_ dari NumPy sehingga tidak perlu melakukan iterasi secara manual, dengan catatan bahwa kita perlu konversi `List` menjadi NumPy array, misalnya dengan fungsi `np.array(Y)` atau jika kita mau konversi semua isinya menjadi NumPy array bertipe float, kita gunakan `np.float_(P)`.
* lalu kita hitung perkalian $y\_1 * log(p\_1)$ sebagai `_y * np.log(_p)`, dengan fungsi logaritma dari NumPy. di NumPy `np.log` itu menggunakan basis $e$ sedangkan basis 10 adalah `np.log10`. jangan lupa bahwa `np.log` itu bisa melakukan operasi _broadcast_ sehingga perkalian dengan setiap elemen tidak perlu iterasi.
* lalu kita hitung perkalian $(1-y\_1) * log(1-p\_1)$ sebagai `(1-_y) * np.log(1-_p)`
* tambahkan semuanya, untuk penggunaan _broadcast_ bisa menggunakan fungsi `np.sum` dari NumPy, terakhir jangan lupa kalikan dengan `-1.0` untuk mendapatkan _cross-entropy_

Jika berhasil, artinya kita sudah membuat kode _cross-entropy_ untuk 4 events (kemungkinan hadiah akan muncul di pintu), yaitu pada contoh sebelumnya artinya untuk 4 pintu. :raised_hands:

### Multi-class Cross-Entropy

Jika sebelumnya hanya membahas klasifikasi 2 (_binary_) tentang ada atau tidaknya hadiah di balik pintu, lalu bagaimana kalo mencoba melakukan klasifikasi 3 (atau lebih), misalnya menebak binatang apa di balik pintu, apakah bebek (_duck_), berang-berang (_beaver_), atau anjing laut (_walrus_)?

Sebagai contoh gambar berikut berisi probabilitas dari bebek berada di pintu ke-1 adalah 0.7, berang-berang berada di pintu ke-1 adalah 0.2, anjing laut berada di pintu ke-1 adalah 0.1, dan selanjutnya untuk pintu ke-2 serta ke-3. Jika diperhatikan <u>total dari probabilitas di setiap pintu (kolom) adalah 1</u>, artinya ada sudah pasti 1 satu binatang di balik pintu, hanya saja kemungkinannya berbeda-beda, dan juga artinya ada kemungkinan lebih dari satu pintu akan muncul binatang yang sama, misalnya anjing laut di pintu 2 dan juga anjing laut di pintu 3.

{{< figure src="/images/2018/nn040.webp" title="Tabel probabilitas untuk Multi-class Cross-Entropy" alt="Tabel probabilitas untuk Multi-class Cross-Entropy" position="center" >}}

Perhitungan _cross-entropy_-nya akan sama dengan sebelumnya, penjumlahan negatif logaritma dari probabilitas event.

{{< figure src="/images/2018/nn041.webp" title="Multi-class Cross-Entropy" alt="Multi-class Cross-Entropy" position="center" >}}

Berikut adalah formula dari _Multi-class Cross-Entropy_:

> Multi-class Cross-Entropy = $-\sum\_{i=1}^n \sum\_{j=1}^m y\_{ij} ln(p\_{ij})$

dengan catatan:

* $n$ adalah jumlah (multi) class, dalam hal ini n = 3 (duck, beaver, walrus). ya, aku tau bahwa ini **tidak sesuai seperti gambar di bawah** dan penjelasan di video, meskipun nilainya sama dengan $m$. Dalam formula di atas, variable $i$ akan iterasi dari `1` sampai $n$.
* $m$ adalah jumlah pintu, dalam hal ini m = 3. Dalam formula di atas, variabel $j$ akan iterasi dari `1` sampai $m$. Terlihat dalam gambar di bawah, $j$ tertulis di pintu.
* $p\_{ij}$ adalah probabilitas untuk _event_ (kemunculan) dari class $i$ di pintu $j$.
* $y\_{ij}$ akan bernilai `1` jika muncul class $i$ di pintu $j$, sebaliknya akan bernilai `0`.

{{< figure src="/images/2018/nn042.webp" title="Formula Multi-class Cross-Entropy" alt="Formula Multi-class Cross-Entropy" position="center" >}}

Seperti sebelumnya, muncul pertanyaan, jika formula _Multi-class Cross-Entropy_ ini diberikan nilai class = 2, apakah sama formulanya dengan _Cross-Entropy_? Jawabnya sama. (masih butuh referensi tambahan untuk penurunan formulanya :thinking: ).





**TBD**

[^1]: https://en.wikipedia.org/wiki/XOR_gate
[^2]: https://en.wikipedia.org/wiki/Exclusive_or
[^3]: [XOR from only OR and AND](https://stackoverflow.com/a/4715330/3991504)
[^4]: [SoftmaxRegression tutorial deeplearning.stanford.edu](http://deeplearning.stanford.edu/tutorial/supervised/SoftmaxRegression/)
[^5]: https://en.wikipedia.org/wiki/Logarithm
