# Python Tuples - Complete Reference Guide

## Table of Contents
1. [Tuple Basics](#tuple-basics)
2. [Tuple Methods](#tuple-methods)
3. [Tuple Operations](#tuple-operations)
4. [Tuple Packing and Unpacking](#tuple-packing-and-unpacking)
5. [Named Tuples](#named-tuples)
6. [Tuples vs Lists](#tuples-vs-lists)
7. [Practice Questions](#practice-questions)

---

## Tuple Basics

### What are Tuples?

Tuples are **immutable** sequences - once created, they cannot be modified. They are similar to lists but with key differences.

### Creating Tuples

```python
# Empty tuple
empty_tuple = ()
empty_tuple2 = tuple()

# Tuple with elements
numbers = (1, 2, 3, 4, 5)
fruits = ("apple", "banana", "cherry")
mixed = (1, "hello", 3.14, True)

# Single element tuple (comma required!)
single = (5,)  # Correct
not_tuple = (5)  # This is just an integer!
print(type(single))    # <class 'tuple'>
print(type(not_tuple)) # <class 'int'>

# Tuple without parentheses (tuple packing)
coordinates = 10, 20, 30
print(type(coordinates))  # <class 'tuple'>

# Tuple from other iterables
tuple_from_list = tuple([1, 2, 3])
tuple_from_string = tuple("Python")  # ('P', 'y', 't', 'h', 'o', 'n')
tuple_from_range = tuple(range(5))   # (0, 1, 2, 3, 4)
```

### Accessing Elements

```python
fruits = ("apple", "banana", "cherry", "date", "elderberry")

# Positive indexing
print(fruits[0])   # apple
print(fruits[2])   # cherry

# Negative indexing
print(fruits[-1])  # elderberry
print(fruits[-2])  # date

# Slicing
print(fruits[1:4])   # ('banana', 'cherry', 'date')
print(fruits[:3])    # ('apple', 'banana', 'cherry')
print(fruits[2:])    # ('cherry', 'date', 'elderberry')
print(fruits[::2])   # ('apple', 'cherry', 'elderberry')
print(fruits[::-1])  # Reverse tuple
```

### Immutability

```python
# Tuples cannot be modified
numbers = (1, 2, 3, 4, 5)

# These will raise TypeError
# numbers[0] = 10  # TypeError: 'tuple' object does not support item assignment
# numbers.append(6)  # AttributeError: 'tuple' object has no attribute 'append'
# del numbers[0]  # TypeError: 'tuple' object doesn't support item deletion

# However, you can create a new tuple
numbers = numbers + (6, 7)
print(numbers)  # (1, 2, 3, 4, 5, 6, 7)

# Mutable objects inside tuples CAN be modified
mixed = ([1, 2, 3], "hello", 42)
mixed[0].append(4)  # This works!
print(mixed)  # ([1, 2, 3, 4], 'hello', 42)
```

---

## Tuple Methods

Tuples have only 2 methods because they are immutable.

### count() - Count Occurrences

```python
numbers = (1, 2, 3, 2, 4, 2, 5)

# Count occurrences
print(numbers.count(2))  # 3
print(numbers.count(10)) # 0

# With strings
fruits = ("apple", "banana", "apple", "cherry", "apple")
print(fruits.count("apple"))  # 3

# With nested tuples
nested = ((1, 2), (3, 4), (1, 2), (5, 6))
print(nested.count((1, 2)))  # 2
```

### index() - Find Index

```python
fruits = ("apple", "banana", "cherry", "banana", "date")

# Find first occurrence
print(fruits.index("banana"))  # 1

# Find with start position
print(fruits.index("banana", 2))  # 3

# Find with start and end
print(fruits.index("cherry", 0, 4))  # 2

# Raises ValueError if not found
# print(fruits.index("mango"))  # ValueError

# Safe index finding
if "mango" in fruits:
    print(fruits.index("mango"))
else:
    print("Not found")
```

---

## Tuple Operations

### Concatenation (+)

```python
tuple1 = (1, 2, 3)
tuple2 = (4, 5, 6)
combined = tuple1 + tuple2
print(combined)  # (1, 2, 3, 4, 5, 6)

# Multiple concatenation
result = (1,) + (2, 3) + (4, 5)
print(result)  # (1, 2, 3, 4, 5)
```

### Repetition (*)

```python
numbers = (1, 2, 3)
repeated = numbers * 3
print(repeated)  # (1, 2, 3, 1, 2, 3, 1, 2, 3)

# Create tuple with repeated elements
zeros = (0,) * 5
print(zeros)  # (0, 0, 0, 0, 0)
```

### Membership (in, not in)

```python
fruits = ("apple", "banana", "cherry")

print("apple" in fruits)      # True
print("mango" in fruits)      # False
print("mango" not in fruits)  # True

# Check in nested tuple
nested = ((1, 2), (3, 4), (5, 6))
print((3, 4) in nested)  # True
```

### Length (len())

```python
fruits = ("apple", "banana", "cherry")
print(len(fruits))  # 3

# Empty tuple
empty = ()
print(len(empty))  # 0

# Nested tuple
nested = ((1, 2), (3, 4, 5), (6,))
print(len(nested))  # 3 (number of elements, not total items)
```

### Min, Max, Sum

```python
numbers = (3, 1, 4, 1, 5, 9, 2)

print(min(numbers))  # 1
print(max(numbers))  # 9
print(sum(numbers))  # 25

# With strings (lexicographic)
words = ("apple", "banana", "cherry")
print(min(words))  # apple
print(max(words))  # cherry
```

### Comparison

```python
# Tuples are compared element by element
print((1, 2, 3) < (1, 2, 4))  # True
print((1, 2, 3) == (1, 2, 3)) # True
print((1, 2) < (1, 2, 3))     # True (shorter is less)

# Lexicographic comparison with strings
print(("apple",) < ("banana",))  # True
```

---

## Tuple Packing and Unpacking

### Tuple Packing

```python
# Packing values into a tuple
coordinates = 10, 20, 30  # Parentheses optional
print(coordinates)  # (10, 20, 30)

# Explicit packing
person = ("John", 25, "New York")
```

### Tuple Unpacking

```python
# Basic unpacking
coordinates = (10, 20, 30)
x, y, z = coordinates
print(x, y, z)  # 10 20 30

# Unpacking in one line
name, age, city = ("Alice", 25, "Paris")
print(name)  # Alice
print(age)   # 25
print(city)  # Paris

# Swapping variables
a, b = 10, 20
a, b = b, a  # Swap using tuple unpacking
print(a, b)  # 20 10
```

### Extended Unpacking (*)

```python
# Unpack with * (Python 3+)
numbers = (1, 2, 3, 4, 5)
first, *middle, last = numbers
print(first)   # 1
print(middle)  # [2, 3, 4] (list!)
print(last)    # 5

# Get first and rest
first, *rest = (1, 2, 3, 4, 5)
print(first)  # 1
print(rest)   # [2, 3, 4, 5]

# Get all but last
*most, last = (1, 2, 3, 4, 5)
print(most)  # [1, 2, 3, 4]
print(last)  # 5

# Ignore values with _
first, _, third, *_ = (1, 2, 3, 4, 5)
print(first)  # 1
print(third)  # 3
```

### Unpacking in Loops

```python
# Unpack tuples in for loop
people = [
    ("Alice", 25),
    ("Bob", 30),
    ("Charlie", 35)
]

for name, age in people:
    print(f"{name} is {age} years old")

# Unpack with enumerate
fruits = ("apple", "banana", "cherry")
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

### Function Return Values

```python
# Functions can return tuples
def get_min_max(numbers):
    return min(numbers), max(numbers)

# Unpack return value
minimum, maximum = get_min_max([1, 5, 3, 9, 2])
print(f"Min: {minimum}, Max: {maximum}")

# Multiple return values
def get_stats(numbers):
    return len(numbers), sum(numbers), sum(numbers) / len(numbers)

count, total, average = get_stats([1, 2, 3, 4, 5])
print(f"Count: {count}, Total: {total}, Average: {average}")
```

---

## Named Tuples

Named tuples are tuples with named fields, providing more readable code.

```python
from collections import namedtuple

# Define a named tuple
Point = namedtuple('Point', ['x', 'y'])

# Create instances
p1 = Point(10, 20)
p2 = Point(x=30, y=40)

# Access by name
print(p1.x)  # 10
print(p1.y)  # 20

# Access by index (still works)
print(p1[0])  # 10
print(p1[1])  # 20

# Unpack like regular tuple
x, y = p1
print(x, y)  # 10 20

# Named tuples are immutable
# p1.x = 15  # AttributeError

# Convert to dictionary
print(p1._asdict())  # {'x': 10, 'y': 20}

# Replace values (creates new tuple)
p3 = p1._replace(x=50)
print(p3)  # Point(x=50, y=20)
```

### Practical Named Tuple Examples

```python
from collections import namedtuple

# Person record
Person = namedtuple('Person', ['name', 'age', 'city'])
person1 = Person("Alice", 25, "New York")
print(f"{person1.name} is {person1.age} years old")

# RGB Color
Color = namedtuple('Color', ['red', 'green', 'blue'])
white = Color(255, 255, 255)
black = Color(0, 0, 0)
print(f"White RGB: ({white.red}, {white.green}, {white.blue})")

# Card in a deck
Card = namedtuple('Card', ['rank', 'suit'])
ace_of_spades = Card('A', 'Spades')
print(f"{ace_of_spades.rank} of {ace_of_spades.suit}")

# Student record
Student = namedtuple('Student', ['id', 'name', 'grade'])
student = Student(101, "Bob", 85)
print(f"Student {student.name} (ID: {student.id}) scored {student.grade}")
```

---

## Tuples vs Lists

### When to Use Tuples

```python
# 1. Immutable data (coordinates, RGB values, etc.)
coordinates = (10, 20)
rgb_color = (255, 128, 0)

# 2. Dictionary keys (lists can't be keys)
locations = {
    (0, 0): "Origin",
    (1, 0): "East",
    (0, 1): "North"
}

# 3. Function return values
def get_dimensions():
    return 1920, 1080  # Returns tuple

# 4. Data that shouldn't change
DAYS_OF_WEEK = ("Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun")

# 5. Slightly faster than lists
import sys
my_list = [1, 2, 3, 4, 5]
my_tuple = (1, 2, 3, 4, 5)
print(sys.getsizeof(my_list))   # Larger
print(sys.getsizeof(my_tuple))  # Smaller
```

### When to Use Lists

```python
# 1. Data that needs to be modified
shopping_cart = ["apple", "banana"]
shopping_cart.append("cherry")  # Can modify

# 2. Unknown size or growing data
numbers = []
for i in range(10):
    numbers.append(i)

# 3. Need list methods (sort, reverse, etc.)
scores = [85, 92, 78, 95]
scores.sort()

# 4. Homogeneous data collections
temperatures = [20.5, 21.0, 19.8, 22.3]
```

### Comparison Table

| Feature | Tuple | List |
|---------|-------|------|
| Mutability | Immutable | Mutable |
| Syntax | `(1, 2, 3)` | `[1, 2, 3]` |
| Performance | Faster | Slower |
| Memory | Less | More |
| Methods | 2 (count, index) | 11+ methods |
| Use as dict key | Yes | No |
| Iteration speed | Faster | Slower |

---

## Practice Questions

### Beginner Level

**Question 1:** Create a tuple of your favorite colors and print each color.

```python
# Solution
colors = ("red", "blue", "green", "yellow")
for color in colors:
    print(color)
```

**Question 2:** Swap two variables using tuple unpacking.

```python
# Solution
a = 10
b = 20
a, b = b, a
print(f"a = {a}, b = {b}")  # a = 20, b = 10
```

**Question 3:** Find the maximum and minimum in a tuple.

```python
# Solution
numbers = (45, 23, 67, 12, 89, 34)
print(f"Max: {max(numbers)}")  # 89
print(f"Min: {min(numbers)}")  # 12
```

### Intermediate Level

**Question 4:** Convert a list of tuples to a dictionary.

```python
# Solution
pairs = [("name", "Alice"), ("age", 25), ("city", "Paris")]
person_dict = dict(pairs)
print(person_dict)  # {'name': 'Alice', 'age': 25, 'city': 'Paris'}
```

**Question 5:** Find all unique elements in a tuple.

```python
# Solution
numbers = (1, 2, 3, 2, 4, 3, 5, 1)
unique = tuple(set(numbers))
print(unique)  # (1, 2, 3, 4, 5) - order may vary
```

**Question 6:** Merge two tuples and sort the result.

```python
# Solution
tuple1 = (3, 1, 4)
tuple2 = (1, 5, 9, 2)
merged = tuple1 + tuple2
sorted_tuple = tuple(sorted(merged))
print(sorted_tuple)  # (1, 1, 2, 3, 4, 5, 9)
```

### Advanced Level

**Question 7:** Create a function that returns multiple statistics.

```python
# Solution
def get_statistics(numbers):
    return (
        len(numbers),
        sum(numbers),
        sum(numbers) / len(numbers),
        min(numbers),
        max(numbers)
    )

data = (10, 20, 30, 40, 50)
count, total, avg, minimum, maximum = get_statistics(data)
print(f"Count: {count}")
print(f"Total: {total}")
print(f"Average: {avg}")
print(f"Min: {minimum}")
print(f"Max: {maximum}")
```

**Question 8:** Implement a simple point class using named tuples.

```python
# Solution
from collections import namedtuple
import math

Point = namedtuple('Point', ['x', 'y'])

def distance(p1, p2):
    return math.sqrt((p2.x - p1.x)**2 + (p2.y - p1.y)**2)

def midpoint(p1, p2):
    return Point((p1.x + p2.x) / 2, (p1.y + p2.y) / 2)

p1 = Point(0, 0)
p2 = Point(3, 4)

print(f"Distance: {distance(p1, p2)}")  # 5.0
print(f"Midpoint: {midpoint(p1, p2)}")  # Point(x=1.5, y=2.0)
```

**Question 9:** Flatten a nested tuple.

```python
# Solution
def flatten_tuple(nested):
    result = []
    for item in nested:
        if isinstance(item, tuple):
            result.extend(flatten_tuple(item))
        else:
            result.append(item)
    return tuple(result)

nested = (1, (2, 3), (4, (5, 6)), 7)
flat = flatten_tuple(nested)
print(flat)  # (1, 2, 3, 4, 5, 6, 7)
```

**Question 10:** Create a simple database using tuples.

```python
# Solution
# Database of students (id, name, grade)
students = (
    (101, "Alice", 85),
    (102, "Bob", 92),
    (103, "Charlie", 78),
    (104, "Diana", 95),
    (105, "Eve", 88)
)

# Find student by ID
def find_student(student_id):
    for student in students:
        if student[0] == student_id:
            return student
    return None

# Get students with grade above threshold
def get_high_performers(threshold):
    return tuple(s for s in students if s[2] >= threshold)

# Calculate average grade
def average_grade():
    total = sum(s[2] for s in students)
    return total / len(students)

# Test functions
print(find_student(102))  # (102, 'Bob', 92)
print(get_high_performers(90))  # ((102, 'Bob', 92), (104, 'Diana', 95))
print(f"Average: {average_grade():.2f}")  # Average: 87.60
```

---

## Key Takeaways

1. **Tuples are immutable** - cannot be modified after creation
2. **Tuples are ordered** - maintain insertion order
3. **Tuples allow duplicates** - same value can appear multiple times
4. **Single element tuple** requires comma: `(5,)`
5. **Only 2 methods**: count() and index()
6. **Tuple unpacking** is powerful for multiple assignments
7. **Named tuples** provide readable code with named fields
8. **Tuples are hashable** - can be dictionary keys
9. **Tuples are faster** than lists
10. **Use tuples** for immutable data, lists for mutable data

---

## Common Mistakes

```python
# Mistake 1: Forgetting comma for single element
single = (5)  # This is int, not tuple!
single = (5,)  # Correct

# Mistake 2: Trying to modify tuple
numbers = (1, 2, 3)
# numbers[0] = 10  # TypeError

# Mistake 3: Unpacking wrong number of values
a, b = (1, 2, 3)  # ValueError: too many values to unpack

# Mistake 4: Using list methods on tuple
numbers = (1, 2, 3)
# numbers.append(4)  # AttributeError

# Mistake 5: Modifying tuple in loop
numbers = (1, 2, 3, 4, 5)
# for i in range(len(numbers)):
#     numbers[i] *= 2  # TypeError
```

---

## Next Steps

Now that you understand tuples, move on to:
- **Sets** (unordered unique elements)
- **Dictionaries** (key-value pairs)
- **Functions** (code reusability)
- **Advanced unpacking techniques**

Keep practicing with immutable data structures!
