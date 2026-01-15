# Python Object-Oriented Programming - Classes

## 📚 Table of Contents
1. [Introduction to OOP](#introduction-to-oop)
2. [Creating Classes](#creating-classes)
3. [The __init__ Constructor](#the-init-constructor)
4. [Instance Variables](#instance-variables)
5. [Instance Methods](#instance-methods)
6. [The self Parameter](#the-self-parameter)
7. [Creating Objects](#creating-objects)
8. [String Representation](#string-representation)
9. [Practical Examples](#practical-examples)
10. [Practice Questions](#practice-questions)

---

## 1. Introduction to OOP

**Object-Oriented Programming (OOP)** is a programming paradigm that organizes code around objects and classes.

### Key Concepts
- **Class**: Blueprint for creating objects
- **Object**: Instance of a class
- **Attributes**: Variables that belong to an object
- **Methods**: Functions that belong to an object

### Why Use OOP?
- **Encapsulation**: Bundle data and methods together
- **Reusability**: Create multiple objects from one class
- **Organization**: Structure code logically
- **Maintainability**: Easier to update and debug

---

## 2. Creating Classes

### Basic Class Syntax
```python
class Dog:
    pass  # Empty class

# Create an object
my_dog = Dog()
print(my_dog)  # <__main__.Dog object at 0x...>
```

### Class with Attributes
```python
class Car:
    # Class attribute (shared by all instances)
    wheels = 4
    
    pass

car1 = Car()
car2 = Car()

print(car1.wheels)  # 4
print(car2.wheels)  # 4
```

### Naming Conventions
```python
# Class names use PascalCase
class MyClass:
    pass

class StudentRecord:
    pass

class BankAccount:
    pass
```

---

## 3. The __init__ Constructor

The `__init__` method is called automatically when an object is created.

### Basic Constructor
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

# Create objects
person1 = Person("Alice", 25)
person2 = Person("Bob", 30)

print(person1.name)  # Alice
print(person2.age)   # 30
```

### Constructor with Default Values
```python
class Student:
    def __init__(self, name, grade="A", school="Unknown"):
        self.name = name
        self.grade = grade
        self.school = school

student1 = Student("Alice")
student2 = Student("Bob", "B")
student3 = Student("Charlie", "A", "MIT")

print(student1.name, student1.grade, student1.school)
# Alice A Unknown
```

### Constructor with Validation
```python
class BankAccount:
    def __init__(self, account_number, balance=0):
        if balance < 0:
            raise ValueError("Balance cannot be negative")
        
        self.account_number = account_number
        self.balance = balance

account = BankAccount("12345", 1000)
# account2 = BankAccount("67890", -500)  # ValueError
```

---

## 4. Instance Variables

Instance variables are unique to each object.

### Creating Instance Variables
```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width    # Instance variable
        self.height = height  # Instance variable

rect1 = Rectangle(10, 5)
rect2 = Rectangle(20, 15)

print(rect1.width)   # 10
print(rect2.width)   # 20
```

### Accessing Instance Variables
```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages

book = Book("1984", "George Orwell", 328)

# Access attributes
print(book.title)   # 1984
print(book.author)  # George Orwell
print(book.pages)   # 328

# Modify attributes
book.pages = 350
print(book.pages)   # 350
```

### Adding Attributes After Creation
```python
class Person:
    def __init__(self, name):
        self.name = name

person = Person("Alice")
person.age = 25  # Add new attribute
person.city = "New York"

print(person.name)  # Alice
print(person.age)   # 25
print(person.city)  # New York
```

---

## 5. Instance Methods

Instance methods are functions that belong to a class.

### Basic Methods
```python
class Circle:
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def circumference(self):
        return 2 * 3.14159 * self.radius

circle = Circle(5)
print(f"Area: {circle.area()}")              # Area: 78.53975
print(f"Circumference: {circle.circumference()}")  # Circumference: 31.4159
```

### Methods with Parameters
```python
class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance
    
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            return f"Deposited ${amount}. New balance: ${self.balance}"
        return "Invalid amount"
    
    def withdraw(self, amount):
        if amount > self.balance:
            return "Insufficient funds"
        if amount > 0:
            self.balance -= amount
            return f"Withdrew ${amount}. New balance: ${self.balance}"
        return "Invalid amount"

account = BankAccount(1000)
print(account.deposit(500))   # Deposited $500. New balance: $1500
print(account.withdraw(200))  # Withdrew $200. New balance: $1300
```

### Methods Calling Other Methods
```python
class Calculator:
    def __init__(self, value=0):
        self.value = value
    
    def add(self, n):
        self.value += n
        return self
    
    def subtract(self, n):
        self.value -= n
        return self
    
    def multiply(self, n):
        self.value *= n
        return self
    
    def get_result(self):
        return self.value

calc = Calculator(10)
result = calc.add(5).multiply(2).subtract(10).get_result()
print(result)  # 20
```

---

## 6. The self Parameter

`self` refers to the current instance of the class.

### Understanding self
```python
class Counter:
    def __init__(self):
        self.count = 0  # self.count is instance variable
    
    def increment(self):
        self.count += 1  # Access instance variable with self
    
    def get_count(self):
        return self.count  # Return instance variable

counter1 = Counter()
counter2 = Counter()

counter1.increment()
counter1.increment()
counter2.increment()

print(counter1.get_count())  # 2
print(counter2.get_count())  # 1
```

### self is Automatic
```python
class Example:
    def __init__(self, value):
        self.value = value
    
    def show(self):
        print(f"Value: {self.value}")

obj = Example(42)

# These are equivalent:
obj.show()              # Value: 42
Example.show(obj)       # Value: 42 (explicit self)
```

### Why self is Needed
```python
class Person:
    def __init__(self, name):
        # Without self, this would be a local variable
        name = name  # Wrong!
        
        # With self, it's an instance variable
        self.name = name  # Correct!
    
    def greet(self):
        # self allows access to instance variables
        print(f"Hello, I'm {self.name}")

person = Person("Alice")
person.greet()  # Hello, I'm Alice
```

---

## 7. Creating Objects

### Multiple Objects
```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age
    
    def bark(self):
        return f"{self.name} says Woof!"
    
    def info(self):
        return f"{self.name} is a {self.age} year old {self.breed}"

# Create multiple dog objects
dog1 = Dog("Buddy", "Golden Retriever", 3)
dog2 = Dog("Max", "German Shepherd", 5)
dog3 = Dog("Luna", "Husky", 2)

print(dog1.bark())  # Buddy says Woof!
print(dog2.info())  # Max is a 5 year old German Shepherd
print(dog3.name)    # Luna
```

### Objects in Collections
```python
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade

# List of objects
students = [
    Student("Alice", 95),
    Student("Bob", 87),
    Student("Charlie", 92)
]

# Access objects in list
for student in students:
    print(f"{student.name}: {student.grade}")

# Dictionary of objects
student_dict = {
    "001": Student("Alice", 95),
    "002": Student("Bob", 87)
}

print(student_dict["001"].name)  # Alice
```

---

## 8. String Representation

### __str__ Method
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def __str__(self):
        return f"Person(name={self.name}, age={self.age})"

person = Person("Alice", 25)
print(person)  # Person(name=Alice, age=25)
```

### __repr__ Method
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __repr__(self):
        return f"Point({self.x}, {self.y})"
    
    def __str__(self):
        return f"({self.x}, {self.y})"

point = Point(3, 4)
print(point)        # (3, 4) - uses __str__
print(repr(point))  # Point(3, 4) - uses __repr__
```

---

## 9. Practical Examples

### Example 1: Shopping Cart
```python
class Product:
    def __init__(self, name, price):
        self.name = name
        self.price = price
    
    def __str__(self):
        return f"{self.name}: ${self.price:.2f}"

class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, product, quantity=1):
        self.items.append({"product": product, "quantity": quantity})
        print(f"Added {quantity}x {product.name}")
    
    def remove_item(self, product_name):
        self.items = [item for item in self.items 
                      if item["product"].name != product_name]
        print(f"Removed {product_name}")
    
    def get_total(self):
        total = sum(item["product"].price * item["quantity"] 
                   for item in self.items)
        return total
    
    def show_cart(self):
        if not self.items:
            print("Cart is empty")
            return
        
        print("\n=== Shopping Cart ===")
        for item in self.items:
            product = item["product"]
            quantity = item["quantity"]
            subtotal = product.price * quantity
            print(f"{product.name} x{quantity}: ${subtotal:.2f}")
        print(f"Total: ${self.get_total():.2f}")

# Usage
cart = ShoppingCart()
cart.add_item(Product("Apple", 0.99), 5)
cart.add_item(Product("Bread", 2.50), 2)
cart.add_item(Product("Milk", 3.99), 1)
cart.show_cart()
```

### Example 2: Library System
```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_borrowed = False
    
    def borrow(self):
        if self.is_borrowed:
            return f"'{self.title}' is already borrowed"
        self.is_borrowed = True
        return f"You borrowed '{self.title}'"
    
    def return_book(self):
        if not self.is_borrowed:
            return f"'{self.title}' was not borrowed"
        self.is_borrowed = False
        return f"You returned '{self.title}'"
    
    def __str__(self):
        status = "Borrowed" if self.is_borrowed else "Available"
        return f"{self.title} by {self.author} [{status}]"

class Library:
    def __init__(self, name):
        self.name = name
        self.books = []
    
    def add_book(self, book):
        self.books.append(book)
        print(f"Added '{book.title}' to {self.name}")
    
    def find_book(self, title):
        for book in self.books:
            if book.title.lower() == title.lower():
                return book
        return None
    
    def show_available_books(self):
        available = [book for book in self.books if not book.is_borrowed]
        if not available:
            print("No books available")
            return
        
        print(f"\n=== Available Books at {self.name} ===")
        for book in available:
            print(f"- {book}")

# Usage
library = Library("City Library")
library.add_book(Book("1984", "George Orwell", "123"))
library.add_book(Book("To Kill a Mockingbird", "Harper Lee", "456"))
library.add_book(Book("The Great Gatsby", "F. Scott Fitzgerald", "789"))

library.show_available_books()

book = library.find_book("1984")
if book:
    print(book.borrow())

library.show_available_books()
```

### Example 3: Temperature Converter
```python
class Temperature:
    def __init__(self, celsius=0):
        self.celsius = celsius
    
    def to_fahrenheit(self):
        return (self.celsius * 9/5) + 32
    
    def to_kelvin(self):
        return self.celsius + 273.15
    
    def set_fahrenheit(self, fahrenheit):
        self.celsius = (fahrenheit - 32) * 5/9
    
    def set_kelvin(self, kelvin):
        self.celsius = kelvin - 273.15
    
    def __str__(self):
        return f"{self.celsius}°C = {self.to_fahrenheit()}°F = {self.to_kelvin()}K"

# Usage
temp = Temperature(25)
print(temp)  # 25°C = 77.0°F = 298.15K

temp.set_fahrenheit(98.6)
print(f"Body temperature: {temp}")
```

### Example 4: Todo List
```python
class Task:
    def __init__(self, description, priority="Medium"):
        self.description = description
        self.priority = priority
        self.completed = False
    
    def mark_complete(self):
        self.completed = True
    
    def mark_incomplete(self):
        self.completed = False
    
    def __str__(self):
        status = "✓" if self.completed else "✗"
        return f"[{status}] {self.description} (Priority: {self.priority})"

class TodoList:
    def __init__(self, name):
        self.name = name
        self.tasks = []
    
    def add_task(self, task):
        self.tasks.append(task)
        print(f"Added: {task.description}")
    
    def remove_task(self, index):
        if 0 <= index < len(self.tasks):
            removed = self.tasks.pop(index)
            print(f"Removed: {removed.description}")
        else:
            print("Invalid task index")
    
    def complete_task(self, index):
        if 0 <= index < len(self.tasks):
            self.tasks[index].mark_complete()
            print(f"Completed: {self.tasks[index].description}")
        else:
            print("Invalid task index")
    
    def show_tasks(self):
        if not self.tasks:
            print("No tasks in the list")
            return
        
        print(f"\n=== {self.name} ===")
        for i, task in enumerate(self.tasks):
            print(f"{i + 1}. {task}")
    
    def show_pending_tasks(self):
        pending = [task for task in self.tasks if not task.completed]
        if not pending:
            print("No pending tasks!")
            return
        
        print(f"\n=== Pending Tasks ===")
        for task in pending:
            print(f"- {task}")

# Usage
todo = TodoList("My Tasks")
todo.add_task(Task("Buy groceries", "High"))
todo.add_task(Task("Call dentist", "Medium"))
todo.add_task(Task("Read book", "Low"))

todo.show_tasks()
todo.complete_task(0)
todo.show_tasks()
todo.show_pending_tasks()
```

---

## 10. Practice Questions

### Beginner Level

**Question 1**: Create a `Car` class with attributes for make, model, and year. Add a method to display car information.
```python
# Your code here
```

**Question 2**: Create a `Rectangle` class with methods to calculate area and perimeter.
```python
# Your code here
```

**Question 3**: Create a `Student` class with name and grades list. Add a method to calculate average grade.
```python
# Your code here
```

**Question 4**: Create a `Circle` class with radius. Add methods for area, circumference, and diameter.
```python
# Your code here
```

**Question 5**: Create a `BankAccount` class with deposit and withdraw methods. Track the balance.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Create a `Contact` class for a phone book. Include name, phone, and email. Create a `PhoneBook` class to manage multiple contacts.
```python
# Your code here
```

**Question 7**: Create a `Movie` class with title, director, and rating. Create a `MovieCollection` class to manage movies.
```python
# Your code here
```

**Question 8**: Create a `Player` class for a game with health, attack, and defend methods.
```python
# Your code here
```

**Question 9**: Create an `Employee` class with salary calculation based on hours worked and hourly rate.
```python
# Your code here
```

**Question 10**: Create a `Playlist` class that manages a list of `Song` objects.
```python
# Your code here
```

### Advanced Level

**Question 11**: Create a `Deck` class for playing cards with shuffle, deal, and draw methods.
```python
# Your code here
```

**Question 12**: Create a `Restaurant` class with `MenuItem` and `Order` classes for a restaurant ordering system.
```python
# Your code here
```

**Question 13**: Create a `Inventory` class for a store with add, remove, search, and restock methods.
```python
# Your code here
```

**Question 14**: Create a `Calendar` class with `Event` objects that can schedule and manage events.
```python
# Your code here
```

**Question 15**: Create a `SocialMedia` class with `User` and `Post` classes for a simple social network.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **Classes** are blueprints for creating objects
2. **__init__** is the constructor method
3. **self** refers to the current instance
4. **Instance variables** are unique to each object
5. **Instance methods** are functions that belong to objects
6. **__str__** controls how objects are printed
7. Use **PascalCase** for class names
8. Objects can be stored in lists and dictionaries
9. Methods can call other methods using **self**
10. Validate data in **__init__** when needed

---

## 📚 Additional Resources

- Python Classes Tutorial: https://docs.python.org/3/tutorial/classes.html
- OOP Concepts: https://realpython.com/python3-object-oriented-programming/
- Python Data Model: https://docs.python.org/3/reference/datamodel.html

---

**Next Topic**: Class Variables and Methods
**Previous Topic**: Scope Resolution

Happy Coding! 🐍
