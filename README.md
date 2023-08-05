# Django Cheat Sheet

### Requirement
- virtualenv for django
- django on virtualenv

### Installing package
```
    pip install virtualenv
    pip install virtualenvwrapper-win
    pip install django
```

### Creating Virtual Environment
```
    makevirtualenv <name-of-virtual-environment>
```

### Log in to created Virtual Environment
```
    workon <name-of-virtual-environment>
```

### Creating django project
```
    django-admin startproject <project-name>
```

### Creating app/middleware/api
```
    python manage.py startapp <name-of-app/middleware/api>
```

### Run django project
```
    python manage.py runserver
```

### Insert urls from API to main project
mainProject/urls.py
```py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('<name-of-api>.urls'))
]
```

### Using Templates
mainProject/settings.py
```py
......
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR, '<name-of-templates-foder>'],
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
......
```

name-of-API/urls.py
```py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.<method-you-have>, name='<method-you-have>'),
]    
```

name-of-API/views.py
```py
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def <method-you-have>(request):
    name='this is optional'
    return render(request,'<name-of-templates>.html',{'name':name})
```

name-of-templates-foder/name-of-templates.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Something {{name}}</h1>

    <form action="counter" method="post">
        <!-- insert csrf token for bypass post request -->
        {{% csrf_token %}}

        <textarea name="words" placeholder="WErite Here" id="" cols="30" rows="10"></textarea>
        <br>
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

output
```
    Something This is Optional

    >textarea



    >/textarea

    >/button:submit
```


### Set Static Folder

- Create Static Folder
- Add this on the settings.py on mainProject file
setting.py
```py
.....

STATIC_URL = 'static/'
#Add this
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)

....
```
- Link the css with django static definition
```html
{% load static %}
.....
<head>
    .....
    <link rel="stylesheet" href="{% static 'style.css' %}">
    .....
</head>
.....
```

# Manage Models
### Configure models
- Add your API into your mainProject settings.py
mainProject/settings.py
```py
....
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'name-of-api',
]
....
```
- Set the models on your API
```py
from django.db import models

# Create your models here.
# id is auto-generated
class YourModels(models.Model):
    thisisname = models.CharField()#String Type text

```
- Make migration from commandline
```cmd
python manage.py makemigration
```