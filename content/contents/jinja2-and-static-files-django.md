---
title: "Jinja2 and Static Files on Django"
date: 2015-04-03T08:55:01+07:00
publishDate: 2015-04-03T08:55:01+07:00
image: "/images/2017/django-logo-positive.png"
url: "/content/jinja2-and-static-files-django"
tags: ["Django", "Python"]
draft: false
---

{{< figure src="/images/2017/django-logo-positive.png" title="Jinja2 and Static Files on Django" >}}

I [use Jinja2 as template engine with Django](http://oo.or.id/content/even-though-you-use-django-use-jinja2-template-engine) 1.8.

Here's my settings.py snippet

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.jinja2.Jinja2',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {'environment': 'rdjango.jinja2.environment', },
    },
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

So I have to add directory templates inside the base directory (same level with manage.py),
And also create file jinja2.py (inside project directory, same level with settings.py), to declare the environment, you can copy from this reference [^2].

```
from __future__ import absolute_import  # Python 2 only

from django.contrib.staticfiles.storage import staticfiles_storage
from django.core.urlresolvers import reverse

from jinja2 import Environment

def environment(**options):
    env = Environment(**options)
    env.globals.update({
        'static': staticfiles_storage.url,
        'url': reverse,
    })
    return env
```

The environment declare about static directory (and files), from staticfiles_storage, so you have to declare it on settings.py, my snippet for example:

```
# Static files (CSS, JavaScript, Images)

STATIC_URL = '/static/'
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),
)
```

Now you have fully configured Jinja2 as template engine with defined static directory.

[^1]: https://docs.djangoproject.com/en/1.8/howto/static-files/
[^2]: https://docs.djangoproject.com/en/1.8/topics/templates/