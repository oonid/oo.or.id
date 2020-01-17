---
title: "Kelas Python di Jurusan Sistem Informasi (SI) ITS"
date: 2015-02-28 10:00:53+07:00
publishDate: 2015-02-28 10:00:53+07:00
image: "/images/2015/kelas_python_its_surabaya_3.jpg"
url: "/content/kelas-python-di-jurusan-sistem-informasi-si-its"
tags: ["Python", "kelas teknologi", "google"]
draft: false
---

{{< figure src="/images/2015/kelas_python_its_surabaya_3.jpg" title="Kelas Python 3 ITS Surabaya " width="640" height="480" >}}

Bekerja sama dengan rekan-rekan Himpunan Mahasiswa Sistem Informasi (HMSI) ITS, mengadakan Kelas Python, sekaligus yang ke-3 di Surabaya.

Kelas yang berlangsung sekitar 5 jam ini, **diikuti oleh lebih dari 50 peserta**.

Beberapa liputannya antara lain:

* https://www.its.ac.id/berita/14765/en
* http://youtu.be/X6_-UbAgY0g

Mendapatkan beberapa pengalaman baru antara lain:

* Teman-teman di Surabaya sangat tertarik dengan acara teknologi, tergambar dari acara ini yang dipublikasi ke beberapa kampus, untuk kuota 50 peserta, jumlah **pendaftar mencapai 573 orang**.

* Google App Engine Launcher di Windows harus ekstra konfigurasi kalo menggunakan Web Proxy (di ITS pakai proxy), begitu juga dengan script appcfg.py

* Kebetulan versi Google App Engine for Python yang digunakan adalah 1.9.17, dan ternyata ditemukan bugs yang berhubungan dengan urllib2.py [^2].
	Akhirnya teman-teman dibandingkan download ulang versi 1.9.18 (yang sudah tidak memiliki bugs ini), lebih memilih mengedit urllib2.py untuk mengubah method https_open(self, req).

* Ada problem lain pada saat upload username dan password Google account dianggap salah terus, akhirnya harus mengubah setting google accounts (myaccount.google.com) untuk "allow less secure apps" [^3]

_photo courtesy of Nabilla Sabbaha Audria P._

[^2]: [appcfg.py bug untuk app engine 1.9.17](http://stackoverflow.com/questions/27463779/unexpected-keyword-argument-context-when-using-appcfy-py)

[^3]: [less secure apps setting](https://support.google.com/accounts/answer/6010255?hl=en)