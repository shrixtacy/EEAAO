# Python Functions - Complete Guide

## Table of Contents
1. [Function Basics](#function-basics)
2. [Parameters and Arguments](#parameters-and-arguments)
3. [Return Values](#return-values)
4. [Default Arguments](#default-arguments)
5. [Keyword Arguments](#keyword-arguments)
6. [*args and **kwargs](#args-and-kwargs)
7. [Lambda Functions](#lambda-functions)
8. [Scope](#scope)
9. [Practice Questions](#practice-questions)

---

## Function Basics

### What are Functions?

Functions are reusable blocks of code that perform specific tasks. They help organize code and avoid repetition.

### Defining Functions

```python
# Basic function syntax
def function_name():
    # Function body
    statement1
    statement2

# Example: Simple greeting
def greet():
    print("Hello, World!")

# Call the function
greet()  # Output: Hello, World!
```

### Function with Parameters

```python
# Function with one parameter
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")  # Output: Hello, Alice!
greet("Bob")    # Output: Hello, Bob!

# Function with multiple parameters
def add(a, b):
    result = a + b
    print(f"{a} + {b} = {result}")

add(5, 3)  # Output: 5 + 3 = 8
```

### Docstrings

```python
def calculate_area(length, width):
    """
    Calculate the area of a rectangle.
    
    Args:
        length: The length of the rectangle
        width: The width of the rectangle
    
    Returns:
        The area of the rectangle
    """
    return length * width

# Access docstring
print(calculate_area.__doc__)
help(calculate_area)
```

---

## Parameters and Arguments

### Positional Arguments

```python
def introduce(name, age, city):
    print(f"My name is {name}")
    print(f"I am {age} years old")
    print(f"I live in {city}")

# Arguments passed by position
introduce("Alice", 25, "New York")
```

### Keyword Arguments

```python
def introduce(name, age, city):
    print(f"My name is {name}")
    print(f"I am {age} years old")
    print(f"I live in {city}")

# Arguments passed by name (order doesn't matter)
introduce(city="Paris", name="Bob", age=30)
introduce(age=35, city="London", name="Charlie")

# Mix positional and keyword (positional must come first)
introduce("Diana", age=28, city="Tokyo")
```

---

## Return Values

### Basic Return

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # Output: 8

# Use return value in expression
total = add(10, 20) + add(5, 15)
print(total)  # Output: 50
```

### Multiple Return Values

```python
def get_min_max(numbers):
    return min(numbers), max(numbers)

# Unpack return values
minimum, maximum = get_min_max([1, 5, 3, 9, 2])
print(f"Min: {minimum}, Max: {maximum}")

# Return as tuple
result = get_min_max([1, 5, 3, 9, 2])
print(result)  # Output: (1, 9)
```

### Early Return

```python
def divide(a, b):
    if b == 0:
        return "Cannot divide by zero"
    return a / b

print(divide(10, 2))  # Output: 5.0
print(divide(10, 0))  # Output: Cannot divide by zero

# Multiple return points
def check_grade(score):
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    elif score >= 60:
        return "D"
    else:
        return "F"
```

### No Return (None)

```python
def greet(name):
    print(f"Hello, {name}!")
    # No return statement

result = greet("Alice")
print(result)  # Output: None

# Explicit return None
def do_nothing():
    return None
```

---

## Default Arguments

### Basic Default Values

```python
def greet(name="Guest"):
    print(f"Hello, {name}!")

greet()          # Output: Hello, Guest!
greet("Alice")   # Output: Hello, Alice!

# Multiple defaults
def power(base, exponent=2):
    return base ** exponent

print(power(5))      # Output: 25 (5^2)
print(power(5, 3))   # Output: 125 (5^3)
```

### Default Values Best Practices

```python
# Good: Immutable defaults
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

# Bad: Mutable default (shared between calls!)
def bad_add_item(item, items=[]):
    items.append(item)
    return items

# This causes problems:
print(bad_add_item(1))  # [1]
print(bad_add_item(2))  # [1, 2] - Unexpected!
```

---

## Keyword Arguments

### Keyword-Only Arguments

```python
# Force keyword arguments with *
def create_user(name, *, age, email):
    return {
        "name": name,
        "age": age,
        "email": email
    }

# Must use keywords for age and email
user = create_user("Alice", age=25, email="alice@example.com")

# This raises error:
# user = create_user("Alice", 25, "alice@example.com")  # TypeError
```

### Positional-Only Arguments (Python 3.8+)

```python
# Force positional arguments with /
def divide(a, b, /):
    return a / b

# Must use positional
print(divide(10, 2))  # Output: 5.0

# This raises error:
# print(divide(a=10, b=2))  # TypeError
```

---

## *args and **kwargs

### *args - Variable Positional Arguments

```python
def sum_all(*args):
    """Sum any number of arguments"""
    return sum(args)

print(sum_all(1, 2, 3))           # Output: 6
print(sum_all(1, 2, 3, 4, 5))     # Output: 15
print(sum_all(10, 20, 30, 40))    # Output: 100

# args is a tuple
def print_args(*args):
    print(f"Type: {type(args)}")
    print(f"Args: {args}")
    for i, arg in enumerate(args):
        print(f"  Argument {i}: {arg}")

print_args(1, "hello", 3.14, True)
```

### **kwargs - Variable Keyword Arguments

```python
def print_info(**kwargs):
    """Print any number of keyword arguments"""
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="New York")
# Output:
# name: Alice
# age: 25
# city: New York

# kwargs is a dictionary
def show_kwargs(**kwargs):
    print(f"Type: {type(kwargs)}")
    print(f"Kwargs: {kwargs}")

show_kwargs(a=1, b=2, c=3)
```

### Combining *args and **kwargs

```python
def flexible_function(*args, **kwargs):
    print("Positional arguments:")
    for arg in args:
        print(f"  {arg}")
    
    print("\nKeyword arguments:")
    for key, value in kwargs.items():
        print(f"  {key}: {value}")

flexible_function(1, 2, 3, name="Alice", age=25)
# Output:
# Positional arguments:
#   1
#   2
#   3
# Keyword arguments:
#   name: Alice
#   age: 25
```

### Order of Parameters

```python
# Correct order: regular, *args, default, **kwargs
def complete_function(a, b, *args, c=10, **kwargs):
    print(f"a: {a}")
    print(f"b: {b}")
    print(f"args: {args}")
    print(f"c: {c}")
    print(f"kwargs: {kwargs}")

complete_function(1, 2, 3, 4, 5, c=20, x=100, y=200)
```

### Unpacking with * and **

```python
# Unpack list with *
def add(a, b, c):
    return a + b + c

numbers = [1, 2, 3]
result = add(*numbers)  # Same as add(1, 2, 3)
print(result)  # Output: 6

# Unpack dictionary with **
def greet(name, age):
    print(f"{name} is {age} years old")

person = {"name": "Alice", "age": 25}
greet(**person)  # Same as greet(name="Alice", age=25)
```

---

## Lambda Functions

### Basic Lambda

```python
# Regular function
def square(x):
    return x ** 2

# Lambda equivalent
square_lambda = lambda x: x ** 2

print(square(5))         # Output: 25
print(square_lambda(5))  # Output: 25

# Lambda with multiple arguments
add = lambda a, b: a + b
print(add(3, 5))  # Output: 8
```

### Lambda in Sorting

```python
# Sort by length
words = ["python", "is", "awesome", "language"]
sorted_words = sorted(words, key=lambda x: len(x))
print(sorted_words)  # ['is', 'python', 'awesome', 'language']

# Sort tuples by second element
pairs = [(1, 5), (3, 2), (2, 8), (4, 1)]
sorted_pairs = sorted(pairs, key=lambda x: x[1])
print(sorted_pairs)  # [(4, 1), (3, 2), (1, 5), (2, 8)]

# Sort dictionaries
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]
sorted_students = sorted(students, key=lambda x: x["grade"], reverse=True)
```

### Lambda with map(), filter(), reduce()

```python
# map() - Apply function to each element
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))
print(squared)  # [1, 4, 9, 16, 25]

# filter() - Keep elements that match condition
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)  # [2, 4]

# reduce() - Reduce to single value
from functools import reduce
product = reduce(lambda x, y: x * y, numbers)
print(product)  # 120 (1*2*3*4*5)
```

---

## Scope

### Local Scope

```python
def my_function():
    x = 10  # Local variable
    print(x)

my_function()  # Output: 10
# print(x)  # NameError: x is not defined
```

### Global Scope

```python
x = 10  # Global variable

def my_function():
    print(x)  # Can access global

my_function()  # Output: 10
print(x)      # Output: 10
```

### global Keyword

```python
count = 0  # Global variable

def increment():
    global count  # Declare we're using global
    count += 1

increment()
increment()
print(count)  # Output: 2

# Without global keyword
def bad_increment():
    count += 1  # UnboundLocalError

# This raises error:
# bad_increment()
```

### Enclosing Scope

```python
def outer():
    x = 10  # Enclosing variable
    
    def inner():
        print(x)  # Can access enclosing
    
    inner()

outer()  # Output: 10
```

### nonlocal Keyword

```python
def outer():
    x = 10
    
    def inner():
        nonlocal x  # Modify enclosing variable
        x += 5
        print(f"Inner: {x}")
    
    inner()
    print(f"Outer: {x}")

outer()
# Output:
# Inner: 15
# Outer: 15
```

### LEGB Rule

```python
# LEGB: Local, Enclosing, Global, Built-in

x = "global"

def outer():
    x = "enclosing"
    
    def inner():
        x = "local"
        print(x)  # Prints "local"
    
    inner()
    print(x)  # Prints "enclosing"

outer()
print(x)  # Prints "global"

# Built-in
print(len([1, 2, 3]))  # len is built-in function
```

---

## Practice Questions

### Beginner Level

**Question 1:** Create a function that calculates the area of a circle.

```python
# Solution
import math

def circle_area(radius):
    return math.pi * radius ** 2

print(f"Area: {circle_area(5):.2f}")
```

**Question 2:** Create a function that checks if a number is even.

```python
# Solution
def is_even(number):
    return number % 2 == 0

print(is_even(4))  # True
print(is_even(7))  # False
```

**Question 3:** Create a function that returns the larger of two numbers.

```python
# Solution
def max_of_two(a, b):
    return a if a > b else b

print(max_of_two(10, 20))  # 20
```

### Intermediate Level

**Question 4:** Create a function that calculates factorial.

```python
# Solution
def factorial(n):
    if n == 0 or n == 1:
        return 1
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

print(factorial(5))  # 120
```

**Question 5:** Create a function that checks if a string is palindrome.

```python
# Solution
def is_palindrome(text):
    text = text.lower().replace(" ", "")
    return text == text[::-1]

print(is_palindrome("racecar"))  # True
print(is_palindrome("hello"))    # False
print(is_palindrome("A man a plan a canal Panama"))  # True
```

**Question 6:** Create a function that counts vowels in a string.

```python
# Solution
def count_vowels(text):
    vowels = "aeiouAEIOU"
    return sum(1 for char in text if char in vowels)

print(count_vowels("Hello World"))  # 3
```

### Advanced Level

**Question 7:** Create a function with *args that finds the average.

```python
# Solution
def average(*args):
    if not args:
        return 0
    return sum(args) / len(args)

print(average(1, 2, 3, 4, 5))  # 3.0
print(average(10, 20, 30))     # 20.0
```

**Question 8:** Create a decorator function.

```python
# Solution
def timer_decorator(func):
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer_decorator
def slow_function():
    import time
    time.sleep(1)
    return "Done"

slow_function()
```

**Question 9:** Create a recursive function for Fibonacci.

```python
# Solution
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Print first 10 Fibonacci numbers
for i in range(10):
    print(fibonacci(i), end=" ")
# Output: 0 1 1 2 3 5 8 13 21 34
```

**Question 10:** Create a function that uses **kwargs for flexible configuration.

```python
# Solution
def create_profile(**kwargs):
    profile = {
        "name": kwargs.get("name", "Unknown"),
        "age": kwargs.get("age", 0),
        "city": kwargs.get("city", "Unknown"),
        "email": kwargs.get("email", "Not provided")
    }
    
    # Add any extra fields
    for key, value in kwargs.items():
        if key not in profile:
            profile[key] = value
    
    return profile

# Usage
profile1 = create_profile(name="Alice", age=25, city="New York")
profile2 = create_profile(name="Bob", age=30, phone="123-456")

print(profile1)
print(profile2)
```

---

## Key Takeaways

1. **Functions** organize code into reusable blocks
2. **Parameters** define what function expects
3. **Arguments** are values passed to function
4. **return** sends value back to caller
5. **Default arguments** provide fallback values
6. **Keyword arguments** improve readability
7. ***args** accepts variable positional arguments
8. ****kwargs** accepts variable keyword arguments
9. **Lambda** creates anonymous functions
10. **Scope** determines variable visibility (LEGB rule)

---

## Common Mistakes

```python
# Mistake 1: Mutable default arguments
def bad_function(items=[]):
    items.append(1)
    return items

# Use None instead
def good_function(items=None):
    if items is None:
        items = []
    items.append(1)
    return items

# Mistake 2: Forgetting to return
def add(a, b):
    result = a + b  # Forgot return!

# Mistake 3: Modifying global without declaration
x = 10
def increment():
    x += 1  # UnboundLocalError

# Mistake 4: Wrong parameter order
# def func(a=1, b):  # SyntaxError
def func(b, a=1):  # Correct

# Mistake 5: Calling function without ()
def greet():
    print("Hello")

greet  # Just references function
greet()  # Actually calls it
```

---

## Next Steps

Now that you understand functions, move on to:
- **Random Numbers and Madlib Game**
- **File I/O** (reading/writing files)
- **Exception Handling**
- **Object-Oriented Programming**

Keep practicing with function-based problem solving!
