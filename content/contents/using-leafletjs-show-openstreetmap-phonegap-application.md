---
title: "Using Leaflet.js to show OpenStreetmap in PhoneGap Application"
date: 2013-01-29T22:11:29+07:00
publishDate: 2013-01-29T22:11:29+07:00
image: "/images/2013/phonegapleaflet-2013-02-01-091542.png"
url: "/content/using-leafletjs-show-openstreetmap-phonegap-application"
tags: ["Geolocation", "mobilisasi", "OpenStreetMap", "PhoneGap"]
draft: false
---

{{< figure src="/images/2013/phonegapleaflet-2013-02-01-091542.png" title="Using Leaflet.js to show OpenStreetmap in PhoneGap Application" alt="Using Leaflet.js to show OpenStreetmap in PhoneGap Application" position="center" >}}


Dari artikel sebelumnya tentang Geolocation [^1] dan menampilkan Google Static Maps [^2], kali ini dilengkapi dengan menampilkan Peta OpenStreetMap dengan menggunakan Leaflet.js [^3].

Untuk percobaan awal tidak perlu mendownload dan memasukkan leaflet.js ke dalam project PhoneGap, tetapi dilakukan load online dari cdn.leafletjs.com.

Pada file index.html tambahkan beberapa baris berikut ini pada bagian `<head>`

```
<link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.4/leaflet.css" />
<!--[if lte IE 8]>
    <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.4/leaflet.ie.css" />
<![endif]-->
<script src="http://cdn.leafletjs.com/leaflet-0.4/leaflet.js"></script>
```

Masih pada bagian index.html tambahkan area untuk gambar peta dengan menggunakan tag div.

```
<div id="map" style="width: 256px; height: 256px"></div>
```

Kemudian pada index.js diubah untuk fungsi menampilkan peta setelah sukses mendapatkan titik Geolocation.
```
    navigator.geolocation.getCurrentPosition(onSuccess, onError);
    // onSuccess Geolocation
    //
    function onSuccess(position) {
        console.log("geo: "+position.coords.latitude+","+position.coords.longitude);
        var map = L.map('map').setView([position.coords.latitude, position.coords.longitude], 14);
    	L.tileLayer('http://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    		maxZoom: 18
    	}).addTo(map);
    	L.marker([position.coords.latitude, position.coords.longitude]).addTo(map);
    }
    
    // onError Callback receives a PositionError object
    //
    function onError(error) {
       console.log('code: '    + error.code    + '\n' +
              'message: ' + error.message + '\n');
    }
```

Hasil peta dan marker tampak seperti gambar pada artikel ini.

[^1]: http://oo.or.id/content/working-geolocation-phonegap
[^2]: http://oo.or.id/content/show-static-map-location-phonegap
[^3]: http://leafletjs.com
