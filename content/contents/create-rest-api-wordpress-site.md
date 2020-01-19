---
title: "Create REST API from WordPress Site"
date: 2013-05-05T13:19:17+07:00
publishDate: 2013-05-05T13:19:17+07:00
image: "/images/2013/wordpress-logo-stacked-rgb.png"
url: "/content/create-rest-api-wordpress-site"
tags: ["WordPress", "API"]
draft: false
---

{{< figure src="/images/2013/wordpress-logo-stacked-rgb.png" title="Create REST API from WordPress Site" alt="Create REST API from WordPress Site" position="center">}}


WordPress adalah salah satu blog engine yang terkenal dan banyak penggunanya di Indonesia.

Jika kita ingin melakukan koneksi ke WordPress dari aplikasi pihak ketiga, misalnya sebuah aplikasi mobile atau web lain, maka cara yang kita perlukan adalah membuat REST API dari situs WordPress kita tersebut. Dalam artikel ini akan dibahas bagaimana menghasilkan REST API, meski di sisi lain kita bisa menggunakan XML-RPC bawaan [^1] [^2] dari WordPress untuk melakukan koneksi.

Salah satu cara yang cukup banyak digunakan antara lain:

* Menggunakan plugin JSON-API [^3]
* Untuk pengguna WordPress.com bisa menggunakan API bawaah [^4], atau menggunakan API serupa untuk WordPress.org yang sudah dipasang plugin JetPack [^5]
* Cara yang lainnya adalah membuat sendiri API menggunakan WordPress API seperti halnya membuat plugin [^6]

ps: bagaimana dengan mekanisme authentication? sehingga API hanya diakses oleh yang berhak. (akan dibahas dalam artikel yang lain).

[^1]: http://codex.wordpress.org/XML-RPC_WordPress_API
[^2]: http://wp.tutsplus.com/tutorials/creative-coding/xml-rpc-in-wordpress/
[^3]: http://wordpress.org/plugins/json-api/other_notes/
[^4]: http://developer.wordpress.com/docs/api/
[^5]: http://jetpack.me/support/json-api/
[^6]: http://codex.wordpress.org/Plugin_API
