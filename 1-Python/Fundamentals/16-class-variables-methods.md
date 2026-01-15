# Python Class Variables and Methods - Complete Guide

## 📚 Table of Contents
1. [Class Variables vs Instance Variables](#class-variables-vs-instance-variables)
2. [Class Variables](#class-variables)
3. [Class Methods](#class-methods)
4. [Static Methods](#static-methods)
5. [When to Use Each](#when-to-use-each)
6. [Practical Examples](#practical-examples)
7. [Practice Questions](#practice-questions)

---

## 1. Class Variables vs Instance Variables

### Instance Variables
- Unique to each object
- Defined in `__init__` with `self`
- Different for each instance

### Class Variables
- Shared by all objects
- Defined in class body
- Same for all instances

```python
class Dog:
    # Class variable (shared by all dogs)
    species = "Canis familiaris"
    
    def __init__(self, name, age):
        # Instance variables (unique to each dog)
        self.name = name
        self.age = age

dog1 = Dog("Buddy", 3)
dog2 = Dog("Max", 5)

# Instance variables are different
print(dog1.name)  # Buddy
print(dog2.name)  # Max

# Class variable is the same
print(dog1.species)  # Canis familiaris
print(dog2.species)  # Canis familiaris
print(Dog.species)   # Canis familiaris
```

---

## 2. Class Variables

### Defining Class Variables
```python
class Circle:
    # Class variable
    pi = 3.14159
    
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return Circle.pi * self.radius ** 2

circle1 = Circle(5)
circle2 = Circle(10)

print(circle1.area())  # 78.53975
print(circle2.area())  # 314.159
```

### Accessing Class Variables
```python
class Counter:
    count = 0  # Class variable
    
    def __init__(self):
        Counter.count += 1  # Access via class name
        self.id = Counter.count

obj1 = Counter()
obj2 = Counter()
obj3 = Counter()

print(obj1.id)      # 1
print(obj2.id)      # 2
print(obj3.id)      # 3
print(Counter.count)  # 3
```

### Modifying Class Variables
```python
class BankAccount:
    interest_rate = 0.05  # Class variable
    
    def __init__(self, balance):
        self.balance = balance
    
    def apply_interest(self):
        self.balance += self.balance * BankAccount.interest_rate
    
    @classmethod
    def set_interest_rate(cls, rate):
        cls.interest_rate = rate

account1 = BankAccount(1000)
account2 = BankAccount(2000)

print(account1.balance)  # 1000
account1.apply_interest()
print(account1.balance)  # 1050.0

# Change interest rate for all accounts
BankAccount.set_interest_rate(0.10)
account2.apply_interest()
print(account2.balance)  # 2200.0
```

### Class Variables for Tracking
```python
class Student:
    total_students = 0  # Track total number of students
    
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade
        Student.total_students += 1
    
    @classmethod
    def get_total_students(cls):
        return cls.total_students

student1 = Student("Alice", 95)
student2 = Student("Bob", 87)
student3 = Student("Charlie", 92)

print(f"Total students: {Student.get_total_students()}")  # 3
```

---

## 3. Class Methods

Class methods work with class variables and are defined with `@classmethod` decorator.

### Basic Class Method
```python
class Person:
    population = 0
    
    def __init__(self, name):
        self.name = name
        Person.population += 1
    
    @classmethod
    def get_population(cls):
        return cls.population
    
    @classmethod
    def create_anonymous(cls):
        return cls("Anonymous")

person1 = Person("Alice")
person2 = Person("Bob")

print(Person.get_population())  # 2

# Create object using class method
anon = Person.create_anonymous()
print(anon.name)  # Anonymous
print(Person.get_population())  # 3
```

### Alternative Constructors
```python
class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day
    
    @classmethod
    def from_string(cls, date_string):
        """Create Date from string 'YYYY-MM-DD'"""
        year, month, day = map(int, date_string.split('-'))
        return cls(year, month, day)
    
    @classmethod
    def today(cls):
        """Create Date for today"""
        from datetime import date
        today = date.today()
        return cls(today.year, today.month, today.day)
    
    def __str__(self):
        return f"{self.year}-{self.month:02d}-{self.day:02d}"

# Different ways to create Date objects
date1 = Date(2024, 1, 15)
date2 = Date.from_string("2024-12-25")
date3 = Date.today()

print(date1)  # 2024-01-15
print(date2)  # 2024-12-25
print(date3)  # Current date
```

### Factory Methods
```python
class Pizza:
    def __init__(self, ingredients):
        self.ingredients = ingredients
    
    @classmethod
    def margherita(cls):
        return cls(['mozzarella', 'tomatoes', 'basil'])
    
    @classmethod
    def pepperoni(cls):
        return cls(['mozzarella', 'tomatoes', 'pepperoni'])
    
    @classmethod
    def hawaiian(cls):
        return cls(['mozzarella', 'tomatoes', 'ham', 'pineapple'])
    
    def __str__(self):
        return f"Pizza with {', '.join(self.ingredients)}"

# Create pizzas using factory methods
pizza1 = Pizza.margherita()
pizza2 = Pizza.pepperoni()
pizza3 = Pizza.hawaiian()

print(pizza1)  # Pizza with mozzarella, tomatoes, basil
print(pizza2)  # Pizza with mozzarella, tomatoes, pepperoni
```

---

## 4. Static Methods

Static methods don't access instance or class variables. They're defined with `@staticmethod` decorator.

### Basic Static Method
```python
class Math:
    @staticmethod
    def add(x, y):
        return x + y
    
    @staticmethod
    def multiply(x, y):
        return x * y
    
    @staticmethod
    def is_even(n):
        return n % 2 == 0

# Call static methods without creating an object
print(Math.add(5, 3))        # 8
print(Math.multiply(4, 7))   # 28
print(Math.is_even(10))      # True
```

### Utility Functions
```python
class StringUtils:
    @staticmethod
    def reverse(text):
        return text[::-1]
    
    @staticmethod
    def is_palindrome(text):
        text = text.lower().replace(" ", "")
        return text == text[::-1]
    
    @staticmethod
    def count_vowels(text):
        return sum(1 for char in text.lower() if char in 'aeiou')

print(StringUtils.reverse("Python"))           # nohtyP
print(StringUtils.is_palindrome("racecar"))    # True
print(StringUtils.count_vowels("Hello World")) # 3
```

### Validation Methods
```python
class Validator:
    @staticmethod
    def is_valid_email(email):
        return '@' in email and '.' in email
    
    @staticmethod
    def is_valid_phone(phone):
        return len(phone) == 10 and phone.isdigit()
    
    @staticmethod
    def is_strong_password(password):
        return (len(password) >= 8 and
                any(c.isupper() for c in password) and
                any(c.islower() for c in password) and
                any(c.isdigit() for c in password))

print(Validator.is_valid_email("user@example.com"))  # True
print(Validator.is_valid_phone("1234567890"))        # True
print(Validator.is_strong_password("Pass123"))       # True
```

---

## 5. When to Use Each

### Instance Methods
Use when you need to access or modify instance variables.

```python
class BankAccount:
    def __init__(self, balance):
        self.balance = balance
    
    # Instance method - accesses instance variable
    def deposit(self, amount):
        self.balance += amount
```

### Class Methods
Use when you need to access or modify class variables, or create alternative constructors.

```python
class Employee:
    raise_amount = 1.05  # Class variable
    
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
    
    # Class method - modifies class variable
    @classmethod
    def set_raise_amount(cls, amount):
        cls.raise_amount = amount
    
    # Class method - alternative constructor
    @classmethod
    def from_string(cls, emp_str):
        name, salary = emp_str.split('-')
        return cls(name, int(salary))
```

### Static Methods
Use for utility functions that don't need access to instance or class variables.

```python
class Calculator:
    # Static method - doesn't need instance or class data
    @staticmethod
    def add(x, y):
        return x + y
```

---

## 6. Practical Examples

### Example 1: Employee Management System
```python
class Employee:
    # Class variables
    num_employees = 0
    raise_amount = 1.04
    
    def __init__(self, first, last, salary):
        self.first = first
        self.last = last
        self.salary = salary
        Employee.num_employees += 1
    
    @property
    def email(self):
        return f"{self.first}.{self.last}@company.com".lower()
    
    @property
    def fullname(self):
        return f"{self.first} {self.last}"
    
    def apply_raise(self):
        self.salary = int(self.salary * self.raise_amount)
    
    @classmethod
    def set_raise_amount(cls, amount):
        cls.raise_amount = amount
    
    @classmethod
    def from_string(cls, emp_str):
        first, last, salary = emp_str.split('-')
        return cls(first, last, int(salary))
    
    @staticmethod
    def is_workday(day):
        # Monday = 0, Sunday = 6
        return day not in [5, 6]
    
    def __str__(self):
        return f"{self.fullname} - ${self.salary}"

# Create employees
emp1 = Employee("John", "Doe", 50000)
emp2 = Employee("Jane", "Smith", 60000)

print(emp1)  # John Doe - $50000
print(emp1.email)  # john.doe@company.com

# Apply raise
emp1.apply_raise()
print(emp1)  # John Doe - $52000

# Create from string
emp3 = Employee.from_string("Mike-Johnson-55000")
print(emp3)  # Mike Johnson - $55000

# Check workday
import datetime
today = datetime.date.today().weekday()
print(f"Is today a workday? {Employee.is_workday(today)}")

# Total employees
print(f"Total employees: {Employee.num_employees}")  # 3
```

### Example 2: Product Inventory
```python
class Product:
    # Class variables
    total_products = 0
    tax_rate = 0.08
    
    def __init__(self, name, price, quantity):
        self.name = name
        self.price = price
        self.quantity = quantity
        Product.total_products += 1
    
    def get_total_value(self):
        return self.price * self.quantity
    
    def get_price_with_tax(self):
        return self.price * (1 + Product.tax_rate)
    
    @classmethod
    def set_tax_rate(cls, rate):
        cls.tax_rate = rate
    
    @classmethod
    def from_dict(cls, data):
        return cls(data['name'], data['price'], data['quantity'])
    
    @staticmethod
    def is_valid_price(price):
        return price > 0
    
    @staticmethod
    def calculate_discount(price, discount_percent):
        return price * (1 - discount_percent / 100)
    
    def __str__(self):
        return f"{self.name}: ${self.price} x {self.quantity}"

# Create products
product1 = Product("Laptop", 999.99, 5)
product2 = Product("Mouse", 29.99, 20)

print(product1)  # Laptop: $999.99 x 5
print(f"Total value: ${product1.get_total_value()}")
print(f"Price with tax: ${product1.get_price_with_tax():.2f}")

# Create from dictionary
product_data = {'name': 'Keyboard', 'price': 79.99, 'quantity': 10}
product3 = Product.from_dict(product_data)

# Static methods
print(Product.is_valid_price(50))  # True
print(f"Discounted price: ${Product.calculate_discount(100, 20)}")  # $80.0

print(f"Total products: {Product.total_products}")  # 3
```

### Example 3: Temperature Converter
```python
class Temperature:
    # Class variable
    absolute_zero_celsius = -273.15
    
    def __init__(self, celsius):
        if celsius < Temperature.absolute_zero_celsius:
            raise ValueError("Temperature below absolute zero!")
        self.celsius = celsius
    
    def to_fahrenheit(self):
        return (self.celsius * 9/5) + 32
    
    def to_kelvin(self):
        return self.celsius - Temperature.absolute_zero_celsius
    
    @classmethod
    def from_fahrenheit(cls, fahrenheit):
        celsius = (fahrenheit - 32) * 5/9
        return cls(celsius)
    
    @classmethod
    def from_kelvin(cls, kelvin):
        celsius = kelvin + cls.absolute_zero_celsius
        return cls(celsius)
    
    @staticmethod
    def celsius_to_fahrenheit(celsius):
        return (celsius * 9/5) + 32
    
    @staticmethod
    def fahrenheit_to_celsius(fahrenheit):
        return (fahrenheit - 32) * 5/9
    
    def __str__(self):
        return f"{self.celsius}°C = {self.to_fahrenheit()}°F = {self.to_kelvin()}K"

# Create temperature objects
temp1 = Temperature(25)
temp2 = Temperature.from_fahrenheit(98.6)
temp3 = Temperature.from_kelvin(300)

print(temp1)  # 25°C = 77.0°F = 298.15K
print(temp2)  # 37.0°C = 98.6°F = 310.15K

# Use static methods
print(f"100°C = {Temperature.celsius_to_fahrenheit(100)}°F")  # 212.0°F
```

---

## 7. Practice Questions

### Beginner Level

**Question 1**: Create a `Car` class with a class variable for total cars created. Track how many cars have been instantiated.
```python
# Your code here
```

**Question 2**: Create a `Circle` class with a class variable for pi. Add instance methods for area and circumference.
```python
# Your code here
```

**Question 3**: Create a `Student` class with a class method to set the passing grade for all students.
```python
# Your code here
```

**Question 4**: Create a `Math` class with static methods for basic operations (add, subtract, multiply, divide).
```python
# Your code here
```

**Question 5**: Create a `Person` class with a class method to create a person from a full name string.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Create a `BankAccount` class with class variables for interest rate and total accounts. Include class methods to modify these.
```python
# Your code here
```

**Question 7**: Create a `Date` class with class methods for creating dates from different formats (string, timestamp, etc.).
```python
# Your code here
```

**Question 8**: Create a `Product` class with static methods for price calculations (discount, tax, etc.).
```python
# Your code here
```

**Question 9**: Create an `Employee` class that tracks total employees and average salary using class variables and methods.
```python
# Your code here
```

**Question 10**: Create a `Shape` class with static methods for geometric calculations that don't require object state.
```python
# Your code here
```

### Advanced Level

**Question 11**: Create a `Database` class with class variables for connection pool and class methods for connection management.
```python
# Your code here
```

**Question 12**: Create a `Logger` class with class variables for log level and class methods for different logging operations.
```python
# Your code here
```

**Question 13**: Create a `Config` class with class methods to load configuration from different sources (file, environment, etc.).
```python
# Your code here
```

**Question 14**: Create a `Cache` class with class variables for cache storage and class methods for cache operations.
```python
# Your code here
```

**Question 15**: Create a `Factory` class with multiple class methods that act as factory methods for creating different types of objects.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **Instance variables** are unique to each object
2. **Class variables** are shared by all objects
3. **Instance methods** use `self` and access instance data
4. **Class methods** use `@classmethod` and `cls` parameter
5. **Static methods** use `@staticmethod` and don't access class/instance data
6. Class methods are great for **alternative constructors**
7. Static methods are great for **utility functions**
8. Access class variables via **class name** to avoid confusion
9. Use `@classmethod` when you need to work with **class state**
10. Use `@staticmethod` when function is **logically related** to class but doesn't need its data

---

## 📚 Comparison Table

| Feature | Instance Method | Class Method | Static Method |
|---------|----------------|--------------|---------------|
| Decorator | None | `@classmethod` | `@staticmethod` |
| First Parameter | `self` | `cls` | None |
| Access Instance Variables | ✅ Yes | ❌ No | ❌ No |
| Access Class Variables | ✅ Yes | ✅ Yes | ❌ No |
| Can Modify Instance | ✅ Yes | ❌ No | ❌ No |
| Can Modify Class | ✅ Yes | ✅ Yes | ❌ No |
| Called On | Instance | Class or Instance | Class or Instance |
| Use Case | Work with instance data | Alternative constructors, modify class state | Utility functions |

---

## 📚 Additional Resources

- Python Class Methods: https://docs.python.org/3/library/functions.html#classmethod
- Python Static Methods: https://docs.python.org/3/library/functions.html#staticmethod
- Real Python - Class vs Static Methods: https://realpython.com/instance-class-and-static-methods-demystified/

---

**Next Topic**: Inheritance
**Previous Topic**: Object-Oriented Programming - Classes

Happy Coding! 🐍
