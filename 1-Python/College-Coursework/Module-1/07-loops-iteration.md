# Loops and Iteration

## 📚 Theoretical Understanding

### What are Loops?

Loops are programming constructs that allow you to execute a block of code repeatedly. Instead of writing the same code multiple times, you write it once inside a loop and specify how many times or under what conditions it should repeat. Loops are fundamental to programming because they enable automation - performing repetitive tasks efficiently without manual repetition. Whether you're processing items in a list, reading lines from a file, or performing calculations until a condition is met, loops are the tool that makes it possible.

In Python, there are two main types of loops: `for` loops and `while` loops. Each serves different purposes and is suited for different scenarios. `for` loops are used when you know how many times you want to iterate or when you want to iterate over a collection of items. `while` loops are used when you want to repeat something until a condition becomes false, and you don't necessarily know in advance how many iterations that will take. Understanding when to use each type of loop is crucial for writing efficient, readable code.

Loops transform programming from a linear, step-by-step process into a powerful tool for handling large amounts of data and complex repetitive tasks. Without loops, processing a list of 1000 items would require writing 1000 separate statements. With loops, you write one statement that executes 1000 times. This capability is what makes computers so powerful for data processing, automation, and solving complex problems.

### The for Loop

The `for` loop in Python is designed for iterating over sequences - lists, strings, tuples, dictionaries, ranges, and any other iterable object. Unlike many other programming languages where `for` loops use counters and conditions, Python's `for` loop is more intuitive: it takes each item from a sequence one at a time and executes the loop body with that item. This design makes Python's `for` loops particularly readable and Pythonic.

The syntax is straightforward: `for item in sequence:` followed by an indented block of code. On each iteration, the variable `item` is assigned the next value from the sequence, and the code block executes. This continues until all items in the sequence have been processed. The loop variable (`item` in this example) is a regular variable that you can use within the loop body, and it retains its last value after the loop completes.

Python's `for` loop is incredibly versatile. You can iterate over lists of numbers, strings of characters, lines in a file, keys in a dictionary, or any custom iterable object. The `range()` function is commonly used with `for` loops to generate sequences of numbers, making it easy to repeat an action a specific number of times or to iterate with indices. Understanding `for` loops and the objects you can iterate over is essential for effective Python programming.

### The while Loop

The `while` loop repeats a block of code as long as a condition remains true. Unlike `for` loops that iterate over a known sequence, `while` loops continue until their condition becomes false. This makes them ideal for situations where you don't know in advance how many iterations you'll need - for example, reading user input until they enter "quit", processing data until you reach the end of a file, or performing calculations until a result converges to a desired accuracy.

The syntax is `while condition:` followed by an indented code block. Before each iteration, Python evaluates the condition. If it's true, the loop body executes. If it's false, the loop terminates and execution continues with the code after the loop. It's crucial that something inside the loop eventually makes the condition false; otherwise, you create an infinite loop that never terminates (though sometimes infinite loops are intentional, like in server programs that run continuously).

`while` loops require more careful management than `for` loops. You must ensure the loop variable or condition is properly initialized before the loop, and you must update it within the loop so the condition eventually becomes false. Forgetting to update the loop condition is a common mistake that leads to infinite loops. However, when used correctly, `while` loops provide flexibility that `for` loops cannot match, especially for event-driven programming or situations where the number of iterations depends on runtime conditions.

### Loop Control Statements

Python provides three statements that give you fine-grained control over loop execution: `break`, `continue`, and `else`. These statements allow you to exit loops early, skip iterations, or execute code when a loop completes normally.

The `break` statement immediately terminates the loop, regardless of the loop condition or remaining items. Execution jumps to the first statement after the loop. This is useful when you're searching for something and want to stop as soon as you find it, or when an error condition occurs that makes continuing the loop pointless.

The `continue` statement skips the rest of the current iteration and moves to the next iteration. The loop doesn't terminate; it just skips to the next item or checks the condition again. This is useful when you want to skip certain items that don't meet specific criteria without processing them.

The `else` clause on loops is unique to Python and often misunderstood. The `else` block executes when the loop completes normally (exhausts all items or the condition becomes false) but NOT if the loop is terminated by a `break` statement. This is useful for search operations: if you find what you're looking for, you `break` (and the `else` doesn't run); if you don't find it, the loop completes normally and the `else` block can handle the "not found" case.


### Nested Loops

Nested loops are loops placed inside other loops. The inner loop completes all its iterations for each single iteration of the outer loop. If the outer loop runs 5 times and the inner loop runs 3 times, the inner loop body executes 15 times total (5 × 3). Nested loops are essential for working with multi-dimensional data structures like matrices, generating combinations, or processing hierarchical data.

While powerful, nested loops can quickly become computationally expensive. The time complexity multiplies: two nested loops each running n times result in O(n²) complexity. Three nested loops create O(n³) complexity. For large values of n, this can make programs very slow. Understanding the performance implications of nested loops is important for writing efficient code, especially when working with large datasets.

---

## 💻 Practical Examples

### Basic for Loop

```python
# Iterating over a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Output:
# apple
# banana
# cherry

# Iterating over a string
word = "Python"
for char in word:
    print(char)

# Output:
# P
# y
# t
# h
# o
# n

# Using range() for numbers
for i in range(5):
    print(i)

# Output:
# 0
# 1
# 2
# 3
# 4

# range() with start and end
for i in range(1, 6):
    print(i)

# Output:
# 1
# 2
# 3
# 4
# 5

# range() with step
for i in range(0, 10, 2):
    print(i)

# Output:
# 0
# 2
# 4
# 6
# 8
```

### for Loop with Calculations

```python
# Sum of numbers
total = 0
for i in range(1, 11):
    total += i
print(f"Sum of 1 to 10: {total}")  # 55

# Factorial calculation
n = 5
factorial = 1
for i in range(1, n + 1):
    factorial *= i
print(f"{n}! = {factorial}")  # 120

# Multiplication table
number = 7
for i in range(1, 11):
    print(f"{number} × {i} = {number * i}")

# Finding maximum in a list
numbers = [45, 23, 67, 12, 89, 34]
maximum = numbers[0]
for num in numbers:
    if num > maximum:
        maximum = num
print(f"Maximum: {maximum}")  # 89
```

### while Loop

```python
# Basic while loop
count = 0
while count < 5:
    print(count)
    count += 1

# Output:
# 0
# 1
# 2
# 3
# 4

# User input validation
password = ""
while password != "secret":
    password = input("Enter password: ")
    if password != "secret":
        print("Wrong password, try again")
print("Access granted!")

# Countdown
n = 5
while n > 0:
    print(n)
    n -= 1
print("Blast off!")

# Sum until condition
total = 0
number = 1
while total < 100:
    total += number
    number += 1
print(f"Sum reached {total} after adding {number-1} numbers")
```

### break Statement

```python
# Search for a number
numbers = [12, 45, 23, 67, 89, 34]
target = 67

for num in numbers:
    if num == target:
        print(f"Found {target}!")
        break
    print(f"Checking {num}...")

# Output:
# Checking 12...
# Checking 45...
# Checking 23...
# Found 67!

# Exit loop on condition
count = 0
while True:  # Infinite loop
    count += 1
    print(count)
    if count >= 5:
        break  # Exit the loop

# Password attempts with limit
attempts = 0
max_attempts = 3

while attempts < max_attempts:
    password = input("Enter password: ")
    attempts += 1
    
    if password == "secret":
        print("Login successful!")
        break
    else:
        remaining = max_attempts - attempts
        if remaining > 0:
            print(f"Wrong password. {remaining} attempts remaining.")
        else:
            print("Account locked!")
```

### continue Statement

```python
# Skip even numbers
for i in range(1, 11):
    if i % 2 == 0:
        continue  # Skip even numbers
    print(i)

# Output:
# 1
# 3
# 5
# 7
# 9

# Process only valid data
numbers = [10, -5, 20, -3, 30, 0, 40]

for num in numbers:
    if num <= 0:
        continue  # Skip non-positive numbers
    print(f"Processing {num}")

# Skip specific items
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

for fruit in fruits:
    if fruit == "cherry":
        continue  # Skip cherry
    print(fruit)

# Output:
# apple
# banana
# date
# elderberry
```

### Loop with else Clause

```python
# Search with else
numbers = [12, 45, 23, 67, 89]
target = 50

for num in numbers:
    if num == target:
        print(f"Found {target}!")
        break
else:
    print(f"{target} not found in list")

# Output:
# 50 not found in list

# Prime number checker
n = 17
is_prime = True

for i in range(2, n):
    if n % i == 0:
        print(f"{n} is not prime (divisible by {i})")
        is_prime = False
        break
else:
    print(f"{n} is prime")

# Output:
# 17 is prime

# Password validation with attempts
max_attempts = 3
attempt = 0

while attempt < max_attempts:
    password = input("Enter password: ")
    if password == "secret":
        print("Login successful!")
        break
    attempt += 1
else:
    print("Too many failed attempts. Account locked.")
```

### Nested Loops

```python
# Multiplication table
for i in range(1, 6):
    for j in range(1, 6):
        print(f"{i} × {j} = {i*j:2}", end="  ")
    print()  # New line after each row

# Pattern printing
for i in range(1, 6):
    for j in range(i):
        print("*", end="")
    print()

# Output:
# *
# **
# ***
# ****
# *****

# Matrix operations
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Print matrix
for row in matrix:
    for element in row:
        print(element, end=" ")
    print()

# Sum of all elements
total = 0
for row in matrix:
    for element in row:
        total += element
print(f"Sum of all elements: {total}")  # 45

# Finding element in 2D list
target = 5
found = False

for i in range(len(matrix)):
    for j in range(len(matrix[i])):
        if matrix[i][j] == target:
            print(f"Found {target} at position ({i}, {j})")
            found = True
            break
    if found:
        break
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

# Modify list using index
numbers = [10, 20, 30, 40, 50]

for index, num in enumerate(numbers):
    numbers[index] = num * 2

print(numbers)  # [20, 40, 60, 80, 100]
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

# Create dictionary from two lists
keys = ["name", "age", "city"]
values = ["Alice", 25, "New York"]

person = {}
for key, value in zip(keys, values):
    person[key] = value

print(person)  # {'name': 'Alice', 'age': 25, 'city': 'New York'}

# Or use dict() with zip()
person = dict(zip(keys, values))
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain the difference between `for` loops and `while` loops in Python. When should you use each type? Provide examples demonstrating scenarios where each loop type is most appropriate.

**Detailed Answer:**

Understanding the difference between `for` and `while` loops is fundamental to writing effective Python code. While both loops repeat code, they serve different purposes and are suited for different scenarios.

**for Loops - Iteration Over Sequences**

`for` loops are designed for iterating over sequences or iterables. They automatically handle the iteration process, taking each item from a collection one at a time. You use `for` loops when you know what you want to iterate over, even if you don't know the exact number of items in advance.

```python
# Iterating over a list
fruits = ["apple", "banana", "cherry", "date"]

for fruit in fruits:
    print(fruit)

# Python handles:
# - Getting each item
# - Knowing when to stop
# - Moving to the next item
```

**Key characteristics of for loops:**
- Iterate over a known sequence (list, string, range, etc.)
- Automatically handle iteration logic
- Loop variable is automatically updated
- Cleaner syntax for iterating over collections
- Less prone to infinite loops

**while Loops - Condition-Based Repetition**

`while` loops repeat as long as a condition is true. They're used when you don't have a sequence to iterate over, but rather a condition that determines when to stop. You use `while` loops when the number of iterations depends on runtime conditions.

```python
# Keep asking until valid input
password = ""

while password != "secret":
    password = input("Enter password: ")

# Loop continues until condition becomes false
```

**Key characteristics of while loops:**
- Repeat based on a condition
- You must manually update loop variables
- Can create infinite loops if condition never becomes false
- More flexible for complex termination conditions
- Better for event-driven or state-based logic

**When to Use for Loops:**

**1. Iterating Over Collections**

```python
# Processing list items
numbers = [1, 2, 3, 4, 5]
total = 0

for num in numbers:
    total += num

print(f"Sum: {total}")

# Processing string characters
word = "Python"
vowels = 0

for char in word:
    if char.lower() in "aeiou":
        vowels += 1

print(f"Vowels: {vowels}")
```

**2. Known Number of Iterations**

```python
# Print multiplication table
for i in range(1, 11):
    print(f"7 × {i} = {7 * i}")

# Repeat action specific number of times
for _ in range(5):  # _ indicates we don't use the variable
    print("Hello!")
```

**3. Iterating with Indices**

```python
# Access both index and value
fruits = ["apple", "banana", "cherry"]

for i in range(len(fruits)):
    print(f"Index {i}: {fruits[i]}")

# Better: use enumerate()
for index, fruit in enumerate(fruits):
    print(f"Index {index}: {fruit}")
```

**4. Processing Files**

```python
# Read file line by line
with open("data.txt", "r") as file:
    for line in file:
        print(line.strip())

# Python handles file iteration automatically
```

**5. Dictionary Iteration**

```python
# Iterate over dictionary
person = {"name": "Alice", "age": 25, "city": "New York"}

for key in person:
    print(f"{key}: {person[key]}")

# Or iterate over items
for key, value in person.items():
    print(f"{key}: {value}")
```

**When to Use while Loops:**

**1. Unknown Number of Iterations**

```python
# Keep reading until end marker
data = []
value = input("Enter value (or 'done' to finish): ")

while value != "done":
    data.append(value)
    value = input("Enter value (or 'done' to finish): ")

print(f"Collected {len(data)} values")
```

**2. Condition-Based Termination**

```python
# Calculate until convergence
value = 100
tolerance = 0.01

while abs(value - target) > tolerance:
    value = calculate_next(value)
    iterations += 1

print(f"Converged after {iterations} iterations")
```

**3. Game Loops**

```python
# Game continues until player quits or loses
game_running = True
player_health = 100

while game_running and player_health > 0:
    action = get_player_action()
    
    if action == "quit":
        game_running = False
    else:
        player_health = process_action(action)

print("Game over!")
```

**4. Retry Logic**

```python
# Retry operation until success or max attempts
max_attempts = 3
attempt = 0
success = False

while attempt < max_attempts and not success:
    try:
        result = perform_operation()
        success = True
    except Exception as e:
        attempt += 1
        print(f"Attempt {attempt} failed: {e}")

if success:
    print("Operation successful!")
else:
    print("Operation failed after maximum attempts")
```

**5. Waiting for Events**

```python
# Wait for condition to be met
import time

server_ready = False

while not server_ready:
    server_ready = check_server_status()
    if not server_ready:
        print("Waiting for server...")
        time.sleep(1)

print("Server is ready!")
```

**Converting Between for and while:**

Many `for` loops can be rewritten as `while` loops (but not always vice versa):

```python
# for loop
for i in range(5):
    print(i)

# Equivalent while loop
i = 0
while i < 5:
    print(i)
    i += 1

# for loop over list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Equivalent while loop (more complex)
fruits = ["apple", "banana", "cherry"]
i = 0
while i < len(fruits):
    print(fruits[i])
    i += 1

# The for loop is clearly simpler and more readable!
```

**Common Mistakes:**

**1. Using while when for is better:**

```python
# BAD - Unnecessarily complex
i = 0
while i < len(numbers):
    print(numbers[i])
    i += 1

# GOOD - Simple and clear
for num in numbers:
    print(num)
```

**2. Forgetting to update while loop condition:**

```python
# INFINITE LOOP - count never changes!
count = 0
while count < 5:
    print("Hello")
    # Forgot: count += 1

# CORRECT
count = 0
while count < 5:
    print("Hello")
    count += 1  # Update condition variable
```

**3. Using for when while is better:**

```python
# AWKWARD - Using for with break
for _ in range(1000000):  # Arbitrary large number
    password = input("Enter password: ")
    if password == "secret":
        break

# BETTER - while loop is clearer
password = ""
while password != "secret":
    password = input("Enter password: ")
```

**Decision Tree:**

```
Need to repeat code?
│
├─ Iterating over a collection? → Use for loop
├─ Known number of iterations? → Use for loop with range()
├─ Condition-based termination? → Use while loop
├─ Unknown number of iterations? → Use while loop
└─ Waiting for event/input? → Use while loop
```

**Performance Considerations:**

```python
# for loops are generally faster for iteration
import time

# for loop
start = time.time()
for i in range(1000000):
    pass
print(f"for loop: {time.time() - start:.4f}s")

# while loop
start = time.time()
i = 0
while i < 1000000:
    i += 1
print(f"while loop: {time.time() - start:.4f}s")

# for loop is typically faster because:
# - Optimized at C level
# - Less overhead per iteration
# - No manual variable updates
```

**Conclusion:**

Use `for` loops when iterating over sequences or when you know the number of iterations. Use `while` loops when repeating based on conditions or when the number of iterations is unknown. `for` loops are generally preferred when applicable because they're more readable, less error-prone, and often faster. `while` loops provide flexibility for complex termination conditions and event-driven scenarios.

---

### Question 2: Explain the `break`, `continue`, and `else` statements in loops. How do they affect loop execution? Provide practical examples demonstrating when and why you would use each statement.

**Detailed Answer:**

Loop control statements (`break`, `continue`, and `else`) provide fine-grained control over loop execution, allowing you to exit loops early, skip iterations, or execute code when loops complete normally. Understanding these statements is essential for writing efficient, readable loop logic.

**The break Statement**

`break` immediately terminates the loop, regardless of the loop condition or remaining items. Execution jumps to the first statement after the loop. It's used when you've found what you're looking for or when continuing the loop is pointless.

```python
# Basic break example
for i in range(10):
    if i == 5:
        break  # Exit loop when i is 5
    print(i)

# Output:
# 0
# 1
# 2
# 3
# 4
# Loop exits, 5-9 are never printed
```

**When to Use break:**

**1. Search Operations - Stop When Found**

```python
# Search for a value
numbers = [12, 45, 23, 67, 89, 34, 56]
target = 67

for num in numbers:
    if num == target:
        print(f"Found {target}!")
        break  # No need to continue searching
    print(f"Checking {num}...")

# Output:
# Checking 12...
# Checking 45...
# Checking 23...
# Found 67!
# Stops here - doesn't check 89, 34, 56

# Without break (inefficient)
found = False
for num in numbers:
    if num == target and not found:
        print(f"Found {target}!")
        found = True
    elif not found:
        print(f"Checking {num}...")
# Continues checking even after finding target!
```

**2. Input Validation - Exit on Valid Input**

```python
# Get valid input
while True:
    age = input("Enter your age: ")
    
    if age.isdigit():
        age = int(age)
        if 0 < age < 150:
            print(f"Age {age} accepted")
            break  # Valid input, exit loop
        else:
            print("Age must be between 1 and 149")
    else:
        print("Please enter a number")

# Loop continues until valid input
```

**3. Error Conditions - Stop on Failure**

```python
# Process files until error
files = ["file1.txt", "file2.txt", "corrupted.txt", "file4.txt"]

for filename in files:
    try:
        with open(filename, 'r') as f:
            content = f.read()
            process(content)
    except FileNotFoundError:
        print(f"Error: {filename} not found")
        break  # Stop processing on error
    except Exception as e:
        print(f"Error processing {filename}: {e}")
        break

print("Processing stopped")
```

**4. Resource Limits - Stop When Limit Reached**

```python
# Process until quota exceeded
max_size = 1000
total_size = 0

for item in items:
    item_size = get_size(item)
    
    if total_size + item_size > max_size:
        print("Quota exceeded, stopping")
        break  # Don't exceed limit
    
    process(item)
    total_size += item_size

print(f"Processed {total_size} bytes")
```

**The continue Statement**

`continue` skips the rest of the current iteration and moves to the next iteration. The loop doesn't terminate; it just skips to the next item or checks the condition again.

```python
# Basic continue example
for i in range(10):
    if i % 2 == 0:
        continue  # Skip even numbers
    print(i)

# Output:
# 1
# 3
# 5
# 7
# 9
# Even numbers (0,2,4,6,8) are skipped
```

**When to Use continue:**

**1. Skip Invalid Data**

```python
# Process only valid numbers
numbers = [10, -5, 20, 0, -3, 30, 40]

for num in numbers:
    if num <= 0:
        continue  # Skip non-positive numbers
    
    # Process only positive numbers
    result = calculate(num)
    print(f"{num} → {result}")

# Output only shows positive numbers
```

**2. Filter Items**

```python
# Process only specific file types
files = ["doc.txt", "image.jpg", "data.csv", "photo.png", "report.txt"]

for filename in files:
    if not filename.endswith('.txt'):
        continue  # Skip non-text files
    
    # Process only .txt files
    with open(filename, 'r') as f:
        content = f.read()
        process(content)
```

**3. Skip Based on Conditions**

```python
# Process users with specific criteria
users = [
    {"name": "Alice", "age": 25, "active": True},
    {"name": "Bob", "age": 17, "active": True},
    {"name": "Charlie", "age": 30, "active": False},
    {"name": "David", "age": 22, "active": True}
]

for user in users:
    # Skip inactive users
    if not user["active"]:
        continue
    
    # Skip underage users
    if user["age"] < 18:
        continue
    
    # Process only active adult users
    send_email(user["name"])

# Only Alice and David receive emails
```

**4. Avoid Nested Conditions**

```python
# WITHOUT continue - nested conditions
for item in items:
    if item.is_valid():
        if item.is_active():
            if item.has_permission():
                process(item)

# WITH continue - flatter structure (better)
for item in items:
    if not item.is_valid():
        continue
    if not item.is_active():
        continue
    if not item.has_permission():
        continue
    
    process(item)

# Easier to read and maintain
```

**The else Clause**

The `else` clause on loops executes when the loop completes normally (exhausts all items or condition becomes false) but NOT if the loop is terminated by `break`. This is unique to Python and often misunderstood.

```python
# Basic else example
for i in range(5):
    print(i)
else:
    print("Loop completed normally")

# Output:
# 0
# 1
# 2
# 3
# 4
# Loop completed normally

# With break - else doesn't execute
for i in range(5):
    if i == 3:
        break
    print(i)
else:
    print("Loop completed normally")  # This doesn't print

# Output:
# 0
# 1
# 2
# (else block skipped because of break)
```

**When to Use else:**

**1. Search Operations - Handle "Not Found"**

```python
# Search with not-found handling
numbers = [12, 45, 23, 89, 34]
target = 67

for num in numbers:
    if num == target:
        print(f"Found {target}!")
        break
else:
    # Executes only if break was NOT called
    print(f"{target} not found in list")

# Output:
# 67 not found in list

# If target was in list:
target = 45
for num in numbers:
    if num == target:
        print(f"Found {target}!")
        break
else:
    print(f"{target} not found in list")

# Output:
# Found 45!
# (else block doesn't execute)
```

**2. Validation - All Items Pass**

```python
# Check if all numbers are positive
numbers = [10, 20, 30, 40, 50]

for num in numbers:
    if num <= 0:
        print("Found non-positive number")
        break
else:
    print("All numbers are positive")

# Output:
# All numbers are positive

# With negative number:
numbers = [10, 20, -5, 40, 50]

for num in numbers:
    if num <= 0:
        print("Found non-positive number")
        break
else:
    print("All numbers are positive")

# Output:
# Found non-positive number
```

**3. Prime Number Checking**

```python
# Check if number is prime
n = 17

for i in range(2, n):
    if n % i == 0:
        print(f"{n} is not prime (divisible by {i})")
        break
else:
    # Executes if no divisor found
    print(f"{n} is prime")

# Output:
# 17 is prime

# With non-prime:
n = 15

for i in range(2, n):
    if n % i == 0:
        print(f"{n} is not prime (divisible by {i})")
        break
else:
    print(f"{n} is prime")

# Output:
# 15 is not prime (divisible by 3)
```

**4. File Processing - Success/Failure**

```python
# Process all files or report failure
files = ["file1.txt", "file2.txt", "file3.txt"]

for filename in files:
    try:
        with open(filename, 'r') as f:
            process(f.read())
    except Exception as e:
        print(f"Error processing {filename}: {e}")
        break
else:
    print("All files processed successfully")

# If all succeed:
# All files processed successfully

# If one fails:
# Error processing file2.txt: [error message]
```

**Combining break, continue, and else:**

```python
# Complex example: Find first valid item
items = [
    {"id": 1, "value": -5, "active": True},
    {"id": 2, "value": 10, "active": False},
    {"id": 3, "value": 20, "active": True},
    {"id": 4, "value": 30, "active": True}
]

for item in items:
    # Skip inactive items
    if not item["active"]:
        print(f"Skipping inactive item {item['id']}")
        continue
    
    # Skip invalid values
    if item["value"] < 0:
        print(f"Skipping invalid item {item['id']}")
        continue
    
    # Found valid item
    print(f"Processing item {item['id']}")
    process(item)
    break
else:
    print("No valid items found")

# Output:
# Skipping invalid item 1
# Skipping inactive item 2
# Processing item 3
```

**Common Patterns:**

**Pattern 1: Early Exit on Success**

```python
# Stop when condition met
for attempt in range(max_attempts):
    if try_operation():
        print("Success!")
        break
else:
    print("Failed after all attempts")
```

**Pattern 2: Skip and Continue**

```python
# Process only valid items
for item in items:
    if not is_valid(item):
        continue
    process(item)
```

**Pattern 3: Search and Report**

```python
# Find and report result
for item in collection:
    if matches(item):
        print(f"Found: {item}")
        break
else:
    print("Not found")
```

**Best Practices:**

1. **Use break for early exit** - Don't continue looping when you've found what you need
2. **Use continue to avoid nesting** - Flatten conditional logic
3. **Use else for "not found" cases** - Cleaner than flag variables
4. **Don't overuse** - Sometimes a simple if statement is clearer
5. **Comment complex logic** - Make intent clear

**Conclusion:**

`break` exits loops immediately, `continue` skips to the next iteration, and `else` executes when loops complete without `break`. These statements provide powerful control over loop execution, enabling efficient search operations, data filtering, and validation logic. Use them to write clearer, more efficient code, but don't overuse them - sometimes simpler approaches are better.

---

## ✅ Key Takeaways

1. Loops enable repetitive execution of code blocks
2. `for` loops iterate over sequences; `while` loops repeat based on conditions
3. Use `for` loops when you know what to iterate over
4. Use `while` loops when iterations depend on runtime conditions
5. `break` exits loops immediately
6. `continue` skips to the next iteration
7. `else` on loops executes when loop completes without `break`
8. Nested loops multiply iterations (be careful with performance)
9. `enumerate()` provides index and value when iterating
10. `zip()` iterates over multiple sequences simultaneously

---

**Next Topic**: Module 2 - Python Data Structures
**Previous Topic**: Conditional Statements
**Module**: 1 - Programming for Everybody

Happy Learning! 🐍
