---
title: "Radio Streaming (shoutcast, icecast) in PhoneGap"
date: 2013-03-03T00:51:20+07:00
publishDate: 2013-03-03T00:51:20+07:00
image: "/images/2013/phonegapradiostreaming-2013-03-09-114503.png"
url: "/content/radio-streaming-shoutcast-icecast-phonegap"
tags: ["mobilisasi", "Music Player", "PhoneGap"]
draft: false
---

{{< figure src="/images/2013/phonegapradiostreaming-2013-03-09-114503.png" title="Radio Streaming (shoutcast, icecast) in PhoneGap" >}}


Implementasi radio streaming pada PhoneGap tidak berbeda dengan penggunaan Audio Media lainnya [^1][^2], iya, memang sangat sederhana dan mudah sekali :)

Pada artikel ini kita akan membuat sebuah aplikasi Radio sederhana, yang bisa streaming pada icecast atau shoutcast streaming server. Biasanya URL streaming berupa M3U atau PLS, untuk dimasukkan sebagai URL dalam aplikasi HTML5 dibutuhkan URL yang ada dalam file M3U atau PLS tersebut (buka saja file tersebut dengan teks editor, misalnya TextEdit atau command more atau notepad). Kalau masih tidak yakin, URL tersebut bisa dibuka dahulu menggunakan aplikasi VLC atau browser yang bisa streaming.


Berbasis kode dari PhoneGap example (misalnya untuk platform Android ada di PhoneGap/lib/android/example/), maka tinggal mengubah 2 buah file: index.html dan index.js. Untuk kebutuhan tampilan pada contoh ini menggunakan jQuery Mobile, detilnya ada di artikel [^3].


Sebagai contoh bisa menggunakan beberapa URL Radio Streaming berikut (silahkan beri komentar jika ingin URL radionya dimasukkan pada artikel ini):


* Ardan FM Streaming URL:&nbsp;http://live.ardanradio.com:8228/ardan
* Swaragama FM Streaming URL:&nbsp;http://173.224.124.217:8160/

Implementasi pada index.html
```
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    	<link rel="stylesheet" href="css/jquery.mobile-1.3.0.min.css" />
    	<script src="js/jquery-1.9.1.min.js"></script>
    	<script src="js/jquery.mobile-1.3.0.min.js"></script>
    	<title>Radio Streaming</title>
    </head>
    <body>
        <div>
            <p style='margin-left:5%;' id="audio_title">&lt;3</p>
            <p style='margin-left:5%;' id="audio_position">&lt;3</p>
        	<div class="ui-grid-a">
            	<div class="ui-block-a">
            		<a data-role="button" data-theme="a" id="play" onclick="app.playAudio()" href="#">
            			<span class="ui-btn-text">STREAM</span>
            		</a>
            	</div>
            	<div class="ui-block-b">
            		<a data-role="button" data-theme="a" id="stop" onclick="app.stopAudio()" href="#">
            			<span class="ui-btn-text">S T O P</span>
            		</a>
            	</div>
            </div>
        </div>
        <script type="text/javascript" src="cordova-2.5.0.js"></script>
        <script type="text/javascript" src="js/index.js"></script>
        <script type="text/javascript">
            app.initialize();
        </script>
    </body>
```


Kemudian implementasi pada index.js
```
    onDeviceReady: function() {
        app.receivedEvent('deviceready');
        app.updateMedia("http://live.ardanradio.com:8228/ardan");
    },
    updateMedia: function(radioUrl) {
    	if(myMedia != null) {
    		myMedia.release();
    	}
       	document.getElementById('audio_title').innerHTML = radioUrl;
    	myMedia = new Media(radioUrl,
    				function() { // success callback
    					console.log("Media instance success.");
    				},
    				function() { // error callback
    					console.log("Media error");
    				},
    				function(status) {
    					///console.log("status: "+status);
    					mediaState = status;
    					if(status == Media.MEDIA_NONE) {
    						console.log("MEDIA_NONE");
    					} else if(status == Media.MEDIA_STARTING) {
    						console.log("MEDIA_STARTING");
    				       	document.getElementById('audio_position').innerHTML = 'buffering';
    						$('#play .ui-btn-text').text("P A U S E");
    					} else if(status == Media.MEDIA_RUNNING) {
    						console.log("MEDIA_RUNNING");
    						$('#play .ui-btn-text').text("P A U S E");
    					} else if(status == Media.MEDIA_PAUSED) {
    						console.log("MEDIA_PAUSED");
    						$('#play .ui-btn-text').text("STREAM");
    					} else if(status == Media.MEDIA_STOPPED) {
    						console.log("MEDIA_STOPPED");
    				       	document.getElementById('audio_position').innerHTML = '<3';
    						$('#play .ui-btn-text').text("STREAM");
    					} else {
    						console.log("MEDIA_UNKNOWN");
    					}
    				});
    },
    playAudio: function() {
    	if(mediaState != Media.MEDIA_STARTING && mediaState != Media.MEDIA_RUNNING) {
    		myMedia.play();
    		// Update myMedia position every second
            if (mediaTimer == null) {
                mediaTimer = setInterval(function() {
                    // get myMedia position
                    myMedia.getCurrentPosition(
                        // success callback
                        function(position) {
                            if (mediaState == 2 && position > -1) {
                            	document.getElementById('audio_position').innerHTML = position + '/' + myMedia.getDuration() + ' secs.';
                            }
                        },
                        // error callback
                        function(e) {
                            console.log("Error getting pos=" + e);
                        	document.getElementById('audio_position').innerHTML = "Error: " + e;
                        }
                    );
                }, 1000);
            }
    	} else {
    		myMedia.pause();
    	}
    },
    stopAudio: function() {
    	myMedia.stop();
    	clearInterval(mediaTimer);
    	mediaTimer = null;
    },
```

Hasilnya seperti pada gambar di artikel ini.


[^1]: http://oo.or.id/content/simple-example-how-play-music-audio-media-phonegap
[^2]: http://oo.or.id/content/how-play-music-phonegap-complete-example-audio-media-player
[^3]: http://oo.or.id/content/hello-jquery-mobile