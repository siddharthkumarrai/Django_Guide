# Django_Guide
## Requirement
1. install python
2. install uv
## Make a virtual environment
method 1 :- 
```python
$ python -m venv .venv
```
method 2 :- 
```python
$ uv venv
```
## To activate virtual environment
```python
.venv\scripts\activate
```
## Install Django
```python
(.venv) $uv pip install Django
```
## Make a Django project
```python
(.venv) $django-admin startproject basicProject01
```
## Run a Django project
```python
$ python manage.py runserver
```
- ERROR if port is already in use Error
```python
$ python manage.py runserver 8001
```
## Django server architecture
#### Django Request-Response Flow
![Flowchart of User to views.py](https://res.cloudinary.com/dnknslaku/image/upload/v1742497915/Screenshot_2025-03-21_004132_a9erfp.png)
```mermaid
flowchart LR
    User -->|REQ| urls.py --> views.py
    views.py -->|RES| User
```
> basicProject01/basicProject01/views.py
```python
from django.http import HttpResponse

def home(request):
  return HttpResponse("home route")

def sidd(request):
  return HttpResponse("hello i am sidd")
```
>  basicProject01/basicProject01/urls.py
```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/',admin.site.urls)
    path('',views.home, name='home')
    path('sidd',views.sidd, name='sidd')
]
```
## Django Folder Structure
- .venv
- basicProject01
    - basicProject01
- static
- - style.css
- templates
    - index.html
- db.sqlite3
- manage.py
## Render html page
> views.py
```python
from django.shortcuts import render

def home(request):
    return render(request, 'website/index.html')
```
```diff 
- Error :- TemplateDoesNotExist at /
```
### Resolve Error
> basicProject01/basicProject01/settings.py
```diff
TEMPLATES = [
    {
        'BACKEND': 'django.template.backenss.django.DjangoTemplates',
        'DIRS': ['templates'], ✅ 
        'APP_DIRS': True,
    }
]
```
## Link style.css
```diff
+ {% load static %}
<!DOCTYPE>
+ <link rel="stylesheet" href="{% static 'style.css' %}">
```
> basicProject01/basicProject01/settings.py
```diff
import os

STATIC_URL = 'static/'
+ STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
```
## How to make Django app
```python
$python manage.py startapp sidd
```
step 1:- To aware main project in our new app
> basicProject01/basicProject01/settings.py
```python
INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'sidd',                                ⚠️
]
```
## to configure emmit abbreviations in vsCode
- press CTRL + , and search emmit
    - Emmet: Include Languages -> click on Add Item
    - Item = django-html, Value = html
  
> sidd/templates/sidd/all_sidd.html
> sidd/views.py
```python
from django.shortcuts import render

def all_sidd(request):
    return render(request,'sidd/all_sidd.html
```
## url transfer in a app
> basicProject01/basicProject01/urls.py
```python
from django.contrib import admin
from django.urls import path, include
from . import views
urlpatterns = [
    path('admin/',admin.site.urls)
    path('',views.home, name='home')
    path('sidd',views.sidd, name='sidd')
    path('sidd/', include('sidd.urls))

]
```
> sidd/urls.py
```python
from django.urls import path
from . import views
urlpatterns = [
    path('', views.all_sidd, name="all_home),
    path('order/', viewa.order, name='all_sidd'),
]
```
## How to make Layout file
> basicProject01/templates/layout.html
```html
{% load static %}
<!doctype html>
<html>
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="description" content="{{ page_description | escape }}">
    <title>
        {% block title %}
            Default value
        {% endblock %}
    </title>
    <link rel="stylesheet" href="{% static 'style.css'%}">
  </head>
  <body>
    <nav>this is navbar </nav>
{% block content %}{% endblock%}
  </body>
</html>
```
## How to use layout templates
> basicProject01/templates/website/index.html
```python
{% extends "layout.html" %}

{% block title %}
    Home Page
{% endblock %}

{% block content %}
    <h1>hello jii </h1>
{% endblock %}
```
> basicProject01/sidd/templates/sidd/all_sidd.html
```python
{% extends "layout.html" %}

{% block title %}
    Home Page
{% endblock %}

{% block content %}
    <h1>all sidd page</h1>
{% endblock %}
```
## How to install tailwind css
```python
(.venv)$uv pip install pytailwindcss
```
#### with hot reload
```python
(.venv)$uv pip install 'django-tailwind[reload]'
```
- if this cmd give
  package audited (package will not installed)
  then install pip our project
  - command 1 :
  ```python
  (.venv)$python -m ensurepip --upgrade
  ```
  - command 2 :
  ```python
  (.venv)$python -m pip install --upgrade pip
  ```
  again try reload command
```python
(.venv)$pip install 'django-tailwind[reload]'
```
> basicProject01/basicProject01/settings.py
```python
INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'sidd',                                
    'tailwind'                             ⚠️
]
```
- To run tailwind css
```python
(.venv)$python manage.py tailwind init
```
> basicProject01/basicProject01/settings.py
```python
INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'sidd',                                
    'tailwind',                           
    'theme',                                 ⚠️
]

TAILWIND_APP_NAME = 'theme'                  ⚠️
INTERNAL_IPS=['127.0.0.1']
```
##### INSTALL TAILWIND
```python
(.venv)$python manage.py tailwind install
```
> basicProject01/templates/layout.html
```html
{% load static tailwind_tags %}                              ⚠️
{% load static %}
<!doctype html>
<html>
  <head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="description" content="{{ page_description | escape }}">
    <title>
        {% block title %}
            Default value
        {% endblock %}
    </title>
    <link rel="stylesheet" href="{% static 'style.css'%}">
    {% tailwind_css %}                                         ⚠️
  </head>
  <body>
    <nav>this is navbar </nav>
{% block content %}{% endblock%}
  </body>
</html>
```
## run this command in new terminal
```python
(.venv)$python manage.py tailwind start
```
## To resolve tailwind errors
- open command prompt
```python
C:\Users\siddh>where npm
C:\Program Files\nodejs\npm
C:\Program Files\nodejs\npm.cmd
```
> basicProject01/basicProject01/settings.py
```python
INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'sidd',                                
    'tailwind',                           
    'theme',                                 
]

TAILWIND_APP_NAME = 'theme'                  
INTERNAL_IPS=['127.0.0.1']

NPM_BIN_PATH=r"C:\Program Files\nodejs\npm.cmd"    ⚠️
```
## To enable Hot reload
> basicProject01/basicProject01/settings.py
```python
INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'sidd',                                
    'tailwind',                           
    'theme',
    'django_browser_reload',                                     ⚠️                               
]

MIDDLEWARE = [

    'django.middleware.clickjacking.XFrameOptionsMiddleware',

    'django_browser_reload.middleware.BrowserReloadMiddleware",    ⚠️ 
]
```
>  basicProject01/basicProject01/urls.py
```python
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/',admin.site.urls)
    path('',views.home, name='home')
    path('sidd',views.sidd, name='sidd')

    path("_reload_/", include("django_browser_reload.urls")           ⚠️ 
]
```
## console errors and warning
```diff
! you have 18 unapplied migration(s)
```
- run this command to resolve this waring and error
```python
(.venv)$python manage.py migrate
```
## Create a super user
```python
(.venv)$python manage.py createsuperuser
```
## Forget Password ( super user )
```python
(.venv)$python manage.py changepassword user_name
```
## TODO: // model
## Handle image
```python
(.venv)$python -m pip install Pillow
```
> basicProject01/basicProject01/settings.py
```python
import os

STATIC_URL = 'static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]

MEDIA_URL = '/media/'                                         ⚠️ 
MEDIA_ROOT = os.path.join(BASE_DIR,'media')                   ⚠️ 
```
>  basicProject01/basicProject01/urls.py
```python
from django.contrib import admin
from django.urls import path
from django.conf import settings                                 ⚠️ 
from django.conf.urls.static import static                       ⚠️  
from . import views

urlpatterns = [
    path('admin/',admin.site.urls)
    path('',views.home, name='home')
    path('sidd',views.sidd, name='sidd')

    path("_reload_/", include("django_browser_reload.urls")          
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT     ⚠️ 
```
## Run command
```python
(.venv)$python manage.py makemigrations sidd
```
```python
(.venv)$python manage.py migrate
```
## Register your model in database
> basicProject01/sidd/admin.py
```python
from django.contrib import admin
from .models import ProductVarity                                        ⚠️ 

admin.site.register(ProductVarity)                                        ⚠️   
```
