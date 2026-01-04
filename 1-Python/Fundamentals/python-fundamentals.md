# Python Fundamentals

Master Python basics through practical examples and real-world applications. This isn't about memorizing syntax - it's about building problem-solving skills with Python as your tool.

## Learning Approach

**Build, Don't Memorize**: Every concept includes practical examples
**Debug-Driven Learning**: Learn to fix problems, not just write perfect code
**Real-World Context**: Connect every concept to actual use cases

## Phase 1: Python Basics

### Variables and Data Types

**What You're Learning**: How to store and manipulate different types of information

```python
# Variables - containers for data
name = "Alice"           # String (text)
age = 25                 # Integer (whole number)
height = 5.6             # Float (decimal number)
is_student = True        # Boolean (True/False)

# Why this matters: Every program needs to store and work with data
print(f"{name} is {age} years old and {height} feet tall")
```

**Practical Exercise**: Create a personal information tracker
```python
# Store information about yourself
first_name = "Your Name"
last_name = "Your Last Name"
birth_year = 2000
current_year = 2024
age = current_year - birth_year

print(f"Hello, {first_name} {last_name}!")
print(f"You are {age} years old.")
```

**Real-World Application**: Configuration files, user data, system information

### Working with Strings

**What You're Learning**: Text manipulation - the foundation of most programming tasks

```python
# String basics
message = "Hello, World!"
email = "user@example.com"

# String methods (functions that work on strings)
print(message.upper())           # HELLO, WORLD!
print(message.lower())           # hello, world!
print(message.replace("World", "Python"))  # Hello, Python!

# String formatting - building dynamic messages
name = "Alice"
score = 95
result = f"{name} scored {score}% on the test"
print(result)  # Alice scored 95% on the test
```

**Practical Exercise**: Email processor
```python
email = "john.doe@company.com"

# Extract username and domain
username = email.split("@")[0]
domain = email.split("@")[1]

print(f"Username: {username}")
print(f"Domain: {domain}")
print(f"Display name: {username.replace('.', ' ').title()}")
```

**Real-World Application**: Data cleaning, file processing, user interface text

### Lists - Working with Collections

**What You're Learning**: Managing multiple pieces of related data

```python
# Creating and using lists
fruits = ["apple", "banana", "orange"]
numbers = [1, 2, 3, 4, 5]
mixed = ["Alice", 25, True, 3.14]

# Adding items
fruits.append("grape")          # Add to end
fruits.insert(0, "strawberry") # Add at position

# Accessing items
first_fruit = fruits[0]         # First item
last_fruit = fruits[-1]        # Last item

# List operations
print(f"We have {len(fruits)} fruits")
print("banana" in fruits)      # Check if item exists
```

**Practical Exercise**: Shopping list manager
```python
shopping_list = []

# Add items
shopping_list.append("milk")
shopping_list.append("bread")
shopping_list.append("eggs")

# Display list
print("Shopping List:")
for i, item in enumerate(shopping_list, 1):
    print(f"{i}. {item}")

# Remove completed items
shopping_list.remove("milk")
print(f"Items remaining: {len(shopping_list)}")
```

**Real-World Application**: User data, file lists, configuration options

### Dictionaries - Key-Value Storage

**What You're Learning**: Organizing data with labels (like a real dictionary)

```python
# Creating dictionaries
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York",
    "is_student": True
}

# Accessing values
print(person["name"])           # Alice
print(person.get("age"))        # 25
print(person.get("country", "Unknown"))  # Unknown (default value)

# Adding/updating values
person["email"] = "alice@email.com"
person["age"] = 26

# Working with keys and values
for key, value in person.items():
    print(f"{key}: {value}")
```

**Practical Exercise**: Contact manager
```python
contacts = {
    "alice": {"phone": "123-456-7890", "email": "alice@email.com"},
    "bob": {"phone": "098-765-4321", "email": "bob@email.com"}
}

# Add new contact
contacts["charlie"] = {"phone": "555-123-4567", "email": "charlie@email.com"}

# Look up contact
name = "alice"
if name in contacts:
    print(f"{name.title()}'s phone: {contacts[name]['phone']}")
```

**Real-World Application**: Configuration files, database records, API responses

### Control Structures - Making Decisions

**What You're Learning**: How to make your programs smart and responsive

```python
# If statements - making decisions
age = 18

if age >= 18:
    print("You can vote!")
elif age >= 16:
    print("You can drive!")
else:
    print("You're still young!")

# Practical example: Grade calculator
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Score: {score}, Grade: {grade}")
```

**Practical Exercise**: Password strength checker
```python
password = input("Enter a password: ")

strength = 0
feedback = []

if len(password) >= 8:
    strength += 1
else:
    feedback.append("Use at least 8 characters")

if any(c.isupper() for c in password):
    strength += 1
else:
    feedback.append("Include uppercase letters")

if any(c.isdigit() for c in password):
    strength += 1
else:
    feedback.append("Include numbers")

if strength == 3:
    print("Strong password!")
else:
    print("Weak password. Suggestions:")
    for suggestion in feedback:
        print(f"- {suggestion}")
```

### Loops - Repeating Actions

**What You're Learning**: Automating repetitive tasks

```python
# For loops - when you know how many times
fruits = ["apple", "banana", "orange"]

for fruit in fruits:
    print(f"I like {fruit}")

# Range for numbers
for i in range(5):          # 0, 1, 2, 3, 4
    print(f"Count: {i}")

for i in range(1, 6):       # 1, 2, 3, 4, 5
    print(f"Number: {i}")

# While loops - when you don't know how many times
count = 0
while count < 3:
    print(f"Loop iteration: {count}")
    count += 1
```

**Practical Exercise**: File organizer
```python
files = ["document.pdf", "image.jpg", "script.py", "data.csv", "photo.png"]

# Organize by file type
file_types = {}

for file in files:
    extension = file.split(".")[-1]
    
    if extension not in file_types:
        file_types[extension] = []
    
    file_types[extension].append(file)

# Display organization
for file_type, file_list in file_types.items():
    print(f"{file_type.upper()} files:")
    for file in file_list:
        print(f"  - {file}")
```

**Real-World Application**: Data processing, file operations, user interfaces

## Phase 2: Intermediate Concepts

### Functions - Reusable Code Blocks

**What You're Learning**: Breaking problems into smaller, manageable pieces

```python
# Basic function
def greet(name):
    return f"Hello, {name}!"

# Function with multiple parameters
def calculate_area(length, width):
    area = length * width
    return area

# Function with default parameters
def create_email(username, domain="gmail.com"):
    return f"{username}@{domain}"

# Using functions
message = greet("Alice")
room_area = calculate_area(10, 12)
email = create_email("john")
work_email = create_email("john", "company.com")
```

**Practical Exercise**: Temperature converter
```python
def celsius_to_fahrenheit(celsius):
    """Convert Celsius to Fahrenheit"""
    fahrenheit = (celsius * 9/5) + 32
    return fahrenheit

def fahrenheit_to_celsius(fahrenheit):
    """Convert Fahrenheit to Celsius"""
    celsius = (fahrenheit - 32) * 5/9
    return celsius

def temperature_converter():
    """Interactive temperature converter"""
    print("Temperature Converter")
    print("1. Celsius to Fahrenheit")
    print("2. Fahrenheit to Celsius")
    
    choice = input("Choose conversion (1 or 2): ")
    temp = float(input("Enter temperature: "))
    
    if choice == "1":
        result = celsius_to_fahrenheit(temp)
        print(f"{temp}°C = {result:.1f}°F")
    elif choice == "2":
        result = fahrenheit_to_celsius(temp)
        print(f"{temp}°F = {result:.1f}°C")
    else:
        print("Invalid choice")

# Run the converter
temperature_converter()
```

### Working with Files

**What You're Learning**: Reading from and writing to files - essential for data processing

```python
# Writing to files
def save_shopping_list(items, filename):
    with open(filename, 'w') as file:
        for item in items:
            file.write(f"{item}\n")

# Reading from files
def load_shopping_list(filename):
    items = []
    try:
        with open(filename, 'r') as file:
            for line in file:
                items.append(line.strip())
    except FileNotFoundError:
        print(f"File {filename} not found")
    return items

# Using file functions
shopping_items = ["milk", "bread", "eggs", "apples"]
save_shopping_list(shopping_items, "shopping.txt")
loaded_items = load_shopping_list("shopping.txt")
print(f"Loaded items: {loaded_items}")
```

**Practical Exercise**: Simple log analyzer
```python
def analyze_log_file(filename):
    """Analyze a log file and count different log levels"""
    log_counts = {"INFO": 0, "WARNING": 0, "ERROR": 0}
    
    try:
        with open(filename, 'r') as file:
            for line in file:
                line = line.strip()
                if "INFO" in line:
                    log_counts["INFO"] += 1
                elif "WARNING" in line:
                    log_counts["WARNING"] += 1
                elif "ERROR" in line:
                    log_counts["ERROR"] += 1
        
        print("Log Analysis:")
        for level, count in log_counts.items():
            print(f"{level}: {count}")
            
    except FileNotFoundError:
        print(f"Log file {filename} not found")

# Create sample log file for testing
sample_logs = [
    "2024-01-01 10:00:00 INFO Application started",
    "2024-01-01 10:01:00 INFO User logged in",
    "2024-01-01 10:02:00 WARNING Low disk space",
    "2024-01-01 10:03:00 ERROR Database connection failed",
    "2024-01-01 10:04:00 INFO User logged out"
]

with open("app.log", "w") as f:
    for log in sample_logs:
        f.write(f"{log}\n")

analyze_log_file("app.log")
```

### Error Handling

**What You're Learning**: Dealing with problems gracefully instead of crashing

```python
# Basic error handling
def safe_divide(a, b):
    try:
        result = a / b
        return result
    except ZeroDivisionError:
        print("Error: Cannot divide by zero")
        return None
    except TypeError:
        print("Error: Please provide numbers")
        return None

# Multiple error types
def process_user_input():
    try:
        age = int(input("Enter your age: "))
        if age < 0:
            raise ValueError("Age cannot be negative")
        print(f"You are {age} years old")
    except ValueError as e:
        print(f"Invalid input: {e}")
    except KeyboardInterrupt:
        print("\nOperation cancelled by user")
```

**Practical Exercise**: Robust file processor
```python
def process_data_file(filename):
    """Process a data file with proper error handling"""
    try:
        with open(filename, 'r') as file:
            lines = file.readlines()
            
        processed_data = []
        for line_num, line in enumerate(lines, 1):
            try:
                # Assume each line should be a number
                number = float(line.strip())
                processed_data.append(number)
            except ValueError:
                print(f"Warning: Invalid data on line {line_num}: {line.strip()}")
                continue
        
        if processed_data:
            average = sum(processed_data) / len(processed_data)
            print(f"Processed {len(processed_data)} numbers")
            print(f"Average: {average:.2f}")
        else:
            print("No valid data found")
            
    except FileNotFoundError:
        print(f"Error: File '{filename}' not found")
    except PermissionError:
        print(f"Error: Permission denied to read '{filename}'")
    except Exception as e:
        print(f"Unexpected error: {e}")

# Create test file
with open("data.txt", "w") as f:
    f.write("10.5\n")
    f.write("20.3\n")
    f.write("invalid\n")
    f.write("15.7\n")

process_data_file("data.txt")
```

### Classes and Objects

**What You're Learning**: Organizing code and data together (object-oriented programming)

```python
# Basic class
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance
    
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            print(f"Deposited ${amount}. New balance: ${self.balance}")
        else:
            print("Deposit amount must be positive")
    
    def withdraw(self, amount):
        if amount > self.balance:
            print("Insufficient funds")
        elif amount > 0:
            self.balance -= amount
            print(f"Withdrew ${amount}. New balance: ${self.balance}")
        else:
            print("Withdrawal amount must be positive")
    
    def get_balance(self):
        return self.balance

# Using the class
account = BankAccount("Alice", 100)
account.deposit(50)
account.withdraw(30)
print(f"Final balance: ${account.get_balance()}")
```

**Practical Exercise**: Task manager class
```python
class TaskManager:
    def __init__(self):
        self.tasks = []
        self.next_id = 1
    
    def add_task(self, description):
        task = {
            "id": self.next_id,
            "description": description,
            "completed": False
        }
        self.tasks.append(task)
        self.next_id += 1
        print(f"Added task: {description}")
    
    def complete_task(self, task_id):
        for task in self.tasks:
            if task["id"] == task_id:
                task["completed"] = True
                print(f"Completed task: {task['description']}")
                return
        print(f"Task {task_id} not found")
    
    def list_tasks(self):
        if not self.tasks:
            print("No tasks")
            return
        
        print("Tasks:")
        for task in self.tasks:
            status = "✓" if task["completed"] else "○"
            print(f"{status} {task['id']}: {task['description']}")
    
    def get_pending_count(self):
        return sum(1 for task in self.tasks if not task["completed"])

# Using the task manager
tm = TaskManager()
tm.add_task("Learn Python basics")
tm.add_task("Build a project")
tm.add_task("Apply for jobs")
tm.list_tasks()
tm.complete_task(1)
tm.list_tasks()
print(f"Pending tasks: {tm.get_pending_count()}")
```

## Phase 3: Advanced Topics

### Modules and Packages

**What You're Learning**: Organizing code across multiple files and using external libraries

```python
# Creating a module (save as math_utils.py)
def calculate_compound_interest(principal, rate, time, compounds_per_year=1):
    """Calculate compound interest"""
    amount = principal * (1 + rate/compounds_per_year) ** (compounds_per_year * time)
    return amount

def calculate_simple_interest(principal, rate, time):
    """Calculate simple interest"""
    interest = principal * rate * time
    return interest

# Using the module (in another file)
import math_utils

# Or import specific functions
from math_utils import calculate_compound_interest

# Using external libraries
import requests
import json

def get_weather(city):
    """Get weather data for a city (requires API key)"""
    # This is a simplified example
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid=YOUR_API_KEY"
    try:
        response = requests.get(url)
        data = response.json()
        return data
    except requests.RequestException as e:
        print(f"Error fetching weather: {e}")
        return None
```

### List Comprehensions and Generators

**What You're Learning**: Writing more efficient and readable code

```python
# List comprehensions - creating lists efficiently
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]  # [1, 4, 9, 16, 25]

# With conditions
even_squares = [x**2 for x in numbers if x % 2 == 0]  # [4, 16]

# Dictionary comprehensions
word_lengths = {"hello": 5, "world": 5, "python": 6}
reversed_dict = {length: word for word, length in word_lengths.items()}

# Generators - memory-efficient iteration
def fibonacci_generator(n):
    """Generate fibonacci numbers up to n"""
    a, b = 0, 1
    count = 0
    while count < n:
        yield a
        a, b = b, a + b
        count += 1

# Using generators
fib_gen = fibonacci_generator(10)
for num in fib_gen:
    print(num)
```

### Decorators and Context Managers

**What You're Learning**: Advanced Python features for cleaner, more maintainable code

```python
# Simple decorator
def timer(func):
    """Decorator to time function execution"""
    import time
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    import time
    time.sleep(1)
    return "Done"

# Context managers
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()

# Using context manager
with FileManager("test.txt", "w") as f:
    f.write("Hello, World!")
```

## Building Your First Real Project

**Project: Personal Finance Tracker**

This project combines everything you've learned:

```python
import json
from datetime import datetime

class FinanceTracker:
    def __init__(self, filename="finances.json"):
        self.filename = filename
        self.transactions = self.load_data()
    
    def load_data(self):
        try:
            with open(self.filename, 'r') as f:
                return json.load(f)
        except FileNotFoundError:
            return []
    
    def save_data(self):
        with open(self.filename, 'w') as f:
            json.dump(self.transactions, f, indent=2)
    
    def add_transaction(self, amount, category, description=""):
        transaction = {
            "date": datetime.now().isoformat(),
            "amount": amount,
            "category": category,
            "description": description
        }
        self.transactions.append(transaction)
        self.save_data()
        print(f"Added: ${amount} - {category}")
    
    def get_balance(self):
        return sum(t["amount"] for t in self.transactions)
    
    def get_spending_by_category(self):
        categories = {}
        for transaction in self.transactions:
            if transaction["amount"] < 0:  # Expenses
                category = transaction["category"]
                categories[category] = categories.get(category, 0) + abs(transaction["amount"])
        return categories
    
    def generate_report(self):
        balance = self.get_balance()
        spending = self.get_spending_by_category()
        
        print(f"\n=== Financial Report ===")
        print(f"Current Balance: ${balance:.2f}")
        print(f"\nSpending by Category:")
        for category, amount in spending.items():
            print(f"  {category}: ${amount:.2f}")

# Using the finance tracker
tracker = FinanceTracker()
tracker.add_transaction(1000, "salary", "Monthly salary")
tracker.add_transaction(-50, "groceries", "Weekly shopping")
tracker.add_transaction(-30, "entertainment", "Movie tickets")
tracker.add_transaction(-200, "rent", "Monthly rent")
tracker.generate_report()
```

## Next Steps

1. **Practice Daily**: Write small programs to solve real problems
2. **Build Projects**: Create applications that interest you
3. **Read Code**: Study well-written Python projects on GitHub
4. **Join Communities**: Python Discord, Reddit r/Python, local meetups
5. **Choose Your Specialization**: Pick a domain path and dive deeper

## Common Debugging Tips

**Read Error Messages**: They tell you exactly what's wrong
**Use Print Statements**: Add `print()` to see what your variables contain
**Check Indentation**: Python is picky about spaces and tabs
**Test Small Pieces**: Don't write 100 lines before testing
**Use the Python REPL**: Test code snippets interactively

Remember: Every expert was once a beginner. Focus on building things, not perfecting syntax. The best way to learn Python is to use it to solve problems you actually care about.