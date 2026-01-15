# Python Dictionaries - Complete Reference Guide

## Table of Contents
1. [Dictionary Basics](#dictionary-basics)
2. [Dictionary Methods - All Methods](#dictionary-methods)
3. [Dictionary Operations](#dictionary-operations)
4. [Nested Dictionaries](#nested-dictionaries)
5. [Dictionary Comprehensions](#dictionary-comprehensions)
6. [Concession Stand Program](#concession-stand-program)
7. [Practice Questions](#practice-questions)

---

## Dictionary Basics

### What are Dictionaries?

Dictionaries are **unordered** collections of **key-value pairs**. Keys must be unique and immutable (hashable).

### Creating Dictionaries

```python
# Empty dictionary
empty_dict = {}
empty_dict2 = dict()

# Dictionary with elements
person = {
    "name": "John",
    "age": 30,
    "city": "New York"
}

# Different key types
mixed = {
    1: "one",
    "two": 2,
    (3, 4): "tuple key",
    True: "boolean key"
}

# Using dict() constructor
person2 = dict(name="Alice", age=25, city="Paris")

# From list of tuples
pairs = [("name", "Bob"), ("age", 35)]
person3 = dict(pairs)

# From two lists using zip
keys = ["name", "age", "city"]
values = ["Charlie", 40, "London"]
person4 = dict(zip(keys, values))
```

### Accessing Values

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Using square brackets
print(person["name"])  # John
print(person["age"])   # 30

# KeyError if key doesn't exist
# print(person["country"])  # KeyError

# Using get() method (safer)
print(person.get("name"))     # John
print(person.get("country"))  # None
print(person.get("country", "USA"))  # USA (default value)
```

### Modifying Dictionaries

```python
person = {"name": "John", "age": 30}

# Add new key-value pair
person["city"] = "New York"
print(person)  # {'name': 'John', 'age': 30, 'city': 'New York'}

# Modify existing value
person["age"] = 31
print(person)  # {'name': 'John', 'age': 31, 'city': 'New York'}

# Delete key-value pair
del person["city"]
print(person)  # {'name': 'John', 'age': 31}
```

---

## Dictionary Methods

### get() - Get Value with Default

```python
person = {"name": "John", "age": 30}

# Get existing key
print(person.get("name"))  # John

# Get non-existing key (returns None)
print(person.get("city"))  # None

# Get with custom default
print(person.get("city", "Unknown"))  # Unknown

# Compare with bracket notation
# print(person["city"])  # KeyError
print(person.get("city"))  # None (no error)
```

### keys() - Get All Keys

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Get keys view
keys = person.keys()
print(keys)  # dict_keys(['name', 'age', 'city'])

# Convert to list
keys_list = list(person.keys())
print(keys_list)  # ['name', 'age', 'city']

# Iterate over keys
for key in person.keys():
    print(key)
```

### values() - Get All Values

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Get values view
values = person.values()
print(values)  # dict_values(['John', 30, 'New York'])

# Convert to list
values_list = list(person.values())
print(values_list)  # ['John', 30, 'New York']

# Iterate over values
for value in person.values():
    print(value)
```

### items() - Get Key-Value Pairs

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Get items view
items = person.items()
print(items)  # dict_items([('name', 'John'), ('age', 30), ('city', 'New York')])

# Convert to list
items_list = list(person.items())
print(items_list)

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")
```

### update() - Update Dictionary

```python
person = {"name": "John", "age": 30}

# Update with another dictionary
person.update({"city": "New York", "age": 31})
print(person)  # {'name': 'John', 'age': 31, 'city': 'New York'}

# Update with keyword arguments
person.update(country="USA", zip=10001)
print(person)

# Update with list of tuples
person.update([("phone", "123-456"), ("email", "john@example.com")])
print(person)
```

### pop() - Remove and Return Value

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Pop existing key
age = person.pop("age")
print(age)     # 30
print(person)  # {'name': 'John', 'city': 'New York'}

# Pop with default (if key doesn't exist)
country = person.pop("country", "Unknown")
print(country)  # Unknown

# Pop without default raises KeyError
# person.pop("country")  # KeyError
```

### popitem() - Remove and Return Last Item

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Pop last item (Python 3.7+: LIFO order)
item = person.popitem()
print(item)    # ('city', 'New York')
print(person)  # {'name': 'John', 'age': 30}

# Pop from empty dictionary raises KeyError
# empty = {}
# empty.popitem()  # KeyError
```

### setdefault() - Get Value or Set Default

```python
person = {"name": "John", "age": 30}

# Get existing key
name = person.setdefault("name", "Unknown")
print(name)  # John

# Get non-existing key (sets default)
city = person.setdefault("city", "New York")
print(city)    # New York
print(person)  # {'name': 'John', 'age': 30, 'city': 'New York'}

# Useful for counting
word_count = {}
for word in ["apple", "banana", "apple", "cherry"]:
    word_count.setdefault(word, 0)
    word_count[word] += 1
print(word_count)  # {'apple': 2, 'banana': 1, 'cherry': 1}
```

### clear() - Remove All Items

```python
person = {"name": "John", "age": 30, "city": "New York"}
person.clear()
print(person)  # {}
```

### copy() - Create Shallow Copy

```python
original = {"name": "John", "age": 30}
copied = original.copy()
copied["age"] = 31

print(original)  # {'name': 'John', 'age': 30}
print(copied)    # {'name': 'John', 'age': 31}

# Shallow copy with nested structures
original = {"name": "John", "scores": [85, 90, 95]}
copied = original.copy()
copied["scores"].append(100)  # Modifies original too!
print(original)  # {'name': 'John', 'scores': [85, 90, 95, 100]}

# Deep copy for nested structures
import copy
original = {"name": "John", "scores": [85, 90, 95]}
deep_copied = copy.deepcopy(original)
deep_copied["scores"].append(100)
print(original)     # {'name': 'John', 'scores': [85, 90, 95]}
print(deep_copied)  # {'name': 'John', 'scores': [85, 90, 95, 100]}
```

### fromkeys() - Create Dictionary from Keys

```python
# Create dictionary with default value
keys = ["name", "age", "city"]
person = dict.fromkeys(keys)
print(person)  # {'name': None, 'age': None, 'city': None}

# With custom default value
person = dict.fromkeys(keys, "Unknown")
print(person)  # {'name': 'Unknown', 'age': 'Unknown', 'city': 'Unknown'}

# From string
vowels = dict.fromkeys("aeiou", 0)
print(vowels)  # {'a': 0, 'e': 0, 'i': 0, 'o': 0, 'u': 0}
```

---

## Dictionary Operations

### Membership (in, not in)

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Check if key exists
print("name" in person)     # True
print("country" in person)  # False
print("country" not in person)  # True

# Note: checks keys, not values
print("John" in person)  # False
print("John" in person.values())  # True
```

### Length (len())

```python
person = {"name": "John", "age": 30, "city": "New York"}
print(len(person))  # 3

empty = {}
print(len(empty))  # 0
```

### Merging Dictionaries

```python
# Using update()
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
dict1.update(dict2)
print(dict1)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Using ** unpacking (Python 3.5+)
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = {**dict1, **dict2}
print(merged)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Using | operator (Python 3.9+)
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = dict1 | dict2
print(merged)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Overlapping keys (later value wins)
dict1 = {"a": 1, "b": 2}
dict2 = {"b": 3, "c": 4}
merged = {**dict1, **dict2}
print(merged)  # {'a': 1, 'b': 3, 'c': 4}
```

### Iterating Dictionaries

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Iterate over keys (default)
for key in person:
    print(key)

# Iterate over keys explicitly
for key in person.keys():
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")

# With enumerate
for index, (key, value) in enumerate(person.items()):
    print(f"{index}: {key} = {value}")
```

---

## Nested Dictionaries

### Creating Nested Dictionaries

```python
# Students with grades
students = {
    "Alice": {"age": 20, "grade": 85, "major": "CS"},
    "Bob": {"age": 21, "grade": 92, "major": "Math"},
    "Charlie": {"age": 19, "grade": 78, "major": "Physics"}
}

# Company structure
company = {
    "Engineering": {
        "Frontend": ["Alice", "Bob"],
        "Backend": ["Charlie", "David"]
    },
    "Marketing": {
        "Social": ["Eve"],
        "Content": ["Frank", "Grace"]
    }
}
```

### Accessing Nested Values

```python
students = {
    "Alice": {"age": 20, "grade": 85},
    "Bob": {"age": 21, "grade": 92}
}

# Access nested value
print(students["Alice"]["grade"])  # 85

# Safe access with get()
print(students.get("Alice", {}).get("grade", 0))  # 85
print(students.get("Charlie", {}).get("grade", 0))  # 0 (default)
```

### Modifying Nested Dictionaries

```python
students = {
    "Alice": {"age": 20, "grade": 85},
    "Bob": {"age": 21, "grade": 92}
}

# Modify nested value
students["Alice"]["grade"] = 90
print(students["Alice"])  # {'age': 20, 'grade': 90}

# Add new nested key
students["Alice"]["major"] = "CS"
print(students["Alice"])  # {'age': 20, 'grade': 90, 'major': 'CS'}

# Add new student
students["Charlie"] = {"age": 19, "grade": 78}
print(students)
```

### Iterating Nested Dictionaries

```python
students = {
    "Alice": {"age": 20, "grade": 85},
    "Bob": {"age": 21, "grade": 92},
    "Charlie": {"age": 19, "grade": 78}
}

# Iterate nested dictionary
for name, info in students.items():
    print(f"\n{name}:")
    for key, value in info.items():
        print(f"  {key}: {value}")
```

---

## Dictionary Comprehensions

### Basic Syntax

```python
# Syntax: {key_expr: value_expr for item in iterable}

# Create dictionary from list
numbers = [1, 2, 3, 4, 5]
squares = {x: x**2 for x in numbers}
print(squares)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# From string
word = "hello"
char_index = {char: index for index, char in enumerate(word)}
print(char_index)  # {'h': 0, 'e': 1, 'l': 3, 'o': 4}
```

### With Condition

```python
# Syntax: {key_expr: value_expr for item in iterable if condition}

# Even numbers only
numbers = range(1, 11)
even_squares = {x: x**2 for x in numbers if x % 2 == 0}
print(even_squares)  # {2: 4, 4: 16, 6: 36, 8: 64, 10: 100}

# Filter by value
scores = {"Alice": 85, "Bob": 92, "Charlie": 78, "Diana": 95}
high_scores = {name: score for name, score in scores.items() if score >= 90}
print(high_scores)  # {'Bob': 92, 'Diana': 95}
```

### Transforming Dictionaries

```python
# Swap keys and values
original = {"a": 1, "b": 2, "c": 3}
swapped = {value: key for key, value in original.items()}
print(swapped)  # {1: 'a', 2: 'b', 3: 'c'}

# Transform values
prices = {"apple": 1.0, "banana": 0.5, "cherry": 2.0}
discounted = {item: price * 0.9 for item, price in prices.items()}
print(discounted)  # {'apple': 0.9, 'banana': 0.45, 'cherry': 1.8}

# Transform keys
original = {"name": "John", "age": 30}
upper_keys = {key.upper(): value for key, value in original.items()}
print(upper_keys)  # {'NAME': 'John', 'AGE': 30}
```

---

## Concession Stand Program

Complete program for managing a concession stand with menu, cart, and checkout.

```python
class ConcessionStand:
    def __init__(self):
        # Menu with prices
        self.menu = {
            "popcorn": 3.00,
            "soda": 2.50,
            "candy": 1.50,
            "nachos": 4.00,
            "hot dog": 3.50,
            "pretzel": 2.75,
            "water": 1.00
        }
        
        # Shopping cart
        self.cart = {}
    
    def display_menu(self):
        """Display available items and prices"""
        print("\n" + "="*40)
        print("CONCESSION STAND MENU".center(40))
        print("="*40)
        
        for item, price in sorted(self.menu.items()):
            print(f"{item.title():<20} ${price:>6.2f}")
        
        print("="*40)
    
    def add_to_cart(self, item, quantity=1):
        """Add item to cart"""
        item = item.lower()
        
        if item not in self.menu:
            print(f"Sorry, {item} is not on the menu.")
            return False
        
        if item in self.cart:
            self.cart[item] += quantity
        else:
            self.cart[item] = quantity
        
        print(f"Added {quantity}x {item.title()} to cart")
        return True
    
    def remove_from_cart(self, item):
        """Remove item from cart"""
        item = item.lower()
        
        if item in self.cart:
            del self.cart[item]
            print(f"Removed {item.title()} from cart")
            return True
        else:
            print(f"{item.title()} not in cart")
            return False
    
    def update_quantity(self, item, quantity):
        """Update item quantity in cart"""
        item = item.lower()
        
        if item not in self.cart:
            print(f"{item.title()} not in cart")
            return False
        
        if quantity <= 0:
            self.remove_from_cart(item)
        else:
            self.cart[item] = quantity
            print(f"Updated {item.title()} quantity to {quantity}")
        
        return True
    
    def view_cart(self):
        """Display cart contents"""
        if not self.cart:
            print("\nYour cart is empty")
            return
        
        print("\n" + "="*50)
        print("YOUR CART".center(50))
        print("="*50)
        
        subtotal = 0
        for item, quantity in self.cart.items():
            price = self.menu[item]
            total = price * quantity
            subtotal += total
            print(f"{item.title():<20} x{quantity:<3} ${price:>6.2f}  ${total:>7.2f}")
        
        tax = subtotal * 0.08  # 8% tax
        total = subtotal + tax
        
        print("-"*50)
        print(f"{'Subtotal:':<35} ${subtotal:>10.2f}")
        print(f"{'Tax (8%):':<35} ${tax:>10.2f}")
        print("="*50)
        print(f"{'TOTAL:':<35} ${total:>10.2f}")
        print("="*50)
    
    def checkout(self):
        """Process checkout"""
        if not self.cart:
            print("\nYour cart is empty. Nothing to checkout.")
            return
        
        self.view_cart()
        
        subtotal = sum(self.menu[item] * qty for item, qty in self.cart.items())
        tax = subtotal * 0.08
        total = subtotal + tax
        
        print("\n" + "="*50)
        confirm = input("Proceed with checkout? (yes/no): ").lower()
        
        if confirm == "yes":
            print("\nProcessing payment...")
            while True:
                try:
                    payment = float(input(f"Enter payment amount (${total:.2f}): $"))
                    if payment >= total:
                        change = payment - total
                        print(f"\nPayment received: ${payment:.2f}")
                        print(f"Change: ${change:.2f}")
                        print("\nThank you for your purchase!")
                        print("Enjoy your snacks!")
                        self.cart.clear()
                        return True
                    else:
                        print(f"Insufficient payment. Need ${total - payment:.2f} more.")
                except ValueError:
                    print("Invalid amount. Please enter a number.")
        else:
            print("\nCheckout cancelled.")
            return False
    
    def run(self):
        """Main program loop"""
        print("\n" + "="*50)
        print("WELCOME TO THE CONCESSION STAND!".center(50))
        print("="*50)
        
        while True:
            print("\n" + "-"*50)
            print("OPTIONS:")
            print("1. View Menu")
            print("2. Add Item to Cart")
            print("3. Remove Item from Cart")
            print("4. Update Quantity")
            print("5. View Cart")
            print("6. Checkout")
            print("7. Exit")
            print("-"*50)
            
            choice = input("Enter your choice (1-7): ").strip()
            
            if choice == "1":
                self.display_menu()
            
            elif choice == "2":
                item = input("Enter item name: ").strip()
                try:
                    quantity = int(input("Enter quantity (default 1): ") or "1")
                    self.add_to_cart(item, quantity)
                except ValueError:
                    print("Invalid quantity. Using 1.")
                    self.add_to_cart(item, 1)
            
            elif choice == "3":
                item = input("Enter item name to remove: ").strip()
                self.remove_from_cart(item)
            
            elif choice == "4":
                item = input("Enter item name: ").strip()
                try:
                    quantity = int(input("Enter new quantity: "))
                    self.update_quantity(item, quantity)
                except ValueError:
                    print("Invalid quantity.")
            
            elif choice == "5":
                self.view_cart()
            
            elif choice == "6":
                if self.checkout():
                    continue_shopping = input("\nContinue shopping? (yes/no): ").lower()
                    if continue_shopping != "yes":
                        break
            
            elif choice == "7":
                print("\nThank you for visiting! Goodbye!")
                break
            
            else:
                print("Invalid choice. Please enter 1-7.")

# Run the program
if __name__ == "__main__":
    stand = ConcessionStand()
    stand.run()
```

---

## Practice Questions

### Beginner Level

**Question 1:** Create a dictionary of your favorite movies with ratings.

```python
# Solution
movies = {
    "The Matrix": 9.0,
    "Inception": 8.8,
    "Interstellar": 8.6,
    "The Dark Knight": 9.0
}

for movie, rating in movies.items():
    print(f"{movie}: {rating}/10")
```

**Question 2:** Count word frequency in a sentence.

```python
# Solution
sentence = "the quick brown fox jumps over the lazy dog the fox"
words = sentence.split()

word_count = {}
for word in words:
    word_count[word] = word_count.get(word, 0) + 1

print(word_count)
# {'the': 3, 'quick': 1, 'brown': 1, 'fox': 2, ...}
```

**Question 3:** Merge two dictionaries.

```python
# Solution
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}

merged = {**dict1, **dict2}
print(merged)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

### Intermediate Level

**Question 4:** Invert a dictionary (swap keys and values).

```python
# Solution
original = {"a": 1, "b": 2, "c": 3}
inverted = {value: key for key, value in original.items()}
print(inverted)  # {1: 'a', 2: 'b', 3: 'c'}
```

**Question 5:** Find the key with maximum value.

```python
# Solution
scores = {"Alice": 85, "Bob": 92, "Charlie": 78, "Diana": 95}

max_student = max(scores, key=scores.get)
max_score = scores[max_student]

print(f"{max_student} has the highest score: {max_score}")
```

**Question 6:** Group items by category.

```python
# Solution
items = [
    ("apple", "fruit"),
    ("carrot", "vegetable"),
    ("banana", "fruit"),
    ("broccoli", "vegetable"),
    ("cherry", "fruit")
]

grouped = {}
for item, category in items:
    if category not in grouped:
        grouped[category] = []
    grouped[category].append(item)

print(grouped)
# {'fruit': ['apple', 'banana', 'cherry'], 
#  'vegetable': ['carrot', 'broccoli']}
```

### Advanced Level

**Question 7:** Implement a simple phonebook.

```python
# Solution
class Phonebook:
    def __init__(self):
        self.contacts = {}
    
    def add_contact(self, name, number):
        self.contacts[name] = number
        print(f"Added {name}: {number}")
    
    def remove_contact(self, name):
        if name in self.contacts:
            del self.contacts[name]
            print(f"Removed {name}")
        else:
            print(f"{name} not found")
    
    def search(self, name):
        return self.contacts.get(name, "Not found")
    
    def list_all(self):
        if not self.contacts:
            print("Phonebook is empty")
            return
        
        for name, number in sorted(self.contacts.items()):
            print(f"{name}: {number}")

# Usage
phonebook = Phonebook()
phonebook.add_contact("Alice", "123-456-7890")
phonebook.add_contact("Bob", "234-567-8901")
phonebook.list_all()
print(phonebook.search("Alice"))
```

**Question 8:** Calculate grade statistics.

```python
# Solution
def calculate_statistics(grades):
    stats = {
        "count": len(grades),
        "average": sum(grades.values()) / len(grades),
        "highest": max(grades.values()),
        "lowest": min(grades.values()),
        "passing": sum(1 for g in grades.values() if g >= 60)
    }
    
    # Find students with highest/lowest grades
    stats["top_student"] = max(grades, key=grades.get)
    stats["bottom_student"] = min(grades, key=grades.get)
    
    return stats

grades = {
    "Alice": 85,
    "Bob": 92,
    "Charlie": 78,
    "Diana": 95,
    "Eve": 88
}

stats = calculate_statistics(grades)
for key, value in stats.items():
    print(f"{key}: {value}")
```

**Question 9:** Flatten nested dictionary.

```python
# Solution
def flatten_dict(d, parent_key='', sep='_'):
    items = []
    for k, v in d.items():
        new_key = f"{parent_key}{sep}{k}" if parent_key else k
        if isinstance(v, dict):
            items.extend(flatten_dict(v, new_key, sep=sep).items())
        else:
            items.append((new_key, v))
    return dict(items)

nested = {
    "person": {
        "name": "John",
        "age": 30,
        "address": {
            "city": "New York",
            "zip": 10001
        }
    }
}

flat = flatten_dict(nested)
print(flat)
# {'person_name': 'John', 'person_age': 30, 
#  'person_address_city': 'New York', 'person_address_zip': 10001}
```

**Question 10:** Implement a simple cache with LRU.

```python
# Solution
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.cache = OrderedDict()
        self.capacity = capacity
    
    def get(self, key):
        if key not in self.cache:
            return None
        # Move to end (most recently used)
        self.cache.move_to_end(key)
        return self.cache[key]
    
    def put(self, key, value):
        if key in self.cache:
            # Update and move to end
            self.cache.move_to_end(key)
        self.cache[key] = value
        
        # Remove oldest if over capacity
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
    
    def display(self):
        print("Cache:", dict(self.cache))

# Usage
cache = LRUCache(3)
cache.put("a", 1)
cache.put("b", 2)
cache.put("c", 3)
cache.display()  # {'a': 1, 'b': 2, 'c': 3}

cache.get("a")  # Access 'a'
cache.put("d", 4)  # 'b' gets removed (least recently used)
cache.display()  # {'c': 3, 'a': 1, 'd': 4}
```

---

## All Dictionary Methods Summary

```python
# Accessing
dict.get(key[, default])     # Get value with optional default
dict.keys()                  # Get all keys
dict.values()                # Get all values
dict.items()                 # Get key-value pairs

# Modifying
dict.update(other)           # Update with another dict
dict.pop(key[, default])     # Remove and return value
dict.popitem()               # Remove and return last item
dict.setdefault(key[, default])  # Get or set default
dict.clear()                 # Remove all items

# Creating
dict.copy()                  # Create shallow copy
dict.fromkeys(keys[, value]) # Create dict from keys

# Operators
dict[key]                    # Access/set value
del dict[key]                # Delete key-value pair
key in dict                  # Check membership
len(dict)                    # Get number of items
```

---

## Key Takeaways

1. **Dictionaries are unordered** (Python 3.7+: insertion order preserved)
2. **Keys must be unique and hashable** (immutable types)
3. **Values can be any type** including other dictionaries
4. **Fast lookup** - O(1) average case
5. **get() is safer** than [] for accessing values
6. **items() for iteration** over key-value pairs
7. **Dictionary comprehensions** for concise creation
8. **Nested dictionaries** for complex data structures
9. **Use dictionaries** for mappings, counting, grouping
10. **Empty dict** uses {}, empty set uses set()

---

## Common Mistakes

```python
# Mistake 1: Modifying dict during iteration
d = {"a": 1, "b": 2, "c": 3}
# for key in d:
#     del d[key]  # RuntimeError

# Mistake 2: Using mutable keys
# d = {[1, 2]: "value"}  # TypeError: unhashable type: 'list'
d = {(1, 2): "value"}  # Correct (tuple is hashable)

# Mistake 3: Assuming order (Python < 3.7)
# In Python 3.6 and earlier, dict order was not guaranteed

# Mistake 4: Not using get() for optional keys
# value = d["key"]  # KeyError if key doesn't exist
value = d.get("key", default)  # Safe

# Mistake 5: Shallow copy issues with nested dicts
original = {"a": [1, 2, 3]}
copied = original.copy()
copied["a"].append(4)  # Modifies original too!
```

---

## Next Steps

Now that you master dictionaries, move on to:
- **Functions** (code reusability)
- **Lambda functions**
- **Advanced data structures**
- **JSON and data serialization**

Keep practicing with real-world data mapping problems!
