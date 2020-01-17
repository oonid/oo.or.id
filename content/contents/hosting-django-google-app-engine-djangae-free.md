---
title: "Hosting Django on Google App Engine with djangae for FREE"
date: 2017-05-28 22:37:20+07:00
image: "/images/2017/django-logo-positive.png"
url: "/content/hosting-django-google-app-engine-djangae-free"
tags: ["App Engine", "Django", "kelas teknologi", "Python"]
draft: false
---

{{< figure src="/images/2017/django-logo-positive.png" title="Hosting Django on Google App Engine with djangae for FREE" >}}

> note: this post based on my experience hosting Django on Google App Engine. FREE on this article means I didn't have any billing every month, it's only personal site with not-too-much load or traffic. BUT, I set this with "activated billing setting" on my Google Cloud account, still have no time to try without activate billing.

This article written in Bahasa Indonesia.

Banyak orang menulis di artikel atau diskusi tentang free hosting (hosting gratis) untuk Django, tapi sangat sedikit yang bisa ditemui membahas bahwa hal tersebut bisa dilakukan di Google App Engine.

Sampai saat artikel ini dibuat, Google App Engine Standard environment&nbsp;(dimana kita bisa hosting secara free) hanya mendukung python versi 2, python versi 3 bisa digunakan di Google App Engine Flexible environment.

Seluruh proses instalasi dilakukan dalam virtual environment bernama sb, semua yang tertulis (sb) artinya berada dalam virtual environment:

```
# virtualenv sb
# cd sb
# source bin/activate
(sb)# 
...
(sb)# deactivate
#
```

Kemudian install Django (dalam artikel ini digunakan versi 1.11) dan djangae (dalam artikel ini menggunakan versi 0.9.9).

```
(sb)# pip freeze
(sb)# pip install Django
(sb)# pip install djangae
(sb)# pip freeze
```

Buat Django project (dan django apps yang dibutuhkan):

```
(sb)# django-admin startproject sbproject
```

Lakukan konfigurasi instalasi djangae mengikuti [1].

**(tbd)**

Buat file konfigurasi app.yaml untuk App Engine

```
runtime: python27
api_version: 1
threadsafe: true
service: default

handlers:

- url: /_ah/(mapreduce|queue|warmup).*
  script: relif.wsgi.application
  login: admin
  secure: always

- url: /_ah/internalupload.*
  script: relif.wsgi.application
  secure: always

- url: /static
  static_dir: static
  secure: always

# Set Django admin to be login:admin as well as Django's is_staff restriction
- url: /admin.*
  script: sbproject.wsgi.application
  secure: always
  login: admin

- url: /.*
  script: sbproject.wsgi.application
  secure: always

libraries:
- name: ssl
  version: "latest"

skip_files:
    - manage.py
    - README.md
    - install_deps
    - requirements.txt
    - sitepackages/dev*
    - \.storage.*
    - \.git
    - (.*)\.pyc
```

Upload files ke App Engine menggunakan aplikasi gcloud dari [2].

```
# /home/oon/google-cloud-sdk/bin/gcloud app deploy --project proj-test-only --verbosity=info app.yaml
```

[1] https://djangae.readthedocs.io/en/latest/installation/

[2]https://cloud.google.com/sdk/gcloud/
