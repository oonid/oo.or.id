---
title: "Flutter Web: Service Worker & Progressive Web App (PWA)"
date: 2020-01-17T17:00:00+08:00
publishDate: 2020-01-17T17:00:00+08:00
image: "/images/2020/logo_lockup_flutter_horizontal.png"
url: "/content/flutter-web-service-worker-progressive-web-app-pwa"
tags: ["Dart", "Flutter"]
draft: false
---

{{< figure src="/images/2020/logo_lockup_flutter_horizontal.png" title="Flutter Web Service Worker & Progressive Web App (PWA)" alt="Flutter Web Service Worker & Progressive Web App (PWA)" position="center">}}

Sejak pertengahan Januari 2020, ada penambahan fitur service worker di Flutter Web [^1]. Versi yang dikembangkan saat penambahan fitur tersebut adalah v1.14.x. Dengan adanya service worker, artinya kita bisa membuat Progressive Web App (PWA) [^2] dengan Flutter Web.

Sebelum mulai membahas detailnya, kalo butuh diskusi seputar [pemrograman web dengan Dart, gabung di komunitas dart_web](https://oo.or.id/content/dart-web-group-discussion-bahasa-indonesia/).

Pertama buat dulu proyek Flutter baru dengan dukungan web [^3].

```
flutter channel beta
flutter upgrade
flutter config --enable-web
```

Setelah aktif dukungan web-nya, kita akan punya perangkat Web Server kalo mengetikkan `flutter devices`. Ini dijalankan di sistem operasi Linux, makanya download linux-x64.

```
> flutter devices
Downloading package sky_engine...                                   1.8s
Downloading flutter_patched_sdk tools...                            1.7s
Downloading flutter_patched_sdk_product tools...                    1.4s
Downloading linux-x64 tools...                                      1.4s
Downloading linux-x64/font-subset tools...                          1.8s
1 connected device:

Web Server • web-server • web-javascript • Flutter Tools
```

Karena pakai browser chromium (bukan google chrome), perlu tambahkan environment variable.

```
export CHROME_EXECUTABLE=/usr/bin/chromium-browser
```

Buat proyek Flutter "dartweb" lalu lanjutkan dengan proses `run` di browser chrome.

```
flutter create dartweb
cd dartweb
flutter run -d chrome
```

yeay! aplikasi Flutter web berhasil run di browser.

Kalo mau rilis aplikasi, gunakan perintah `flutter build web`, sehingga kodenya akan dikompilasi dengan `dart2js`, tidak seperti halnya saat development (run) menggunakan `dartdevc`. Atau proses development juga bisa menggunakan `dart2js` kalo menggunakan parameter `flutter run --release`. Proses `build` akan menghasilkan direktori `build/web/` yang berisi berkas-berkas hasil kompilasi juga termasuk direktori `assets`.

Web-nya akan digelar (_deploy_) ke Firebase _hosting_.

```
> firebase login
> firebase init
Hosting: Configure and deploy Firebase Hosting sites
? Please select an option: Use an existing project
? Select a default Firebase project for this directory: dartweb (Dart Web)
? What do you want to use as your public directory? build/web
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
✔  Wrote build/web/404.html
✔  Wrote build/web/index.html

i  Writing configuration info to firebase.json...
i  Writing project information to .firebaserc...

> flutter build web
> ls build/web/
assets       flutter_service_worker.js  index.html    main.dart.js.map
favicon.png  icons                      main.dart.js  manifest.json
> firebase deploy
```

selesai! kodenya sudah dipublikasi di GitHub [^4]. webnya tersedia di

> https://dartweb.web.app

Terlihat bahwa secara bawaan (belum edit kodenya sama sekali), sudah tersedia `flutter_service_worker.js` sehingga saat cek situsnya dengan "chrome dev tool", di bagian "Application" > "Service Workers" sudah aktif  dan beroperasi (_running_) service worker untuk situs web tersebut.

{{< figure src="/images/2020/dartweb1.webp" title="Flutter Web Service Worker activated and running" alt="Flutter Web Service Worker activated and running" position="center">}}

Hasil audit dengan Lighthouse cukup oke hasilnya.

{{< figure src="/images/2020/dartweb2.webp" title="Flutter Web Service Worker audit" alt="Flutter Web Service Worker audit" position="center">}}

Lanjut lagi, edit kodenya, menggunakan... Android Studio! :joy:

Melihat `manifest.json` yang dihasilkan secara bawaan dari Flutter Web sudah lengkap, silakan cek acuannya [^5]. Sudah termasuk fitur Add to Home Screen (A2HS), dalam gambar ini di desktop tampil dengan tanda (+) yang ditandai lingkaran warna merah.

{{< figure src="/images/2020/dartweb3.webp" title="Flutter Web Service Worker manifest" alt="Flutter Web Service Worker manifest" position="center">}}

Eksplorasi fitur Progressive Web App (PWA) yang lain...

[^1]: https://github.com/flutter/flutter/pull/48344
[^2]: https://web.dev/pwa-checklist/
[^3]: https://flutter.dev/docs/get-started/web
[^4]: https://github.com/oonid/dartweb

[^5]: https://web.dev/add-manifest/