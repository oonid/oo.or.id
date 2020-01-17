---
title: "Simple Open Source Music Player based on PhoneGap: Nusantara Berdendang"
date: 2013-03-07T14:06:36+07:00
publishDate: 2013-03-07T14:06:36+07:00
image: "/images/2013/phonegap-nusantaraberdendang-2013-03-14-005607.png"
url: "/content/simple-open-source-music-player-based-phonegap-nusantara-berdendang"
tags: ["Learn-by-Example", "mobilisasi", "Open Source", "PhoneGap", "Music Player", "jQueryMobile"]
draft: false
---

{{< figure src="/images/2013/phonegap-nusantaraberdendang-2013-03-14-005607.png" title="Simple Open Source Music Player based on PhoneGap: Nusantara Berdendang" >}}


Punya Android? (sudah tidak tersedia) http://oo.or.id/nusantaraberdendang.apk

[Untuk fitur aplikasi yang lebih lengkap lagi ada pada artikel <a href="http://oo.or.id/content/open-source-music-player-based-phonegap-haloaci">HaloACI</a>]

Membuat sebuah aplikasi berbasis PhoneGap&nbsp;yang memainkan lagu memang mudah dan sudah pernah dibahas di [^1].

Kali ini kita akan punya contoh sederhana sebuah aplikasi yang punya satu album lengkap di dalamnya, jadi tinggal download aplikasinya selanjutnya bisa memainkan lagu dalam aplikasi tersebut sepuasnya. :-)

Lagu-lagu free & LEGAL yang akan kita gunakan adalah Album Nusantara Berdendang dari [^2], kemudian diproses di iTunes untuk dikonversi agar ukurannya kecil (total ukuran aplikasi hanya sekitar 12MB untuk 10 lagu).

Artikel ini menyediakan sebuah aplikasi komplit, _open source_, yang bisa didapat [^3] di https://github.com/oonid/nusantaraberdendang

Tampilan aplikasinya menggunakan jQuery Mobile untuk memudahkan membuat PlayList lagu-lagu dan slider untuk progress bar. Seperti pada gambar aplikasi memiliki dua tombol navigasi dan PlayList yang bisa langsung dipilih jika ingin dimainkan lagunya.

KEKURANGAN (CONS) dari aplikasi minim sekali fitur ini adalah belum memiliki fitur pada PlayList, misalnya loop seluruh playlist, atau random playlist dan lain sebagainya.

Berikut adalah langkah-langkah yang dilakukan dalam proses pembuatan aplikasi Nusantara Berdendang:

* cd /Users/oonarfiandwi/Downloads/phonegap-2.5.0/lib/android/bin
* ./create ~/Documents/nusantaraberdendang id.or.oo.nusantaraberdendang nusantaraberdendang
* import to Eclipse SDK
* refactor oo.or.id.nb.NusantaraBerdendang to NusantaraBerdendang
* copy songs to assets/ww/audio (make the directory if not exist)
* copy jquery-1.9.1.min.js to www/js/
* copy jquery.mobile-1.3.0.min.js to www/js/
* copy jquery.mobile-1.3.0.min.css to www/css/
* create index.html
* copy functions to index.js
* overwrite drawable/icon.png with icon.png 96x96px
* overwrite drawable-hdpi/icon.png with icon.png 72x72px
* overwrite drawable-ldpi/icon.png with icon.png 36x36px
* overwrite drawable-mdpi/icon.png with icon.png 48x48px
* overwrite drawable-xhdpi/icon.png with icon.png 96x96px

Kemudian setelah kode teruji, clean project, kemudian dilakukan git commit dan push remote ke GitHub!

[^1]: http://oo.or.id/content/simple-example-how-play-music-audio-media-phonegap
[^2]: http://indonesiakreatif.net/article/freebies/audio/album-nusantara-berdendang/
[^3]: https://github.com/arfiandwi/nusantaraberdendang
