---
title: "Add New Resolution to Xorg on Kubuntu"
date: 2016-10-14T01:33:50+07:00
publishDate: 2016-10-14T01:33:50+07:00
image: "/images/2016/kubuntu_logo.png"
url: "/content/add-new-resolution-xorg-kubuntu"
tags: ["Linux", "Xorg"]
draft: false
---

{{< figure src="/images/2016/kubuntu_logo.png" title="Add New Resolution to Xorg on Kubuntu" >}}


Setelah rilis (Ubuntu dan) Kubuntu 16.10, langsung coba update dari Kubuntu 16.04 LTS ke Kubuntu 16.10 [1].

Perangkat yang dipakai adalah XPS 9250.

Setelah selesai update, ternyata secara bawaan masuk ke resolusi maksimal, yaitu 4K (3840x2160). Dan tidak ada di Display Configuration, resolusi yang biasa dipakai 2560x1440.

Karena satu-satunya bekal setting Xorg yang dimengerti cuma perintah xrandr, dicoba-coba untuk setting kembali ke resolusi 2560x1440. Tapi ternyata di xrandr bawaannya juga tidak ada pilihan resolusi tersebut, jadi harus menambahkan mode baru (newmode) ke setting layar [^2].

Setelah terbukti berhasil membuat output xrandr dengan resolusi yang diinginkan, berikutnya mau menambahkannya ke setting Xorg, agar setiap kalo boot tidak perlu ubah xrandr lagi.

Oh ternyata sekarang setting Xorg untuk Kubuntu (dan Ubuntu) ada di /usr/share/X11/xorg.conf.d/<br />
jadi berikutnya buat setting untuk monitor dan screen di direktori tersebut[^3].

Nah akhirnya bisa menikmati Kubuntu 16.10 "Yakkety Yak"! [^1]

[^1]: https://wiki.ubuntu.com/YakketyYak/ReleaseNotes/Kubuntu
[^2]: https://wiki.ubuntu.com/X/Config/Resolution
[^3]: https://samuelmartin.wordpress.com/2012/05/29/enabling-resolutions-in-ubuntu-12-04-lubuntu-12-04/
