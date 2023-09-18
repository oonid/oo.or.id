---
title: "Mengelola Wiki di GitHub dengan Gollum"
date: 2021-02-02T23:45:00+08:00
publishDate: 2021-02-02T23:45:00+08:00
image: "/images/2021/wiki_github_gollum.png"
url: "/content/mengelola-wiki-github-gollum"
tags: ["Wiki", "GitHub", "Gollum", "Knowledge Base", "Ruby"]
draft: false
---

{{< figure src="/images/2021/wiki_github_gollum.png" title="Mengelola Wiki di GitHub dengan Gollum" alt="Mengelola Wiki di GitHub dengan Gollum" position="center">}}

> Logo aplikasi Gollum berbentuk cincin (ring)[^1], sesuai dengan adaptasi dari karakter fiksi Gollum di The Lord of The Rings[^2].

Bulan Februari 2021 ini rencananya akan belajar topik Data Analyst, serta sebagian Data Science dimana materi keduanya beririsan. Melihat topik yang luas ini terbayang perlunya membuat "dasar pengetahuan" (_Knowledge Base_) untuk bisa merangkai dan menghubungkan berbagai pemahaman yang masih baru dipelajari.

Awalnya memang terpikir untuk membuat _mindmap_, lalu merasa bahwa tidak cocok untuk kebutuhan menuliskan catatan panjang bahkan sampai notasi matematika. Begitu juga papan kanban semacam Trello, meskipun cocok untuk membuat catatan yang canggih, tapi sulit mengakses relasi antar entitas (kartu), apalagi mendapatkan gambaran relasinya.

Saat ini yang terbayang adalah **Wiki**, ya seperti halnya Wikipedia[^3], kolaborasi penulisan sehingga satu pengetahuan dengan pengetahuan lain relasinya cukup bisa digambarkan dengan baik. Mungkin ini hanya masalah preferensi, jadi belum tentu "dasar pengetahuan" model ini cocok untuk orang lain. Selain membaca wikipedia, yang cukup sering aku baca belakangan adalah Wiki Hyperledger[^4] dimana kontributornya membuat berbagai "dasar pengetahuan" di situ.

Kalo Wikipedia[^3] menggunakan aplikasi MediaWiki[^5], Wiki Hyperledger menggunakan Confluence[^6] produk berbayar dari Atlassian. Oh satu lagi yang akan aku sebutkan adalah MoinMoin[^7] aplikasi yang digunakan sebagai Wiki Python.

Nah yang akan aku gunakan ini fitur _free_ Wiki[^8] dari GitHub (_free_ untuk repo publik, atau berbayar untuk repo privat). Selain Wiki ini bisa langsung disunting (_edit_) di situs GitHub, ada aplikasi pendukung bernama Gollum[^1] yang bersifat _open source_ dari tim GitHub juga. Salah satu hal yang aku perlukan dalam menggunakan Gollum ini adalah mekanisme pencarian (_search_) dari Wiki yang sudah ditulis, fitur ini tidak tersedia pada Wiki di GitHub.

Pemilihan aplikasi Wiki yang aku gunakan fokus pada kemudahan instalasi dan penggunaan. Dengan menggunakan Gollum[^1] maka tidak membutuhkan mekanisme penyimpanan seperti basis data, karena menggunakan Git sebagai _backend_. Kemudian dukungan sistem kontainer seperti Docker sehingga memudahkan proses instalasi dan memperbarui versinya di kemudian hari. Dari sisi penggunaan, aplikasi iberbasis web ini mudah digunakan dan tentunya kebutuhannya hanya peramban (_browser_).

Sebagai contoh, aku buat Wiki di repo blog (publik) ini[^9]. Kemudian aku _clone_ ke lokal komputer.

```bash
$ git clone https://github.com/oonid/oo.or.id.wiki.git
Cloning into 'oo.or.id.wiki'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 230 bytes | 38.00 KiB/s, done.
$ cd oo.or.id.wiki/
$ ls -a
.git    Home.md
```

Selanjutnya, aku gunakan Gollum untuk menampilkan serta menyunting (_edit_) Wiki ini, menggunakan sistem Docker resmi dari Gollum[^10]. Ngomong-ngomong Gollum ini dikembangkan dengan bahasa pemrograman Ruby.

```bash
$ docker pull gollumorg/gollum
$ docker run -v `pwd`:/wiki -p 4567:4567 gollumorg/gollum
```

Berikutnya membuka web Gollum ini peramban (_browser_) `http://localhost:4567` lalu mengembangkan "dasar pengetahuan" yang akan aku tulis dalam format `markdown` (.md)!

Tambahan catatan, Gollum ini dikembangkan dengan bahasa pemrograman Ruby. Membuatku tertarik melirik pemrograman Ruby. :raising_hands:


[^1]: https://github.com/gollum
[^2]: https://en.wikipedia.org/wiki/Gollum
[^3]: https://wikipedia.org
[^4]: https://wiki.hyperledger.org

[^5]: https://mediawiki.org
[^6]: https://www.atlassian.com/software/confluence/why-wiki-collaboration-software
[^7]: https://moinmo.in
[^8]: https://docs.github.com/en/github/building-a-strong-community/about-wikis
[^9]: https://github.com/oonid/oo.or.id
[^10]: https://github.com/gollum/docker

