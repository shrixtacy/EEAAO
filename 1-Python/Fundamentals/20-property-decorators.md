# Python Property Decorators - Complete Guide

## 📚 Table of Contents
1. [Introduction to Properties](#introduction-to-properties)
2. [@property Decorator](#property-decorator)
3. [Getters and Setters](#getters-and-setters)
4. [Deleters](#deleters)
5. [Read-only Properties](#read-only-properties)
6. [Practice Questions](#practice-questions)

---

## 1. Introduction to Properties

Properties allow you to add getter, setter, and deleter functionality to class attributes.

### Without Properties
```python
class Person:
    def __init__(self, name):
        self._name = name
    
    def get_name(self):
        return self._name
    
    def set_name(self, name):
        self._name = name

person = Person("Alice")
print(person.get_name())  # Alice
person.set_name("Bob")
print(person.get_name())  # Bob
```

### With Properties
```python
class Person:
    def __init__(self, name):
        self._name = name
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, name):
        self._name = name

person = Person("Alice")
print(person.name)  # Alice (looks like attribute access)
person.name = "Bob"  # (looks like attribute assignment)
print(person.name)  # Bob
```

---

## 2. @property Decorator

The `@property` decorator creates a getter method.

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius
    
    @property
    def radius(self):
        return self._radius
    
    @property
    def diameter(self):
        return self._radius * 2
    
    @property
    def area(self):
        return 3.14159 * self._radius ** 2

circle = Circle(5)
print(circle.radius)    # 5
print(circle.diameter)  # 10
print(circle.area)      # 78.53975
```

---

## 3. Getters and Setters

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        """Getter for celsius"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """Setter for celsius with validation"""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """Getter for fahrenheit"""
        return (self._celsius * 9/5) + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """Setter for fahrenheit"""
        self._celsius = (value - 32) * 5/9

temp = Temperature(25)
print(temp.celsius)     # 25
print(temp.fahrenheit)  # 77.0

temp.fahrenheit = 98.6
print(temp.celsius)     # 37.0
```

---

## 4. Deleters

```python
class Person:
    def __init__(self, name):
        self._name = name
    
    @property
    def name(self):
        return self._name
    
    @name.setter
    def name(self, value):
        self._name = value
    
    @name.deleter
    def name(self):
        print(f"Deleting {self._name}")
        del self._name

person = Person("Alice")
print(person.name)  # Alice
del person.name     # Deleting Alice
# print(person.name)  # AttributeError
```

---

## 5. Read-only Properties

```python
class Product:
    def __init__(self, name, price):
        self._name = name
        self._price = price
        self._id = id(self)  # Unique ID
    
    @property
    def name(self):
        return self._name
    
    @property
    def price(self):
        return self._price
    
    @property
    def id(self):
        """Read-only property"""
        return self._id

product = Product("Laptop", 999.99)
print(product.id)  # Unique ID
# product.id = 123  # AttributeError: can't set attribute
```

---

## 6. Practice Questions

**Question 1**: Create a `Rectangle` class with width and height properties that validate positive values.
**Question 2**: Create a `BankAccount` class with a balance property that prevents negative balances.
**Question 3**: Create a `Person` class with age property that validates age range (0-150).

---

## 🎯 Key Takeaways

1. `@property` creates getter methods
2. `@property_name.setter` creates setter methods
3. `@property_name.deleter` creates deleter methods
4. Properties provide controlled attribute access
5. Use properties for validation and computed attributes

---

**Next Topic**: Decorators
**Previous Topic**: Polymorphism

Happy Coding! 🐍
