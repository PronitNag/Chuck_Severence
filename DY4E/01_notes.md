# Django for Everybody by Charles Severance
## Comprehensive Hinglish Notes

## Course Overview

"Django for Everybody" Professor Charles Severance (Dr. Chuck) dwara banaya gaya hai, yeh course Django web framework ke fundamentals se lekar advanced topics tak cover karta hai. Yeh notes course ke structure ko follow karte hain aur har chapter ke key concepts ko Hinglish mein explain karte hain.

---

## Section 1: Web Technology and Basic Concepts

### Chapter 1: Web Applications and Django Introduction

#### Internet and HTTP Overview
- **Internet** connected computers ka global network hai
- **HTTP (Hypertext Transfer Protocol)** ek request-response protocol hai
- **Client** (browser) **server** ko request bhejta hai, aur server response deta hai
- **Request Methods**: GET (data retrieve), POST (data submit), etc.

#### Web Application Components
- **Frontend**: HTML, CSS, JavaScript (user interface)
- **Backend**: Server-side code (business logic)
- **Database**: Data storage and retrieval

#### Django Introduction
- Django ek **Python web framework** hai
- **"Batteries included"** philosophy - yani built-in functionality provide karta hai
- **MVT (Model-View-Template)** architecture ka use karta hai
  - **Model**: Database structure and data handling
  - **View**: Business logic
  - **Template**: User interface presentation

---

### Chapter 2: Installing Django on PythonAnywhere

#### PythonAnywhere Introduction
- **PythonAnywhere** ek cloud-based Python development environment hai
- Free tier available hai beginners ke liye
- Built-in web hosting and databases provide karta hai

#### Account Setup and Installation
```bash
# PythonAnywhere bash console mein:

# Virtual environment create karein
mkvirtualenv django3 --python=/usr/bin/python3

# Django install karein
pip install django

# Django version check karein
python -m django --version
```

#### First Project Setup
```bash
# New project create karein
django-admin startproject mysite

# Project directory mein jaaiye
cd mysite

# Development server start karein (localhost par)
python manage.py runserver
```

#### PythonAnywhere Web App Configuration
- Web tab mein new web app create karein
- Manual configuration select karein
- WSGI configuration file edit karein (provided path par)
- Virtual environment path set karein
- Static files configuration karein
- Web app reload karein

---

### Chapter 3: Installing Django Locally

#### Local Environment Setup
- Python 3.x install karein (python.org se)
- Command line/terminal ka use karein

#### Django Local Installation
```bash
# Virtual environment create karein
python -m venv djangoenv

# Virtual environment activate karein
# Windows par:
djangoenv\Scripts\activate
# Mac/Linux par:
source djangoenv/bin/activate

# Django install karein
pip install django

# Django version check karein
python -m django --version
```

#### Project Creation and Structure
```bash
# Project create karein
django-admin startproject mysite

# Project directory mein jaaiye
cd mysite

# Server start karein
python manage.py runserver
```

#### Project Directory Structure
```
mysite/
    manage.py          # Command-line utility for admin tasks
    mysite/
        __init__.py    # Python package indicator
        settings.py    # Project configuration settings
        urls.py        # URL declarations
        asgi.py        # ASGI config for deployment
        wsgi.py        # WSGI config for deployment
```

---

### Chapter 4: Python and Objects

#### Object-Oriented Programming (OOP) Basics
- **Class**: Object ka blueprint/template
- **Object**: Class ka instance
- **Attributes**: Object ki properties/variables
- **Methods**: Object ke functions

#### Python Class Example
```python
class Car:
    # Constructor method
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0
    
    # Method
    def get_descriptive_name(self):
        return f"{self.year} {self.make} {self.model}"
    
    def read_odometer(self):
        print(f"Is car ne {self.odometer_reading} km travel kiya hai")
    
    def update_odometer(self, km):
        if km >= self.odometer_reading:
            self.odometer_reading = km
        else:
            print("Odometer ko peeche nahi kar sakte!")

# Car object create karna
my_car = Car('Honda', 'City', 2020)
print(my_car.get_descriptive_name())
```

#### Inheritance in Python
```python
class ElectricCar(Car):
    def __init__(self, make, model, year, battery_size):
        # Parent class constructor call
        super().__init__(make, model, year)
        self.battery_size = battery_size
    
    def describe_battery(self):
        print(f"Is car mein {self.battery_size}-kWh battery hai")
```

#### Django mein OOP
- Django framework poori tarah se OOP par based hai
- **Models** classes hain jo database tables represent karte hain
- **Views** functions ya classes hain jo requests handle karte hain
- **Templates** presentation logic ko handle karte hain

---

## Section 2: Building Basic Django Applications

### Chapter 5: Django Models

#### Models Introduction
- Django **ORM (Object-Relational Mapping)** ka use karta hai
- Database tables ko Python classes ke through represent karta hai
- Each model class represents one database table

#### Creating a Simple Model
```python
# models.py
from django.db import models

class Cat(models.Model):
    name = models.CharField(max_length=100)
    breed = models.CharField(max_length=100)
    weight = models.FloatField(null=True, blank=True)
    
    # String representation
    def __str__(self):
        return self.name
```

#### Common Field Types
- **CharField**: Fixed-length text (max_length required)
- **TextField**: Variable-length text
- **IntegerField**: Integer values
- **FloatField**: Floating point numbers
- **BooleanField**: True/False values
- **DateField**: Date values
- **DateTimeField**: Date and time values
- **EmailField**: Email addresses
- **ForeignKey**: One-to-many relationships

#### Database Migrations
```bash
# Model changes ko track karne ke liye migration files create karein
python manage.py makemigrations

# Database mein actual changes apply karein
python manage.py migrate
```

#### Admin Site Registration
```python
# admin.py
from django.contrib import admin
from .models import Cat

admin.site.register(Cat)
```

---

### Chapter 6: Django Views

#### Views Introduction
- Views HTTP requests ko handle karte hain aur HTTP responses return karte hain
- Function-based ya class-based ho sakte hain

#### Simple Function-Based View
```python
# views.py
from django.http import HttpResponse
from django.shortcuts import render
from .models import Cat

def index(request):
    return HttpResponse("Hello, Django world!")

def cat_list(request):
    cats = Cat.objects.all()
    context = {'cat_list': cats}
    return render(request, 'cats/cat_list.html', context)
```

#### URL Configuration
```python
# Project level urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('cats/', include('cats.urls')),
]

# App level urls.py
from django.urls import path
from . import views

app_name = 'cats'  # URL namespace
urlpatterns = [
    path('', views.cat_list, name='all'),
    path('create/', views.cat_create, name='create'),
    path('update/<int:pk>/', views.cat_update, name='update'),
]
```

#### Request and Response Objects
- **request.method**: Request ka HTTP method (GET, POST, etc.)
- **request.GET**: URL query parameters (dictionary)
- **request.POST**: Form data (dictionary)
- **request.user**: Current logged-in user (or AnonymousUser)

---

### Chapter 7: Django Templates

#### Templates Introduction
- Templates HTML pages with placeholders for dynamic content hote hain
- Django Template Language (DTL) dynamic content ko insert karne ke liye use hota hai

#### Template Directory Structure
```
myapp/
    templates/
        myapp/  # Namespace to avoid conflicts
            base.html
            cat_list.html
            cat_detail.html
```

#### Template Example
```html
<!-- base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Cat App{% endblock %}</title>
</head>
<body>
    <header>
        <h1>Meri Cat App</h1>
        <nav>
            <a href="{% url 'cats:all' %}">All Cats</a>
        </nav>
    </header>
    
    <main>
        {% block content %}{% endblock %}
    </main>
    
    <footer>&copy; 2025</footer>
</body>
</html>

<!-- cat_list.html -->
{% extends 'cats/base.html' %}

{% block title %}All Cats{% endblock %}

{% block content %}
    <h2>All Cats</h2>
    
    <ul>
        {% for cat in cat_list %}
            <li>{{ cat.name }} - {{ cat.breed }}</li>
        {% empty %}
            <li>Koi cats nahi mile</li>
        {% endfor %}
    </ul>
    
    <p><a href="{% url 'cats:create' %}">Add a Cat</a></p>
{% endblock %}
```

#### Template Tags and Filters
- **{% for %}** / **{% endfor %}**: Looping
- **{% if %}** / **{% endif %}**: Conditional statements
- **{% url %}**: URL generation
- **{% block %}** / **{% endblock %}**: Template inheritance
- **{{ variable|filter }}**: Variables and filters
  - Common filters: `upper`, `lower`, `date`, `truncatewords`

---

### Chapter 8: Forms in Django

#### HTML Forms Basics
```html
<form method="POST" action="{% url 'cats:create' %}">
    {% csrf_token %}
    
    <div>
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required>
    </div>
    
    <div>
        <label for="breed">Breed:</label>
        <input type="text" id="breed" name="breed" required>
    </div>
    
    <button type="submit">Save</button>
</form>
```

#### Processing Form Data in Views
```python
def cat_create(request):
    if request.method == 'POST':
        # Form submission handling
        name = request.POST.get('name')
        breed = request.POST.get('breed')
        
        # Validation (minimal example)
        if not name or not breed:
            return render(request, 'cats/cat_form.html', {
                'error_message': 'Name and breed are required.'
            })
        
        # Create and save the cat
        cat = Cat(name=name, breed=breed)
        cat.save()
        
        # Redirect to list view
        return redirect('cats:all')
    
    # GET request - display empty form
    return render(request, 'cats/cat_form.html')
```

#### CSRF Protection
- **{% csrf_token %}** Django mein forms ke security ke liye zaroori hai
- Cross-Site Request Forgery attacks se protect karta hai
- Ye form submission ke time ek unique token verify karta hai

---

### Chapter 9: Form Validation

#### Django ModelForm
```python
# forms.py
from django import forms
from .models import Cat

class CatForm(forms.ModelForm):
    class Meta:
        model = Cat
        fields = ['name', 'breed', 'weight']
        labels = {
            'name': 'Cat Ka Naam',
            'breed': 'Breed',
            'weight': 'Weight (kg)',
        }
        widgets = {
            'name': forms.TextInput(attrs={'class': 'form-control'}),
            'breed': forms.TextInput(attrs={'class': 'form-control'}),
            'weight': forms.NumberInput(attrs={'class': 'form-control'}),
        }
```

#### Using ModelForm in Views
```python
from .forms import CatForm

def cat_create(request):
    if request.method == 'POST':
        form = CatForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('cats:all')
    else:
        form = CatForm()
    
    context = {'form': form}
    return render(request, 'cats/cat_form.html', context)
```

#### Form in Template
```html
<form method="POST">
    {% csrf_token %}
    
    {{ form.as_p }}
    <!-- Alternatives:
    {{ form.as_table }}
    {{ form.as_ul }}
    -->
    
    <button type="submit">Save</button>
</form>
```

#### Custom Form Validation
```python
class CatForm(forms.ModelForm):
    # Field-level validation
    def clean_name(self):
        name = self.cleaned_data.get('name')
        if len(name) < 3:
            raise forms.ValidationError("Name kam se kam 3 characters ka hona chahiye")
        return name
    
    # Form-level validation
    def clean(self):
        cleaned_data = super().clean()
        name = cleaned_data.get('name')
        breed = cleaned_data.get('breed')
        
        if name and breed and name.lower() == breed.lower():
            raise forms.ValidationError("Name aur breed same nahi ho sakte")
        
        return cleaned_data
```

---

### Chapter 10: Sessions in Django

#### Sessions Introduction
- **Sessions** user information ko temporarily store karne ka tarika hai
- Multiple requests ke across user state maintain karte hain
- Shopping carts, user preferences, etc. ke liye useful

#### Session Configuration
```python
# settings.py (typically already configured)
INSTALLED_APPS = [
    # ...
    'django.contrib.sessions',
    # ...
]

MIDDLEWARE = [
    # ...
    'django.contrib.sessions.middleware.SessionMiddleware',
    # ...
]
```

#### Session Usage
```python
# views.py
def visit_counter(request):
    # Get visit count from session, default to 0 if not present
    num_visits = request.session.get('num_visits', 0)
    
    # Increment the count
    num_visits += 1
    
    # Store the new count in the session
    request.session['num_visits'] = num_visits
    
    # Other session usage
    if 'last_visit_date' not in request.session:
        request.session['last_visit_date'] = str(datetime.now())
    
    context = {
        'num_visits': num_visits,
        'last_visit': request.session['last_visit_date'],
    }
    
    return render(request, 'app/visits.html', context)
```

#### Session in Templates
```html
<p>You have visited this page {{ num_visits }} times</p>
<p>Your last visit was: {{ last_visit }}</p>
```

---

### Chapter 11: Owned Rows

#### User-Specific Content
- Users ko sirf unhi rows ko edit/delete karne ka permission dena jinhein unhone create kiya

#### Owner Field in Model
```python
# models.py
from django.db import models
from django.contrib.auth.models import User

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    owner = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return self.title
```

#### Owner Check in Views
```python
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import UpdateView, DeleteView
from django.http import HttpResponse

class ArticleUpdateView(LoginRequiredMixin, UpdateView):
    model = Article
    fields = ['title', 'content']
    
    def get_queryset(self):
        # Filter by the current user
        return super().get_queryset().filter(owner=self.request.user)

class ArticleDeleteView(LoginRequiredMixin, DeleteView):
    model = Article
    success_url = '/articles/'
    
    def get_queryset(self):
        # Filter by the current user
        return super().get_queryset().filter(owner=self.request.user)
```

---

### Chapter 12: Django Users

#### Authentication System
- Django built-in authentication system provide karta hai
- Users, permissions, aur groups manage karta hai

#### User Authentication Setup
```python
# settings.py
INSTALLED_APPS = [
    # ...
    'django.contrib.auth',
    'django.contrib.contenttypes',
    # ...
]
```

#### Login View
```python
# views.py
from django.contrib.auth import authenticate, login, logout
from django.shortcuts import render, redirect

def login_view(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        
        user = authenticate(request, username=username, password=password)
        
        if user is not None:
            login(request, user)
            return redirect('home')
        else:
            return render(request, 'accounts/login.html', {'error': 'Invalid credentials'})
    
    return render(request, 'accounts/login.html')

def logout_view(request):
    logout(request)
    return redirect('home')
```

#### Login Template
```html
<form method="POST">
    {% csrf_token %}
    
    <div>
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required>
    </div>
    
    <div>
        <label for="password">Password:</label>
        <input type="password" id="password" name="password" required>
    </div>
    
    <button type="submit">Login</button>
</form>
```

#### User Registration
```python
from django.contrib.auth.models import User
from django.contrib.auth.forms import UserCreationForm

def register_view(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request, user)
            return redirect('home')
    else:
        form = UserCreationForm()
    
    return render(request, 'accounts/register.html', {'form': form})
```

---

### Chapter 13: Django Forms (Advanced)

#### Forms Customization
```python
from django import forms
from .models import Profile

class ProfileForm(forms.ModelForm):
    birth_date = forms.DateField(widget=forms.DateInput(attrs={'type': 'date'}))
    bio = forms.CharField(widget=forms.Textarea(attrs={'rows': 5}))
    
    class Meta:
        model = Profile
        fields = ['full_name', 'birth_date', 'bio', 'location']
```

#### Form with File Uploads
```python
class ProfilePictureForm(forms.ModelForm):
    class Meta:
        model = Profile
        fields = ['profile_picture']
        widgets = {
            'profile_picture': forms.FileInput(attrs={'accept': 'image/*'})
        }
```

#### Processing File Uploads
```python
def update_profile_picture(request):
    if request.method == 'POST':
        form = ProfilePictureForm(request.POST, request.FILES, instance=request.user.profile)
        if form.is_valid():
            form.save()
            return redirect('profile')
    else:
        form = ProfilePictureForm(instance=request.user.profile)
    
    return render(request, 'accounts/update_picture.html', {'form': form})
```

```html
<form method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Upload</button>
</form>
```

---

### Chapter 14: One-to-Many Relationships

#### Foreign Key Relationships
```python
# models.py
from django.db import models
from django.contrib.auth.models import User

class Author(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    bio = models.TextField(blank=True)
    
    def __str__(self):
        return self.user.username

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.ForeignKey(Author, on_delete=models.CASCADE, related_name='books')
    published_date = models.DateField()
    summary = models.TextField()
    
    def __str__(self):
        return self.title
```

#### Accessing Related Objects
```python
# Forward access (book -> author)
book = Book.objects.get(id=1)
author_name = book.author.user.username

# Reverse access (author -> books)
author = Author.objects.get(id=1)
books = author.books.all()  # Using related_name
```

#### Creating Related Objects
```python
# views.py
def create_book(request, author_id):
    author = Author.objects.get(id=author_id)
    
    if request.method == 'POST':
        form = BookForm(request.POST)
        if form.is_valid():
            # Create book but don't save yet
            book = form.save(commit=False)
            book.author = author  # Set the foreign key
            book.save()
            return redirect('book_detail', pk=book.pk)
    else:
        form = BookForm()
    
    return render(request, 'books/book_form.html', {'form': form})
```

---

### Chapter 15: Many-to-Many Relationships

#### Many-to-Many Model Example
```python
# models.py
class Category(models.Model):
    name = models.CharField(max_length=100)
    
    def __str__(self):
        return self.name
    
    class Meta:
        verbose_name_plural = "Categories"

class Product(models.Model):
    name = models.CharField(max_length=200)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    description = models.TextField()
    categories = models.ManyToManyField(Category, related_name='products')
    
    def __str__(self):
        return self.name
```

#### Working with Many-to-Many Fields
```python
# Create objects
product = Product.objects.get(id=1)
category1 = Category.objects.get(name="Electronics")
category2 = Category.objects.get(name="Gadgets")

# Add categories to product
product.categories.add(category1)
product.categories.add(category2)
# Or add multiple at once
product.categories.add(category1, category2)

# Remove categories
product.categories.remove(category1)

# Set specific categories (replaces all existing)
product.categories.set([category1, category2])

# Clear all categories
product.categories.clear()

# Check if product has a category
if category1 in product.categories.all():
    print("Product has this category")
```

#### Many-to-Many in Forms
```python
class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = ['name', 'price', 'description', 'categories']
        widgets = {
            'categories': forms.CheckboxSelectMultiple()
        }
```

---

## Section 3: Django Features and Libraries

### Chapter 16: Favorites Feature

#### Creating a Favorites System
```python
# models.py
class Favorite(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    product = models.ForeignKey(Product, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)
    
    class Meta:
        # Ensure each user-product pair is unique
        unique_together = ['user', 'product']
```

#### Toggle Favorite View
```python
# views.py
from django.http import JsonResponse
from django.contrib.auth.decorators import login_required

@login_required
def toggle_favorite(request, product_id):
    product = Product.objects.get(id=product_id)
    user = request.user
    
    try:
        # Try to find existing favorite
        favorite = Favorite.objects.get(user=user, product=product)
        # If found, delete it (unfavorite)
        favorite.delete()
        is_favorite = False
    except Favorite.DoesNotExist:
        # If not found, create it (favorite)
        Favorite.objects.create(user=user, product=product)
        is_favorite = True
    
    return JsonResponse({
        'success': True,
        'is_favorite': is_favorite
    })
```

#### Favorites in Template
```html
<button class="favorite-button" data-product-id="{{ product.id }}">
    {% if product in user_favorites %}
        ★ Unfavorite
    {% else %}
        ☆ Favorite
    {% endif %}
</button>
```

---

### Chapter 17: Django Crispy Forms

#### Installation and Setup
```bash
# Install django-crispy-forms
pip install django-crispy-forms

# settings.py mein add karein
INSTALLED_APPS = [
    # ...
    'crispy_forms',
]

CRISPY_TEMPLATE_PACK = 'bootstrap4'  # Ya bootstrap5, ya koi aur template pack
```

#### Using Crispy Forms
```html
{% load crispy_forms_tags %}

<form method="POST">
    {% csrf_token %}
    {{ form|crispy }}
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

#### Form Helper
```python
# forms.py
from crispy_forms.helper import FormHelper
from crispy_forms.layout import Submit, Layout, Div, Field

class ContactForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)
    
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        
        self.helper = FormHelper()
        self.helper.form_method = 'post'
        self.helper.add_input(Submit('submit', 'Send Message'))
        
        self.helper.layout = Layout(
            Div(
                Field('name', css_class='form-control-lg'),
                Field('email'),
                css_class='row'
            ),
            Field('message', rows=5)
        )
```

---

### Chapter 18: Bootstrap in Django

#### Adding Bootstrap
```html
<!-- base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
    
    {% block extra_css %}{% endblock %}
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="{% url 'home' %}">MySite</a>
            
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'home' %}">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'products:list' %}">Products</a>
                    </li>
                </ul>
                
                <ul class="navbar-nav ms-auto">
                    {% if user.is_authenticated %}
                        <li class="nav-item">
                            <a class="nav-link" href="{% url 'profile' %}">Profile</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{% url 'logout' %}">Logout</a>
                        </li>
                    {% else %}
                        <li class="nav-item">
                            <a class="nav-link" href="{% url 'login' %}">Login</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" href="{% url 'register' %}">Register</a>
                        </li>
                    {% endif %}
                </ul>
            </div>
        </div>
    </nav>
    
    <div class="container mt-4">
        {% if messages %}
            {% for message in messages %}
                <div class="alert alert-{{ message.tags }}">
                    {{ message }}
                </div>
            {% endfor %}
        {% endif %}
        
        {% block content %}{% endblock %}
    </div>
    
    <!-- Bootstrap JS Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    
    {% block extra_js %}{% endblock %}
</body>
</html>
```

#### Using Bootstrap in Templates
```html
{% extends 'base.html' %}

{% block content %}
    <div class="row">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header">
                    Product List
                </div>
                <div class="card-body">
                    <div class="row row-cols-1 row-cols-md-2 g-4">
                        {% for product in products %}
                            <div class="col">
                                <div class="card h-100">
                                    {% if product.image %}
                                        <img src="{{ product.image.url }}" class="card-img-top" alt="{{ product.name }}">
                                    {% endif %}
                                    <div class="card-body">
                                        <h5 class="card-title">{{ product.name }}</h5>
                                        <p class="card-text">{{ product.description|truncatechars:100 }}</p>
                                        <a href="{% url 'products:detail' product.id %}" class="btn btn-primary">View Details</a>
                                    </div>
                                </div>
                            </div>
                        {% empty %}
                            <div class="col-12">
                                <p>No products available</p>
                            </div>
                        {% endfor %}
                    </div>
                </div>
            </div>
        </div>
        
        <div class="col-md-4">
            <div class="card">
                <div class="card-header">
                    Categories
                </div>
                <div class="card-body">
                    <ul class="list-group">
                        {% for category in categories %}
                            <li class="list-group-item">
                                <a href="{% url 'products:by_category' category.id %}">
                                    {{ category.name }}
