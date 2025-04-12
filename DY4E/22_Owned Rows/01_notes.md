# Django mein "Owned Rows" ke Notes

## Introduction (परिचय)

"Owned Rows" ka matlab hai aisa system design karna jahan database objects kisi specific user ke "owned" (ya uske dwara banaye gaye) hon. Ye concept bahut useful hai jab aap chahte hain ki users sirf apne hi data ko edit/delete kar sakein.

## Key Concepts (मुख्य अवधारणाएं)

### 1. Owner Field (मालिक फील्ड)

```python
# Model mein owner field add karna
from django.conf import settings

class Article(models.Model):
    title = models.CharField(max_length=200)
    text = models.TextField()
    owner = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```

* Owner field ek ForeignKey hota hai jo current user ko point karta hai
* `settings.AUTH_USER_MODEL` ka use karna best practice hai direct `User` model ki jagah

### 2. Form Submission ke Time Owner Set Karna

```python
def post(self, request, *args, **kwargs):
    form = self.form_class(request.POST, request.FILES)
    if form.is_valid():
        # form save karne se pehle owner set karein
        pic = form.save(commit=False)
        pic.owner = self.request.user
        pic.save()
        return redirect(self.success_url)
```

* `commit=False` form ko save karta hai lekin database tak commit nahi karta
* Phir hum manually `owner` set karke final save karte hain

### 3. Permissions Check Karna (अनुमतियां जांचना)

```python
from django.contrib.auth.mixins import LoginRequiredMixin

class OwnerDeleteView(LoginRequiredMixin, DeleteView):
    def get_queryset(self):
        # Filter the queryset to only include objects owned by the current user
        qs = super().get_queryset()
        return qs.filter(owner=self.request.user)
```

* `get_queryset()` ko override karke hum sirf current user ke objects return karte hain
* Isse users sirf apne hi objects delete/edit kar sakte hain

## Reusable Mixins Banana (पुन: प्रयोज्य मिक्सिन बनाना)

Django mein reusable code banane ke liye "mixins" bahut powerful hote hain:

```python
class OwnerListView(LoginRequiredMixin, ListView):
    """
    Sub-classes must define model attribute
    """
    def get_queryset(self):
        qs = super().get_queryset()
        return qs.filter(owner=self.request.user)

class OwnerDetailView(LoginRequiredMixin, DetailView):
    """
    Sub-classes must define model attribute
    """
    def get_queryset(self):
        qs = super().get_queryset()
        return qs.filter(owner=self.request.user)

# Aur bhi views: OwnerCreateView, OwnerUpdateView, OwnerDeleteView
```

## Owner.py File Banane ka Pattern

Aap apne application ke saath ek `owner.py` file bana sakte hain jisme ye sare mixins define ho:

```python
# In myapp/owner.py
from django.views.generic import ListView, DetailView, CreateView, UpdateView, DeleteView
from django.contrib.auth.mixins import LoginRequiredMixin

class OwnerListView(LoginRequiredMixin, ListView):
    # Functionality here...

class OwnerDetailView(LoginRequiredMixin, DetailView):
    # Functionality here...

# Etc...
```

Phir apne views.py mein import karein:
```python
from myapp.owner import OwnerListView, OwnerCreateView
```

## Implementation Steps (क्रियान्वयन के चरण)

1. Model mein owner field add karein
2. Form submit karte samay owner field set karein
3. Permissions check karne ke liye custom views banayein
4. Check karein ki users sirf apne objects hi edit/delete kar sakte hain
5. Optional: reusable mixins banayein alag `owner.py` file mein

## Benefits (फायदे)

1. Security: Users sirf apne data ko modify kar sakte hain
2. Clean code: Repetitive permission logic ko mixins mein encapsulate kiya
3. Django ke generic views ka power use karne ko milta hai
4. Automated filtering: Har view automatically user-specific data dikhaega

## Common Patterns (आम पैटर्न)

1. ListView: Sirf current user ke objects dikhana
2. CreateView: New object banate samay automatically owner set karna 
3. UpdateView/DeleteView: Permission check to ensure users only edit their objects

## Conclusion (निष्कर्ष)

"Owned Rows" pattern Django applications mein multi-user environment banane ke liye crucial hai. Is pattern ko follow karke aap ensure kar sakte hain ki users sirf apne data ko access aur modify kar sakein, jo security ke liye bahut zaroori hai.
