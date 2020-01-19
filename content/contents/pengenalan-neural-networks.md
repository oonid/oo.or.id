---
title: "Pengenalan Neural Networks"
date: 2018-12-01T23:46:00+08:00
publishDate: 2018-12-01T23:46:00+08:00
image: "/images/2018/pytorchudacityscholar.png"
url: "/content/pengenalan-neural-networks"
tags: ["Udacity", "Machine Learning"]
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






**TBD**

[^1]: https://en.wikipedia.org/wiki/XOR_gate
[^2]: https://en.wikipedia.org/wiki/Exclusive_or
[^3]: https://stackoverflow.com/a/4715330/3991504
