---
title: "Python Web Sample Project: di.lingkari.com"
date: 2014-12-18 12:24:06+07:00
publishDate: 2014-12-18 12:24:06+07:00
image: "/images/2014/develop-python-web-with-intellij.png"
url: "/content/python-web-sample-project-di-lingkari-com"
tags: ["Python", "kelas teknologi", "Open Source", "IntelliJ"]
draft: false
---

{{< figure src="/images/2014/develop-python-web-with-intellij.png" title="Develop Python Web with IntelliJ" >}}

My latest weekend project [Kelas Python](http://oo.or.id/content/kelas-python) now growth very well, in several cities.

Some of the students ask me if I can give them <em>simple-but-completely-use-the-contents-from-class-material</em> example of the python web app.

That is why I create new open source project at my github account [^1], a python web app that is integrated with Google+ API (and next version will support store data to Google Datastore). The (deployed) web is available at

> http://di.lingkari.com

an alias domain of http://dilingkari.appspot.com

Dilingkari means circled (in english). The project named <em>circled</em> because it is related to Google+ (Circle relations) API.

How to clone the source-code, build the app, and deploy as your own website? Here is an example, how I build it using [IntelliJ IDEA](https://www.jetbrains.com/idea/).

1. Install IntelliJ with Plugins: Python, Google App Engine, HTML5. (HTML5 and some other specific plugin required the paid version, but do not worry, you can use trial version for 30 days, or if you are a students please apply for the free license).
2. Check-out from Version Control, choose git, and put https://github.com/oonid/dilingkari.git as the URL. then press clone.
3. AND NOW THE JOURNEY JUST STARTED :grin:
 OOPS.. WAIT, I WILL TRANSLATE TO ENGLISH SOON xD

4. berikut adalah beberapa gambar proses import project dari git tersebut menjadi project dari IntelliJ (Python Project with Google App Engine Library)
    {{< figure src="/images/2014/develop-python-web-with-intellij-1.png" title="Develop Python Web with IntelliJ 1" width="403" height="166" >}}
Setelah membuat IntelliJ IDEA project, jika sudah pernah melakukan setting SDK Python akan muncul tahapan seperti dalam gambar berikut, tetapi jika tidak muncul project SDK tidak perlu kuatir karena nanti bisa dibuat sendiri (mengacu ke Python yang terinstall di komputer).
    {{< figure src="/images/2014/develop-python-web-with-intellij-2.png" title="Develop Python Web with IntelliJ 2" width="640" height="329" >}}
Selanjutnya adalah pengenalan Framework, dalam project dilingkari ini menggunakan framework Flask, tapi sayangnya di IntelliJ belum ada dukungan plugin-nya, sehingga akan dikenali sebagai Framework Django. Kemudian app.yaml akan dikenali sebagai bagian dari app-engine.
    {{< figure src="/images/2014/develop-python-web-with-intellij-3.png" title="Develop Python Web with IntelliJ 3" width="641" height="146" >}}

5. And finally the project openned on IntelliJ.
	For some of you that the process is not same with my previous steps, you can edit the Module settings by press right-click at the project name.
    {{< figure src="/images/2014/develop-python-web-with-intellij-4.png" title="Develop Python Web with IntelliJ 4" width="564" height="811" >}}
Here is my Module Settings, so you can make your settings equal to mine. :grin:
    {{< figure src="/images/2014/develop-python-web-with-intellij-5a.png" title="Develop Python Web with IntelliJ 5a" width="640" height="285" >}} 
    {{< figure src="/images/2014/develop-python-web-with-intellij-5b.png" title="Develop Python Web with IntelliJ 5b" width="639" height="264" >}} 
    {{< figure src="/images/2014/develop-python-web-with-intellij-5c.png" title="Develop Python Web with IntelliJ 5c" width="641" height="252" >}} 
    {{< figure src="/images/2014/develop-python-web-with-intellij-5d.png" title="Develop Python Web with IntelliJ 5d" width="640" height="312" >}} 
    {{< figure src="/images/2014/develop-python-web-with-intellij-5e.png" title="Develop Python Web with IntelliJ 5e" width="641" height="190" >}} 
    {{< figure src="/images/2014/develop-python-web-with-intellij-5f.png" title="Develop Python Web with IntelliJ 5f" width="640" height="316" >}} 

7. To be recognized as Python Project with Google App Engine Library (if you project not recognized yet), you can add 1 file named Google_App_Engine_SDK.xml under directory dilingkari &gt; .idea &gt; libraries. Please refer image below.
    {{< figure src="/images/2014/develop-python-web-with-intellij-6.png" title="Develop Python Web with IntelliJ 6" width="639" height="239" >}} 

8. to-be-continued

Do you like project dilingkari [^1]? Please give <strong>star</strong> :star: on the github project, fork or join me to develop the web.

ps: sometime the website is over-user-limit of the daily Google+ API quota, I will find a way to dispatch the API quota load.

[^1]: https://github.com/oonid/dilingkari
