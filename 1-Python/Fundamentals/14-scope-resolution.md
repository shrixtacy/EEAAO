# Python Scope Resolution - Complete Guide

## 📚 Table of Contents
1. [Introduction to Scope](#introduction-to-scope)
2. [LEGB Rule](#legb-rule)
3. [Local Scope](#local-scope)
4. [Enclosing Scope](#enclosing-scope)
5. [Global Scope](#global-scope)
6. [Built-in Scope](#built-in-scope)
7. [The global Keyword](#the-global-keyword)
8. [The nonlocal Keyword](#the-nonlocal-keyword)
9. [Namespace](#namespace)
10. [Best Practices](#best-practices)
11. [Practice Questions](#practice-questions)

---

## 1. Introduction to Scope

**Scope** determines where variables can be accessed in your code. It defines the visibility and lifetime of variables.

### Why Scope Matters
- Prevents naming conflicts
- Controls variable lifetime
- Encapsulates data
- Makes code more maintainable

### Types of Scope in Python
1. **Local** - Inside a function
2. **Enclosing** - In enclosing functions (nested functions)
3. **Global** - At module level
4. **Built-in** - Python's built-in names

---

## 2. LEGB Rule

Python follows the **LEGB** rule to resolve variable names:

**L** - Local (function)  
**E** - Enclosing (outer functions)  
**G** - Global (module)  
**B** - Built-in (Python)

### Search Order
```python
# Python searches in this order:
# 1. Local scope (current function)
# 2. Enclosing scope (outer functions)
# 3. Global scope (module level)
# 4. Built-in scope (Python built-ins)

x = "global"  # Global scope

def outer():
    x = "enclosing"  # Enclosing scope
    
    def inner():
        x = "local"  # Local scope
        print(x)  # Prints "local"
    
    inner()
    print(x)  # Prints "enclosing"

outer()
print(x)  # Prints "global"
```

---

## 3. Local Scope

Variables defined inside a function are in **local scope**.

### Basic Local Scope
```python
def greet():
    message = "Hello"  # Local variable
    print(message)

greet()  # Hello
# print(message)  # NameError: name 'message' is not defined
```

### Function Parameters are Local
```python
def add(a, b):  # a and b are local variables
    result = a + b  # result is also local
    return result

print(add(5, 3))  # 8
# print(a)  # NameError: name 'a' is not defined
```

### Local Variables Lifetime
```python
def counter():
    count = 0  # Created when function is called
    count += 1
    return count  # Destroyed when function returns

print(counter())  # 1
print(counter())  # 1 (new count variable each time)
print(counter())  # 1
```

### Multiple Local Scopes
```python
def func1():
    x = 10  # Local to func1
    print(f"func1: x = {x}")

def func2():
    x = 20  # Local to func2 (different from func1's x)
    print(f"func2: x = {x}")

func1()  # func1: x = 10
func2()  # func2: x = 20
```

---

## 4. Enclosing Scope

Variables in outer functions are in **enclosing scope** for inner functions.

### Nested Functions
```python
def outer():
    x = "outer"  # Enclosing scope for inner()
    
    def inner():
        print(x)  # Accesses x from enclosing scope
    
    inner()

outer()  # outer
```

### Multiple Levels of Nesting
```python
def level1():
    x = "level1"
    
    def level2():
        y = "level2"
        
        def level3():
            z = "level3"
            print(x)  # From level1 (enclosing)
            print(y)  # From level2 (enclosing)
            print(z)  # From level3 (local)
        
        level3()
    
    level2()

level1()
# level1
# level2
# level3
```

### Shadowing Enclosing Variables
```python
def outer():
    x = "outer"
    
    def inner():
        x = "inner"  # Shadows outer's x
        print(f"Inner: {x}")
    
    inner()
    print(f"Outer: {x}")  # outer's x unchanged

outer()
# Inner: inner
# Outer: outer
```

### Closures
```python
def make_multiplier(n):
    def multiplier(x):
        return x * n  # n from enclosing scope
    return multiplier

times3 = make_multiplier(3)
times5 = make_multiplier(5)

print(times3(10))  # 30
print(times5(10))  # 50
```

---

## 5. Global Scope

Variables defined at module level are in **global scope**.

### Global Variables
```python
# Global variable
name = "Alice"

def greet():
    print(f"Hello, {name}")  # Access global variable

greet()  # Hello, Alice
print(name)  # Alice
```

### Reading vs Writing Global Variables
```python
count = 0  # Global

def increment():
    # count += 1  # UnboundLocalError!
    # Python thinks count is local because of assignment
    print(count)  # This works (reading only)

increment()
```

### Global Variables in Multiple Functions
```python
total = 0  # Global

def add_to_total(amount):
    global total  # Declare we're using global total
    total += amount

def get_total():
    return total  # Reading global (no declaration needed)

add_to_total(10)
add_to_total(20)
print(get_total())  # 30
```

---

## 6. Built-in Scope

Python's built-in names (functions, exceptions, etc.) are in **built-in scope**.

### Built-in Names
```python
# These are all built-in
print(len([1, 2, 3]))      # 3
print(max([1, 5, 3]))      # 5
print(abs(-10))            # 10
print(type(42))            # <class 'int'>

# Built-in exceptions
try:
    x = 1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
```

### Viewing Built-ins
```python
import builtins

# List all built-in names
print(dir(builtins))
```

### Shadowing Built-ins (Bad Practice!)
```python
# DON'T DO THIS!
list = [1, 2, 3]  # Shadows built-in list()
print(list)  # [1, 2, 3]

# Now list() function is not accessible
# my_list = list(range(5))  # TypeError!

# Fix: Delete the variable
del list
my_list = list(range(5))  # Works again
print(my_list)  # [0, 1, 2, 3, 4]
```

---

## 7. The global Keyword

Use `global` to modify global variables from within a function.

### Basic Usage
```python
counter = 0  # Global

def increment():
    global counter  # Declare we're modifying global counter
    counter += 1

def decrement():
    global counter
    counter -= 1

increment()
increment()
increment()
print(counter)  # 3

decrement()
print(counter)  # 2
```

### Creating Global Variables
```python
def create_global():
    global new_var  # Create new global variable
    new_var = "I'm global!"

create_global()
print(new_var)  # I'm global!
```

### Multiple Global Variables
```python
x = 10
y = 20

def modify_globals():
    global x, y  # Declare multiple globals
    x = 100
    y = 200

modify_globals()
print(x, y)  # 100 200
```

### Global in Nested Functions
```python
count = 0

def outer():
    def inner():
        global count  # Refers to module-level count
        count += 1
    
    inner()

outer()
outer()
print(count)  # 2
```

---

## 8. The nonlocal Keyword

Use `nonlocal` to modify variables in enclosing (but not global) scope.

### Basic Usage
```python
def outer():
    count = 0  # Enclosing variable
    
    def inner():
        nonlocal count  # Modify enclosing count
        count += 1
        print(f"Inner count: {count}")
    
    inner()
    inner()
    inner()
    print(f"Outer count: {count}")

outer()
# Inner count: 1
# Inner count: 2
# Inner count: 3
# Outer count: 3
```

### nonlocal vs global
```python
x = "global"

def outer():
    x = "enclosing"
    
    def inner_global():
        global x  # Modifies global x
        x = "modified global"
    
    def inner_nonlocal():
        nonlocal x  # Modifies enclosing x
        x = "modified enclosing"
    
    print(f"Before: {x}")
    inner_nonlocal()
    print(f"After nonlocal: {x}")

outer()
print(f"Global: {x}")
# Before: enclosing
# After nonlocal: modified enclosing
# Global: global
```

### Multiple Levels with nonlocal
```python
def level1():
    x = "level1"
    
    def level2():
        x = "level2"
        
        def level3():
            nonlocal x  # Refers to level2's x
            x = "modified level2"
        
        print(f"Before level3: {x}")
        level3()
        print(f"After level3: {x}")
    
    level2()
    print(f"Level1: {x}")

level1()
# Before level3: level2
# After level3: modified level2
# Level1: level1
```

### Closure with nonlocal
```python
def make_counter():
    count = 0
    
    def increment():
        nonlocal count
        count += 1
        return count
    
    def decrement():
        nonlocal count
        count -= 1
        return count
    
    def get_count():
        return count
    
    return increment, decrement, get_count

inc, dec, get = make_counter()

print(inc())  # 1
print(inc())  # 2
print(inc())  # 3
print(dec())  # 2
print(get())  # 2
```

---

## 9. Namespace

A **namespace** is a mapping from names to objects.

### Types of Namespaces
1. **Built-in namespace** - Python's built-in names
2. **Global namespace** - Module-level names
3. **Local namespace** - Function-level names

### Viewing Namespaces
```python
# Global namespace
x = 10
y = 20

def func():
    # Local namespace
    a = 1
    b = 2
    print("Local namespace:", locals())

print("Global namespace:", globals())
func()
```

### vars() Function
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

person = Person("Alice", 25)
print(vars(person))  # {'name': 'Alice', 'age': 25}
```

### dir() Function
```python
# List names in current scope
print(dir())

# List names in a module
import math
print(dir(math))

# List names in an object
class MyClass:
    x = 10
    def method(self):
        pass

print(dir(MyClass))
```

---

## 10. Best Practices

### Minimize Global Variables
```python
# BAD: Too many globals
count = 0
total = 0
average = 0

def calculate():
    global count, total, average
    # ...

# GOOD: Use class or return values
class Statistics:
    def __init__(self):
        self.count = 0
        self.total = 0
    
    def calculate(self):
        return self.total / self.count if self.count > 0 else 0
```

### Use Function Parameters
```python
# BAD: Relying on global
name = "Alice"

def greet():
    print(f"Hello, {name}")

# GOOD: Pass as parameter
def greet(name):
    print(f"Hello, {name}")

greet("Alice")
```

### Avoid Shadowing Built-ins
```python
# BAD
list = [1, 2, 3]
dict = {"a": 1}
str = "hello"

# GOOD
my_list = [1, 2, 3]
my_dict = {"a": 1}
my_str = "hello"
```

### Use Constants for Global Configuration
```python
# config.py
DATABASE_URL = "postgresql://localhost/mydb"
API_KEY = "your-api-key"
MAX_CONNECTIONS = 100

# main.py
import config

def connect_database():
    return connect(config.DATABASE_URL)
```

### Prefer Closures Over Global State
```python
# BAD: Global state
counter = 0

def increment():
    global counter
    counter += 1
    return counter

# GOOD: Closure
def make_counter():
    counter = 0
    
    def increment():
        nonlocal counter
        counter += 1
        return counter
    
    return increment

my_counter = make_counter()
print(my_counter())  # 1
print(my_counter())  # 2
```

---

## 11. Practice Questions

### Beginner Level

**Question 1**: What will this code print?
```python
x = 10

def func():
    x = 20
    print(x)

func()
print(x)
```

**Question 2**: Fix this code to modify the global variable:
```python
count = 0

def increment():
    count += 1  # Error!

increment()
```

**Question 3**: What will this code print?
```python
def outer():
    x = "outer"
    
    def inner():
        print(x)
    
    inner()

outer()
```

**Question 4**: Create a function that uses a global variable to count how many times it's been called.
```python
# Your code here
```

**Question 5**: What's wrong with this code?
```python
def func():
    print(len([1, 2, 3]))

len = 5
func()
```

### Intermediate Level

**Question 6**: Create a closure that maintains a private counter.
```python
# Your code here
```

**Question 7**: Fix this code using `nonlocal`:
```python
def outer():
    count = 0
    
    def inner():
        count += 1  # Error!
        return count
    
    return inner
```

**Question 8**: What will this code print?
```python
x = "global"

def outer():
    x = "enclosing"
    
    def inner():
        global x
        x = "local"
    
    inner()
    print(x)

outer()
print(x)
```

**Question 9**: Create a function factory that creates functions with different behaviors based on a parameter.
```python
# Your code here
```

**Question 10**: Implement a simple bank account using closures (balance should be private).
```python
# Your code here
```

### Advanced Level

**Question 11**: Create a decorator that counts how many times a function is called (use closure).
```python
# Your code here
```

**Question 12**: Implement a memoization decorator using closures.
```python
# Your code here
```

**Question 13**: Create a class that demonstrates all four scopes (L, E, G, B).
```python
# Your code here
```

**Question 14**: Implement a simple state machine using closures and nonlocal.
```python
# Your code here
```

**Question 15**: Create a function that can access and modify variables in multiple enclosing scopes.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **LEGB Rule**: Local → Enclosing → Global → Built-in
2. **Local scope**: Variables inside functions
3. **Enclosing scope**: Variables in outer functions
4. **Global scope**: Module-level variables
5. **Built-in scope**: Python's built-in names
6. **global keyword**: Modify global variables
7. **nonlocal keyword**: Modify enclosing variables
8. **Namespace**: Mapping from names to objects
9. **Avoid shadowing**: Don't override built-in names
10. **Minimize globals**: Use parameters and return values

---

## 📚 Scope Resolution Examples

### Example 1: LEGB in Action
```python
# Built-in
len([1, 2, 3])  # Built-in len()

# Global
x = "global"

def outer():
    # Enclosing
    x = "enclosing"
    
    def inner():
        # Local
        x = "local"
        print(f"Local: {x}")
    
    inner()
    print(f"Enclosing: {x}")

outer()
print(f"Global: {x}")
```

### Example 2: Closure Example
```python
def make_adder(n):
    def adder(x):
        return x + n
    return adder

add5 = make_adder(5)
add10 = make_adder(10)

print(add5(3))   # 8
print(add10(3))  # 13
```

### Example 3: Global vs Nonlocal
```python
x = 0  # Global

def outer():
    x = 0  # Enclosing
    
    def use_global():
        global x
        x += 1
    
    def use_nonlocal():
        nonlocal x
        x += 1
    
    use_nonlocal()
    print(f"After nonlocal: enclosing x = {x}")
    
    use_global()
    print(f"After global: enclosing x = {x}")

outer()
print(f"Global x = {x}")
```

---

## 📚 Additional Resources

- Python Scopes and Namespaces: https://docs.python.org/3/tutorial/classes.html#scopes-and-namespaces
- PEP 227 - Statically Nested Scopes: https://www.python.org/dev/peps/pep-0227/
- Python Execution Model: https://docs.python.org/3/reference/executionmodel.html

---

**Next Topic**: Object-Oriented Programming - Classes
**Previous Topic**: Modules and Packages

Happy Coding! 🐍
