---
title: "Django REST Framework File Upload"
date: 2015-12-25T16:10:06+07:00
publishDate: 2015-12-25T16:10:06+07:00
image: "/images/2015/django-rest-framework-logo.png"
url: "/content/django-rest-framework-file-upload"
tags: ["Django", "Django REST Framework", "Python"]
draft: false
---

{{< figure src="/images/2015/django-rest-framework-logo.png" title="Django REST Framework File Upload" >}}


This article inspired from same title article by jxstanford on [^1].

so, what's the difference?

One of the main difference is I use JWT Authentication (django-rest-framework-jwt [^2]) instead of Token Authentication.

so here's my test.py code


```python
# tests.py

from rest_framework import status
# from rest_framework.authtoken.models import Token
from rest_framework_jwt.settings import api_settings
from rest_framework.test import APITestCase
from rest_framework.test import APIClient
from django.core import serializers
from django.core.urlresolvers import reverse
from django.core.exceptions import ObjectDoesNotExist
from django.contrib.auth.models import User
from myapp.models import FileUpload
from myproject import settings
from urlparse import urlparse


class FileUploadTests(APITestCase):

    def setUp(self):    
        self.tearDown()
        u = User.objects.create_user('test', password='test', email='test@test.test')    
        u.save()    
        # print(serializers.serialize('json', User.objects.all()))

    def tearDown(self):
        try:
            u = User.objects.get_by_natural_key('test')
            u.delete()
        except ObjectDoesNotExist:
            pass
    
        FileUpload.objects.all().delete()

    def _create_test_file(self, path):
        f = open(path, 'w')
        f.write('test123\n')
        f.close()
        f = open(path, 'rb')
        return {'datafile': f}

    def test_upload_file(self):
        url = reverse('fileupload-list')
        data = self._create_test_file('/tmp/test_upload')

        # assert authenticated user can upload file    
        # token = Token.objects.get(user__username='test')
        # client = APIClient()
        # client.credentials(HTTP_AUTHORIZATION='Token ' + token.key)

        jwt_payload_handler = api_settings.JWT_PAYLOAD_HANDLER
        jwt_encode_handler = api_settings.JWT_ENCODE_HANDLER

        user = User.objects.get_by_natural_key('test')
        payload = jwt_payload_handler(user)
        jwt_token = jwt_encode_handler(payload)

        client = APIClient()
        client.credentials(HTTP_AUTHORIZATION='JWT ' + jwt_token)

        response = client.post(url, data, format='multipart')
        # print(response.data)
    
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
        self.assertIn('created', response.data)
    
        self.assertTrue(urlparse(response.data['datafile'])
                        .path.startswith(settings.MEDIA_URL))
        self.assertEqual(response.data['owner'], 
                         User.objects.get_by_natural_key('test').id)
        self.assertIn('created', response.data)
        # assert unauthenticated user can not upload file
    
        client.logout()
        response = client.post(url, data, format='multipart')
        # print(response.data)
        # self.assertEqual(response.status_code, status.HTTP_401_UNAUTHORIZED)

        # JWT Authentication credentials not provided
        self.assertEqual(response.status_code, status.HTTP_403_FORBIDDEN)  

```

hope it will help someone out there..


[^1]: https://medium.com/@jxstanford/django-rest-framework-file-upload-e4bc8de669c0
[^2]: https://github.com/GetBlimp/django-rest-framework-jwt