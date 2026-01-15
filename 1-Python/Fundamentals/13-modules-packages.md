# Python Modules and Packages - Complete Guide

## 📚 Table of Contents
1. [Introduction to Modules](#introduction-to-modules)
2. [Importing Modules](#importing-modules)
3. [Creating Custom Modules](#creating-custom-modules)
4. [The __name__ Variable](#the-name-variable)
5. [Module Search Path](#module-search-path)
6. [Packages](#packages)
7. [Standard Library Modules](#standard-library-modules)
8. [Third-Party Packages](#third-party-packages)
9. [Best Practices](#best-practices)
10. [Practice Questions](#practice-questions)

---

## 1. Introduction to Modules

A **module** is a file containing Python code (functions, classes, variables). Modules help organize code and promote reusability.

### Why Use Modules?
- **Code Organization**: Break large programs into smaller files
- **Reusability**: Use same code in multiple programs
- **Namespace**: Avoid naming conflicts
- **Maintainability**: Easier to maintain and debug

### Types of Modules
1. **Built-in modules**: Part of Python standard library (math, random, os)
2. **Custom modules**: Created by you
3. **Third-party modules**: Installed via pip (numpy, pandas, requests)

---

## 2. Importing Modules

### Basic Import
```python
# Import entire module
import math

print(math.pi)           # 3.141592653589793
print(math.sqrt(16))     # 4.0
print(math.pow(2, 3))    # 8.0
```

### Import Specific Items
```python
# Import specific functions/variables
from math import pi, sqrt, pow

print(pi)           # 3.141592653589793
print(sqrt(16))     # 4.0
print(pow(2, 3))    # 8.0
```

### Import with Alias
```python
# Import with alias (shorter name)
import math as m

print(m.pi)         # 3.141592653589793
print(m.sqrt(25))   # 5.0

# Import specific item with alias
from math import sqrt as square_root

print(square_root(9))  # 3.0
```

### Import All (Not Recommended)
```python
# Import everything from module
from math import *

print(pi)           # 3.141592653589793
print(sqrt(16))     # 4.0
print(cos(0))       # 1.0

# Warning: Can cause naming conflicts!
```

### Multiple Imports
```python
# Import multiple items
from math import pi, sqrt, pow, sin, cos

# Import multiple modules
import math, random, os

# Better style (PEP 8)
import math
import random
import os
```

---

## 3. Creating Custom Modules

### Example 1: Simple Module

**File: mymath.py**
```python
"""
Custom math module with basic operations
"""

def add(a, b):
    """Add two numbers"""
    return a + b

def subtract(a, b):
    """Subtract b from a"""
    return a - b

def multiply(a, b):
    """Multiply two numbers"""
    return a * b

def divide(a, b):
    """Divide a by b"""
    if b == 0:
        return "Error: Division by zero"
    return a / b

# Module-level variable
PI = 3.14159
```

**Using the module:**
```python
import mymath

print(mymath.add(10, 5))        # 15
print(mymath.subtract(10, 5))   # 5
print(mymath.multiply(10, 5))   # 50
print(mymath.divide(10, 5))     # 2.0
print(mymath.PI)                # 3.14159
```

### Example 2: Module with Classes

**File: shapes.py**
```python
"""
Module for geometric shapes
"""

class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def circumference(self):
        return 2 * 3.14159 * self.radius

class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Square:
    def __init__(self, side):
        self.side = side
    
    def area(self):
        return self.side ** 2
    
    def perimeter(self):
        return 4 * self.side
```

**Using the module:**
```python
from shapes import Circle, Rectangle, Square

circle = Circle(5)
print(f"Circle area: {circle.area()}")

rectangle = Rectangle(4, 6)
print(f"Rectangle area: {rectangle.area()}")

square = Square(5)
print(f"Square area: {square.area()}")
```

### Example 3: Module with Data

**File: constants.py**
```python
"""
Module containing constants
"""

# Mathematical constants
PI = 3.14159265359
E = 2.71828182846
GOLDEN_RATIO = 1.61803398875

# Physical constants
SPEED_OF_LIGHT = 299792458  # m/s
GRAVITY = 9.81  # m/s²

# Conversion factors
KM_TO_MILES = 0.621371
LBS_TO_KG = 0.453592
CELSIUS_TO_FAHRENHEIT = lambda c: (c * 9/5) + 32
```

**Using the module:**
```python
import constants

print(f"Pi: {constants.PI}")
print(f"Speed of light: {constants.SPEED_OF_LIGHT} m/s")
print(f"10 km = {10 * constants.KM_TO_MILES} miles")
print(f"25°C = {constants.CELSIUS_TO_FAHRENHEIT(25)}°F")
```

---

## 4. The __name__ Variable

The `__name__` variable helps distinguish between running a module directly vs importing it.

### Understanding __name__

**File: demo.py**
```python
def greet(name):
    print(f"Hello, {name}!")

def main():
    print("Running demo.py")
    greet("Alice")
    greet("Bob")

# This code only runs when file is executed directly
if __name__ == "__main__":
    main()
```

**When run directly:**
```bash
python demo.py
# Output:
# Running demo.py
# Hello, Alice!
# Hello, Bob!
```

**When imported:**
```python
import demo

demo.greet("Charlie")  # Hello, Charlie!
# main() is NOT called automatically
```

### Practical Example

**File: calculator.py**
```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        return "Error: Division by zero"
    return a / b

def test_calculator():
    """Test all calculator functions"""
    print("Testing calculator...")
    print(f"10 + 5 = {add(10, 5)}")
    print(f"10 - 5 = {subtract(10, 5)}")
    print(f"10 * 5 = {multiply(10, 5)}")
    print(f"10 / 5 = {divide(10, 5)}")

if __name__ == "__main__":
    # Run tests when executed directly
    test_calculator()
```

---

## 5. Module Search Path

Python searches for modules in specific locations.

### Viewing Search Path
```python
import sys

# Display module search path
for path in sys.path:
    print(path)
```

### Search Order
1. Current directory
2. PYTHONPATH environment variable
3. Standard library directories
4. Site-packages (third-party modules)

### Adding to Search Path
```python
import sys

# Add custom directory to search path
sys.path.append('/path/to/my/modules')

# Now you can import from that directory
import mymodule
```

### Module Location
```python
import math

# Find where module is located
print(math.__file__)
# Output: /usr/lib/python3.x/lib-dynload/math.cpython-3x.so
```

---

## 6. Packages

A **package** is a directory containing multiple modules and a special `__init__.py` file.

### Package Structure
```
mypackage/
    __init__.py
    module1.py
    module2.py
    subpackage/
        __init__.py
        module3.py
        module4.py
```

### Creating a Package

**Directory structure:**
```
calculator/
    __init__.py
    basic.py
    scientific.py
    statistics.py
```

**File: calculator/__init__.py**
```python
"""
Calculator package
"""

__version__ = "1.0.0"
__author__ = "Your Name"

# Import key functions to package level
from .basic import add, subtract, multiply, divide
from .scientific import power, sqrt, log
from .statistics import mean, median, mode
```

**File: calculator/basic.py**
```python
"""Basic arithmetic operations"""

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
```

**File: calculator/scientific.py**
```python
"""Scientific calculations"""
import math

def power(base, exponent):
    return base ** exponent

def sqrt(n):
    return math.sqrt(n)

def log(n, base=10):
    return math.log(n, base)
```

**File: calculator/statistics.py**
```python
"""Statistical functions"""

def mean(numbers):
    return sum(numbers) / len(numbers)

def median(numbers):
    sorted_nums = sorted(numbers)
    n = len(sorted_nums)
    mid = n // 2
    if n % 2 == 0:
        return (sorted_nums[mid - 1] + sorted_nums[mid]) / 2
    return sorted_nums[mid]

def mode(numbers):
    from collections import Counter
    count = Counter(numbers)
    return count.most_common(1)[0][0]
```

### Using the Package
```python
# Import entire package
import calculator

print(calculator.add(10, 5))
print(calculator.sqrt(16))
print(calculator.mean([1, 2, 3, 4, 5]))

# Import specific module
from calculator import basic

print(basic.multiply(4, 5))

# Import specific function
from calculator.scientific import power

print(power(2, 8))

# Import from submodule
from calculator.statistics import median

print(median([1, 3, 5, 7, 9]))
```

### Relative Imports

**Within package modules:**
```python
# In calculator/scientific.py
from .basic import add, multiply  # Same package
from ..otherpackage import something  # Parent package
```

---

## 7. Standard Library Modules

Python comes with many built-in modules.

### math Module
```python
import math

# Constants
print(math.pi)      # 3.141592653589793
print(math.e)       # 2.718281828459045

# Functions
print(math.sqrt(16))        # 4.0
print(math.pow(2, 3))       # 8.0
print(math.ceil(4.3))       # 5
print(math.floor(4.7))      # 4
print(math.factorial(5))    # 120
print(math.gcd(48, 18))     # 6
```

### random Module
```python
import random

# Random numbers
print(random.random())              # 0.0 to 1.0
print(random.randint(1, 10))        # 1 to 10
print(random.uniform(1.0, 10.0))    # 1.0 to 10.0

# Random choice
colors = ['red', 'green', 'blue']
print(random.choice(colors))

# Shuffle list
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(numbers)

# Random sample
print(random.sample(range(1, 50), 6))  # 6 unique numbers
```

### datetime Module
```python
from datetime import datetime, date, time, timedelta

# Current date and time
now = datetime.now()
print(now)

# Current date
today = date.today()
print(today)

# Create specific date
birthday = date(1990, 5, 15)
print(birthday)

# Date arithmetic
tomorrow = today + timedelta(days=1)
next_week = today + timedelta(weeks=1)
print(tomorrow, next_week)

# Format date
print(today.strftime("%B %d, %Y"))  # May 15, 2024
```

### os Module
```python
import os

# Current directory
print(os.getcwd())

# List files
print(os.listdir('.'))

# Check if file exists
print(os.path.exists('file.txt'))

# Create directory
# os.mkdir('new_folder')

# Environment variables
print(os.environ.get('HOME'))

# Path operations
path = os.path.join('folder', 'subfolder', 'file.txt')
print(path)
```

### sys Module
```python
import sys

# Python version
print(sys.version)

# Command line arguments
print(sys.argv)

# Exit program
# sys.exit()

# Module search path
print(sys.path)

# Platform
print(sys.platform)  # 'win32', 'linux', 'darwin'
```

### json Module
```python
import json

# Python to JSON
data = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

json_string = json.dumps(data)
print(json_string)

# JSON to Python
json_data = '{"name": "Bob", "age": 30}'
python_dict = json.loads(json_data)
print(python_dict)

# Write to file
with open('data.json', 'w') as file:
    json.dump(data, file, indent=4)

# Read from file
with open('data.json', 'r') as file:
    loaded_data = json.load(file)
    print(loaded_data)
```

### collections Module
```python
from collections import Counter, defaultdict, namedtuple, deque

# Counter
words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple']
count = Counter(words)
print(count)  # Counter({'apple': 3, 'banana': 2, 'cherry': 1})
print(count.most_common(2))  # [('apple', 3), ('banana', 2)]

# defaultdict
dd = defaultdict(int)
dd['a'] += 1
dd['b'] += 2
print(dd)  # defaultdict(<class 'int'>, {'a': 1, 'b': 2})

# namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)  # 10 20

# deque (double-ended queue)
dq = deque([1, 2, 3])
dq.append(4)        # Add to right
dq.appendleft(0)    # Add to left
print(dq)  # deque([0, 1, 2, 3, 4])
```

---

## 8. Third-Party Packages

### Installing Packages with pip
```bash
# Install package
pip install requests

# Install specific version
pip install requests==2.28.0

# Install from requirements.txt
pip install -r requirements.txt

# Uninstall package
pip uninstall requests

# List installed packages
pip list

# Show package info
pip show requests

# Upgrade package
pip install --upgrade requests
```

### Popular Third-Party Packages
```python
# requests - HTTP library
import requests
response = requests.get('https://api.github.com')
print(response.json())

# numpy - Numerical computing
import numpy as np
arr = np.array([1, 2, 3, 4, 5])
print(arr.mean())

# pandas - Data analysis
import pandas as pd
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})
print(df)

# matplotlib - Plotting
import matplotlib.pyplot as plt
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.show()
```

---

## 9. Best Practices

### Import Organization (PEP 8)
```python
# 1. Standard library imports
import os
import sys
from datetime import datetime

# 2. Third-party imports
import numpy as np
import pandas as pd
import requests

# 3. Local application imports
from mypackage import mymodule
from mypackage.subpackage import another_module
```

### Avoid Circular Imports
```python
# BAD: module_a.py imports module_b.py, and module_b.py imports module_a.py

# GOOD: Restructure code or use local imports
def function_that_needs_module_b():
    from module_b import something
    # Use something here
```

### Use __all__ to Control Exports
```python
# mymodule.py
__all__ = ['public_function', 'PublicClass']

def public_function():
    pass

def _private_function():
    pass

class PublicClass:
    pass

class _PrivateClass:
    pass
```

### Module Documentation
```python
"""
Module Name: mymodule.py
Description: This module provides utility functions for...
Author: Your Name
Date: 2024-01-15
Version: 1.0.0
"""

def function():
    """
    Function description
    
    Args:
        param1: Description
        param2: Description
    
    Returns:
        Description of return value
    """
    pass
```

---

## 10. Practice Questions

### Beginner Level

**Question 1**: Create a module called `temperature.py` with functions to convert between Celsius, Fahrenheit, and Kelvin.
```python
# Your code here
```

**Question 2**: Create a module called `string_utils.py` with functions for common string operations (reverse, capitalize_words, count_vowels).
```python
# Your code here
```

**Question 3**: Import the `random` module and create a dice rolling simulator.
```python
# Your code here
```

**Question 4**: Create a module called `list_utils.py` with functions to find max, min, and average of a list.
```python
# Your code here
```

**Question 5**: Use the `datetime` module to calculate your age in days.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Create a package called `geometry` with modules for 2D shapes (circle, rectangle, triangle) and 3D shapes (sphere, cube, cylinder).
```python
# Your code here
```

**Question 7**: Create a module that reads and writes JSON files with error handling.
```python
# Your code here
```

**Question 8**: Use the `os` module to create a file organizer that sorts files by extension.
```python
# Your code here
```

**Question 9**: Create a module with a decorator that times function execution.
```python
# Your code here
```

**Question 10**: Build a package for a simple database (using JSON files) with CRUD operations.
```python
# Your code here
```

### Advanced Level

**Question 11**: Create a package with lazy loading of submodules to improve import performance.
```python
# Your code here
```

**Question 12**: Implement a plugin system where modules can be dynamically loaded from a directory.
```python
# Your code here
```

**Question 13**: Create a module that uses `__getattr__` to provide dynamic attribute access.
```python
# Your code here
```

**Question 14**: Build a package with proper logging, configuration, and error handling.
```python
# Your code here
```

**Question 15**: Create a module that can reload itself when source code changes (useful for development).
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **Modules** organize code into reusable files
2. **Import** modules using `import` or `from...import`
3. **__name__ == "__main__"** distinguishes direct execution from import
4. **Packages** are directories with `__init__.py` file
5. **Standard library** provides many useful modules
6. **pip** installs third-party packages
7. Follow **PEP 8** for import organization
8. Use **relative imports** within packages
9. **Document** your modules with docstrings
10. Avoid **circular imports** and **import ***

---

## 📚 Additional Resources

- Python Module Index: https://docs.python.org/3/py-modindex.html
- PEP 8 - Import Guidelines: https://pep8.org/#imports
- Python Packaging Guide: https://packaging.python.org/
- PyPI (Python Package Index): https://pypi.org/

---

**Next Topic**: Scope Resolution (LEGB Rule)
**Previous Topic**: Match-Case Statements

Happy Coding! 🐍
