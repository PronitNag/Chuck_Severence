# Django Users aur Authentication ke Notes

## Introduction

Django mein users aur authentication bahut hi important concepts hain. Ye hamein website par users ko manage karne, login/logout functionality provide karne, aur secure access control implement karne mein help karte hain.

## Users ko Create aur Manage karna

### Django ka User Model

Django mein by default ek User model aata hai jo `django.contrib.auth.models.User` mein define hai. Is model mein ye fields hoti hain:

- **username**: Unique username
- **password**: Password (hashed form mein store hota hai)
- **email**: User ka email
- **first_name** aur **last_name**: User ka naam
- **is_active**: User active hai ya nahi
- **is_staff**: Admin site access ke liye
- **is_superuser**: Sabhi permissions ke liye

### Users Create karne ke tarike

1. **Admin Interface se**:
   - Django admin panel mein jakar "Users" section mein add kar sakte hain
   - Superuser create karne ke liye command line par: `python manage.py createsuperuser`

2. **Programmatically**:
   ```python
   from django.contrib.auth.models import User
   
   # Create user
   user = User.objects.create_user(username='rahul', 
                                  email='rahul@example.com',
                                  password='secure_password123')
   
   # Create superuser
   admin_user = User.objects.create_superuser(username='admin',
                                             email='admin@example.com',
                                             password='admin_password123')
   ```

3. **User Registration Form se**:
   Django's built-in UserCreationForm ka use karke

## Login aur Logout URLs

Django mein authentication ke liye pre-defined URLs hain jinhe ham apne project mein include kar sakte hain.

### URLs Setup

`urls.py` mein:

```python
from django.urls import path, include
from django.contrib.auth import views as auth_views

urlpatterns = [
    # ...
    path('accounts/', include('django.contrib.auth.urls')),
    # ...
]
```

Is se ye saare URLs automatically add ho jaate hain:

- `accounts/login/` - Login page
- `accounts/logout/` - Logout page
- `accounts/password_change/` - Password change form
- `accounts/password_change/done/` - Password change success
- `accounts/password_reset/` - Reset password form
- `accounts/password_reset/done/` - Password reset sent
- `accounts/reset/<uidb64>/<token>/` - Password reset confirmation
- `accounts/reset/done/` - Password reset complete

### Login Template Banana

Django login view ke liye ek template banana hoga: `templates/registration/login.html`

```html
{% extends "base.html" %}

{% block content %}
  <h2>Login Kijiye</h2>
  <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Login</button>
  </form>
{% endblock %}
```

### Login/Logout ke baad Redirect

`settings.py` mein hum login/logout ke baad redirect ke URLs set kar sakte hain:

```python
LOGIN_REDIRECT_URL = 'home'  # Login ke baad is URL par redirect karega
LOGOUT_REDIRECT_URL = 'home'  # Logout ke baad is URL par redirect karega
```

## Login aur Views

### @login_required Decorator

Kisi view ko sirf logged-in users ke liye restrict karne ke liye:

```python
from django.contrib.auth.decorators import login_required

@login_required
def my_protected_view(request):
    # Sirf logged-in users is view ko access kar sakte hain
    return render(request, 'protected_page.html')
```

Agar koi non-logged-in user is view ko access karne ki koshish karta hai, toh use login page par redirect kar diya jayega.

### Template mein User Information Access Karna

Template mein current user ki information access karne ke liye:

```html
{% if user.is_authenticated %}
  <p>Hello, {{ user.username }}!</p>
  <a href="{% url 'logout' %}">Logout</a>
{% else %}
  <p>Aap logged in nahi hain.</p>
  <a href="{% url 'login' %}">Login</a>
{% endif %}
```

### Class-Based Views ke sath Authentication

Class-based views mein authentication mixins ka use:

```python
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import ListView

class ProtectedListView(LoginRequiredMixin, ListView):
    model = MyModel
    template_name = 'protected_list.html'
    login_url = '/login/'  # Optional: custom login URL
```

## User Registration

Django mein built-in UserCreationForm ka use karke registration implement kar sakte hain:

```python
from django.contrib.auth.forms import UserCreationForm
from django.urls import reverse_lazy
from django.views.generic.edit import CreateView

class SignUpView(CreateView):
    form_class = UserCreationForm
    success_url = reverse_lazy('login')
    template_name = 'registration/signup.html'
```

URLs mein add karein:

```python
path('signup/', SignUpView.as_view(), name='signup'),
```

## User Permissions aur Groups

Django mein permissions aur groups ke through access control implement kiya ja sakta hai:

```python
from django.contrib.auth.models import Group, Permission
from django.contrib.auth.decorators import permission_required

# Permission check with decorator
@permission_required('app.add_model')
def add_model_view(request):
    # Sirf wahi users jinke paas 'app.add_model' permission hai
    # ...
```

## Authentication ko Test Karna

Django mein authentication ko test karne ke liye:

```python
from django.test import TestCase, Client
from django.urls import reverse
from django.contrib.auth.models import User

class LoginTestCase(TestCase):
    def setUp(self):
        self.client = Client()
        self.user = User.objects.create_user(
            username='testuser', 
            password='testpassword123'
        )
    
    def test_login(self):
        response = self.client.post(reverse('login'), {
            'username': 'testuser',
            'password': 'testpassword123'
        })
        self.assertEqual(response.status_code, 302)  # Redirect to success URL
        self.assertTrue(response.wsgi_request.user.is_authenticated)
```

## Practical Tips

1. **Custom User Model**: Project shuru karte waqt hi custom user model ko configure karna best practice hai:
   ```python
   # models.py
   from django.contrib.auth.models import AbstractUser
   
   class CustomUser(AbstractUser):
       # Additional fields
       phone = models.CharField(max_length=15, blank=True)
   ```

2. **Password Reset**: Email-based password reset functionality enable karne ke liye email settings configure karein.

3. **Social Authentication**: `django-allauth` jaise packages ka use karke social media authentication add kar sakte hain.

4. **Security Tips**:
   - HTTPS ka use karein
   - Password validation rules set karein
   - Session timeout set karein

## Conclusion

Django mein authentication system bahut powerful hai aur iske through ham easily user management, login/logout, permissions aur security implement kar sakte hain. Ye chapter ka understanding hamare web applications ko more secure aur user-friendly banane mein help karega.
