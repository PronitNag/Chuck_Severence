# Django Autos CRUD Application Notes (Hinglish)

## Introduction (परिचय)

Iss chapter mein hum ek chota sa CRUD application banayenge jo ek autos database ko support karega. Yeh exercise humein Django ke concepts ko review karne mein help karega jo humne pehle tutorials mein thode jaldi se cover kiye the.

CRUD ka matlab hai:
- **C**reate (बनाना) - Nayi entries create karna
- **R**ead (पढ़ना) - Database se data retrieve karna
- **U**pdate (अपडेट करना) - Existing entries ko modify karna
- **D**elete (हटाना) - Entries ko delete karna

## Prerequisites (आवश्यकताएँ)

- Django ka basic knowledge
- Python programming experience
- HTML, CSS aur forms ka basic understanding
- Django tutorial ka completion

## Project Setup (प्रोजेक्ट सेटअप)

### Step 1: New Django Project Create Karna

```bash
django-admin startproject mysite
cd mysite
python manage.py startapp autos
```

### Step 2: Settings.py Mein App Register Karna

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'autos.apps.AutosConfig',  # Add kiya gaya app
]
```

## Models (मॉडल्स)

Autos app ke liye do models create karenge - Make (car manufacturer) aur Auto (car details).

### models.py

```python
from django.db import models
from django.core.validators import MinLengthValidator

class Make(models.Model):
    name = models.CharField(
        max_length=200,
        validators=[MinLengthValidator(2, "Make must be greater than 1 character")]
    )
    
    def __str__(self):
        return self.name

class Auto(models.Model):
    nickname = models.CharField(
        max_length=200,
        validators=[MinLengthValidator(2, "Nickname must be greater than 1 character")]
    )
    mileage = models.PositiveIntegerField()
    comments = models.CharField(max_length=300)
    make = models.ForeignKey('Make', on_delete=models.CASCADE, null=False)
    
    def __str__(self):
        return self.nickname
```

## Migrations (माइग्रेशन्स)

Models create karne ke baad, migrations run karein:

```bash
python manage.py makemigrations
python manage.py migrate
```

## URLs Configuration (यूआरएल कॉन्फिगरेशन)

### mysite/urls.py (Main URL File)

```python
from django.contrib import admin
from django.urls import path, include
from django.views.generic import TemplateView

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('django.contrib.auth.urls')),
    path('autos/', include('autos.urls')),
    path('', TemplateView.as_view(template_name='home.html')),
]
```

### autos/urls.py (App Specific URLs)

```python
from django.urls import path
from . import views

app_name = 'autos'
urlpatterns = [
    path('', views.MainView.as_view(), name='all'),
    path('main/create/', views.AutoCreate.as_view(), name='auto_create'),
    path('main/<int:pk>/update/', views.AutoUpdate.as_view(), name='auto_update'),
    path('main/<int:pk>/delete/', views.AutoDelete.as_view(), name='auto_delete'),
    path('lookup/', views.MakeView.as_view(), name='make_list'),
    path('lookup/create/', views.MakeCreate.as_view(), name='make_create'),
    path('lookup/<int:pk>/update/', views.MakeUpdate.as_view(), name='make_update'),
    path('lookup/<int:pk>/delete/', views.MakeDelete.as_view(), name='make_delete'),
]
```

## Views (व्यूज़)

Django ke generic class-based views ka use karke CRUD operations implement karenge.

### views.py

```python
from django.contrib.auth.mixins import LoginRequiredMixin
from django.shortcuts import render
from django.views import View
from django.views.generic.edit import CreateView, UpdateView, DeleteView
from django.urls import reverse_lazy

from autos.models import Auto, Make

# Main Auto Views
class MainView(LoginRequiredMixin, View):
    def get(self, request):
        make_count = Make.objects.all().count()
        auto_list = Auto.objects.all()
        context = {'make_count': make_count, 'auto_list': auto_list}
        return render(request, 'autos/auto_list.html', context)

class AutoCreate(LoginRequiredMixin, CreateView):
    model = Auto
    fields = '__all__'
    success_url = reverse_lazy('autos:all')

class AutoUpdate(LoginRequiredMixin, UpdateView):
    model = Auto
    fields = '__all__'
    success_url = reverse_lazy('autos:all')

class AutoDelete(LoginRequiredMixin, DeleteView):
    model = Auto
    fields = '__all__'
    success_url = reverse_lazy('autos:all')

# Make Views
class MakeView(LoginRequiredMixin, View):
    def get(self, request):
        ml = Make.objects.all()
        context = {'make_list': ml}
        return render(request, 'autos/make_list.html', context)

class MakeCreate(LoginRequiredMixin, CreateView):
    model = Make
    fields = '__all__'
    success_url = reverse_lazy('autos:make_list')

class MakeUpdate(LoginRequiredMixin, UpdateView):
    model = Make
    fields = '__all__'
    success_url = reverse_lazy('autos:make_list')

class MakeDelete(LoginRequiredMixin, DeleteView):
    model = Make
    fields = '__all__'
    success_url = reverse_lazy('autos:make_list')
```

## Templates (टेम्पलेट्स)

Project mein templates folder structure:

```
mysite/
    templates/
        home.html
        base_bootstrap.html
    autos/
        templates/
            autos/
                auto_list.html
                auto_form.html
                auto_confirm_delete.html
                make_list.html
                make_form.html
                make_confirm_delete.html
```

### Template Examples

#### base_bootstrap.html (Base Template)

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Django Autos{% endblock %}</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        {% block content %}{% endblock %}
    </div>
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</body>
</html>
```

#### auto_list.html (Main List View)

```html
{% extends "base_bootstrap.html" %}

{% block content %}
<h1>Auto List</h1>

{% if auto_list %}
<ul>
    {% for auto in auto_list %}
    <li>
        {{ auto.nickname }} ({{ auto.make }}) - {{ auto.mileage }} miles
        (<a href="{% url 'autos:auto_update' auto.id %}">Update</a> |
        <a href="{% url 'autos:auto_delete' auto.id %}">Delete</a>)
    </li>
    {% endfor %}
</ul>
{% else %}
<p>There are no autos in the database.</p>
{% endif %}

<p>
    {% if make_count > 0 %}
    <a href="{% url 'autos:auto_create' %}">Add an auto</a>
    {% else %}
    Please add a make before adding an auto.
    {% endif %}
</p>
<p>
    <a href="{% url 'autos:make_list' %}">View makes</a> ({{ make_count }})
</p>
<p>
    <a href="{% url 'logout' %}">Logout</a>
</p>
{% endblock %}
```

## Login System (लॉगिन सिस्टम)

Hum Django ke built-in authentication system ka use karenge.

### Settings.py Configuration

```python
# Login configuration
LOGIN_REDIRECT_URL = '/'
LOGOUT_REDIRECT_URL = '/'
```

## Testing (टेस्टिंग)

Application ko test karne ke liye:

1. Server start karein: `python manage.py runserver`
2. Admin user create karein: `python manage.py createsuperuser`
3. Browser mein visit karein: `http://localhost:8000/autos/`
4. Login karein and application ke different features test karein

## Important Django Concepts (महत्वपूर्ण Django कॉन्सेप्ट्स)

1. **Models** - Database structure define karte hain
2. **Views** - Business logic handle karte hain
3. **Templates** - UI define karte hain
4. **URLs** - Routing configure karte hain
5. **Forms** - User input handle karte hain
6. **Authentication** - User login/logout manage karta hai

## Generic Class-Based Views (जेनेरिक क्लास-बेस्ड व्यूज़)

Django mein common operations ke liye built-in class-based views hote hain:

1. **CreateView** - Naye objects create karne ke liye
2. **UpdateView** - Existing objects update karne ke liye
3. **DeleteView** - Objects delete karne ke liye
4. **ListView** - Objects ki list display karne ke liye
5. **DetailView** - Single object ka detail display karne ke liye

## Conclusion (निष्कर्ष)

Iss chapter mein humne ek complete CRUD application banaya hai jo autos database ko manage karta hai. Humne Django ke important concepts jaise models, views, templates, URLs aur authentication ke baare mein seekha hai. Yeh foundation aage ke projects ke liye bahut useful rahega.

## Next Steps (अगले कदम)

1. Application mein aur features add karna (jaise pagination, search)
2. UI ko improve karna CSS aur JavaScript ke saath
3. Testing implement karna
4. Security features add karna
5. Performance optimize karna
