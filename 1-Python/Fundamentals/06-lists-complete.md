# Python Lists - Complete Reference Guide

## Table of Contents
1. [List Basics](#list-basics)
2. [List Methods - All Methods](#list-methods)
3. [List Operations](#list-operations)
4. [List Comprehensions](#list-comprehensions)
5. [2D Lists (Matrices)](#2d-lists-matrices)
6. [Practice Questions](#practice-questions)

---

## List Basics

### Creating Lists

```python
# Empty list
empty_list = []
empty_list2 = list()

# List with elements
numbers = [1, 2, 3, 4, 5]
fruits = ["apple", "banana", "cherry"]
mixed = [1, "hello", 3.14, True, [1, 2]]

# List from range
numbers = list(range(1, 6))  # [1, 2, 3, 4, 5]

# List from string
chars = list("Python")  # ['P', 'y', 't', 'h', 'o', 'n']

# List with repeated elements
zeros = [0] * 5  # [0, 0, 0, 0, 0]
pattern = [1, 2] * 3  # [1, 2, 1, 2, 1, 2]
```

### Accessing Elements

```python
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

# Positive indexing (0-based)
print(fruits[0])   # apple
print(fruits[2])   # cherry

# Negative indexing (from end)
print(fruits[-1])  # elderberry
print(fruits[-2])  # date

# Index out of range raises error
# print(fruits[10])  # IndexError
```

### Slicing Lists

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# Basic slicing [start:end]
print(numbers[2:5])    # [2, 3, 4]
print(numbers[:5])     # [0, 1, 2, 3, 4]
print(numbers[5:])     # [5, 6, 7, 8, 9]

# Step parameter [start:end:step]
print(numbers[::2])    # [0, 2, 4, 6, 8]
print(numbers[1::2])   # [1, 3, 5, 7, 9]

# Reverse list
print(numbers[::-1])   # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

# Negative indices in slicing
print(numbers[-5:])    # [5, 6, 7, 8, 9]
print(numbers[:-3])    # [0, 1, 2, 3, 4, 5, 6]
```

---

## List Methods

### append() - Add Element to End

```python
fruits = ["apple", "banana"]
fruits.append("cherry")
print(fruits)  # ['apple', 'banana', 'cherry']

# Append different types
numbers = [1, 2, 3]
numbers.append(4)
numbers.append([5, 6])  # Adds list as single element
print(numbers)  # [1, 2, 3, 4, [5, 6]]
```

### extend() - Add Multiple Elements

```python
fruits = ["apple", "banana"]
fruits.extend(["cherry", "date"])
print(fruits)  # ['apple', 'banana', 'cherry', 'date']

# Extend with any iterable
numbers = [1, 2, 3]
numbers.extend(range(4, 7))
print(numbers)  # [1, 2, 3, 4, 5, 6]

# Extend with string (adds each character)
letters = ['a', 'b']
letters.extend("cd")
print(letters)  # ['a', 'b', 'c', 'd']
```

### insert() - Insert at Specific Position

```python
fruits = ["apple", "cherry"]
fruits.insert(1, "banana")  # Insert at index 1
print(fruits)  # ['apple', 'banana', 'cherry']

# Insert at beginning
numbers = [2, 3, 4]
numbers.insert(0, 1)
print(numbers)  # [1, 2, 3, 4]

# Insert at end (same as append)
numbers.insert(len(numbers), 5)
print(numbers)  # [1, 2, 3, 4, 5]
```

### remove() - Remove First Occurrence

```python
fruits = ["apple", "banana", "cherry", "banana"]
fruits.remove("banana")  # Removes first occurrence
print(fruits)  # ['apple', 'cherry', 'banana']

# Raises ValueError if not found
# fruits.remove("mango")  # ValueError

# Safe removal
if "mango" in fruits:
    fruits.remove("mango")
```

### pop() - Remove and Return Element

```python
fruits = ["apple", "banana", "cherry"]

# Pop last element (default)
last = fruits.pop()
print(last)    # cherry
print(fruits)  # ['apple', 'banana']

# Pop specific index
first = fruits.pop(0)
print(first)   # apple
print(fruits)  # ['banana']

# Pop from empty list raises error
# empty = []
# empty.pop()  # IndexError
```

### clear() - Remove All Elements

```python
fruits = ["apple", "banana", "cherry"]
fruits.clear()
print(fruits)  # []
```

### index() - Find Index of Element

```python
fruits = ["apple", "banana", "cherry", "banana"]

# Find first occurrence
print(fruits.index("banana"))  # 1

# Find with start position
print(fruits.index("banana", 2))  # 3

# Find with start and end
print(fruits.index("cherry", 0, 3))  # 2

# Raises ValueError if not found
# print(fruits.index("mango"))  # ValueError

# Safe index finding
if "mango" in fruits:
    print(fruits.index("mango"))
else:
    print("Not found")
```

### count() - Count Occurrences

```python
numbers = [1, 2, 3, 2, 4, 2, 5]
print(numbers.count(2))  # 3
print(numbers.count(10)) # 0

fruits = ["apple", "banana", "apple", "cherry"]
print(fruits.count("apple"))  # 2
```

### sort() - Sort List In-Place

```python
# Sort numbers ascending
numbers = [3, 1, 4, 1, 5, 9, 2]
numbers.sort()
print(numbers)  # [1, 1, 2, 3, 4, 5, 9]

# Sort descending
numbers.sort(reverse=True)
print(numbers)  # [9, 5, 4, 3, 2, 1, 1]

# Sort strings (alphabetically)
fruits = ["cherry", "apple", "banana"]
fruits.sort()
print(fruits)  # ['apple', 'banana', 'cherry']

# Sort by length
words = ["python", "is", "awesome"]
words.sort(key=len)
print(words)  # ['is', 'python', 'awesome']

# Sort by custom function
students = ["Alice", "bob", "Charlie"]
students.sort(key=str.lower)  # Case-insensitive
print(students)  # ['Alice', 'bob', 'Charlie']
```

### reverse() - Reverse List In-Place

```python
numbers = [1, 2, 3, 4, 5]
numbers.reverse()
print(numbers)  # [5, 4, 3, 2, 1]

# Alternative: slicing (creates new list)
numbers = [1, 2, 3, 4, 5]
reversed_numbers = numbers[::-1]
print(reversed_numbers)  # [5, 4, 3, 2, 1]
```

### copy() - Create Shallow Copy

```python
# Shallow copy
original = [1, 2, 3]
copied = original.copy()
copied.append(4)
print(original)  # [1, 2, 3]
print(copied)    # [1, 2, 3, 4]

# Alternative ways to copy
copied2 = original[:]
copied3 = list(original)

# Shallow copy with nested lists
original = [[1, 2], [3, 4]]
copied = original.copy()
copied[0].append(3)  # Modifies original too!
print(original)  # [[1, 2, 3], [3, 4]]

# Deep copy for nested structures
import copy
original = [[1, 2], [3, 4]]
deep_copied = copy.deepcopy(original)
deep_copied[0].append(3)
print(original)     # [[1, 2], [3, 4]]
print(deep_copied)  # [[1, 2, 3], [3, 4]]
```

---

## List Operations

### Concatenation (+)

```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = list1 + list2
print(combined)  # [1, 2, 3, 4, 5, 6]

# Multiple concatenation
result = [1] + [2, 3] + [4, 5]
print(result)  # [1, 2, 3, 4, 5]
```

### Repetition (*)

```python
numbers = [1, 2, 3]
repeated = numbers * 3
print(repeated)  # [1, 2, 3, 1, 2, 3, 1, 2, 3]

# Create list of zeros
zeros = [0] * 5
print(zeros)  # [0, 0, 0, 0, 0]
```

### Membership (in, not in)

```python
fruits = ["apple", "banana", "cherry"]

print("apple" in fruits)      # True
print("mango" in fruits)      # False
print("mango" not in fruits)  # True
```

### Length (len())

```python
fruits = ["apple", "banana", "cherry"]
print(len(fruits))  # 3

# Empty list
empty = []
print(len(empty))  # 0
```

### Min, Max, Sum

```python
numbers = [3, 1, 4, 1, 5, 9, 2]

print(min(numbers))  # 1
print(max(numbers))  # 9
print(sum(numbers))  # 25

# With strings (lexicographic)
words = ["apple", "banana", "cherry"]
print(min(words))  # apple
print(max(words))  # cherry
```

### Modifying Elements

```python
fruits = ["apple", "banana", "cherry"]

# Modify single element
fruits[1] = "blueberry"
print(fruits)  # ['apple', 'blueberry', 'cherry']

# Modify slice
numbers = [1, 2, 3, 4, 5]
numbers[1:4] = [20, 30, 40]
print(numbers)  # [1, 20, 30, 40, 5]

# Delete elements
del numbers[0]
print(numbers)  # [20, 30, 40, 5]

# Delete slice
del numbers[1:3]
print(numbers)  # [20, 5]
```

---

## List Comprehensions

### Basic Syntax

```python
# Syntax: [expression for item in iterable]

# Create list of squares
squares = [x**2 for x in range(1, 6)]
print(squares)  # [1, 4, 9, 16, 25]

# Convert to uppercase
fruits = ["apple", "banana", "cherry"]
upper_fruits = [fruit.upper() for fruit in fruits]
print(upper_fruits)  # ['APPLE', 'BANANA', 'CHERRY']
```

### With Condition

```python
# Syntax: [expression for item in iterable if condition]

# Even numbers
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = [x for x in numbers if x % 2 == 0]
print(evens)  # [2, 4, 6, 8, 10]

# Long words
words = ["cat", "elephant", "dog", "butterfly"]
long_words = [word for word in words if len(word) > 5]
print(long_words)  # ['elephant', 'butterfly']

# Positive numbers
numbers = [-2, -1, 0, 1, 2, 3]
positives = [x for x in numbers if x > 0]
print(positives)  # [1, 2, 3]
```

### With If-Else

```python
# Syntax: [expression_if_true if condition else expression_if_false for item in iterable]

# Label even/odd
numbers = [1, 2, 3, 4, 5]
labels = ["Even" if x % 2 == 0 else "Odd" for x in numbers]
print(labels)  # ['Odd', 'Even', 'Odd', 'Even', 'Odd']

# Absolute values
numbers = [-2, -1, 0, 1, 2]
absolutes = [x if x >= 0 else -x for x in numbers]
print(absolutes)  # [2, 1, 0, 1, 2]
```

### Nested Comprehensions

```python
# Flatten 2D list
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [num for row in matrix for num in row]
print(flattened)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create multiplication table
table = [[i * j for j in range(1, 6)] for i in range(1, 6)]
print(table)
# [[1, 2, 3, 4, 5],
#  [2, 4, 6, 8, 10],
#  [3, 6, 9, 12, 15],
#  [4, 8, 12, 16, 20],
#  [5, 10, 15, 20, 25]]
```

---

## 2D Lists (Matrices)

### Creating 2D Lists

```python
# Method 1: Literal
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Method 2: List comprehension
rows, cols = 3, 4
matrix = [[0 for j in range(cols)] for i in range(rows)]

# Wrong way (creates references to same list)
# matrix = [[0] * cols] * rows  # Don't do this!
```

### Accessing 2D Elements

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Access element
print(matrix[0][0])  # 1
print(matrix[1][2])  # 6
print(matrix[2][1])  # 8

# Modify element
matrix[1][1] = 50
print(matrix[1])  # [4, 50, 6]
```

### Iterating 2D Lists

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Method 1: Nested loops
for row in matrix:
    for element in row:
        print(element, end=" ")
    print()

# Method 2: With indices
for i in range(len(matrix)):
    for j in range(len(matrix[i])):
        print(matrix[i][j], end=" ")
    print()

# Method 3: Enumerate
for i, row in enumerate(matrix):
    for j, element in enumerate(row):
        print(f"[{i}][{j}] = {element}")
```

---

## Practice Questions

### Beginner Level

**Question 1:** Remove duplicates from a list.

```python
# Solution
numbers = [1, 2, 2, 3, 4, 4, 5]
unique = list(set(numbers))  # or list(dict.fromkeys(numbers))
print(unique)
```

**Question 2:** Find second largest number.

```python
# Solution
numbers = [10, 20, 4, 45, 99, 99]
unique_sorted = sorted(set(numbers), reverse=True)
if len(unique_sorted) >= 2:
    print(f"Second largest: {unique_sorted[1]}")
else:
    print("Not enough unique numbers")
```

**Question 3:** Rotate list by k positions.

```python
# Solution
def rotate_list(lst, k):
    k = k % len(lst)  # Handle k > len(lst)
    return lst[k:] + lst[:k]

numbers = [1, 2, 3, 4, 5]
rotated = rotate_list(numbers, 2)
print(rotated)  # [3, 4, 5, 1, 2]
```

### Intermediate Level

**Question 4:** Merge two sorted lists.

```python
# Solution
def merge_sorted(list1, list2):
    merged = []
    i, j = 0, 0
    
    while i < len(list1) and j < len(list2):
        if list1[i] < list2[j]:
            merged.append(list1[i])
            i += 1
        else:
            merged.append(list2[j])
            j += 1
    
    merged.extend(list1[i:])
    merged.extend(list2[j:])
    return merged

list1 = [1, 3, 5, 7]
list2 = [2, 4, 6, 8]
print(merge_sorted(list1, list2))
```

**Question 5:** Find all pairs that sum to target.

```python
# Solution
def find_pairs(numbers, target):
    pairs = []
    seen = set()
    
    for num in numbers:
        complement = target - num
        if complement in seen:
            pairs.append((complement, num))
        seen.add(num)
    
    return pairs

numbers = [1, 2, 3, 4, 5, 6]
target = 7
print(find_pairs(numbers, target))  # [(1, 6), (2, 5), (3, 4)]
```

**Question 6:** Transpose a matrix.

```python
# Solution
def transpose(matrix):
    rows = len(matrix)
    cols = len(matrix[0])
    transposed = [[matrix[i][j] for i in range(rows)] for j in range(cols)]
    return transposed

matrix = [
    [1, 2, 3],
    [4, 5, 6]
]
print(transpose(matrix))
# [[1, 4],
#  [2, 5],
#  [3, 6]]
```

### Advanced Level

**Question 7:** Implement binary search.

```python
# Solution
def binary_search(sorted_list, target):
    left, right = 0, len(sorted_list) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if sorted_list[mid] == target:
            return mid
        elif sorted_list[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

numbers = [1, 3, 5, 7, 9, 11, 13, 15]
print(binary_search(numbers, 7))  # 3
print(binary_search(numbers, 6))  # -1
```

**Question 8:** Find longest increasing subsequence length.

```python
# Solution
def longest_increasing_subsequence(nums):
    if not nums:
        return 0
    
    dp = [1] * len(nums)
    
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)

numbers = [10, 9, 2, 5, 3, 7, 101, 18]
print(longest_increasing_subsequence(numbers))  # 4
```

**Question 9:** Spiral matrix traversal.

```python
# Solution
def spiral_order(matrix):
    if not matrix:
        return []
    
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    
    while top <= bottom and left <= right:
        # Right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1
        
        # Down
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1
        
        # Left
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1
        
        # Up
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1
    
    return result

matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print(spiral_order(matrix))  # [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

**Question 10:** Shopping Cart Program.

```python
# Solution
class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, name, price, quantity=1):
        self.items.append({"name": name, "price": price, "quantity": quantity})
        print(f"Added {quantity}x {name}")
    
    def remove_item(self, name):
        for item in self.items:
            if item["name"] == name:
                self.items.remove(item)
                print(f"Removed {name}")
                return
        print(f"{name} not found in cart")
    
    def update_quantity(self, name, quantity):
        for item in self.items:
            if item["name"] == name:
                item["quantity"] = quantity
                print(f"Updated {name} quantity to {quantity}")
                return
        print(f"{name} not found in cart")
    
    def get_total(self):
        return sum(item["price"] * item["quantity"] for item in self.items)
    
    def display_cart(self):
        if not self.items:
            print("Cart is empty")
            return
        
        print("\n=== Shopping Cart ===")
        for item in self.items:
            total = item["price"] * item["quantity"]
            print(f"{item['name']}: ${item['price']:.2f} x {item['quantity']} = ${total:.2f}")
        print(f"Total: ${self.get_total():.2f}\n")

# Usage
cart = ShoppingCart()
cart.add_item("Apple", 0.99, 5)
cart.add_item("Banana", 0.59, 3)
cart.add_item("Orange", 1.29, 2)
cart.display_cart()
cart.update_quantity("Apple", 3)
cart.remove_item("Banana")
cart.display_cart()
```

---

## All List Methods Summary

```python
# Adding elements
list.append(x)        # Add x to end
list.extend(iterable) # Add all elements from iterable
list.insert(i, x)     # Insert x at index i

# Removing elements
list.remove(x)        # Remove first occurrence of x
list.pop([i])         # Remove and return element at i (default: last)
list.clear()          # Remove all elements

# Searching
list.index(x[, start[, end]])  # Return index of first occurrence
list.count(x)         # Count occurrences of x

# Sorting and reversing
list.sort(key=None, reverse=False)  # Sort in place
list.reverse()        # Reverse in place

# Copying
list.copy()           # Return shallow copy
```

---

## Key Takeaways

1. **Lists are mutable** - can be modified after creation
2. **Lists are ordered** - maintain insertion order
3. **Lists allow duplicates** - same value can appear multiple times
4. **Indexing starts at 0**, negative indices count from end
5. **Slicing creates new list**, doesn't modify original
6. **List comprehensions** are concise and efficient
7. **sort() modifies in place**, sorted() returns new list
8. **append() vs extend()**: append adds single element, extend adds multiple
9. **Shallow copy** copies references, deep copy copies nested structures
10. **Use list comprehensions** for transformations and filtering

---

## Next Steps

Now that you master lists, move on to:
- **Tuples** (immutable sequences)
- **Sets** (unordered unique elements)
- **Dictionaries** (key-value pairs)
- **Functions** (code reusability)

Keep practicing with list algorithms and data structures!
