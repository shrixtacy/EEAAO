# Python Inheritance - Complete Guide

## 📚 Table of Contents
1. [Introduction to Inheritance](#introduction-to-inheritance)
2. [Single Inheritance](#single-inheritance)
3. [Method Overriding](#method-overriding)
4. [The super() Function](#the-super-function)
5. [isinstance() and issubclass()](#isinstance-and-issubclass)
6. [Practical Examples](#practical-examples)
7. [Practice Questions](#practice-questions)

---

## 1. Introduction to Inheritance

**Inheritance** is one of the four fundamental pillars of Object-Oriented Programming (along with Encapsulation, Polymorphism, and Abstraction). It allows a class to inherit attributes and methods from another class, creating a parent-child relationship between classes.

Think of inheritance like family genetics - just as children inherit traits from their parents, child classes inherit properties and behaviors from parent classes. This powerful concept allows you to build upon existing code without rewriting it, promoting code reuse and creating logical hierarchies in your programs.

In Python, inheritance enables you to create a new class that is based on an existing class. The new class (child/derived/subclass) automatically gets all the attributes and methods of the existing class (parent/base/superclass), and you can then add new features or modify existing ones. This is particularly useful when you have classes that share common functionality but also have their own unique characteristics.

For example, if you're building a game with different types of characters (warriors, mages, archers), they all share common attributes like health and name, but each has unique abilities. Instead of duplicating the common code in each class, you create a base Character class with the shared functionality, then create specialized classes that inherit from it.

### Key Terms
- **Parent Class** (Base/Super class): The class being inherited from - contains the common functionality
- **Child Class** (Derived/Sub class): The class that inherits - extends or modifies the parent's functionality
- **Inheritance Hierarchy**: The tree-like structure of parent-child relationships between classes
- **Method Overriding**: When a child class provides its own implementation of a parent's method

### Why Use Inheritance?
- **Code Reusability**: Write common code once in the parent class, use it in all child classes
- **Extensibility**: Add new features to existing code without modifying the original
- **Hierarchy**: Model real-world "is-a" relationships (a Dog "is-a" Animal)
- **Polymorphism**: Use child classes interchangeably through their common parent interface
- **Maintainability**: Changes to common functionality only need to be made in one place

---

## 2. Single Inheritance

Single inheritance is the simplest form of inheritance where a child class inherits from exactly one parent class. This creates a straightforward parent-child relationship and is the most common type of inheritance you'll use in Python programming.

When a class inherits from another, it automatically receives all the attributes and methods defined in the parent class. This means you don't have to rewrite code that's already been implemented - you simply inherit it. The child class can then use these inherited features as if they were defined in the child class itself.

The beauty of single inheritance is that it allows you to specialize behavior. You start with a general parent class that defines common functionality, then create child classes that inherit this functionality and add their own specific features. For instance, an Animal class might define basic behaviors like eating and sleeping, while Dog and Cat child classes inherit these behaviors and add their own unique methods like bark() or meow().

Python makes inheritance incredibly simple with its clean syntax. You simply put the parent class name in parentheses after the child class name in the class definition. From that point on, the child class has access to everything the parent class provides, and you can extend or modify this functionality as needed.

### Basic Inheritance
```python
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"
    
    def info(self):
        return f"I am {self.name}"

# Child class
class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

# Create objects
dog = Dog("Buddy")
cat = Cat("Whiskers")

print(dog.info())   # I am Buddy (inherited)
print(dog.speak())  # Woof! (overridden)
print(cat.speak())  # Meow! (overridden)
```

### Extending Parent Class
```python
class Vehicle:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def info(self):
        return f"{self.brand} {self.model}"

class Car(Vehicle):
    def __init__(self, brand, model, doors):
        super().__init__(brand, model)
        self.doors = doors
    
    def info(self):
        return f"{super().info()} with {self.doors} doors"

car = Car("Toyota", "Camry", 4)
print(car.info())  # Toyota Camry with 4 doors
```

---

## 3. Method Overriding

Child classes can override parent methods.

### Basic Override
```python
class Shape:
    def area(self):
        return 0

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

rect = Rectangle(5, 10)
circle = Circle(7)

print(rect.area())    # 50
print(circle.area())  # 153.93804
```

---

## 4. The super() Function

`super()` calls methods from the parent class.

### Using super() in __init__
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def introduce(self):
        return f"Hi, I'm {self.name}, {self.age} years old"

class Student(Person):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)
        self.student_id = student_id
    
    def introduce(self):
        return f"{super().introduce()}. Student ID: {self.student_id}"

student = Student("Alice", 20, "S12345")
print(student.introduce())
# Hi, I'm Alice, 20 years old. Student ID: S12345
```

---

## 5. isinstance() and issubclass()

### isinstance()
```python
class Animal:
    pass

class Dog(Animal):
    pass

dog = Dog()

print(isinstance(dog, Dog))     # True
print(isinstance(dog, Animal))  # True
print(isinstance(dog, str))     # False
```

### issubclass()
```python
print(issubclass(Dog, Animal))  # True
print(issubclass(Animal, Dog))  # False
print(issubclass(Dog, Dog))     # True
```

---

## 6. Practical Examples

### Example 1: Employee System
```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
    
    def get_details(self):
        return f"{self.name}: ${self.salary}"

class Manager(Employee):
    def __init__(self, name, salary, department):
        super().__init__(name, salary)
        self.department = department
    
    def get_details(self):
        return f"{super().get_details()}, Dept: {self.department}"

class Developer(Employee):
    def __init__(self, name, salary, language):
        super().__init__(name, salary)
        self.language = language
    
    def get_details(self):
        return f"{super().get_details()}, Language: {self.language}"

manager = Manager("Alice", 80000, "Sales")
developer = Developer("Bob", 75000, "Python")

print(manager.get_details())
print(developer.get_details())
```

---

## 7. Practice Questions

### Beginner Level

**Question 1**: Create a `Vehicle` parent class and `Car`, `Bike` child classes.
```python
# Your code here
```

**Question 2**: Create a `Shape` parent class with `Rectangle` and `Circle` children. Override area() method.
```python
# Your code here
```

**Question 3**: Create an `Animal` class with `Dog` and `Cat` children that override the speak() method.
```python
# Your code here
```

**Question 4**: Create a `Person` class and a `Student` child class that adds a student_id attribute.
```python
# Your code here
```

**Question 5**: Use isinstance() to check if objects are instances of parent or child classes.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **Inheritance** promotes code reuse
2. **Child classes** inherit from parent classes
3. **Method overriding** allows customization
4. **super()** calls parent class methods
5. **isinstance()** checks object type
6. **issubclass()** checks class relationships

---

**Next Topic**: Multiple Inheritance
**Previous Topic**: Class Variables and Methods

Happy Coding! 🐍
