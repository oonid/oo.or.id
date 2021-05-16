---
title: "Mengelola Catatan/Jurnal di GitHub dengan GitJournal"
date: 2021-04-04T23:45:00+08:00
publishDate: 2021-04-04T23:45:00+08:00
image: "/images/2021/gitjournal.svg.png"
url: "/content/mengelola-catatan-jurnal-github-gitjournal"
tags: ["Jurnal", "GitHub", "Markdown", "Flutter"]
draft: false
---

{{< figure src="/images/2021/gitjournal.svg.png" title="Mengelola Catatan/Jurnal di GitHub dengan GitJournal" alt="Mengelola Catatan/Jurnal di GitHub dengan GitJournal" position="center">}}

Seperti pembahasan beberapa artikel sebelumnya, aku sedang banyak eksplorasi Wiki untuk menuliskan "dasar pengetahuan" (_Knowledge Base_), salah satunya [menggunakan aplikasi Gollum di GitHub](https://oo.or.id/content/mengelola-wiki-github-gollum/). Meskipun kebanyakan hanya mengubah berkas Markdown, termasuk artikel-artikel di blog ini juga dituliskan dalam format Markdown, tapi aktivitasnya belum bisa dilakukan dengan mobilitas yang lebih leluasa, misalnya beraktivitas di ponsel.

Dengan semakin "ketergantungan" dengan Version Control System, misalnya Git, beberapa waktu sempat berpikir untuk menggunakan (baca: mencari aplikasi yang cocok atau jika tidak ada maka akan membuat sendiri) implementasi Git di ponsel. Sudah mencoba menggunakan _browser_ untuk mengubah berkas Markdown, tapi interaksinya masih sangat sulit. Termasuk juga mencari aplikasi atau pustaka (_library_) terkait aplikasi ponsel, dimana saat ini sedang aktif [menulis dan mengembangkan dengan Flutter](https://oo.or.id/tags/flutter/) toolkit.

Dan akhirnya dapat aplikasi ponsel untuk membuat catatan/jurnal, yang sesuai dengan keinginan:

* Bisa mengerti format (memiliki editor) Markdown.
* Punya mekanisme Version Control, terutama Git.
* Meski opsional, tapi kalo bisa bersifat open source, ini sih kayak menemukan harta karun!
* Super bonus lagi kalo aplikasi open source dengan bahasa pemrograman dan toolkit yang aku udah paham.

Pilihannya jatuh kepada GitJournal [^1]. 🙌

* Memiliki fungsi editor untuk Markdown
* Terintegrasi dengan Git, bahkan untuk akses menggunakan keys (git+ssh), untuk banyak layanan Git misalnya GitHub, Gitlab, dls
* Lisensinya open source AGPL
* Dibuat dengan bahasa pemrograman Dart menggunakan Flutter toolkit

Sekarang bisa dengan mudah membuat catatan, terutama dalam format Markdown, di ponsel, jadi kalo punya ide tinggal edit aja catatannya tanpa perlu takut salah sunting (edit), karena semua perubahannya tersimpan dengan baik di repository Git!


[^1]: https://github.com/GitJournal/GitJournal