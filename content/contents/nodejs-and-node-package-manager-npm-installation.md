---
title: "Node.js and Node Package Manager (npm) Installation"
date: 2013-04-25T05:22:43+07:00
publishDate: 2013-04-25T05:22:43+07:00
image: "/images/2013/nodejs-light-512.png"
url: "/content/nodejs-and-node-package-manager-npm-installation"
tags: ["nodejs", "npm"]
draft: false
---

{{< figure src="/images/2013/nodejs-light-512.png" title="Node.js and Node Package Manager (npm) Installation" alt="Node.js and Node Package Manager (npm) Installation" position="center">}}


Eksplorasi kali ini tentang Node.js dan pelengkapnya Node Package Manager (npm) [^1] [^2].
Ada beberapa cara untuk menginstall Node.js [^3];

* Tentu saja yang termudah dengan download dan install package yang sesuai sistem operasi dari [^3]
* Kemudian cara yang paling menantang adalah menginstall dari source code
* Dan cara yang penulis pilih adalah menginstall melalui package manager, dengan alasan utama adalah kemudahan untuk merawat versi jika ada yang lebih baru [^4]

Karena penulis buka penganut aliran homebrew di Mac, penulis menginstall menggunakan ports [^5].

> o@x $ sudo su -
>
> o@x # port -v install nodejs

selesai! :) 

cukup test dengan mengetikkan: `node -v`

Sejak Node versi 0.6.3 sudah menyertakan npm dalam paketnya. Tapi beberapa package manager yang ada di sejumlah sistem operasi [^4], memisahkan paket npm dengan paket nodejs, sehingga anda harus melakukan installasi terpisah.

> $ sudo su -
>
> # port -v install npm

selesai! :) 

berikutnya diuji dengan cara sederhana ketik: `npm -v`

Sebagai tambahan, biasanya disejumlah artikel akan menyertakan mekanisme pembuatan "Hello World" dalam bentuk sebuah web server yang ditangani oleh Nodejs.

```
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(7717, '127.0.0.1');

console.log('Server running at http://127.0.0.1:7717/');
```

Untuk mengoperasikan server tersebut, simpan ke file misalnya hello.js lalu kemudian eksekusi dari command-line dengan perintah:

> $ node hello.js
> Server running at http://localhost:7717/

Buka browser dan masukkan alamat http://localhost:7717 untuk membuka Hello World web server yg dibuat dengan Node.

[^1]: http://nodejs.org/
[^2]: https://npmjs.org/
[^3]: http://nodejs.org/download/
[^4]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[^5]: http://www.macports.org/
