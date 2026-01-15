# Python Multiple Inheritance - Complete Guide

## 📚 Table of Contents
1. [Introduction](#introduction)
2. [Multiple Inheritance Syntax](#multiple-inheritance-syntax)
3. [Method Resolution Order (MRO)](#method-resolution-order-mro)
4. [Diamond Problem](#diamond-problem)
5. [Mixins](#mixins)
6. [Practice Questions](#practice-questions)

---

## 1. Introduction

**Multiple Inheritance** is a powerful but complex feature in Python that allows a class to inherit from more than one parent class simultaneously. Unlike single inheritance where a class has one parent, multiple inheritance enables a class to combine features from multiple parent classes, creating a more flexible but potentially complicated class hierarchy.

Think of multiple inheritance like a child inheriting traits from both parents - they get characteristics from both sides of the family. In programming terms, this means a class can access methods and attributes from all of its parent classes, combining their functionality into a single, more capable class.

While multiple inheritance provides great flexibility, it also introduces complexity. The main challenge is determining which parent's method should be called when multiple parents have methods with the same name. Python solves this with a well-defined Method Resolution Order (MRO) that determines the order in which parent classes are searched for methods and attributes.

Multiple inheritance is particularly useful when you want to create classes that combine distinct, independent functionalities. For example, a Duck class might inherit from both a Flyer class (for flying behavior) and a Swimmer class (for swimming behavior), combining both capabilities without duplicating code. However, it should be used judiciously - sometimes composition (having objects as attributes) is a better design choice than multiple inheritance.

### When to Use Multiple Inheritance
- When a class genuinely needs functionality from multiple unrelated sources
- When creating mixin classes that add specific capabilities
- When modeling real-world objects that have multiple distinct characteristics
- When you want to avoid code duplication across different class hierarchies

### When to Avoid Multiple Inheritance
- When the relationship can be modeled with single inheritance
- When it makes the code harder to understand and maintain
- When composition (has-a relationship) would be clearer than inheritance (is-a relationship)
- When it creates the diamond problem (discussed later) without a clear resolution

```python
class Parent1:
    def method1(self):
        return "From Parent1"

class Parent2:
    def method2(self):
        return "From Parent2"

class Child(Parent1, Parent2):
    pass

child = Child()
print(child.method1())  # From Parent1
print(child.method2())  # From Parent2
```

---

## 2. Multiple Inheritance Syntax

```python
class Flyer:
    def fly(self):
        return "Flying"

class Swimmer:
    def swim(self):
        return "Swimming"

class Duck(Flyer, Swimmer):
    def quack(self):
        return "Quack!"

duck = Duck()
print(duck.fly())    # Flying
print(duck.swim())   # Swimming
print(duck.quack())  # Quack!
```

---

## 3. Method Resolution Order (MRO)

Python uses **C3 Linearization** (also called C3 superclass linearization) to determine the Method Resolution Order - the order in which Python searches for methods and attributes in a class hierarchy. This is crucial in multiple inheritance because it determines which parent's method gets called when multiple parents have methods with the same name.

The MRO ensures that:
1. **Children are checked before parents** - A child class's methods take precedence over parent methods
2. **Parents are checked in the order they're listed** - If you write `class Child(Parent1, Parent2)`, Parent1 is checked before Parent2
3. **Each class appears only once** - No class is checked multiple times in the hierarchy
4. **The order is consistent** - The same order is maintained throughout the hierarchy

You can view a class's MRO using the `mro()` method or the `__mro__` attribute. This shows you exactly the order Python will search for methods. Understanding MRO is essential for debugging multiple inheritance issues and predicting which method will be called.

The C3 algorithm is sophisticated enough to handle complex inheritance hierarchies while maintaining a sensible, predictable order. It prevents ambiguities and ensures that the inheritance structure makes logical sense. If you try to create an inheritance structure that violates the C3 rules, Python will raise an error rather than allowing an inconsistent hierarchy.

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):
    pass

d = D()
print(d.method())  # B (follows MRO: D -> B -> C -> A)
print(D.mro())     # View MRO
```

---

## 4. Diamond Problem

```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):
    pass

# D inherits from both B and C, which both inherit from A
# Python's MRO resolves this: D -> B -> C -> A
d = D()
print(d.method())  # B
```

---

## 5. Mixins

Mixins are classes that provide specific functionality to be mixed into other classes.

```python
class JSONMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)

class Person(JSONMixin):
    def __init__(self, name, age):
        self.name = name
        self.age = age

person = Person("Alice", 25)
print(person.to_json())  # {"name": "Alice", "age": 25}
```

---

## 6. Practice Questions

**Question 1**: Create a class that inherits from multiple parent classes.
**Question 2**: Demonstrate the diamond problem and its resolution.
**Question 3**: Create a mixin class for logging functionality.

---

## 🎯 Key Takeaways

1. Multiple inheritance allows inheriting from multiple classes
2. MRO determines method lookup order
3. Diamond problem is resolved by Python's MRO
4. Mixins provide reusable functionality

---

**Next Topic**: Polymorphism
**Previous Topic**: Inheritance

Happy Coding! 🐍
