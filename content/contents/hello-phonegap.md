---
title: "Hello PhoneGap"
date: 2013-01-06 15:13:06+07:00
publishDate: 2013-01-06 15:13:06+07:00
image: "/images/2013/cordovahello.jpg"
url: "/content/hello-phonegap"
tags: ["Android", "mobilisasi", "PhoneGap"]
draft: false
---

{{< figure src="/images/2013/cordovahello.jpg" title="Apache Cordova Hello World" >}}

Melanjutkan artikel tentang memulai [development aplikasi mobile dengan Phonegap](http://oo.or.id/content/develop-mobile-applications-using-phonegap), sekarang saatnya mencoba aplikasi pertama dengan menggunakan Phonegap atau nama resminya Apache Cordova&nbsp;(aplikasi pertama dalam bahasa pemrograman biasanya dianalogikan dengan Hello World).

Dalam percobaan ini akan digunakan platform Android sehingga proses developmentnya dapat dilakukan di berbagai komputer (Mac, Linux, &amp; Windows) dan tanpa bergantung pada device karena bisa menggunakan emulator Android.

Langsung mulai dari situs dokumentasi Phonegap yang "Getting Started with Android", setelah semua prose setup yang tertulis pada artikel tersebut selesai, kita bisa mulai membuat sebuah project menggunakan Phonegap (ada pada point Setup New Project di artikel tsb).

```bash
# masuk dalam direktori dimana phonegap yang sudah didownload dan diekstrak

cd ~/phonegap-2.3.0/lib/android/bin

./create ~/hellop id.or.oo helloproject

# dimana ~/hellop adalah direktori dimana projek akan dibuat
# sedangkan id.or.oo adalah nama package yang akan digunakan 
# dalam projek yg akan dibuat dan helloproject nama projek yang akan dibuat
```

Setelah proses pembuatan project selesai, saatnya memasukkan project tersebut ke IDE yang digunakan, dalam hal ini Eclipse.

Lanjutkan dengan "New Project" (atau create new other project), kemudian pilih "Android Project from Existing Code" dan arahkan pada direktori dimana projek sebelumnya dibuat.

Setelah projek berhasil masuk ke Eclipse, kemudian dilakukan proses Build pada projek tersebut, lalu projek sudah siap untuk dideploy ke emulator Android atau bahkan langsung ke device Android.

Dan hasilnya saat aplikasi running seperti tampak pada gambar, ada halaman splash yang menyatakan aplikasi dalam status ready.

notes:

* Salah satu problem yang ditemui saat tidak bisa dibuild adalah error pada AndroidManifest.xml karena tidak comply dengan versi Android Platform yang digunakan, pada saat tulisan ini dibuat, Phonegap versi 2.3.0, dan kebutuhan platform Android dengan API Level &gt; 10 atau di atas Android 2.3.3. Tidak perlu kuatir dengan kompatibilitas aplikasi untuk Phonegap versi 2.3.0 masih tetap bisa untuk Android 2.2 ke atas (kecuali Android 3.x, langsung ke versi Android 4.x). Pemecahan problemnya dengan memilih properties dari projeknya kemudian menaikkan versi platform Androidnya ke versi terakhir yang terinstall di Eclipse.
