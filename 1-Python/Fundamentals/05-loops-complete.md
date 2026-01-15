# Python Loops - Complete Guide

## Table of Contents
1. [While Loops](#while-loops)
2. [For Loops](#for-loops)
3. [Loop Control Statements](#loop-control-statements)
4. [Nested Loops](#nested-loops)
5. [Loop Patterns](#loop-patterns)
6. [Practice Questions](#practice-questions)

---

## While Loops

### Basic Syntax

```python
while condition:
    # Code block executes while condition is True
    statement1
    statement2
```

### Simple Examples

```python
# Example 1: Count to 5
count = 1
while count <= 5:
    print(count)
    count += 1
# Output: 1 2 3 4 5

# Example 2: Sum of numbers
total = 0
number = 1
while number <= 10:
    total += number
    number += 1
print(f"Sum: {total}")  # Output: Sum: 55

# Example 3: User input validation
password = ""
while password != "secret":
    password = input("Enter password: ")
print("Access granted!")

# Example 4: Countdown
count = 5
while count > 0:
    print(count)
    count -= 1
print("Blast off!")
```

### Infinite Loops

```python
# Infinite loop (use Ctrl+C to stop)
# while True:
#     print("This runs forever")

# Controlled infinite loop with break
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == "quit":
        break
    print(f"You entered: {user_input}")
```

### While with Else

```python
# else block executes when loop completes normally (not broken)
count = 1
while count <= 5:
    print(count)
    count += 1
else:
    print("Loop completed normally")

# else doesn't execute if loop is broken
count = 1
while count <= 10:
    if count == 5:
        break
    print(count)
    count += 1
else:
    print("This won't print")  # Skipped because of break
```

---

## For Loops

### Basic Syntax

```python
for variable in iterable:
    # Code block executes for each item
    statement1
    statement2
```

### Iterating Over Sequences

```python
# Example 1: Iterate over list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
# Output: apple banana cherry

# Example 2: Iterate over string
for char in "Python":
    print(char)
# Output: P y t h o n

# Example 3: Iterate over tuple
coordinates = (10, 20, 30)
for coord in coordinates:
    print(coord)
# Output: 10 20 30

# Example 4: Iterate over dictionary keys
person = {"name": "John", "age": 30, "city": "New York"}
for key in person:
    print(f"{key}: {person[key]}")
```

### range() Function

```python
# range(stop) - from 0 to stop-1
for i in range(5):
    print(i)  # Output: 0 1 2 3 4

# range(start, stop) - from start to stop-1
for i in range(2, 7):
    print(i)  # Output: 2 3 4 5 6

# range(start, stop, step)
for i in range(0, 10, 2):
    print(i)  # Output: 0 2 4 6 8

# Negative step (countdown)
for i in range(10, 0, -1):
    print(i)  # Output: 10 9 8 7 6 5 4 3 2 1

# range() is memory efficient (doesn't create list)
# Convert to list if needed
numbers = list(range(1, 6))
print(numbers)  # Output: [1, 2, 3, 4, 5]
```

### enumerate() Function

```python
# Get index and value
fruits = ["apple", "banana", "cherry"]

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
# Output:
# 0: apple
# 1: banana
# 2: cherry

# Start index from 1
for index, fruit in enumerate(fruits, start=1):
    print(f"{index}. {fruit}")
# Output:
# 1. apple
# 2. banana
# 3. cherry
```

### zip() Function

```python
# Iterate over multiple lists simultaneously
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
cities = ["New York", "London", "Paris"]

for name, age, city in zip(names, ages, cities):
    print(f"{name} is {age} years old and lives in {city}")
# Output:
# Alice is 25 years old and lives in New York
# Bob is 30 years old and lives in London
# Charlie is 35 years old and lives in Paris

# zip stops at shortest list
list1 = [1, 2, 3]
list2 = ['a', 'b']
for num, letter in zip(list1, list2):
    print(num, letter)
# Output: 1 a, 2 b (stops at shortest)
```

### Dictionary Iteration

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Iterate over keys (default)
for key in person:
    print(key)

# Iterate over keys explicitly
for key in person.keys():
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")
```

### For with Else

```python
# else executes when loop completes normally
for i in range(5):
    print(i)
else:
    print("Loop completed")

# else doesn't execute if loop is broken
for i in range(10):
    if i == 5:
        break
    print(i)
else:
    print("This won't print")
```

---

## Loop Control Statements

### break - Exit Loop

```python
# Example 1: Find first even number
numbers = [1, 3, 5, 8, 9, 10]
for num in numbers:
    if num % 2 == 0:
        print(f"First even: {num}")
        break  # Exit loop

# Example 2: Search in list
fruits = ["apple", "banana", "cherry"]
search = "banana"
for fruit in fruits:
    if fruit == search:
        print(f"Found {search}!")
        break
else:
    print(f"{search} not found")

# Example 3: Password attempts
max_attempts = 3
attempts = 0
while attempts < max_attempts:
    password = input("Enter password: ")
    if password == "secret":
        print("Access granted!")
        break
    attempts += 1
    print(f"Wrong! {max_attempts - attempts} attempts left")
else:
    print("Account locked!")
```

### continue - Skip to Next Iteration

```python
# Example 1: Skip even numbers
for i in range(10):
    if i % 2 == 0:
        continue  # Skip rest of loop body
    print(i)  # Only prints odd numbers

# Example 2: Skip negative numbers
numbers = [1, -2, 3, -4, 5]
for num in numbers:
    if num < 0:
        continue
    print(num)  # Output: 1 3 5

# Example 3: Process valid inputs only
items = ["apple", "", "banana", None, "cherry"]
for item in items:
    if not item:
        continue
    print(f"Processing: {item}")

# Example 4: Skip multiples of 3
for i in range(1, 11):
    if i % 3 == 0:
        continue
    print(i)  # Output: 1 2 4 5 7 8 10
```

### pass - Do Nothing

```python
# Placeholder for future code
for i in range(5):
    if i == 3:
        pass  # TODO: implement later
    print(i)

# Empty function
def future_function():
    pass  # Will implement later

# Empty class
class FutureClass:
    pass

# Conditional placeholder
x = 10
if x > 5:
    pass  # No action needed
else:
    print("x is small")
```

---

## Nested Loops

### Basic Nested Loops

```python
# Example 1: Multiplication table
for i in range(1, 6):
    for j in range(1, 6):
        print(f"{i} x {j} = {i*j}")
    print()  # Blank line after each table

# Example 2: Print pattern
for i in range(5):
    for j in range(i + 1):
        print("*", end="")
    print()
# Output:
# *
# **
# ***
# ****
# *****

# Example 3: 2D list iteration
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
for row in matrix:
    for element in row:
        print(element, end=" ")
    print()
```

### Nested Loop Patterns

```python
# Pattern 1: Right triangle
rows = 5
for i in range(1, rows + 1):
    for j in range(i):
        print("*", end="")
    print()

# Pattern 2: Inverted triangle
for i in range(rows, 0, -1):
    for j in range(i):
        print("*", end="")
    print()

# Pattern 3: Pyramid
for i in range(1, rows + 1):
    # Print spaces
    for j in range(rows - i):
        print(" ", end="")
    # Print stars
    for k in range(2 * i - 1):
        print("*", end="")
    print()

# Pattern 4: Number pyramid
for i in range(1, 6):
    for j in range(1, i + 1):
        print(j, end="")
    print()
# Output:
# 1
# 12
# 123
# 1234
# 12345
```

### Breaking Out of Nested Loops

```python
# Using flag variable
found = False
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
search = 5

for row in matrix:
    for element in row:
        if element == search:
            print(f"Found {search}!")
            found = True
            break
    if found:
        break

# Using function with return
def find_in_matrix(matrix, target):
    for row in matrix:
        for element in row:
            if element == target:
                return True
    return False

result = find_in_matrix(matrix, 5)
print(f"Found: {result}")

# Using exception (advanced)
class FoundException(Exception):
    pass

try:
    for row in matrix:
        for element in row:
            if element == 5:
                raise FoundException()
except FoundException:
    print("Found 5!")
```

---

## Loop Patterns

### Accumulator Pattern

```python
# Sum of numbers
total = 0
for i in range(1, 11):
    total += i
print(f"Sum: {total}")  # Output: Sum: 55

# Product of numbers
product = 1
for i in range(1, 6):
    product *= i
print(f"Product: {product}")  # Output: Product: 120 (5!)

# Build string
result = ""
for char in "Python":
    result += char.upper()
print(result)  # Output: PYTHON

# Build list
squares = []
for i in range(1, 6):
    squares.append(i ** 2)
print(squares)  # Output: [1, 4, 9, 16, 25]
```

### Counter Pattern

```python
# Count even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_count = 0
for num in numbers:
    if num % 2 == 0:
        even_count += 1
print(f"Even numbers: {even_count}")  # Output: 6

# Count vowels
text = "Hello World"
vowel_count = 0
for char in text.lower():
    if char in "aeiou":
        vowel_count += 1
print(f"Vowels: {vowel_count}")  # Output: 3
```

### Search Pattern

```python
# Linear search
numbers = [10, 20, 30, 40, 50]
target = 30
found_index = -1

for i in range(len(numbers)):
    if numbers[i] == target:
        found_index = i
        break

if found_index != -1:
    print(f"Found at index {found_index}")
else:
    print("Not found")
```

### Min/Max Pattern

```python
# Find maximum
numbers = [45, 23, 67, 12, 89, 34]
max_value = numbers[0]
for num in numbers:
    if num > max_value:
        max_value = num
print(f"Maximum: {max_value}")  # Output: 89

# Find minimum
min_value = numbers[0]
for num in numbers:
    if num < min_value:
        min_value = num
print(f"Minimum: {min_value}")  # Output: 12
```

### Filter Pattern

```python
# Filter even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = []
for num in numbers:
    if num % 2 == 0:
        evens.append(num)
print(evens)  # Output: [2, 4, 6, 8, 10]

# Filter by length
words = ["cat", "elephant", "dog", "butterfly"]
long_words = []
for word in words:
    if len(word) > 5:
        long_words.append(word)
print(long_words)  # Output: ['elephant', 'butterfly']
```

---

## Practice Questions

### Beginner Level

**Question 1:** Print all numbers from 1 to 100.

```python
# Solution
for i in range(1, 101):
    print(i, end=" ")
```

**Question 2:** Calculate factorial of a number.

```python
# Solution
n = int(input("Enter a number: "))
factorial = 1
for i in range(1, n + 1):
    factorial *= i
print(f"{n}! = {factorial}")
```

**Question 3:** Print multiplication table.

```python
# Solution
num = int(input("Enter a number: "))
for i in range(1, 11):
    print(f"{num} x {i} = {num * i}")
```

### Intermediate Level

**Question 4:** FizzBuzz (1 to 100)
- Print "Fizz" for multiples of 3
- Print "Buzz" for multiples of 5
- Print "FizzBuzz" for multiples of both
- Otherwise print the number

```python
# Solution
for i in range(1, 101):
    if i % 15 == 0:
        print("FizzBuzz")
    elif i % 3 == 0:
        print("Fizz")
    elif i % 5 == 0:
        print("Buzz")
    else:
        print(i)
```

**Question 5:** Find all prime numbers up to n.

```python
# Solution
n = int(input("Enter n: "))
primes = []

for num in range(2, n + 1):
    is_prime = True
    for i in range(2, int(num ** 0.5) + 1):
        if num % i == 0:
            is_prime = False
            break
    if is_prime:
        primes.append(num)

print(f"Primes up to {n}: {primes}")
```

**Question 6:** Reverse a number.

```python
# Solution
num = int(input("Enter a number: "))
reversed_num = 0

while num > 0:
    digit = num % 10
    reversed_num = reversed_num * 10 + digit
    num //= 10

print(f"Reversed: {reversed_num}")
```

### Advanced Level

**Question 7:** Print Pascal's Triangle.

```python
# Solution
def print_pascals_triangle(n):
    for i in range(n):
        # Print spaces
        for j in range(n - i - 1):
            print(" ", end="")
        
        # Calculate and print values
        num = 1
        for j in range(i + 1):
            print(num, end=" ")
            num = num * (i - j) // (j + 1)
        print()

rows = int(input("Enter number of rows: "))
print_pascals_triangle(rows)
```

**Question 8:** Find all Armstrong numbers in a range.
(Armstrong number: sum of cubes of digits equals the number)

```python
# Solution
start = int(input("Enter start: "))
end = int(input("Enter end: "))

armstrong_numbers = []
for num in range(start, end + 1):
    # Calculate sum of cubes
    temp = num
    sum_of_cubes = 0
    while temp > 0:
        digit = temp % 10
        sum_of_cubes += digit ** 3
        temp //= 10
    
    if sum_of_cubes == num:
        armstrong_numbers.append(num)

print(f"Armstrong numbers: {armstrong_numbers}")
```

**Question 9:** Generate Fibonacci sequence up to n terms.

```python
# Solution
n = int(input("Enter number of terms: "))

if n <= 0:
    print("Please enter a positive number")
elif n == 1:
    print("Fibonacci sequence: 0")
else:
    fib_sequence = [0, 1]
    for i in range(2, n):
        next_term = fib_sequence[i-1] + fib_sequence[i-2]
        fib_sequence.append(next_term)
    
    print(f"Fibonacci sequence: {fib_sequence}")
```

**Question 10:** Create a simple number guessing game.

```python
# Solution
import random

number = random.randint(1, 100)
attempts = 0
max_attempts = 10

print("Guess the number between 1 and 100!")

while attempts < max_attempts:
    guess = int(input(f"Attempt {attempts + 1}/{max_attempts}: "))
    attempts += 1
    
    if guess == number:
        print(f"Congratulations! You guessed it in {attempts} attempts!")
        break
    elif guess < number:
        print("Too low!")
    else:
        print("Too high!")
else:
    print(f"Game over! The number was {number}")
```

---

## Key Takeaways

1. **while loops** run while condition is True
2. **for loops** iterate over sequences
3. **range()** generates number sequences
4. **enumerate()** provides index and value
5. **zip()** iterates multiple sequences together
6. **break** exits loop immediately
7. **continue** skips to next iteration
8. **pass** does nothing (placeholder)
9. **else** clause executes if loop completes normally
10. **Nested loops** create multi-dimensional iterations

---

## Common Mistakes

```python
# Mistake 1: Infinite loop
# i = 0
# while i < 5:
#     print(i)
#     # Forgot to increment i!

# Mistake 2: Off-by-one error
# range(5) gives 0-4, not 1-5
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# Mistake 3: Modifying list while iterating
numbers = [1, 2, 3, 4, 5]
# for num in numbers:
#     numbers.remove(num)  # Don't do this!

# Correct way
numbers = [1, 2, 3, 4, 5]
numbers = [num for num in numbers if num % 2 == 0]

# Mistake 4: Using wrong loop type
# Don't use while when for is better
# i = 0
# while i < len(items):
#     print(items[i])
#     i += 1

# Better
for item in items:
    print(item)
```

---

## Next Steps

Now that you master loops, move on to:
- **Lists, Tuples, and Sets**
- **List Comprehensions**
- **Dictionaries**
- **Functions**

Keep practicing with pattern problems and algorithms!
