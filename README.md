```bash
project/
├── project/
│   ├── settings.py 
│   ├── urls.py
|
├── app/
│   └── views.py
└── manage.py


```




```bash
django-admin startproject project
cd project
python manage.py startapp app

```

```bash
pip install djangorestframework
```
```bash
pip install djangorestframework-jwt
```
## settings.py
```python

INSTALLED_APPS = [
    # ...
    'rest_framework',
    'rest_framework_jwt',
    'app'
    # ...
]
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        #'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
        'rest_framework_simplejwt.authentication.JWTAuthentication',
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
}

JWT_AUTH = {
    "SIGNING_KEY": 'your-secret-key',
    "ALGORITHM": "HS256",
    'ALLOW_REFRESH': False,
    'ACCESS_TOKEN_LIFETIME': datetime.timedelta(minutes=2),
    "REFRESH_TOKEN_LIFETIME": datetime.timedelta(days=1),
}


```

## app/view

```python
from rest_framework.decorators import api_view, permission_classes
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated


@api_view(['GET'])
@permission_classes([IsAuthenticated])
def secret_view(request):
    content = {'message': 'Welcome in secret api'}
    return Response(content)

```



## project/urls

```python

from django.urls import path
from rest_framework_simplejwt import views as jwt_views
from app.views import secret_view
  
  
urlpatterns = [
    path('api/token/',
         jwt_views.TokenObtainPairView.as_view(),
         name ='token_obtain_pair'),
    path('api/token/refresh/',
         jwt_views.TokenRefreshView.as_view(),
         name ='token_refresh'),
     path('secret/', secret_view, name ='hello'),
]
  
```

```bash
pip install httpie
```

```bash
python manage.py migrate
```

```bash
python manage.py createsuperuser
```

```bash
python manage.py runserver
```

```bash
'open another terminal'
```
```bash
http post http://127.0.0.1:8000/api/token/ username=spider password=vinayak
```
Screenshot (569).png
Screenshot (570).png


```python
response=
{
  "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

```bash
'just copy both and store in localstorage if you are using react.js'
```

```bash
http http://127.0.0.1:4000/secret/ "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTg3Mjc5NDIxLCJqdGkiOiIzYWMwNDgzOTY3NjE0ZDgxYmFjMjBiMTBjMDlkMmYwOCIsInVzZXJfaWQiOjF9.qtNrUpyPQI8W2K2T22NhcgVZGFTyLN1UL7uqJ0KnF0Y"
```

## Importan

```bash
'Call this above url after access toekn expires'
'You will  get Error : 401'
```
Screenshot (571).png

Screenshot (574).png


## solution

```bash
'use your previous refresh token'
'Now call'
```

screenshot 575 png
```bash
http POST http://localhost:8000/api/token/refresh/ refresh=abc123
```

```bash
'agin you will get 
{
  "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}

just copy both and store in localstorage

now again call secret api using new access token , this time no error'
```
screenshot 576
screesnhot 577





```bash
'The main difference between rest_framework_jwt.authentication.JSONWebTokenAuthentication and rest_framework_simplejwt.authentication.JWTAuthentication is the library they belong to and the way they handle JSON Web Tokens (JWT) for authentication.

rest_framework_jwt is a third-party package that provides JWT authentication for Django REST Framework. It has been deprecated in favor of other JWT authentication packages like rest_framework_simplejwt. JSONWebTokenAuthentication is the authentication class provided by rest_framework_jwt for using JWTs.

On the other hand, rest_framework_simplejwt is also a third-party package that provides JWT authentication for Django REST Framework. It is a newer and more actively maintained package than rest_framework_jwt. JWTAuthentication is the authentication class provided by rest_framework_simplejwt for using JWTs.

While both libraries provide JWT authentication, rest_framework_simplejwt is generally considered to be easier to use and more secure than rest_framework_jwt. Additionally, rest_framework_jwt has been deprecated and is no longer actively maintained.'

```
