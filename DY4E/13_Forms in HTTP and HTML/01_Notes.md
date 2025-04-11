# Django Forms in HTTP and HTML - Hinglish Notes

## HTTP Forms - Web mein data kaise bheja jata hai

Forms HTTP protocol ka ek fundamental part hai. Yeh web applications mein user se data collect karne ke liye use hota hai.

### GET vs POST Methods

**GET Method:**
- URL mein data dikhta hai (query parameters)
- Limited data bhej sakte hain (URL length limitation)
- Bookmarking possible hai
- Data secure nahi hota
- Typically searching ya data retrieval ke liye use hota hai

**POST Method:**
- URL mein data nahi dikhta
- Larger data amounts bhej sakte hain
- Bookmarking nahi ho sakta
- More secure hota hai
- Typically data create/update ke liye use hota hai

### HTML Forms Basics

```html
<form method="post">
  <p><label for="nm">Name:</label>
  <input type="text" name="nm" id="nm"></p>
  <p><label for="pw">Password:</label>
  <input type="password" name="pw" id="pw"></p>
  <p><input type="submit" value="Login"/></p>
</form>
```

## Django mein Forms Use Karna

### Django CSRF Protection

Django automatically CSRF (Cross-Site Request Forgery) protection provide karta hai:

```html
<form method="post">
  {% csrf_token %}
  <!-- Form fields -->
</form>
```

Agar `{% csrf_token %}` nahi use karenge to POST requests fail ho jayenge.

### Form Handling in Django Views

```python
def myview(request):
    if request.method == 'POST':
        # Form submit hua hai, data process karo
        return redirect('/success')
    else:
        # GET request hai, form dikhao
        return render(request, 'mytemplate.html')
```

### Form Data Access Karna

```python
def myview(request):
    if request.method == 'POST':
        name = request.POST.get('nm', False)
        password = request.POST.get('pw', False)
        # Data validate and process karo
```

## Django Form Class

Django form classes complex forms handle karne mein help karte hain:

```python
from django import forms

class MyForm(forms.Form):
    name = forms.CharField(label='Your Name', max_length=100)
    email = forms.EmailField(label='Your Email')
```

### View mein Form Use Karna

```python
def form_view(request):
    if request.method == 'POST':
        form = MyForm(request.POST)
        if form.is_valid():
            # Cleaned data use karo
            name = form.cleaned_data['name']
            email = form.cleaned_data['email']
            # Process data and redirect
            return redirect('/success/')
    else:
        form = MyForm()
    
    return render(request, 'form_template.html', {'form': form})
```

### Template mein Form Render Karna

```html
<form method="post">
  {% csrf_token %}
  {{ form.as_p }}
  <input type="submit" value="Submit">
</form>
```

Forms ko different tarike se render kar sakte hain:
- `{{ form.as_p }}` - Paragraphs mein
- `{{ form.as_table }}` - Table format mein
- `{{ form.as_ul }}` - Unordered list mein

## Model Forms

Model se directly form banane ke liye ModelForm use hota hai:

```python
from django.forms import ModelForm
from .models import Product

class ProductForm(ModelForm):
    class Meta:
        model = Product
        fields = ['name', 'price', 'description']
        # Ya fields = '__all__' for all fields
```

## Form Validation

Django forms automatically validation provide karte hain:

```python
def form_view(request):
    if request.method == 'POST':
        form = MyForm(request.POST)
        if form.is_valid():
            # Valid data process karo
        else:
            # Form errors handle karo
    # ...
```

## Summary

- HTTP forms web applications ka essential part hai
- GET vs POST methods different purposes ke liye use hote hain
- Django CSRF protection provide karta hai
- Django Form classes complex forms handle karne mein help karte hain
- ModelForms database models se direct forms create karne ki facility dete hain
- Django automatic form validation provide karta hai

Yaad rakhen: Secure applications banane ke liye, sensitive data POST method se hi bhejein aur hamesha `{% csrf_token %}` use karein.
