---
title: "Install and Update Drupal Core with Git"
date: 2012-12-21 06:36:15+07:00
publishDate: 2012-12-21 06:36:15+07:00
url: "/content/install-and-update-drupal-core-git"
tags: ["commit", "Drupal", "git", "install", "update", "merge"]
draft: false
---

{{< figure src="/images/2012/Git-Logo-2Color.png" title="Install and Update Drupal Core with Git" >}}

Saat artikel ini dibuat, Drupal sedang sangat aktif sekali melakukan development dan menutup security hole di versi 7.xx.

Akhirnya hampir setiap 1-2 minggu sekali update Drupal di tempat hosting.

Setelah mencari cara dari berbagai alternatif yang ada, akhirnya pilihan jatuh ke install dan update Drupal menggunakan Git.

Caranya cukup mudah, pertama buat dulu inisialisasi git di komputer lokal (pilih sebuah direktori dimana Drupal akan diinstal);

<p class="rteindent1"># git init</p>

<p class="rteindent1"># touch readme</p>

<p class="rteindent1"># git add readme</p>

<p class="rteindent1"># git commit -a -m "Initial Commit"</p>

Kadang setelah proses instalasi git dibutuhkan konfigurasi user.name dan user.email misalnya sebagai berikut;

<p class="rteindent1"># git config --global user.name "oon arfiandwi"</p>

<p class="rteindent1"># git config --global user.email "oon(at)oo.or.id"</p>

Kemudian ambil repository Drupal;

<p class="rteindent1"># git remote add core git://drupalcode.org/project/drupal.git</p>

<p class="rteindent1"># git fetch core</p>

nah, berikutnya versi Drupal mana yang mau dipasang? daftarnya bisa dilihat dari daftar tags&nbsp;<span style="line-height:1.6em">berikut &nbsp;</span><span style="line-height:1.6em">http://cgit.drupalcode.org/drupal/refs/tags</span>

pilih commit signature dari versi yang ingin dipasang, kemudian lakukan command (misal contoh signature 7.18 adalah b47f95d3013619e33cafdf8b769b2b6179a07956);

<p class="rteindent1"># git merge&nbsp;b47f95d3013619e33cafdf8b769b2b6179a07956</p>

maka versi 7.18 sudah siap digunakan dengan Root Directory web di direktori tersebut.

untuk melakukan proses update ke versi yang lebih baru, refresh lagi repository lokal git;

<p class="rteindent1"># git fetch core</p>

kemudian merge lagi dengan commit signature dari versi Drupal yang baru;

<p class="rteindent1">#git merge new-version-commit-hash</p>

ada beberapa hal pertimbangan:

<ul>
	<li>modules (3dr party) yang digunakan masih sedikit, jadi belum tau apakah akan problem dengan modules yang banyak</li>
	<li>proses update database apakah perlu dilakukan terpisah</li>
</ul>

[ref:1] http://cweagans.net/blog/2011/10/14/my-drupalgit-workflow
