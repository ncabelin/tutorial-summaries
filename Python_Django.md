# Django tutorials
Model View Template (same as MVC but renamed)

### SETUP
* Create Django
`django-admin startproject <project-name>`
```
// sets these up
<project>/settings.py - project settings
<project>/urls.py - project urls
manage.py
```

* Create app within project
`python manage.py startapp main_app`

* Run app in dev environment
`python manage.py runserver`

* Create Index view
```

```
* Map the URL to /
```
rom django.conf.urls import url
from django.contrib import admin
from main_app import views # import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^', views.index)
]
```

* Refactor the Project's URLs dispatcher
```
from django.conf.urls import <include>, url
from django.contrib import admin

urlpatterns = [
  url(r'^admin/', admin.site.urls),
  # redirect url to app urls.py
  url(r'^', include('<main_app>.urls'))
]

# then go to main_app/urls.py
from django.conf.urls import url
from . import views

urlpatterns = [
  url(r'^$', views.index)
]
```
### TEMPLATES
* create templates directory
```
# first add your app to settings.py on main project folder
INSTALLED_APPS = [
    'main_app', # <-----
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

# create templates folder in app folder with html file for your template

# create `context` to pass to your html template
from django.shortcuts import render
def index(request):
  name = 'Whatever Name'
  value = 1000.00
  context = {
    'treasure_name': name,
    'treasure_val': value
  }
  return render(request, 'index.html', context)
```

* more complex variables
```
# use a class to define info then list

class Treasure:
  def __init__(self, name, value):
    self.name = name
    self.value = value

treasures = [
  Treasure('Gold Nugget', 1000),
  Treasure('Fool Gold', 1200)
]

def index(request):
  return render(request, 'index.html', {'treasures':treasures})
```

* template language tags
```
# create a for loop
{% for treasure in treasures %}
  <p>{{ treasure.name }}</p>
  <p>{{ treasure.value }}</p>
{% endfor %}

# if conditional
{% if treasure.value > 0 %}
  <p></p>
{% else %}
  <p></p>
{% endif %}
```

* how to use static files (e.g.css)
```
# create a 'static' folder and put css file in there
# use 'load staticfiles' in template
{% load staticfiles %}

<link href="{% static 'style.css' %}">
```

### MODELS
* create a Treasure model
```
from django.db import models
class Treasure(models.Model):
  name = models.CharField(max_length=100)
  value = models.DecimalField(max_digits=10,
                            decimal_places=2)

# models.CharField() = string or VARCHAR
# models.IntegerField() = int or INTEGER
# models.FloatField() = float or FLOAT
# models.DecimalField() = decimal or decimal

```

* if you edit the structure of your model you need to do an extra step called Migration
```
python manage.py makemigrations
python manage.py migrate
```

* Django Interactive Shell
```
python manage.py shell
>>> from main_app.models import Treasure
>>> Treasure.objects.all()
# gets [] empty
```

* READ
```
# Retrieve all objects
Treasure.objects.all()

# Filter like SQL WHERE OR LIMIT
Treasure.objects.filter(location = 'Orlando, FL')

# If you know theres only one element matching your query, use get()
Treasure.objects.get(pk = 1)
```

* CREATE
```
t = Treasure(name='Coffee can', value=20.00)
t.save()

# add description for model in your class
class Treasure(models.Model):
  name = models.CharField(max_length=100)
  value = models.DecimalField(max_digits=10, decimal_places=2)

  def __str__(self):
    return self.name
```

* UPDATE

* DELETE

* Integrate with VIEWS
```
from django.shortcutes import render
from .models import Treasure

def index(request):
  treasures = Treasure.objects.all()
  return render(request, 'index.html', {'treasures': treasures})
```

### DJANGO ADMIN
at `localhost:8000/admin`
```
# to use admin site, create super user (create username email password)
python manage.py createsuperuser
```

* Register Models with admin
```
# in your main_app folder open 'admin.py'
from django.contrib import admin
from models import Treasure

admin.site.register(Treasure)
```

### GRIDS
* to be able to create a new row for e.g. every 3 items use `forloop.counter` and `value | divisibleby: 3` in a filter
```
{% if forloop.counter|divisibleby: 3 %}
  </div><div class="row">
{% endif %}
```

### TEMPLATE INHERITANCE
```
# base.html
{% load staticfiles %}

  <navbar etc. etc. header>
{% block content %}
  <footer code etc. etc.>
{% endblock %}

# index.html
{% extends 'base.html' %}
{% load staticfiles %}

{% block content %}
  <content>
{% endblock %}
```

### FORMS
* define a new class that inherits from forms.Form
```
# create main_app/forms.py
from django import forms

class TreasureForm(forms.Form):
  name = forms.CharField(label='Name', max_length=100)
  value = forms.DecimalField(label="Value", max_digits=10)
```

* adding the URL to Post Our Form Submission
```
# at main_app/urls.py add to urlpatterns this code
url(r'^post_url/$), views.post_treasure, name='post_treasure')

# regex that will match localhost/post_url
# view.post_treasure <-- will handle the POST request
```

* using the form
```
# main_app/views.py
from .forms import TreasureForm

def post_treasure(request):
  form = TreasureForm(request.POST)
  if form.is_valid():
    treasure = Treasure(name = form.cleaned_data['name'],
                        form.cleaned_data['value'])
    treasure.save()
  return HttpResponseRedirect('/')

# pass the form in the template
def index(request):
  treasures = Treasure.objects.all()
  form = TreasureForm()
  return render(request, 'index.html',
                {'treasures': treasures, 'form':form})

# in the template html insert this
<form action="post_url" method="post">
  {% csrf_token %} # protects from Cross Site Request Forgery
  {{ form }}
</form>
```
