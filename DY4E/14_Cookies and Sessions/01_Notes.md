# Django Mein Cookies aur Sessions - Notes 📝

## 1. Cookies ka Introduction 🍪

### Cookies Kya Hain?
* Cookies chote text files hote hain jo browser mein store kiye jaate hain
* Web server inko user ke browser mein bhejta hai
* Browser automatically har request ke saath cookies server ko wapas bhejta hai
* Cookies ka size usually 4KB tak limited hota hai

### Cookies ke Uses
* User preferences yaad rakhna
* User ko track karna
* Shopping carts manage karna
* Login information store karna (temporary)

### Browser Mein Cookies Kaise Dekhen
* Chrome mein: Developer Tools > Application > Cookies
* Firefox mein: Developer Tools > Storage > Cookies
* Safari mein: Preferences > Privacy > Manage Website Data

## 2. Django Mein Cookies 🐍

### Cookie Set Karna
```python
def set_cookie_view(request):
    response = HttpResponse("Cookie set kar diya gaya hai!")
    response.set_cookie('username', 'dj4e_user')
    return response
```

### Cookie Ko Read Karna
```python
def read_cookie_view(request):
    username = request.COOKIES.get('username', 'Unknown User')
    return HttpResponse(f"Hello, {username}!")
```

### Cookie Parameters
* **max_age**: Cookie ki validity seconds mein
* **expires**: Specific date jab cookie expire hoga
* **domain**: Kis domain ke liye cookie valid hai
* **secure**: HTTPS connections ke liye cookie restrict karna
* **httponly**: JavaScript se cookie access rokna

## 3. Django Mein Sessions 🔄

### Sessions Kya Hain?
* Sessions ek tarika hai user data server-side store karne ka
* Har user ko ek unique session ID milta hai (cookie mein store hota hai)
* Actual data server par store hota hai, sirf ID client ke paas hota hai
* Isse security improve hoti hai kyunki sensitive data browser mein nahi rehta

### Django Mein Sessions Setup
* Default settings mein sessions already enabled hote hain
* `MIDDLEWARE` mein `'django.contrib.sessions.middleware.SessionMiddleware'` hona chahiye
* `INSTALLED_APPS` mein `'django.contrib.sessions'` hona chahiye

### Session Data Store Karna
```python
def set_session_view(request):
    # Session mein data store karna
    request.session['user_name'] = 'Rahul'
    request.session['last_visit'] = str(datetime.now())
    
    return HttpResponse("Session data set ho gaya hai!")
```

### Session Data Access Karna
```python
def get_session_view(request):
    # Session se data nikalna
    user_name = request.session.get('user_name', 'Anonymous')
    last_visit = request.session.get('last_visit', 'Pehli baar')
    
    return HttpResponse(f"Hello {user_name}! Aapki last visit: {last_visit}")
```

### Session Data Delete Karna
```python
def clear_session_view(request):
    # Specific session data delete karna
    if 'user_name' in request.session:
        del request.session['user_name']
    
    # Ya puri session clear karna
    # request.session.flush()
    
    return HttpResponse("Session data delete ho gaya!")
```

## 4. Session Storage Types 💾

Django mein sessions ko store karne ke kuch tareeke:

### 1. Database Backend (Default)
* Session data Django ke database mein store hota hai
* `django_session` table mein data jata hai
* Setup: `SESSION_ENGINE = 'django.contrib.sessions.backends.db'`

### 2. File Backend
* Session data server ke file system mein store hota hai
* Setup: `SESSION_ENGINE = 'django.contrib.sessions.backends.file'`

### 3. Cache Backend
* Session data memory cache mein store hota hai (jaise Redis/Memcached)
* Fast access lekin server restart par data lost ho sakta hai
* Setup: `SESSION_ENGINE = 'django.contrib.sessions.backends.cache'`

### 4. Cached DB Backend
* Database aur cache dono ka use karta hai
* Read operations cache se, permanent storage database mein
* Setup: `SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'`

## 5. Session Settings ⚙️

Important settings jo aap `settings.py` mein configure kar sakte hain:

```python
# Session cookie ka naam
SESSION_COOKIE_NAME = 'sessionid'

# Session expire hone ka time (seconds mein)
SESSION_COOKIE_AGE = 1209600  # Default: 2 weeks

# Sirf HTTPS ke liye session restrict karna
SESSION_COOKIE_SECURE = True  # Production mein use karein

# Session ki validity browser close hone par khatam ho jaaye
SESSION_EXPIRE_AT_BROWSER_CLOSE = True
```

## 6. Sessions ka Real-World Use 🌐

### User Authentication
```python
def login_view(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        
        user = authenticate(username=username, password=password)
        if user:
            # User login successful, session mein store karein
            request.session['user_id'] = user.id
            request.session['is_logged_in'] = True
            return redirect('dashboard')
    
    return render(request, 'login.html')
```

### Shopping Cart
```python
def add_to_cart(request, product_id):
    # Agar cart session mein nahi hai to create karein
    if 'cart' not in request.session:
        request.session['cart'] = {}
    
    # Product cart mein add karein ya quantity badhayein
    cart = request.session['cart']
    if product_id in cart:
        cart[product_id] += 1
    else:
        cart[product_id] = 1
    
    # Session ko update karein
    request.session.modified = True
    
    return redirect('cart_view')
```

## 7. Security Tips 🔒

* Sensitive data jaise passwords kabhi bhi cookies mein store na karein
* Sessions ke liye HTTPS use karein (`SESSION_COOKIE_SECURE = True`)
* Session timeouts set karein (`SESSION_COOKIE_AGE`)
* CSRF protection hamesha enable rakhein
* `HttpOnly` flag use karein JavaScript se session cookie protect karne ke liye

## 8. Debugging Tips 🐞

### Session Debugging
* `print(request.session.items())` se current session data check karein
* Django Debug Toolbar install karein session data easily dekhne ke liye
* Development server logs mein session errors check karein

### Cookie Debugging
* Browser ke developer tools mein cookies tab check karein
* `print(request.COOKIES)` se current cookies check karein

## 9. Conclusion 🎯

* Cookies client-side (browser) mein data store karte hain
* Sessions server-side data store karte hain aur ek reference cookie mein rakhte hain
* Sessions generally cookies se zyada secure hote hain
* Django mein sessions aur cookies dono ka use karna aasan hai
* Security considerations hamesha yaad rakhein
