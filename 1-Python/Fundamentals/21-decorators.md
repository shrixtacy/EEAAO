# Python Decorators - Complete Guide

## 📚 Table of Contents
1. [Introduction to Decorators](#introduction-to-decorators)
2. [Function Decorators](#function-decorators)
3. [Decorators with Arguments](#decorators-with-arguments)
4. [Class Decorators](#class-decorators)
5. [Built-in Decorators](#built-in-decorators)
6. [Practice Questions](#practice-questions)

---

## 1. Introduction to Decorators

Decorators modify or enhance functions/classes without changing their source code.

```python
def my_decorator(func):
    def wrapper():
        print("Before function")
        func()
        print("After function")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Before function
# Hello!
# After function
```

---

## 2. Function Decorators

### Basic Decorator
```python
def uppercase_decorator(func):
    def wrapper():
        result = func()
        return result.upper()
    return wrapper

@uppercase_decorator
def greet():
    return "hello world"

print(greet())  # HELLO WORLD
```

### Decorator with Arguments
```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hello(name):
    print(f"Hello {name}!")

say_hello("Alice")
# Hello Alice!
# Hello Alice!
# Hello Alice!
```

### Timing Decorator
```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "Done"

slow_function()  # slow_function took 1.0001 seconds
```

---

## 3. Decorators with Arguments

```python
def prefix_decorator(prefix):
    def decorator(func):
        def wrapper(*args, **kwargs):
            result = func(*args, **kwargs)
            return f"{prefix}: {result}"
        return wrapper
    return decorator

@prefix_decorator("INFO")
def log_message(msg):
    return msg

print(log_message("System started"))  # INFO: System started
```

---

## 4. Class Decorators

```python
def singleton(cls):
    instances = {}
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class Database:
    def __init__(self):
        print("Creating database connection")

db1 = Database()  # Creating database connection
db2 = Database()  # (no output - returns same instance)
print(db1 is db2)  # True
```

---

## 5. Built-in Decorators

### @staticmethod
```python
class Math:
    @staticmethod
    def add(x, y):
        return x + y

print(Math.add(5, 3))  # 8
```

### @classmethod
```python
class Person:
    count = 0
    
    def __init__(self, name):
        self.name = name
        Person.count += 1
    
    @classmethod
    def get_count(cls):
        return cls.count

p1 = Person("Alice")
p2 = Person("Bob")
print(Person.get_count())  # 2
```

### @property
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def area(self):
        return 3.14159 * self._radius ** 2

circle = Circle(5)
print(circle.area)  # 78.53975
```

---

## 6. Practice Questions

**Question 1**: Create a decorator that logs function calls.
**Question 2**: Create a decorator that caches function results.
**Question 3**: Create a decorator that validates function arguments.

---

## 🎯 Key Takeaways

1. Decorators modify functions/classes
2. Use `@decorator_name` syntax
3. Decorators can take arguments
4. Built-in decorators: @staticmethod, @classmethod, @property
5. Decorators enable code reuse

---

**Next Topic**: File Detection and Operations
**Previous Topic**: Property Decorators

Happy Coding! 🐍
