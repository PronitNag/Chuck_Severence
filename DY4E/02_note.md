{{ category.name }}
                                </a>
                            </li>
                        {% endfor %}
                    </ul>
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```

---

### Chapter 19: Pagination in Django

#### Basic Pagination
```python
# views.py
from django.core.paginator import Paginator

def product_list(request):
    products = Product.objects.all().order_by('-created_at')
    
    # Create a paginator with 12 items per page
    paginator = Paginator(products, 12)
    
    # Get the page number from the request
    page_number = request.GET.get('page', 1)
    
    # Get the Page object for the requested page
    page_obj = paginator.get_page(page_number)
    
    return render(request, 'products/list.html', {
        'page_obj': page_obj
    })
```

#### Pagination Template
```html
<div class="row">
    {% for product in page_obj %}
        <div class="col-md-3 mb-4">
            <!-- Product card content -->
        </div>
    {% endfor %}
</div>

<!-- Pagination controls -->
<nav aria-label="Page navigation">
    <ul class="pagination justify-content-center">
        {% if page_obj.has_previous %}
            <li class="page-item">
                <a class="page-link" href="?page=1">&laquo; First</a>
            </li>
            <li class="page-item">
                <a class="page-link" href="?page={{ page_obj.previous_page_number }}">Previous</a>
            </li>
        {% else %}
            <li class="page-item disabled">
                <a class="page-link" href="#">&laquo; First</a>
            </li>
            <li class="page-item disabled">
                <a class="page-link" href="#">Previous</a>
            </li>
        {% endif %}
        
        <li class="page-item active">
            <span class="page-link">
                Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}
            </span>
        </li>
        
        {% if page_obj.has_next %}
            <li class="page-item">
                <a class="page-link" href="?page={{ page_obj.next_page_number }}">Next</a>
            </li>
            <li class="page-item">
                <a class="page-link" href="?page={{ page_obj.paginator.num_pages }}">Last &raquo;</a>
            </li>
        {% else %}
            <li class="page-item disabled">
                <a class="page-link" href="#">Next</a>
            </li>
            <li class="page-item disabled">
                <a class="page-link" href="#">Last &raquo;</a>
            </li>
        {% endif %}
    </ul>
</nav>
```

#### Class-Based View with Pagination
```python
from django.views.generic import ListView

class ProductListView(ListView):
    model = Product
    template_name = 'products/list.html'
    context_object_name = 'products'
    paginate_by = 12
    ordering = ['-created_at']
```

---

### Chapter 20: Email in Django

#### Email Configuration
```python
# settings.py
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'  # Ya koi aur SMTP server
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your-email@gmail.com'
EMAIL_HOST_PASSWORD = 'your-app-password'  # App password use karein, normal password nahi
```

#### Sending Simple Email
```python
from django.core.mail import send_mail

def contact_form(request):
    if request.method == 'POST':
        subject = request.POST.get('subject')
        message = request.POST.get('message')
        from_email = request.POST.get('email')
        recipient_list = ['your-email@example.com']
        
        send_mail(
            subject,
            message,
            from_email,
            recipient_list,
            fail_silently=False,
        )
        
        messages.success(request, 'Your message has been sent!')
        return redirect('contact')
    
    return render(request, 'contact.html')
```

#### HTML Email with Attachments
```python
from django.core.mail import EmailMultiAlternatives
from django.template.loader import render_to_string
from django.utils.html import strip_tags

def send_order_confirmation(request, order_id):
    order = Order.objects.get(id=order_id)
    user_email = order.user.email
    
    # HTML Email content from template
    html_content = render_to_string('emails/order_confirmation.html', {
        'order': order,
        'user': request.user
    })
    
    # Plain text alternative
    text_content = strip_tags(html_content)
    
    # Create email
    email = EmailMultiAlternatives(
        subject=f'Order Confirmation #{order.id}',
        body=text_content,
        from_email='noreply@example.com',
        to=[user_email]
    )
    
    # Attach HTML content
    email.attach_alternative(html_content, "text/html")
    
    # Optional: Add attachment
    invoice_pdf = generate_invoice_pdf(order)  # This would be your custom function
    email.attach(f'invoice_{order.id}.pdf', invoice_pdf, 'application/pdf')
    
    # Send email
    email.send()
    
    return redirect('order_detail', order_id=order.id)
```

---

## Section 4: Advanced Django Features

### Chapter 21: Django Generic Views

#### ListView Example
```python
from django.views.generic import ListView

class BookListView(ListView):
    model = Book
    template_name = 'books/book_list.html'  # Default: <app>/<model>_list.html
    context_object_name = 'books'  # Default: object_list
    paginate_by = 10
    ordering = ['-published_date']
    
    def get_queryset(self):
        # Custom queryset - filter by published status
        return Book.objects.filter(is_published=True)
    
    def get_context_data(self, **kwargs):
        # Add extra context data
        context = super().get_context_data(**kwargs)
        context['categories'] = Category.objects.all()
        return context
```

#### DetailView Example
```python
from django.views.generic import DetailView

class BookDetailView(DetailView):
    model = Book
    template_name = 'books/book_detail.html'  # Default: <app>/<model>_detail.html
    context_object_name = 'book'  # Default: object
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['reviews'] = self.object.review_set.all()
        return context
```

#### CreateView Example
```python
from django.views.generic.edit import CreateView
from django.urls import reverse_lazy

class BookCreateView(LoginRequiredMixin, CreateView):
    model = Book
    form_class = BookForm
    template_name = 'books/book_form.html'  # Default: <app>/<model>_form.html
    success_url = reverse_lazy('books:list')
    
    def form_valid(self, form):
        # Set the owner to the current user before saving
        form.instance.author = self.request.user.author
        return super().form_valid(form)
```

#### UpdateView and DeleteView
```python
from django.views.generic.edit import UpdateView, DeleteView

class BookUpdateView(LoginRequiredMixin, UpdateView):
    model = Book
    form_class = BookForm
    template_name = 'books/book_form.html'
    
    def get_success_url(self):
        return reverse('books:detail', kwargs={'pk': self.object.pk})
    
    def get_queryset(self):
        # Only let users edit their own books
        return super().get_queryset().filter(author__user=self.request.user)

class BookDeleteView(LoginRequiredMixin, DeleteView):
    model = Book
    template_name = 'books/book_confirm_delete.html'
    success_url = reverse_lazy('books:list')
    
    def get_queryset(self):
        # Only let users delete their own books
        return super().get_queryset().filter(author__user=self.request.user)
```

---

### Chapter 22: Django Manual Views

#### Function-Based Views
```python
from django.shortcuts import render, get_object_or_404, redirect
from django.contrib.auth.decorators import login_required

def book_list(request):
    search_query = request.GET.get('search', '')
    category_id = request.GET.get('category', None)
    
    books = Book.objects.filter(is_published=True)
    
    if search_query:
        books = books.filter(title__icontains=search_query)
    
    if category_id:
        books = books.filter(categories__id=category_id)
    
    context = {
        'books': books,
        'search_query': search_query,
        'categories': Category.objects.all(),
        'selected_category_id': category_id
    }
    
    return render(request, 'books/book_list.html', context)

def book_detail(request, pk):
    book = get_object_or_404(Book, pk=pk)
    reviews = book.review_set.all().order_by('-created_at')
    
    if request.method == 'POST' and request.user.is_authenticated:
        review_text = request.POST.get('review_text')
        rating = request.POST.get('rating')
        
        if review_text and rating:
            Review.objects.create(
                book=book,
                user=request.user,
                text=review_text,
                rating=int(rating)
            )
            return redirect('books:detail', pk=pk)
    
    context = {
        'book': book,
        'reviews': reviews
    }
    
    return render(request, 'books/book_detail.html', context)

@login_required
def book_create(request):
    if request.method == 'POST':
        form = BookForm(request.POST, request.FILES)
        if form.is_valid():
            book = form.save(commit=False)
            book.author = request.user.author
            book.save()
            form.save_m2m()  # Save many-to-many relationships (categories)
            return redirect('books:detail', pk=book.pk)
    else:
        form = BookForm()
    
    context = {'form': form}
    return render(request, 'books/book_form.html', context)
```

---

### Chapter 23: Understanding Django Features

#### Django Settings
```python
# settings.py

# Debug mode (development only!)
DEBUG = True  # Production mein False hona chahiye

# Secret key (should be kept secret)
SECRET_KEY = 'your-secret-key'

# Allowed hosts
ALLOWED_HOSTS = ['localhost', '127.0.0.1']  # Production mein domain names

# Database configuration
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Static files
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'staticfiles'
STATICFILES_DIRS = [BASE_DIR / 'static']

# Media files
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# File upload permissions
FILE_UPLOAD_PERMISSIONS = 0o644
```

#### URL Configurations
```python
# urls.py (project level)
from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('django.contrib.auth.urls')),
    path('books/', include('books.urls')),
    path('', include('pages.urls')),
]

# Media URLs (development only)
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

#### Django Admin Customization
```python
# admin.py
from django.contrib import admin
from .models import Book, Author, Category

@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = ('name', 'slug')
    prepopulated_fields = {'slug': ('name',)}

class BookInline(admin.TabularInline):
    model = Book
    extra = 1

@admin.register(Author)
class AuthorAdmin(admin.ModelAdmin):
    list_display = ('user', 'bio')
    inlines = [BookInline]

@admin.register(Book)
class BookAdmin(admin.ModelAdmin):
    list_display = ('title', 'author', 'published_date', 'is_published')
    list_filter = ('is_published', 'categories')
    search_fields = ('title', 'author__user__username')
    date_hierarchy = 'published_date'
    filter_horizontal = ('categories',)
    fieldsets = (
        ('Basic Information', {
            'fields': ('title', 'author', 'slug')
        }),
        ('Content', {
            'fields': ('cover_image', 'summary', 'content')
        }),
        ('Publishing', {
            'fields': ('published_date', 'is_published')
        }),
        ('Categorization', {
            'fields': ('categories',)
        }),
    )
```

---

### Chapter 24: Django Class-Based Views

#### Base View Classes
```python
from django.views import View
from django.views.generic.base import TemplateView, RedirectView

# Basic View
class MyView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse('Hello, World!')
    
    def post(self, request, *args, **kwargs):
        # Process POST data
        return HttpResponse('Data received')

# Template View
class HomeView(TemplateView):
    template_name = 'home.html'
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['latest_books'] = Book.objects.all()[:5]
        return context

# Redirect View
class OldBookDetailView(RedirectView):
    permanent = True  # 301 redirect instead of 302
    
    def get_redirect_url(self, *args, **kwargs):
        book_id = kwargs['book_id']
        return reverse('books:detail', kwargs={'pk': book_id})
```

#### FormView Example
```python
from django.views.generic.edit import FormView
from .forms import ContactForm

class ContactView(FormView):
    template_name = 'contact.html'
    form_class = ContactForm
    success_url = '/contact/thank-you/'
    
    def form_valid(self, form):
        # Process data
        name = form.cleaned_data['name']
        email = form.cleaned_data['email']
        message = form.cleaned_data['message']
        
        # Send email
        send_mail(
            f'Contact from {name}',
            message,
            email,
            ['admin@example.com'],
            fail_silently=False,
        )
        
        return super().form_valid(form)
```

#### Mixins
```python
from django.contrib.auth.mixins import LoginRequiredMixin, UserPassesTestMixin

class StaffRequiredMixin(LoginRequiredMixin, UserPassesTestMixin):
    def test_func(self):
        return self.request.user.is_staff

class AuthorCreateView(StaffRequiredMixin, CreateView):
    model = Author
    form_class = AuthorForm
    template_name = 'authors/author_form.html'
    success_url = reverse_lazy('authors:list')
```

---

### Chapter 25: Understanding Django Flow

#### Request-Response Cycle
1. User browser se URL request karta hai
2. Django URLconf (urls.py) URL pattern match karta hai
3. Matching view function/class ko call karta hai
4. View data process karta hai, database se interact karta hai
5. Template render karta hai (optional)
6. HTTP response client ko return karta hai

#### Middleware
```python
# settings.py
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

#### Custom Middleware Example
```python
# middleware.py
class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
        # One-time setup code
    
    def __call__(self, request):
        # Code to be executed before the view is called
        
        response = self.get_response(request)
        
        # Code to be executed after the view is called
        
        return response
```

---

### Chapter 26: One-Shot Django Setup

#### Project Creation Script
```bash
#!/bin/bash

# Environment setup
python -m venv env
source env/bin/activate

# Install dependencies
pip install django django-crispy-forms pillow

# Create Django project
django-admin startproject myproject
cd myproject

# Create apps
python manage.py startapp accounts
python manage.py startapp core
python manage.py startapp products

# Initial migrations
python manage.py makemigrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser --username admin --email admin@example.com
```

#### Base Settings Setup
```python
# settings.py modifications

# Apps
INSTALLED_APPS = [
    # Django built-ins
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # Third-party apps
    'crispy_forms',
    
    # Custom apps
    'accounts',
    'core',
    'products',
]

# Crispy forms settings
CRISPY_TEMPLATE_PACK = 'bootstrap4'

# Media and static files
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']
STATIC_ROOT = BASE_DIR / 'staticfiles'

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'

# Authentication settings
LOGIN_REDIRECT_URL = 'home'
LOGOUT_REDIRECT_URL = 'home'
```

---

### Chapter 27: Django Form Handling

#### Comprehensive Form Example
```python
# forms.py
from django import forms
from django.core.exceptions import ValidationError
from .models import Product

class ProductForm(forms.ModelForm):
    confirm_price = forms.DecimalField(
        max_digits=10,
        decimal_places=2,
        label="Confirm Price",
        help_text="Please enter the price again to confirm"
    )
    
    class Meta:
        model = Product
        fields = ['name', 'description', 'price', 'image', 'categories']
        widgets = {
            'name': forms.TextInput(attrs={'class': 'form-control'}),
            'description': forms.Textarea(attrs={'class': 'form-control', 'rows': 4}),
            'price': forms.NumberInput(attrs={'class': 'form-control'}),
            'categories': forms.CheckboxSelectMultiple()
        }
        labels = {
            'name': 'Product Name',
            'description': 'Product Description',
            'price': 'Product Price (₹)',
        }
        help_texts = {
            'name': 'Enter a unique and descriptive name',
            'image': 'Upload an image of the product (optional)',
        }
    
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        
        # Make some fields required
        self.fields['description'].required = True
        
        # Instance-based field initialization
        if self.instance and self.instance.pk:
            self.fields['confirm_price'].initial = self.instance.price
    
    def clean_name(self):
        name = self.cleaned_data.get('name')
        
        # Check if name is too short
        if len(name) < 3:
            raise ValidationError("Product name must be at least 3 characters long")
        
        # Check for duplicate names (excluding current instance)
        if self.instance and self.instance.pk:
            duplicate = Product.objects.exclude(pk=self.instance.pk).filter(name=name).exists()
        else:
            duplicate = Product.objects.filter(name=name).exists()
        
        if duplicate:
            raise ValidationError("A product with this name already exists")
        
        return name
    
    def clean(self):
        cleaned_data = super().clean()
        price = cleaned_data.get('price')
        confirm_price = cleaned_data.get('confirm_price')
        
        if price and confirm_price and price != confirm_price:
            raise ValidationError("Price and confirmation price don't match")
        
        return cleaned_data
```

#### Form Handling in Views
```python
# views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib import messages
from .models import Product
from .forms import ProductForm

def product_create(request):
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            product = form.save(commit=False)
            product.created_by = request.user
            product.save()
            
            # Save many-to-many fields explicitly when using commit=False
            form.save_m2m()
            
            messages.success(request, f"Product '{product.name}' created successfully!")
            return redirect('products:detail', pk=product.pk)
    else:
        form = ProductForm()
    
    return render(request, 'products/product_form.html', {
        'form': form,
        'title': 'Create Product'
    })

def product_update(request, pk):
    product = get_object_or_404(Product, pk=pk)
    
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES, instance=product)
        if form.is_valid():
            form.save()
            messages.success(request, f"Product '{product.name}' updated successfully!")
            return redirect('products:detail', pk=product.pk)
    else:
        form = ProductForm(instance=product)
    
    return render(request, 'products/product_form.html', {
        'form': form,
        'product': product,
        'title': 'Update Product'
    })
```

---

### Chapter 28: JSON in Django

#### JSON Response Example
```python
from django.http import JsonResponse
from .models import Product

def product_json(request, pk):
    try:
        product = Product.objects.get(pk=pk)
        data = {
            'id': product.id,
            'name': product.name,
            'price': float(product.price),
            'description': product.description,
            'categories': list(product.categories.values_list('name', flat=True))
        }
        return JsonResponse(data)
    except Product.DoesNotExist:
        return JsonResponse({'error': 'Product not found'}, status=404)
```

#### Processing JSON Request
```python
import json
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt  # Not recommended for production without proper authentication
def api_create_product(request):
    if request.method == 'POST':
        try:
            data = json.loads(request.body)
            
            # Validate required fields
            required_fields = ['name', 'price', 'description']
            for field in required_fields:
                if field not in data:
                    return JsonResponse({'error': f'Missing required field: {field}'}, status=400)
            
            # Create product
            product = Product.objects.create(
                name=data['name'],
                price=data['price'],
                description=data['description']
            )
            
            # Handle categories if provided
            if 'categories' in data and isinstance(data['categories'], list):
                for category_name in data['categories']:
                    category, created = Category.objects.get_or_create(name=category_name)
                    product.categories.add(category)
            
            return JsonResponse({
                'id': product.id,
                'name': product.name,
                'message': 'Product created successfully'
            }, status=201)
            
        except json.JSONDecodeError:
            return JsonResponse({'error': 'Invalid JSON'}, status=400)
        except Exception as e:
            return JsonResponse({'error': str(e)}, status=500)
    
    return JsonResponse({'error': 'Method not allowed'}, status=405)
```

#### AJAX in Templates
```html
<script>
    // Get product details via AJAX
    function loadProductDetails(productId) {
        fetch(`/api/products/${productId}/`)
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();
            })
            .then(data => {
                document.getElementById('product-name').textContent = data.name;
                document.getElementById('product-price').textContent = `₹${data.price}`;
                document.getElementById('product-description').textContent = data.description;
                
                // Update categories
                const categoriesList = document.getElementById('product-categories');
                categoriesList.innerHTML = '';
                data.categories.forEach(category => {
                    const li = document.createElement('li');
                    li.textContent = category;
                    categoriesList.appendChild(li);
                });
            })
            .catch(error => {
                console.error('Error fetching product details:', error);
                document.getElementById('product-details').innerHTML = 
                    '<p class="text-danger">Failed to load product details</p>';
            });
    }
</script>
```

---

### Chapter 29: APIs in Django

#### Simple API Views
```python
from django.http import JsonResponse
from django.views.decorators.http import require_http_methods
from .models import Product, Category

def api_products(request):
    products = Product.objects.all()[:20]  # Limit to 20 for performance
    
    # Basic filtering
    category = request.GET.get('category')
    if category:
        products = products.filter(categories__name__icontains=category)
    
    # Convert to list of dicts
    product_list = []
    for product in products:
        product_list.append({
            'id': product.id,
            'name': product.name,
            'price': float(product.price),
            'url': request.build_absolute_uri(f'/products/{product.id}/')
        })
    
    return JsonResponse({'products': product_list})

@require_http_methods(['GET'])
def api_product_detail(request, pk):
    try:
        product = Product.objects.get(pk=pk)
        data = {
            'id': product.id,
            'name': product.name,
            'price': float(product.price),
            'description': product.description,
            'created_at': product.created_at.isoformat(),
            'categories': list(product.categories.values('id', 'name'))
        }
        
        if product.image:
            data['image_url'] = request.build_absolute_uri(product.image.url)
        
        return JsonResponse(data)
    except Product.DoesNotExist:
        return JsonResponse({'error': 'Product not found'}, status=404)
```

#### API URL Configuration
```python
# urls.py
urlpatterns = [
    # ...
    path('api/products/', views.api_products, name='api_products'),
    path('api/products/<int:pk>/', views.api_product_detail, name='api_product_detail'),
]
```

---

### Chapter 30: Django REST Framework

#### Installation and Setup
```bash
pip install djangorestframework

# settings.py
INSTALLED_APPS = [
    # ...
    'rest_framework',
]

REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ],
}
```

#### Serializers
```python
# serializers.py
from rest_framework import serializers
from .models import Category, Product

class CategorySerializer(serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = ['id', 'name', 'slug']

class ProductSerializer(serializers.ModelSerializer):
    categories = CategorySerializer(many=True, read_only=True)
    
    class Meta:
        model = Product
        fields = ['id', 'name', 'description', 'price', 'image', 'created_at', 'categories']
        read_only_fields = ['created_at']
```

#### ViewSets and Routers
```python
# views.py
from rest_framework import viewsets, permissions
from .models import Category, Product
from .serializers import CategorySerializer, ProductSerializer

class CategoryViewSet(viewsets.ModelViewSet):
    queryset = Category.objects.all()
    serializer_class = CategorySerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    lookup_field = 'slug'

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    
    def perform_create(self, serializer):
        serializer.save(created_by=self.request.user)
    
    def get_queryset(self):
        queryset = Product.objects.all()
        
        # Filter by category if provided
        category = self.request.query_params.get('category')
        if category:
            queryset = queryset.filter(categories__slug=category)
        
        # Filter by price range if provided
        min_price = self.request.query_params.get('min_price')
        max_price = self.request.query_params.get('max_price')
        
        if min_price:
            queryset = queryset.filter(price__gte=min_price)
        if max_price:
            queryset = queryset.filter(price__lte=max_price)
        
        return queryset
```

#### URL Configuration with Routers
```python
# urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from . import views

router = DefaultRouter()
router.register(r'categories', views.CategoryViewSet)
router.register(r'products', views.ProductViewSet)

urlpatterns
