# Python Match-Case Statements - Complete Guide (Python 3.10+)

## 📚 Table of Contents
1. [Introduction to Match-Case](#introduction-to-match-case)
2. [Basic Pattern Matching](#basic-pattern-matching)
3. [Literal Patterns](#literal-patterns)
4. [Capture Patterns](#capture-patterns)
5. [Wildcard Pattern](#wildcard-pattern)
6. [OR Patterns](#or-patterns)
7. [Sequence Patterns](#sequence-patterns)
8. [Mapping Patterns](#mapping-patterns)
9. [Class Patterns](#class-patterns)
10. [Guard Clauses](#guard-clauses)
11. [Practical Examples](#practical-examples)
12. [Practice Questions](#practice-questions)

---

## 1. Introduction to Match-Case

The `match-case` statement (also called structural pattern matching) was introduced in Python 3.10. It's similar to switch-case statements in other languages but much more powerful.

### Why Use Match-Case?
- More readable than multiple if-elif-else statements
- Supports complex pattern matching
- Can destructure data structures
- Provides exhaustive checking

### Basic Syntax
```python
match subject:
    case pattern1:
        # code block
    case pattern2:
        # code block
    case _:
        # default case
```

---

## 2. Basic Pattern Matching

### Simple Value Matching
```python
def check_status(status):
    match status:
        case 200:
            return "OK"
        case 404:
            return "Not Found"
        case 500:
            return "Internal Server Error"
        case _:
            return "Unknown Status"

print(check_status(200))  # OK
print(check_status(404))  # Not Found
print(check_status(403))  # Unknown Status
```

### String Matching
```python
def process_command(command):
    match command:
        case "start":
            return "Starting the system..."
        case "stop":
            return "Stopping the system..."
        case "restart":
            return "Restarting the system..."
        case "status":
            return "System is running"
        case _:
            return "Unknown command"

print(process_command("start"))    # Starting the system...
print(process_command("pause"))    # Unknown command
```

### Boolean Matching
```python
def check_boolean(value):
    match value:
        case True:
            return "Value is True"
        case False:
            return "Value is False"
        case _:
            return "Not a boolean"

print(check_boolean(True))   # Value is True
print(check_boolean(False))  # Value is False
print(check_boolean(None))   # Not a boolean
```

---

## 3. Literal Patterns

Literal patterns match exact values.

### Number Literals
```python
def describe_number(n):
    match n:
        case 0:
            return "Zero"
        case 1:
            return "One"
        case 2:
            return "Two"
        case _:
            return f"Number: {n}"

print(describe_number(0))  # Zero
print(describe_number(5))  # Number: 5
```

### String Literals
```python
def get_day_type(day):
    match day:
        case "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday":
            return "Weekday"
        case "Saturday" | "Sunday":
            return "Weekend"
        case _:
            return "Invalid day"

print(get_day_type("Monday"))    # Weekday
print(get_day_type("Saturday"))  # Weekend
```

---

## 4. Capture Patterns

Capture patterns bind matched values to variables.

### Basic Capture
```python
def process_value(value):
    match value:
        case 0:
            return "Zero"
        case x:  # Captures any value
            return f"Got value: {x}"

print(process_value(0))    # Zero
print(process_value(42))   # Got value: 42
print(process_value("hi")) # Got value: hi
```

### Capture with Type Check
```python
def process_data(data):
    match data:
        case int(x):
            return f"Integer: {x}"
        case str(s):
            return f"String: {s}"
        case float(f):
            return f"Float: {f}"
        case _:
            return "Unknown type"

print(process_data(42))      # Integer: 42
print(process_data("hello")) # String: hello
print(process_data(3.14))    # Float: 3.14
```

---

## 5. Wildcard Pattern

The wildcard pattern `_` matches anything but doesn't bind the value.

### Default Case
```python
def categorize_age(age):
    match age:
        case n if n < 0:
            return "Invalid age"
        case n if 0 <= n < 13:
            return "Child"
        case n if 13 <= n < 20:
            return "Teenager"
        case n if 20 <= n < 60:
            return "Adult"
        case n if n >= 60:
            return "Senior"
        case _:
            return "Unknown"

print(categorize_age(10))  # Child
print(categorize_age(25))  # Adult
print(categorize_age(70))  # Senior
```

---

## 6. OR Patterns

OR patterns use `|` to match multiple patterns.

### Multiple Values
```python
def is_vowel(char):
    match char.lower():
        case 'a' | 'e' | 'i' | 'o' | 'u':
            return True
        case _:
            return False

print(is_vowel('a'))  # True
print(is_vowel('E'))  # True
print(is_vowel('b'))  # False
```

### Multiple Types
```python
def process_input(value):
    match value:
        case int() | float():
            return f"Number: {value}"
        case str():
            return f"String: {value}"
        case list() | tuple():
            return f"Sequence: {value}"
        case _:
            return "Unknown type"

print(process_input(42))        # Number: 42
print(process_input(3.14))      # Number: 3.14
print(process_input("hello"))   # String: hello
print(process_input([1, 2, 3])) # Sequence: [1, 2, 3]
```

---

## 7. Sequence Patterns

Sequence patterns match lists, tuples, and other sequences.

### Fixed Length Sequences
```python
def process_point(point):
    match point:
        case [0, 0]:
            return "Origin"
        case [0, y]:
            return f"On Y-axis at {y}"
        case [x, 0]:
            return f"On X-axis at {x}"
        case [x, y]:
            return f"Point at ({x}, {y})"
        case _:
            return "Not a 2D point"

print(process_point([0, 0]))    # Origin
print(process_point([0, 5]))    # On Y-axis at 5
print(process_point([3, 0]))    # On X-axis at 3
print(process_point([2, 4]))    # Point at (2, 4)
```

### Variable Length Sequences
```python
def process_list(items):
    match items:
        case []:
            return "Empty list"
        case [x]:
            return f"Single item: {x}"
        case [x, y]:
            return f"Two items: {x}, {y}"
        case [first, *rest]:
            return f"First: {first}, Rest: {rest}"

print(process_list([]))           # Empty list
print(process_list([1]))          # Single item: 1
print(process_list([1, 2]))       # Two items: 1, 2
print(process_list([1, 2, 3, 4])) # First: 1, Rest: [2, 3, 4]
```

### Nested Sequences
```python
def process_nested(data):
    match data:
        case [[x, y], [a, b]]:
            return f"Two points: ({x}, {y}) and ({a}, {b})"
        case [x, [y, z]]:
            return f"Nested: {x} with pair ({y}, {z})"
        case _:
            return "Other structure"

print(process_nested([[1, 2], [3, 4]]))  # Two points: (1, 2) and (3, 4)
print(process_nested([5, [6, 7]]))       # Nested: 5 with pair (6, 7)
```

---

## 8. Mapping Patterns

Mapping patterns match dictionaries.

### Basic Dictionary Matching
```python
def process_user(user):
    match user:
        case {"name": name, "age": age}:
            return f"{name} is {age} years old"
        case {"name": name}:
            return f"User: {name} (age unknown)"
        case {"age": age}:
            return f"Anonymous user, age {age}"
        case _:
            return "Invalid user data"

print(process_user({"name": "Alice", "age": 25}))  # Alice is 25 years old
print(process_user({"name": "Bob"}))               # User: Bob (age unknown)
print(process_user({"age": 30}))                   # Anonymous user, age 30
```

### Partial Dictionary Matching
```python
def process_config(config):
    match config:
        case {"host": host, "port": port, **rest}:
            return f"Server: {host}:{port}, Extra: {rest}"
        case {"host": host}:
            return f"Server: {host} (default port)"
        case _:
            return "Invalid config"

config1 = {"host": "localhost", "port": 8080, "debug": True}
print(process_config(config1))
# Server: localhost:8080, Extra: {'debug': True}

config2 = {"host": "example.com"}
print(process_config(config2))
# Server: example.com (default port)
```

### Nested Dictionary Matching
```python
def process_person(person):
    match person:
        case {"name": name, "address": {"city": city, "country": country}}:
            return f"{name} lives in {city}, {country}"
        case {"name": name, "address": address}:
            return f"{name}'s address: {address}"
        case {"name": name}:
            return f"Person: {name}"
        case _:
            return "Invalid person data"

person1 = {
    "name": "Alice",
    "address": {"city": "New York", "country": "USA"}
}
print(process_person(person1))  # Alice lives in New York, USA
```

---

## 9. Class Patterns

Class patterns match objects and extract attributes.

### Basic Class Matching
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def describe_point(point):
    match point:
        case Point(x=0, y=0):
            return "Origin"
        case Point(x=0, y=y):
            return f"On Y-axis at {y}"
        case Point(x=x, y=0):
            return f"On X-axis at {x}"
        case Point(x=x, y=y):
            return f"Point at ({x}, {y})"
        case _:
            return "Not a point"

p1 = Point(0, 0)
p2 = Point(0, 5)
p3 = Point(3, 4)

print(describe_point(p1))  # Origin
print(describe_point(p2))  # On Y-axis at 5
print(describe_point(p3))  # Point at (3, 4)
```

### Matching with Dataclasses
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str = "Unknown"

def process_person(person):
    match person:
        case Person(name=name, age=age, city="New York"):
            return f"{name} ({age}) from NYC"
        case Person(name=name, age=age) if age < 18:
            return f"{name} is a minor"
        case Person(name=name, age=age):
            return f"{name} is {age} years old"
        case _:
            return "Not a person"

p1 = Person("Alice", 25, "New York")
p2 = Person("Bob", 15, "Boston")
p3 = Person("Charlie", 30)

print(process_person(p1))  # Alice (25) from NYC
print(process_person(p2))  # Bob is a minor
print(process_person(p3))  # Charlie is 30 years old
```

---

## 10. Guard Clauses

Guard clauses add conditions to patterns using `if`.

### Basic Guards
```python
def categorize_number(n):
    match n:
        case x if x < 0:
            return "Negative"
        case 0:
            return "Zero"
        case x if x > 0 and x < 10:
            return "Small positive"
        case x if x >= 10:
            return "Large positive"

print(categorize_number(-5))  # Negative
print(categorize_number(0))   # Zero
print(categorize_number(5))   # Small positive
print(categorize_number(15))  # Large positive
```

### Guards with Sequences
```python
def process_scores(scores):
    match scores:
        case [x, y] if x > y:
            return f"{x} is greater than {y}"
        case [x, y] if x < y:
            return f"{x} is less than {y}"
        case [x, y] if x == y:
            return f"Both are equal: {x}"
        case _:
            return "Invalid scores"

print(process_scores([90, 80]))  # 90 is greater than 80
print(process_scores([70, 85]))  # 70 is less than 85
print(process_scores([75, 75]))  # Both are equal: 75
```

### Guards with Dictionaries
```python
def check_user(user):
    match user:
        case {"age": age, "name": name} if age >= 18:
            return f"{name} is an adult"
        case {"age": age, "name": name} if age < 18:
            return f"{name} is a minor"
        case _:
            return "Invalid user"

print(check_user({"name": "Alice", "age": 25}))  # Alice is an adult
print(check_user({"name": "Bob", "age": 15}))    # Bob is a minor
```

---

## 11. Practical Examples

### Example 1: HTTP Request Handler
```python
def handle_request(request):
    match request:
        case {"method": "GET", "path": path}:
            return f"Fetching resource: {path}"
        case {"method": "POST", "path": path, "data": data}:
            return f"Creating resource at {path} with data: {data}"
        case {"method": "PUT", "path": path, "data": data}:
            return f"Updating resource at {path} with data: {data}"
        case {"method": "DELETE", "path": path}:
            return f"Deleting resource: {path}"
        case _:
            return "Invalid request"

# Test requests
get_req = {"method": "GET", "path": "/users"}
post_req = {"method": "POST", "path": "/users", "data": {"name": "Alice"}}
delete_req = {"method": "DELETE", "path": "/users/1"}

print(handle_request(get_req))
print(handle_request(post_req))
print(handle_request(delete_req))
```

### Example 2: Calculator
```python
def calculate(expression):
    match expression:
        case ("add", x, y):
            return x + y
        case ("subtract", x, y):
            return x - y
        case ("multiply", x, y):
            return x * y
        case ("divide", x, y) if y != 0:
            return x / y
        case ("divide", x, 0):
            return "Error: Division by zero"
        case ("power", x, y):
            return x ** y
        case _:
            return "Unknown operation"

print(calculate(("add", 10, 5)))       # 15
print(calculate(("multiply", 4, 3)))   # 12
print(calculate(("divide", 10, 2)))    # 5.0
print(calculate(("divide", 10, 0)))    # Error: Division by zero
print(calculate(("power", 2, 3)))      # 8
```

### Example 3: JSON Parser
```python
def parse_json_value(value):
    match value:
        case None:
            return "null"
        case bool():
            return f"boolean: {value}"
        case int() | float():
            return f"number: {value}"
        case str():
            return f"string: '{value}'"
        case list():
            return f"array with {len(value)} items"
        case dict():
            return f"object with {len(value)} keys"
        case _:
            return "unknown type"

print(parse_json_value(None))           # null
print(parse_json_value(True))           # boolean: True
print(parse_json_value(42))             # number: 42
print(parse_json_value("hello"))        # string: 'hello'
print(parse_json_value([1, 2, 3]))      # array with 3 items
print(parse_json_value({"a": 1}))       # object with 1 keys
```

### Example 4: Command Line Parser
```python
def parse_command(args):
    match args:
        case ["help"]:
            return "Showing help..."
        case ["version"]:
            return "Version 1.0.0"
        case ["install", package]:
            return f"Installing {package}..."
        case ["uninstall", package]:
            return f"Uninstalling {package}..."
        case ["search", *terms]:
            return f"Searching for: {' '.join(terms)}"
        case ["config", "set", key, value]:
            return f"Setting {key} = {value}"
        case ["config", "get", key]:
            return f"Getting value of {key}"
        case _:
            return "Unknown command. Type 'help' for usage."

print(parse_command(["help"]))
print(parse_command(["install", "numpy"]))
print(parse_command(["search", "machine", "learning"]))
print(parse_command(["config", "set", "theme", "dark"]))
```

### Example 5: Game State Handler
```python
def handle_game_state(state):
    match state:
        case {"status": "menu", "selected": option}:
            return f"Menu option selected: {option}"
        case {"status": "playing", "level": level, "score": score}:
            return f"Playing level {level}, score: {score}"
        case {"status": "paused", "level": level}:
            return f"Game paused at level {level}"
        case {"status": "game_over", "score": score, "high_score": hs} if score > hs:
            return f"New high score: {score}!"
        case {"status": "game_over", "score": score}:
            return f"Game over. Score: {score}"
        case _:
            return "Unknown game state"

# Test states
menu_state = {"status": "menu", "selected": "start"}
play_state = {"status": "playing", "level": 3, "score": 1500}
pause_state = {"status": "paused", "level": 3}
over_state = {"status": "game_over", "score": 2000, "high_score": 1800}

print(handle_game_state(menu_state))
print(handle_game_state(play_state))
print(handle_game_state(pause_state))
print(handle_game_state(over_state))
```

### Example 6: Shape Area Calculator
```python
def calculate_area(shape):
    match shape:
        case {"type": "circle", "radius": r}:
            return 3.14159 * r ** 2
        case {"type": "rectangle", "width": w, "height": h}:
            return w * h
        case {"type": "square", "side": s}:
            return s ** 2
        case {"type": "triangle", "base": b, "height": h}:
            return 0.5 * b * h
        case _:
            return "Unknown shape"

circle = {"type": "circle", "radius": 5}
rectangle = {"type": "rectangle", "width": 4, "height": 6}
square = {"type": "square", "side": 5}
triangle = {"type": "triangle", "base": 4, "height": 3}

print(f"Circle area: {calculate_area(circle):.2f}")
print(f"Rectangle area: {calculate_area(rectangle)}")
print(f"Square area: {calculate_area(square)}")
print(f"Triangle area: {calculate_area(triangle)}")
```

---

## 12. Practice Questions

### Beginner Level

**Question 1**: Write a match-case statement to convert numeric grades (0-100) to letter grades (A, B, C, D, F).
```python
# Your code here
```

**Question 2**: Create a function using match-case to determine if a year is a leap year.
```python
# Your code here
```

**Question 3**: Write a match-case statement to categorize animals as "mammal", "bird", "fish", or "reptile".
```python
# Your code here
```

**Question 4**: Create a function that uses match-case to convert month numbers (1-12) to month names.
```python
# Your code here
```

**Question 5**: Write a match-case statement to determine the type of triangle (equilateral, isosceles, scalene) given three sides.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Create a function using match-case to parse and evaluate simple mathematical expressions like ("add", 5, 3).
```python
# Your code here
```

**Question 7**: Write a match-case statement to handle different types of user input (int, float, str, list, dict).
```python
# Your code here
```

**Question 8**: Create a function that uses match-case to process coordinates and determine their quadrant.
```python
# Your code here
```

**Question 9**: Write a match-case statement to validate and process email addresses based on their domain.
```python
# Your code here
```

**Question 10**: Create a function using match-case to handle different HTTP status codes and return appropriate messages.
```python
# Your code here
```

### Advanced Level

**Question 11**: Implement a simple state machine using match-case for a traffic light system.
```python
# Your code here
```

**Question 12**: Create a function using match-case to parse and validate JSON-like nested structures.
```python
# Your code here
```

**Question 13**: Write a match-case based router for a web application that handles different URL patterns.
```python
# Your code here
```

**Question 14**: Implement a pattern matcher for algebraic expressions (e.g., simplifying x + 0 = x).
```python
# Your code here
```

**Question 15**: Create a function using match-case to implement a simple interpreter for a stack-based calculator.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **Match-case** is available in Python 3.10+
2. More powerful than traditional switch-case statements
3. Supports **structural pattern matching**
4. Can **destructure** sequences and mappings
5. **Guard clauses** add conditional logic to patterns
6. **Wildcard pattern** `_` matches anything
7. **OR patterns** use `|` to match multiple patterns
8. **Capture patterns** bind values to variables
9. Patterns are checked **top to bottom**
10. First matching pattern is executed

---

## 📚 Comparison with If-Elif-Else

### Using If-Elif-Else
```python
def process_point_old(point):
    if point == [0, 0]:
        return "Origin"
    elif point[0] == 0:
        return f"On Y-axis at {point[1]}"
    elif point[1] == 0:
        return f"On X-axis at {point[0]}"
    else:
        return f"Point at ({point[0]}, {point[1]})"
```

### Using Match-Case
```python
def process_point_new(point):
    match point:
        case [0, 0]:
            return "Origin"
        case [0, y]:
            return f"On Y-axis at {y}"
        case [x, 0]:
            return f"On X-axis at {x}"
        case [x, y]:
            return f"Point at ({x}, {y})"
```

The match-case version is more readable and handles destructuring automatically!

---

## 📚 Additional Resources

- PEP 634 - Structural Pattern Matching: https://www.python.org/dev/peps/pep-0634/
- PEP 635 - Motivation and Rationale: https://www.python.org/dev/peps/pep-0635/
- PEP 636 - Tutorial: https://www.python.org/dev/peps/pep-0636/
- Python 3.10 Release Notes: https://docs.python.org/3/whatsnew/3.10.html

---

**Next Topic**: Modules and Packages
**Previous Topic**: Iterables and Iterators

Happy Coding! 🐍
