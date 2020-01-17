---
title: "Install and Start Project Django"
date: 2015-04-02T01:16:06+07:00
publishDate: 2015-04-02T01:16:06+07:00
image: "/images/2017/django-logo-positive.png"
url: "/content/install-and-start-project-django"
tags: ["Django", "Python"]
draft: false
---

{{< figure src="/images/2017/django-logo-positive.png" title="Install and Start Project Django" >}}

Install Django with pip package manager, for example inside (virtual) environment, install version 1.8 LTS [^1]:

```
pip install "django < 1.9"
```

You can check your install version (not only using pip also) with command django-admin

```
django-admin --version
```

for example it will output: `1.8`

Now we are ready to create Django project:

```
django-admin startproject rdjango
```

it will create directory rdjango, and inside the directory there will be these files:

```
./manage.py

./rdjango

./rdjango/__init__.py

./rdjango/settings.py

./rdjango/urls.py

./rdjango/wsgi.py
```

Configure database on settings.py if you want to use PostgreSQL or MySQL, then run:

```
python manage.py migrate
```

and now you can run the Django (development environment)&nbsp;using:

```
python manage.py runserver
```

[^1]: https://www.djangoproject.com/download/
