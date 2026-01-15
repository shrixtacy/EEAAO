# Python Polymorphism - Complete Guide

## 📚 Table of Contents
1. [Introduction to Polymorphism](#introduction-to-polymorphism)
2. [Method Overriding](#method-overriding)
3. [Duck Typing](#duck-typing)
4. [Operator Overloading](#operator-overloading)
5. [Abstract Base Classes](#abstract-base-classes)
6. [Practice Questions](#practice-questions)

---

## 1. Introduction to Polymorphism

**Polymorphism** means "many forms" - same interface, different implementations.

```python
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

class Bird:
    def speak(self):
        return "Tweet!"

def animal_sound(animal):
    print(animal.speak())

dog = Dog()
cat = Cat()
bird = Bird()

animal_sound(dog)   # Woof!
animal_sound(cat)   # Meow!
animal_sound(bird)  # Tweet!
```

---

## 2. Method Overriding

```python
class Shape:
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2

shapes = [Rectangle(5, 10), Circle(7), Rectangle(3, 4)]

for shape in shapes:
    print(f"Area: {shape.area()}")
```

---

## 3. Duck Typing

"If it walks like a duck and quacks like a duck, it's a duck."

```python
class Duck:
    def swim(self):
        return "Duck swimming"

class Fish:
    def swim(self):
        return "Fish swimming"

class Airplane:
    def fly(self):
        return "Airplane flying"

def make_it_swim(animal):
    print(animal.swim())

duck = Duck()
fish = Fish()

make_it_swim(duck)  # Duck swimming
make_it_swim(fish)  # Fish swimming
# make_it_swim(Airplane())  # AttributeError
```

---

## 4. Operator Overloading

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)
    
    def __str__(self):
        return f"({self.x}, {self.y})"

p1 = Point(1, 2)
p2 = Point(3, 4)
p3 = p1 + p2  # Calls __add__

print(p3)  # (4, 6)
```

### Common Magic Methods
```python
class Number:
    def __init__(self, value):
        self.value = value
    
    def __add__(self, other):
        return Number(self.value + other.value)
    
    def __sub__(self, other):
        return Number(self.value - other.value)
    
    def __mul__(self, other):
        return Number(self.value * other.value)
    
    def __eq__(self, other):
        return self.value == other.value
    
    def __lt__(self, other):
        return self.value < other.value
    
    def __str__(self):
        return str(self.value)

n1 = Number(10)
n2 = Number(5)

print(n1 + n2)  # 15
print(n1 - n2)  # 5
print(n1 * n2)  # 50
print(n1 == n2) # False
print(n1 < n2)  # False
```

---

## 5. Abstract Base Classes

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass
    
    @abstractmethod
    def move(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"
    
    def move(self):
        return "Running"

# animal = Animal()  # TypeError: Can't instantiate abstract class
dog = Dog()
print(dog.speak())  # Woof!
print(dog.move())   # Running
```

---

## 6. Practice Questions

**Question 1**: Create a polymorphic function that works with different shape classes.
**Question 2**: Implement operator overloading for a Vector class.
**Question 3**: Create an abstract base class for a payment system.

---

## 🎯 Key Takeaways

1. Polymorphism allows same interface, different implementations
2. Method overriding enables polymorphism
3. Duck typing focuses on behavior, not type
4. Operator overloading customizes operators
5. Abstract base classes define interfaces

---

**Next Topic**: Property Decorators
**Previous Topic**: Multiple Inheritance

Happy Coding! 🐍
