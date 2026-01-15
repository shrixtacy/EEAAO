# Python Iterables and Iterators - Complete Guide

## 📚 Table of Contents
1. [Understanding Iterables](#understanding-iterables)
2. [Understanding Iterators](#understanding-iterators)
3. [Iterator Protocol](#iterator-protocol)
4. [Creating Custom Iterators](#creating-custom-iterators)
5. [Generator Functions](#generator-functions)
6. [Generator Expressions](#generator-expressions)
7. [Built-in Iterables](#built-in-iterables)
8. [Practical Examples](#practical-examples)
9. [Practice Questions](#practice-questions)

---

## 1. Understanding Iterables

An **iterable** is any Python object that can return its elements one at a time. It implements the `__iter__()` method.

### Common Iterables
```python
# Lists are iterables
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    print(num)

# Strings are iterables
text = "Python"
for char in text:
    print(char)

# Tuples are iterables
coordinates = (10, 20, 30)
for coord in coordinates:
    print(coord)

# Dictionaries are iterables
person = {"name": "Alice", "age": 25}
for key in person:
    print(key, person[key])

# Sets are iterables
unique_nums = {1, 2, 3, 4, 5}
for num in unique_nums:
    print(num)

# Range objects are iterables
for i in range(5):
    print(i)

# Files are iterables
with open("data.txt", "r") as file:
    for line in file:
        print(line)
```

### Checking if Object is Iterable
```python
from collections.abc import Iterable

# Check if object is iterable
print(isinstance([1, 2, 3], Iterable))      # True
print(isinstance("hello", Iterable))        # True
print(isinstance(42, Iterable))             # False
print(isinstance(range(5), Iterable))       # True
```

---

## 2. Understanding Iterators

An **iterator** is an object that represents a stream of data. It implements:
- `__iter__()` - Returns the iterator object itself
- `__next__()` - Returns the next item from the stream

### Creating an Iterator from an Iterable
```python
# Create an iterator from a list
numbers = [1, 2, 3, 4, 5]
iterator = iter(numbers)

# Get next items
print(next(iterator))  # 1
print(next(iterator))  # 2
print(next(iterator))  # 3
print(next(iterator))  # 4
print(next(iterator))  # 5
# print(next(iterator))  # StopIteration exception

# Using try-except to handle StopIteration
numbers = [1, 2, 3]
iterator = iter(numbers)

while True:
    try:
        item = next(iterator)
        print(item)
    except StopIteration:
        break
```

### Iterator vs Iterable
```python
# List is iterable but not an iterator
numbers = [1, 2, 3]
print(hasattr(numbers, '__iter__'))  # True
print(hasattr(numbers, '__next__'))  # False

# Iterator has both __iter__ and __next__
iterator = iter(numbers)
print(hasattr(iterator, '__iter__'))  # True
print(hasattr(iterator, '__next__'))  # True
```

---

## 3. Iterator Protocol

The iterator protocol consists of two methods:

### `__iter__()` Method
Returns the iterator object itself. This allows an iterator to be used in a for loop.

### `__next__()` Method
Returns the next value from the iterator. Raises `StopIteration` when no more items are available.

```python
# Manual iteration using iterator protocol
numbers = [10, 20, 30]
iterator = iter(numbers)

# This is what happens behind the scenes in a for loop
while True:
    try:
        item = next(iterator)
        print(item)
    except StopIteration:
        break

# For loop does this automatically
for item in numbers:
    print(item)
```

---

## 4. Creating Custom Iterators

### Example 1: Counter Iterator
```python
class Counter:
    """Iterator that counts from start to end"""
    
    def __init__(self, start, end):
        self.current = start
        self.end = end
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current > self.end:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1

# Using the custom iterator
counter = Counter(1, 5)
for num in counter:
    print(num)  # 1, 2, 3, 4, 5

# Manual iteration
counter = Counter(10, 13)
print(next(counter))  # 10
print(next(counter))  # 11
print(next(counter))  # 12
print(next(counter))  # 13
# print(next(counter))  # StopIteration
```

### Example 2: Reverse Iterator
```python
class Reverse:
    """Iterator that returns elements in reverse order"""
    
    def __init__(self, data):
        self.data = data
        self.index = len(data)
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index -= 1
        return self.data[self.index]

# Using reverse iterator
rev = Reverse("Python")
for char in rev:
    print(char)  # n, o, h, t, y, P

# With a list
numbers = [1, 2, 3, 4, 5]
rev_nums = Reverse(numbers)
for num in rev_nums:
    print(num)  # 5, 4, 3, 2, 1
```

### Example 3: Fibonacci Iterator
```python
class Fibonacci:
    """Iterator that generates Fibonacci sequence"""
    
    def __init__(self, max_count):
        self.max_count = max_count
        self.count = 0
        self.a, self.b = 0, 1
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.count >= self.max_count:
            raise StopIteration
        
        self.count += 1
        result = self.a
        self.a, self.b = self.b, self.a + self.b
        return result

# Generate first 10 Fibonacci numbers
fib = Fibonacci(10)
for num in fib:
    print(num, end=" ")  # 0 1 1 2 3 5 8 13 21 34
print()

# Using list() to collect all values
fib = Fibonacci(8)
fib_list = list(fib)
print(fib_list)  # [0, 1, 1, 2, 3, 5, 8, 13]
```

### Example 4: Even Numbers Iterator
```python
class EvenNumbers:
    """Iterator that returns even numbers up to a limit"""
    
    def __init__(self, limit):
        self.limit = limit
        self.current = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current > self.limit:
            raise StopIteration
        result = self.current
        self.current += 2
        return result

# Get even numbers up to 20
evens = EvenNumbers(20)
for num in evens:
    print(num, end=" ")  # 0 2 4 6 8 10 12 14 16 18 20
print()
```

### Example 5: Infinite Iterator
```python
class InfiniteCounter:
    """Iterator that counts infinitely"""
    
    def __init__(self, start=0):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        result = self.current
        self.current += 1
        return result

# Use with caution - infinite loop!
# counter = InfiniteCounter()
# for num in counter:
#     print(num)  # Will run forever!

# Use with a break condition
counter = InfiniteCounter(100)
for num in counter:
    if num > 105:
        break
    print(num)  # 100, 101, 102, 103, 104, 105
```

---

## 5. Generator Functions

Generators are a simpler way to create iterators using functions with the `yield` keyword.

### Basic Generator
```python
def count_up_to(n):
    """Generator that counts from 1 to n"""
    count = 1
    while count <= n:
        yield count
        count += 1

# Using the generator
counter = count_up_to(5)
for num in counter:
    print(num)  # 1, 2, 3, 4, 5

# Manual iteration
counter = count_up_to(3)
print(next(counter))  # 1
print(next(counter))  # 2
print(next(counter))  # 3
# print(next(counter))  # StopIteration
```

### Generator vs Regular Function
```python
# Regular function - returns all at once
def get_squares_list(n):
    result = []
    for i in range(1, n + 1):
        result.append(i ** 2)
    return result

# Generator function - yields one at a time
def get_squares_generator(n):
    for i in range(1, n + 1):
        yield i ** 2

# Regular function loads all into memory
squares_list = get_squares_list(5)
print(squares_list)  # [1, 4, 9, 16, 25]

# Generator produces values on demand
squares_gen = get_squares_generator(5)
for square in squares_gen:
    print(square)  # 1, 4, 9, 16, 25
```

### Fibonacci Generator
```python
def fibonacci(n):
    """Generate first n Fibonacci numbers"""
    a, b = 0, 1
    count = 0
    while count < n:
        yield a
        a, b = b, a + b
        count += 1

# Generate Fibonacci numbers
for num in fibonacci(10):
    print(num, end=" ")  # 0 1 1 2 3 5 8 13 21 34
print()

# Convert to list
fib_list = list(fibonacci(8))
print(fib_list)  # [0, 1, 1, 2, 3, 5, 8, 13]
```

### Infinite Generator
```python
def infinite_sequence():
    """Generate infinite sequence of numbers"""
    num = 0
    while True:
        yield num
        num += 1

# Use with break condition
gen = infinite_sequence()
for i in gen:
    if i > 5:
        break
    print(i)  # 0, 1, 2, 3, 4, 5
```

### Generator with Multiple Yields
```python
def multiple_yields():
    """Generator with multiple yield statements"""
    yield "First"
    yield "Second"
    yield "Third"
    yield "Fourth"

gen = multiple_yields()
print(next(gen))  # First
print(next(gen))  # Second
print(next(gen))  # Third
print(next(gen))  # Fourth
```

### Reading Large Files with Generator
```python
def read_large_file(file_path):
    """Generator to read large files line by line"""
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()

# Usage (memory efficient for large files)
# for line in read_large_file('large_file.txt'):
#     process(line)
```

### Generator for Prime Numbers
```python
def prime_generator(limit):
    """Generate prime numbers up to limit"""
    def is_prime(n):
        if n < 2:
            return False
        for i in range(2, int(n ** 0.5) + 1):
            if n % i == 0:
                return False
        return True
    
    for num in range(2, limit + 1):
        if is_prime(num):
            yield num

# Generate primes up to 30
primes = list(prime_generator(30))
print(primes)  # [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

---

## 6. Generator Expressions

Generator expressions are similar to list comprehensions but use parentheses and create generators instead of lists.

### Basic Generator Expression
```python
# List comprehension - creates list in memory
squares_list = [x ** 2 for x in range(10)]
print(squares_list)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Generator expression - creates generator
squares_gen = (x ** 2 for x in range(10))
print(squares_gen)  # <generator object>

# Iterate over generator
for square in squares_gen:
    print(square, end=" ")
print()

# Convert to list
squares_gen = (x ** 2 for x in range(10))
print(list(squares_gen))  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### Generator Expression with Condition
```python
# Even numbers
evens = (x for x in range(20) if x % 2 == 0)
print(list(evens))  # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# Odd squares
odd_squares = (x ** 2 for x in range(10) if x % 2 != 0)
print(list(odd_squares))  # [1, 9, 25, 49, 81]

# Positive numbers from list
numbers = [-5, -3, 0, 2, 4, -1, 7]
positives = (x for x in numbers if x > 0)
print(list(positives))  # [2, 4, 7]
```

### Memory Efficiency Comparison
```python
import sys

# List comprehension
list_comp = [x ** 2 for x in range(10000)]
print(f"List size: {sys.getsizeof(list_comp)} bytes")

# Generator expression
gen_exp = (x ** 2 for x in range(10000))
print(f"Generator size: {sys.getsizeof(gen_exp)} bytes")

# Generator is much smaller!
```

### Using Generator Expressions with Functions
```python
# sum() with generator expression
total = sum(x ** 2 for x in range(10))
print(total)  # 285

# max() with generator expression
maximum = max(x ** 2 for x in range(10))
print(maximum)  # 81

# any() with generator expression
has_even = any(x % 2 == 0 for x in [1, 3, 5, 7, 8])
print(has_even)  # True

# all() with generator expression
all_positive = all(x > 0 for x in [1, 2, 3, 4, 5])
print(all_positive)  # True
```

---

## 7. Built-in Iterables

### iter() Function
```python
# Create iterator from iterable
numbers = [1, 2, 3, 4, 5]
iterator = iter(numbers)

print(next(iterator))  # 1
print(next(iterator))  # 2

# iter() with sentinel value
# Reads until sentinel value is encountered
with open('data.txt', 'r') as file:
    for line in iter(file.readline, ''):
        print(line)
```

### next() Function
```python
# next() with default value
numbers = [1, 2, 3]
iterator = iter(numbers)

print(next(iterator))  # 1
print(next(iterator))  # 2
print(next(iterator))  # 3
print(next(iterator, "No more items"))  # No more items (no exception)
```

### enumerate()
```python
# enumerate() returns iterator of tuples
fruits = ['apple', 'banana', 'cherry']
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Start from different index
for index, fruit in enumerate(fruits, start=1):
    print(f"{index}. {fruit}")
```

### zip()
```python
# zip() combines multiple iterables
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]
cities = ['New York', 'London', 'Paris']

for name, age, city in zip(names, ages, cities):
    print(f"{name} is {age} years old and lives in {city}")

# zip() stops at shortest iterable
list1 = [1, 2, 3, 4, 5]
list2 = ['a', 'b', 'c']
result = list(zip(list1, list2))
print(result)  # [(1, 'a'), (2, 'b'), (3, 'c')]
```

### map()
```python
# map() applies function to each item
numbers = [1, 2, 3, 4, 5]
squared = map(lambda x: x ** 2, numbers)
print(list(squared))  # [1, 4, 9, 16, 25]

# map() with multiple iterables
list1 = [1, 2, 3]
list2 = [10, 20, 30]
result = map(lambda x, y: x + y, list1, list2)
print(list(result))  # [11, 22, 33]
```

### filter()
```python
# filter() returns items where function is True
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = filter(lambda x: x % 2 == 0, numbers)
print(list(evens))  # [2, 4, 6, 8, 10]

# Filter strings by length
words = ['hi', 'hello', 'hey', 'goodbye', 'bye']
long_words = filter(lambda w: len(w) > 3, words)
print(list(long_words))  # ['hello', 'goodbye']
```

### reversed()
```python
# reversed() returns reverse iterator
numbers = [1, 2, 3, 4, 5]
rev = reversed(numbers)
print(list(rev))  # [5, 4, 3, 2, 1]

# Works with strings
text = "Python"
print(list(reversed(text)))  # ['n', 'o', 'h', 't', 'y', 'P']
```

### sorted()
```python
# sorted() returns sorted list (not iterator, but useful)
numbers = [5, 2, 8, 1, 9]
print(sorted(numbers))  # [1, 2, 5, 8, 9]

# Reverse sort
print(sorted(numbers, reverse=True))  # [9, 8, 5, 2, 1]

# Sort by custom key
words = ['apple', 'pie', 'zoo', 'a']
print(sorted(words, key=len))  # ['a', 'pie', 'zoo', 'apple']
```

---

## 8. Practical Examples

### Example 1: Batch Processing
```python
def batch_data(data, batch_size):
    """Generator that yields data in batches"""
    for i in range(0, len(data), batch_size):
        yield data[i:i + batch_size]

# Process data in batches
data = list(range(1, 21))
for batch in batch_data(data, 5):
    print(f"Processing batch: {batch}")
    # Process batch here
```

### Example 2: Infinite Cycle
```python
def cycle(iterable):
    """Generator that cycles through iterable infinitely"""
    while True:
        for item in iterable:
            yield item

# Cycle through colors
colors = ['red', 'green', 'blue']
color_cycle = cycle(colors)

for i in range(10):
    print(next(color_cycle), end=" ")
# red green blue red green blue red green blue red
print()
```

### Example 3: Running Average
```python
def running_average():
    """Generator that calculates running average"""
    total = 0
    count = 0
    average = None
    
    while True:
        value = yield average
        total += value
        count += 1
        average = total / count

# Calculate running average
avg_gen = running_average()
next(avg_gen)  # Prime the generator

print(avg_gen.send(10))  # 10.0
print(avg_gen.send(20))  # 15.0
print(avg_gen.send(30))  # 20.0
print(avg_gen.send(40))  # 25.0
```

### Example 4: Flatten Nested List
```python
def flatten(nested_list):
    """Generator to flatten nested list"""
    for item in nested_list:
        if isinstance(item, list):
            yield from flatten(item)
        else:
            yield item

# Flatten nested structure
nested = [1, [2, 3, [4, 5]], 6, [7, [8, 9]]]
flat = list(flatten(nested))
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Example 5: Countdown Timer
```python
def countdown(n):
    """Generator for countdown"""
    while n > 0:
        yield n
        n -= 1
    yield "Blast off!"

# Countdown from 5
for count in countdown(5):
    print(count)
# 5, 4, 3, 2, 1, Blast off!
```

### Example 6: Data Pipeline
```python
def read_data():
    """Simulate reading data"""
    data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    for item in data:
        yield item

def filter_even(data_gen):
    """Filter even numbers"""
    for item in data_gen:
        if item % 2 == 0:
            yield item

def square(data_gen):
    """Square each number"""
    for item in data_gen:
        yield item ** 2

# Create data pipeline
pipeline = square(filter_even(read_data()))
result = list(pipeline)
print(result)  # [4, 16, 36, 64, 100]
```

---

## 9. Practice Questions

### Beginner Level

**Question 1**: Create an iterator class that counts from 0 to 10.
```python
# Your code here
```

**Question 2**: Write a generator function that yields the first n even numbers.
```python
# Your code here
```

**Question 3**: Create a generator expression that produces squares of numbers from 1 to 10.
```python
# Your code here
```

**Question 4**: Write a function that checks if an object is iterable.
```python
# Your code here
```

**Question 5**: Create an iterator that returns characters of a string in reverse.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Implement a custom iterator class for a range-like object that supports start, stop, and step.
```python
# Your code here
```

**Question 7**: Write a generator function that yields all permutations of a string (without using itertools).
```python
# Your code here
```

**Question 8**: Create a generator that yields lines from multiple files sequentially.
```python
# Your code here
```

**Question 9**: Implement an infinite iterator that alternates between two values.
```python
# Your code here
```

**Question 10**: Write a generator function that yields running sum of numbers.
```python
# Your code here
```

### Advanced Level

**Question 11**: Create a custom iterator that implements a circular buffer of fixed size.
```python
# Your code here
```

**Question 12**: Implement a generator that yields all subsets of a list.
```python
# Your code here
```

**Question 13**: Write a generator function that implements merge sort using generators.
```python
# Your code here
```

**Question 14**: Create an iterator class that implements a binary tree traversal (in-order).
```python
# Your code here
```

**Question 15**: Implement a generator that yields sliding windows of size n from an iterable.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **Iterables** have `__iter__()` method and can be looped over
2. **Iterators** have both `__iter__()` and `__next__()` methods
3. **Generators** are a simple way to create iterators using `yield`
4. **Generator expressions** are memory-efficient alternatives to list comprehensions
5. Use `iter()` to create iterator from iterable
6. Use `next()` to get next item from iterator
7. Generators are lazy - they produce values on demand
8. `StopIteration` exception signals end of iteration
9. `yield from` delegates to another generator
10. Generators are great for large datasets and infinite sequences

---

## 📚 Additional Resources

- Python Iterator Protocol: https://docs.python.org/3/library/stdtypes.html#iterator-types
- Generator Functions: https://docs.python.org/3/howto/functional.html#generators
- itertools Module: https://docs.python.org/3/library/itertools.html
- PEP 255 - Simple Generators: https://www.python.org/dev/peps/pep-0255/

---

**Next Topic**: Match-Case Statements (Python 3.10+)
**Previous Topic**: Functions - Complete Guide

Happy Coding! 🐍
