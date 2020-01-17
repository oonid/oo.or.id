---
title: "Modelling Wikipedia Articles (enwiki) Using Gensim make_wiki.py"
date: 2016-01-01T20:28:30+07:00
publishDate: 2016-01-01T20:28:30+07:00
image: "/images/2016/gensim_logo.png"
url: "/content/modelling-wikipedia-articles-enwiki-using-gensim-makewikipy"
tags: ["Data Science", "gensim", "Python", ""]
draft: false
---

{{< figure src="/images/2016/gensim_logo.png" title="Modelling Wikipedia Articles (enwiki) Using Gensim make_wiki.py" >}}

(Artikel mengenai topik ini akan ditulis dalam bahasa Indonesia.)

Awal mulai penulisan artikel ini karena ingin mencoba (ekplorasi) aplikasi gensim [^3], lalu mencari informasi apa yang bisa dilakukan aplikasi gensim untuk data yang cukup besar, lalu dari beberapa referensi mengacu pada pengolahan data wikipedia yang lebih dari 10GB (dalam format kompresi bz2 yang terkenal sangat efisien).

Ya tentu pertama yang harus dilakukan adalah download file artikel dari wikimedia: download enwiki-latest-pages-articles.xml.bz2 dari situs dumps.wikimedia.org

Oh ya, untuk download file yang ukurannya besar, aku lebih suka menggunakan protokol rsync yang terbukti bisa mengulang proses download jika gagal dengan pengecekan integritas data yang baik, jadi proses download dari [^1].

Setelah mendapatkan file nya (dengan ukuran lebih dari 10GB) dalam format kompresi bz2, kita sudah siap melakukan eksperimen dengan script make_wiki.py dari aplikasi gensim [^2].

Kemudian install paket gensi menggunakan pip atau conda, misalnya instalasi dilakukan di dalam lingkungan virtual (virtual environment).

> pip install gensim

Lalu sekarang kita siap untuk run script (atau tool) make_wiki.py:

> python -m gensim.scripts.make_wiki enwiki-latest-pages-articles.xml.bz2 wiki_en_output

Keluarannya akan menghasilkan beberapa file sebagai berikut:


* wiki_en_output_wordids.txt.bz2
* wiki_en_output.tfidf_model
* wiki_en_output_tfidf.mm
* wiki_en_output_tfidf.mm.index
* wiki_en_output_bow.mm
* wiki_en_output_bow.mm.index

Nanti artikel selanjutnya akan mendiskusikan lebih jauh tentang bagaimana menggunakan file-file tersebut.

Terakhir kali menggunakan script ini untuk file enwiki-20160601-pages-articles.xml.bz2 (12,977,741,652 bytes).

Hasil keluaran script menyimpan corpus dalam format Matrix Market (sparse matrix) dengan nama wiki_en_output_bow.mm (9,802,803,835 bytes), juga menyimpan index MmCorpus dengan nama wiki_en_output_bow.mm.index (45,490,042 bytes), dan pemetaan (mapping) dari kamus (dictionary) pada file keluaran wiki_en_output_wordids.txt.bz2 (785,882 bytes).

Proses ini melakukan iterasi corpus dari Wikipedia sebanyak 4,068,594 documents (total 16,615,834 articles), dengan jumlah features sebanyak 100,000, disimpan pada sebuah matrix berukuran 4068594x100000.

Proses kalkulasi IDF weights untuk 4,068,594 documents dan 99,999 features (634584010 matrix non-zeros), kemudian objekTfidfModel disimpan pada wiki_en_output.tfidf_model, dengan corpus disimpan dalam format Matrix Market (sparse matrix) wiki_en_output_tfidf.mm, dan index MmCorpus disimpan pada file wiki_en_output_tfidf.mm.index

Pemrosesan ini dilakukan pada matrix berukuran 4068594x100000, density=0.156% (634584010/406859400000).

[^1]: https://meta.wikimedia.org/wiki/Mirroring_Wikimedia_project_XML_dumps
[^2]: https://radimrehurek.com/gensim/wiki.html
[^3]: https://github.com/piskvorky/gensim
