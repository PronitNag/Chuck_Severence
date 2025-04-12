# One-to-Many Data Models में Django - Notes (Hinglish)

## Introduction (परिचय)

One-to-many relationship data models Django में bahut important concept hai. Ye models humein allow karte hain ki hum related data ko efficiently store kar sakein without unnecessary duplication.

## Key Concepts (मुख्य अवधारणाएँ)

### One-to-Many Relationship Kya Hai?

Ek one-to-many relationship tab hoti hai jab ek "parent" object ka relation multiple "child" objects se ho, lekin har "child" object sirf ek "parent" se related ho sakta hai.

**Example:**
- Ek Author ke multiple Books ho sakte hain
- Ek Course ke multiple Students ho sakte hain
- Ek Category mein multiple Products ho sakte hain

### Replication Ko Kaise Remove Karen

Data replication (duplication) database design mein ek bada problem hai. One-to-many models help karte hain is problem ko solve karne mein:

1. Data ko alag tables mein organize karein
2. Foreign keys ka use karein related data ko connect karne ke liye
3. Har piece of information sirf ek baar store karein

### Primary aur Foreign Keys

- **Primary Key**: Har table mein ek unique identifier hota hai (usually `id` field)
- **Foreign Key**: Dusre table ke primary key ka reference jo relationship establish karta hai

## Django mein One-to-Many Implementation

### Model Definition

```python
# Author model (Parent)
class Author(models.Model):
    name = models.CharField(max_length=200)
    email = models.CharField(max_length=200)
    
    def __str__(self):
        return self.name

# Book model (Child) with foreign key to Author
class Book(models.Model):
    title = models.CharField(max_length=200)
    price = models.DecimalField(max_digits=10, decimal_places=2)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
    
    def __str__(self):
        return self.title
```

### Important Points

1. `ForeignKey` field Author model ko Book model se connect karta hai
2. `on_delete=models.CASCADE` specify karta hai ki agar parent (Author) delete hota hai, to uske saare books bhi delete ho jayenge
3. Django automatically foreign key ke liye `author_id` column create karega database mein

## Django Shell se One-to-Many Explore Karna

Django shell humein interactive way mein models ko test karne ki facility deta hai:

```python
# Shell mein enter karein
python manage.py shell

# Import models
from myapp.models import Author, Book

# Create author
a = Author(name="Rahul Sharma", email="rahul@example.com")
a.save()

# Create books linked to author
b1 = Book(title="Python Basics", price=299.99, author=a)
b1.save()
b2 = Book(title="Advanced Django", price=499.99, author=a)
b2.save()

# Ek author ke saare books access karna
# Django automatically reverse relationship create karta hai
a.book_set.all()  # Returns all books by this author
```

## Data Loading with Scripts

Data ko manually add karne ke bajaye, hum scripts use kar sakte hain large amount of data load karne ke liye:

```python
# load_data.py example

import csv
from myapp.models import Author, Book

def run():
    # First, delete any existing data
    Author.objects.all().delete()
    Book.objects.all().delete()
    
    # Read data from CSV file
    with open('books.csv') as file:
        reader = csv.reader(file)
        next(reader)  # Skip header row
        
        for row in reader:
            author_name, author_email, book_title, book_price = row
            
            # Get or create author
            author, created = Author.objects.get_or_create(
                name=author_name,
                email=author_email
            )
            
            # Create book linked to author
            book = Book(
                title=book_title,
                price=float(book_price),
                author=author
            )
            book.save()
    
    print("Data successfully loaded!")
```

To run the script:
```
python manage.py runscript load_data
```

## Queries for One-to-Many Relationships

### Forward Relationship (Child → Parent)

```python
# Book se author information access karna
book = Book.objects.get(id=1)
author_name = book.author.name
```

### Reverse Relationship (Parent → Children)

```python
# Author se uske saare books access karna
author = Author.objects.get(id=1)
all_books = author.book_set.all()

# Filter books by some condition
expensive_books = author.book_set.filter(price__gt=400)
```

## Practical Tips

1. Relationship names meaningful rakhen - `ForeignKey` field ka naam relationship ko reflect karna chahiye
2. Database design ko carefully plan karein - bad design performance problems create kar sakta hai
3. `on_delete` parameter ko carefully choose karein - options include:
   - `CASCADE`: Parent delete hone par children bhi delete ho jate hain
   - `PROTECT`: Parent delete nahi ho sakta jab tak uske children exist karte hain
   - `SET_NULL`: Parent delete hone par children ka foreign key NULL ho jata hai
   - `SET_DEFAULT`: Parent delete hone par children ka foreign key default value par set ho jata hai

## Conclusion

One-to-many relationships Django mein data modeling ka fundamental part hain. Ye humein complex data structures ko efficiently represent karne mein help karte hain, aur Django ke ORM (Object-Relational Mapper) ke through in relationships ko easily manage kiya ja sakta hai.
