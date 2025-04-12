# Django Forms - Hinglish Notes

## Introduction (परिचय)

Django forms hamein user se data collect karne ka ek powerful aur convenient tarika dete hain. Ye hamein validation, rendering aur processing ke liye built-in capabilities provide karte hain.

## Django Forms Basics (Django Forms की मूल बातें)

### Forms के फायदे (Benefits of Forms)
- **Code Simplification**: Views mein code ko chota aur manageable banata hai
- **Data Validation**: Input data ka automatic validation karta hai
- **Security**: CSRF protection provide karta hai
- **HTML Generation**: Form ka HTML automatically generate karta hai

### Basic Form Structure (फॉर्म की बेसिक स्ट्रक्चर)

```python
from django import forms

class MyForm(forms.Form):
    name = forms.CharField(max_length=100)
    email = forms.EmailField()
    message = forms.CharField(widget=forms.Textarea)
```

## ModelForm का उपयोग (Using ModelForm)

ModelForm database models ke based par forms create karne ka tarika provide karta hai.

```python
from django import forms
from .models import MyModel

class MyModelForm(forms.ModelForm):
    class Meta:
        model = MyModel
        fields = ['title', 'description', 'price']  # ya 'fields = "__all__"' for all fields
        # exclude = ['created_at']  # kuch fields ko exclude karne ke liye
```

## View में Forms का उपयोग (Using Forms in Views)

### Form Processing की basic flow:
1. GET request: Empty form dikhana
2. POST request: Form data process karna aur validate karna

```python
def my_view(request):
    if request.method == 'POST':
        # Form submitted hai
        form = MyForm(request.POST)
        if form.is_valid():
            # Form valid hai, data process karo
            # form.cleaned_data dictionary mein validated data hai
            name = form.cleaned_data['name']
            # Process data and redirect
            return HttpResponseRedirect('/success/')
    else:
        # GET request, empty form display karo
        form = MyForm()
    
    return render(request, 'form_template.html', {'form': form})
```

## ModelForm से Database Interaction

```python
def create_item(request):
    if request.method == 'POST':
        form = MyModelForm(request.POST)
        if form.is_valid():
            # Create new database object
            form.save()  # directly database mein save kar deta hai
            return HttpResponseRedirect('/success/')
    else:
        form = MyModelForm()
    
    return render(request, 'form_template.html', {'form': form})
```

## Form ko Template mein Display karna

```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}  <!-- Paragraph format mein form display karega -->
    <button type="submit">Submit</button>
</form>
```

Form display karne ke tarike:
- `{{ form.as_p }}` - Paragraph format mein
- `{{ form.as_table }}` - Table format mein
- `{{ form.as_ul }}` - Unordered list format mein

## Manual Form Rendering

Fields ko individual tarike se display karne ke liye:

```html
<form method="post">
    {% csrf_token %}
    
    <div class="field">
        {{ form.name.errors }}
        <label for="{{ form.name.id_for_label }}">Name:</label>
        {{ form.name }}
    </div>
    
    <div class="field">
        {{ form.email.errors }}
        <label for="{{ form.email.id_for_label }}">Email:</label>
        {{ form.email }}
    </div>
    
    <button type="submit">Submit</button>
</form>
```

## Form Validation (फॉर्म वैलिडेशन)

Django automatically basic validation run karta hai lekin custom validation bhi add kar sakte hain:

```python
class MyForm(forms.Form):
    name = forms.CharField(max_length=100)
    
    def clean_name(self):
        # Individual field validation
        data = self.cleaned_data['name']
        if "admin" in data.lower():
            raise forms.ValidationError("Name cannot contain 'admin'")
        return data
    
    def clean(self):
        # Whole form validation (multiple fields)
        cleaned_data = super().clean()
        # Multiple fields compare karne ke liye validation
        return cleaned_data
```

## Form Widgets (फॉर्म विजेट्स)

Widgets control karte hain ki form field HTML mein kaise render hoga:

```python
class MyForm(forms.Form):
    name = forms.CharField(
        max_length=100,
        widget=forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'Enter your name'})
    )
    
    options = forms.ChoiceField(
        choices=[('1', 'Option 1'), ('2', 'Option 2')],
        widget=forms.RadioSelect
    )
    
    agree = forms.BooleanField(
        widget=forms.CheckboxInput(attrs={'class': 'form-check-input'})
    )
```

## CSRF Protection

Django automatically Cross-Site Request Forgery (CSRF) protection provide karta hai. Template mein `{% csrf_token %}` add karna important hai.

## Form mein Files Upload

File upload karne ke liye:

```python
class MyForm(forms.Form):
    name = forms.CharField(max_length=100)
    file = forms.FileField()
```

View mein:
```python
def upload_file(request):
    if request.method == 'POST':
        form = MyForm(request.POST, request.FILES)
        if form.is_valid():
            handle_uploaded_file(request.FILES['file'])
            return HttpResponseRedirect('/success/')
    else:
        form = MyForm()
    return render(request, 'upload.html', {'form': form})
```

Template mein:
```html
<form method="post" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Upload</button>
</form>
```

## Formsets (फॉर्मसेट्स)

Formsets multiple forms ko ek saath handle karne ke liye use hote hain:

```python
from django.forms import formset_factory

MyFormSet = formset_factory(MyForm, extra=3)  # 3 empty forms

def manage_items(request):
    if request.method == 'POST':
        formset = MyFormSet(request.POST)
        if formset.is_valid():
            for form in formset:
                # Process each valid form
                pass
    else:
        formset = MyFormSet()
    
    return render(request, 'formset.html', {'formset': formset})
```

## Best Practices (बेस्ट प्रैक्टिसेज)

1. **Reusable Forms**: Forms.py mein separate file mein forms define karein
2. **Validation**: Proper validation implement karein
3. **Error Handling**: User-friendly error messages provide karein
4. **CSRF Protection**: CSRF token ka use karein
5. **ModelForms**: Database models ke liye ModelForms ka use karein

## Conclusion (निष्कर्ष)

Django Forms user input ko handle karne ke liye ek powerful tool hai. Ye not only code simplify karta hai balki security aur validation bhi improve karta hai. ModelForms database ke saath integration ko aur bhi simple banata hai.

Forms effectively use karne se development time kam hoga aur code quality improve hogi.
