# Python Sets - Complete Reference Guide

## Table of Contents
1. [Set Basics](#set-basics)
2. [Set Methods - All Methods](#set-methods)
3. [Set Operations](#set-operations)
4. [Frozen Sets](#frozen-sets)
5. [Set Comprehensions](#set-comprehensions)
6. [Practice Questions](#practice-questions)

---

## Set Basics

### What are Sets?

Sets are **unordered** collections of **unique** elements. They are mutable but can only contain immutable (hashable) elements.

### Creating Sets

```python
# Empty set (must use set(), not {})
empty_set = set()
# empty = {}  # This creates an empty dictionary!

# Set with elements
numbers = {1, 2, 3, 4, 5}
fruits = {"apple", "banana", "cherry"}
mixed = {1, "hello", 3.14, True}

# Set from other iterables
set_from_list = set([1, 2, 3, 2, 1])  # {1, 2, 3} - duplicates removed
set_from_string = set("hello")  # {'h', 'e', 'l', 'o'} - duplicates removed
set_from_tuple = set((1, 2, 3))  # {1, 2, 3}

# Duplicates are automatically removed
numbers = {1, 2, 2, 3, 3, 3, 4}
print(numbers)  # {1, 2, 3, 4}
```

### Set Properties

```python
# Unordered - no indexing
numbers = {1, 2, 3, 4, 5}
# print(numbers[0])  # TypeError: 'set' object is not subscriptable

# Unique elements only
fruits = {"apple", "banana", "apple", "cherry"}
print(fruits)  # {'apple', 'banana', 'cherry'}

# Mutable - can add/remove elements
numbers = {1, 2, 3}
numbers.add(4)
print(numbers)  # {1, 2, 3, 4}

# Can only contain hashable (immutable) elements
# valid_set = {1, 2, 3}
# invalid_set = {[1, 2], [3, 4]}  # TypeError: unhashable type: 'list'
```

---

## Set Methods

### add() - Add Single Element

```python
fruits = {"apple", "banana"}
fruits.add("cherry")
print(fruits)  # {'apple', 'banana', 'cherry'}

# Adding duplicate has no effect
fruits.add("apple")
print(fruits)  # {'apple', 'banana', 'cherry'}

# Add different types
mixed = {1, 2, 3}
mixed.add("four")
mixed.add(5.0)
print(mixed)  # {1, 2, 3, 'four', 5.0}
```

### update() - Add Multiple Elements

```python
fruits = {"apple", "banana"}
fruits.update(["cherry", "date"])
print(fruits)  # {'apple', 'banana', 'cherry', 'date'}

# Update with multiple iterables
numbers = {1, 2, 3}
numbers.update([4, 5], {6, 7}, (8, 9))
print(numbers)  # {1, 2, 3, 4, 5, 6, 7, 8, 9}

# Update with string (adds each character)
letters = {'a', 'b'}
letters.update("cd")
print(letters)  # {'a', 'b', 'c', 'd'}
```

### remove() - Remove Element (Raises Error if Not Found)

```python
fruits = {"apple", "banana", "cherry"}
fruits.remove("banana")
print(fruits)  # {'apple', 'cherry'}

# Raises KeyError if element doesn't exist
# fruits.remove("mango")  # KeyError: 'mango'

# Safe removal with check
if "mango" in fruits:
    fruits.remove("mango")
```

### discard() - Remove Element (No Error if Not Found)

```python
fruits = {"apple", "banana", "cherry"}
fruits.discard("banana")
print(fruits)  # {'apple', 'cherry'}

# No error if element doesn't exist
fruits.discard("mango")  # No error
print(fruits)  # {'apple', 'cherry'}
```

### pop() - Remove and Return Random Element

```python
fruits = {"apple", "banana", "cherry"}
removed = fruits.pop()
print(f"Removed: {removed}")
print(f"Remaining: {fruits}")

# Pop from empty set raises error
# empty = set()
# empty.pop()  # KeyError: 'pop from an empty set'
```

### clear() - Remove All Elements

```python
fruits = {"apple", "banana", "cherry"}
fruits.clear()
print(fruits)  # set()
```

### copy() - Create Shallow Copy

```python
original = {1, 2, 3}
copied = original.copy()
copied.add(4)
print(original)  # {1, 2, 3}
print(copied)    # {1, 2, 3, 4}

# Alternative way to copy
copied2 = set(original)
```

---

## Set Operations

### union() or | - Combine Sets

```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# Using method
result = set1.union(set2)
print(result)  # {1, 2, 3, 4, 5}

# Using operator
result = set1 | set2
print(result)  # {1, 2, 3, 4, 5}

# Union of multiple sets
set3 = {5, 6, 7}
result = set1.union(set2, set3)
print(result)  # {1, 2, 3, 4, 5, 6, 7}

# Original sets unchanged
print(set1)  # {1, 2, 3}
```

### intersection() or & - Common Elements

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

# Using method
result = set1.intersection(set2)
print(result)  # {3, 4}

# Using operator
result = set1 & set2
print(result)  # {3, 4}

# Intersection of multiple sets
set3 = {3, 4, 7, 8}
result = set1.intersection(set2, set3)
print(result)  # {3, 4}
```

### difference() or - - Elements in First but Not Second

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

# Using method
result = set1.difference(set2)
print(result)  # {1, 2}

# Using operator
result = set1 - set2
print(result)  # {1, 2}

# Reverse difference
result = set2 - set1
print(result)  # {5, 6}

# Difference with multiple sets
set3 = {2, 3}
result = set1.difference(set2, set3)
print(result)  # {1}
```

### symmetric_difference() or ^ - Elements in Either but Not Both

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

# Using method
result = set1.symmetric_difference(set2)
print(result)  # {1, 2, 5, 6}

# Using operator
result = set1 ^ set2
print(result)  # {1, 2, 5, 6}
```

### In-Place Operations

```python
# update() or |= - Add elements from another set
set1 = {1, 2, 3}
set1.update({3, 4, 5})  # or set1 |= {3, 4, 5}
print(set1)  # {1, 2, 3, 4, 5}

# intersection_update() or &= - Keep only common elements
set1 = {1, 2, 3, 4}
set1.intersection_update({3, 4, 5, 6})  # or set1 &= {3, 4, 5, 6}
print(set1)  # {3, 4}

# difference_update() or -= - Remove elements found in another set
set1 = {1, 2, 3, 4}
set1.difference_update({3, 4, 5})  # or set1 -= {3, 4, 5}
print(set1)  # {1, 2}

# symmetric_difference_update() or ^= - Keep elements in either but not both
set1 = {1, 2, 3, 4}
set1.symmetric_difference_update({3, 4, 5, 6})  # or set1 ^= {3, 4, 5, 6}
print(set1)  # {1, 2, 5, 6}
```

### Set Relationships

```python
# issubset() or <= - Check if subset
set1 = {1, 2}
set2 = {1, 2, 3, 4}
print(set1.issubset(set2))  # True
print(set1 <= set2)  # True

# Proper subset (<) - subset but not equal
print(set1 < set2)  # True
print(set2 < set2)  # False

# issuperset() or >= - Check if superset
print(set2.issuperset(set1))  # True
print(set2 >= set1)  # True

# Proper superset (>) - superset but not equal
print(set2 > set1)  # True
print(set2 > set2)  # False

# isdisjoint() - Check if no common elements
set1 = {1, 2, 3}
set2 = {4, 5, 6}
print(set1.isdisjoint(set2))  # True

set3 = {3, 4, 5}
print(set1.isdisjoint(set3))  # False (3 is common)
```

---

## Frozen Sets

Frozen sets are **immutable** sets - cannot be modified after creation.

### Creating Frozen Sets

```python
# Create frozen set
frozen = frozenset([1, 2, 3, 4, 5])
print(frozen)  # frozenset({1, 2, 3, 4, 5})

# From string
frozen_chars = frozenset("hello")
print(frozen_chars)  # frozenset({'h', 'e', 'l', 'o'})

# Empty frozen set
empty_frozen = frozenset()
```

### Frozen Set Properties

```python
frozen = frozenset([1, 2, 3])

# Cannot modify
# frozen.add(4)  # AttributeError: 'frozenset' object has no attribute 'add'
# frozen.remove(1)  # AttributeError

# Can be used as dictionary key (hashable)
my_dict = {
    frozenset([1, 2]): "value1",
    frozenset([3, 4]): "value2"
}
print(my_dict[frozenset([1, 2])])  # value1

# Can be element of set
set_of_sets = {frozenset([1, 2]), frozenset([3, 4])}
print(set_of_sets)
```

### Frozen Set Operations

```python
frozen1 = frozenset([1, 2, 3])
frozen2 = frozenset([3, 4, 5])

# All read-only operations work
print(frozen1 | frozen2)  # frozenset({1, 2, 3, 4, 5})
print(frozen1 & frozen2)  # frozenset({3})
print(frozen1 - frozen2)  # frozenset({1, 2})
print(frozen1 ^ frozen2)  # frozenset({1, 2, 4, 5})

# Relationship checks
print(frozenset([1, 2]).issubset(frozen1))  # True
```

---

## Set Comprehensions

### Basic Syntax

```python
# Syntax: {expression for item in iterable}

# Create set of squares
squares = {x**2 for x in range(1, 6)}
print(squares)  # {1, 4, 9, 16, 25}

# Convert to uppercase
fruits = ["apple", "banana", "cherry"]
upper_fruits = {fruit.upper() for fruit in fruits}
print(upper_fruits)  # {'APPLE', 'BANANA', 'CHERRY'}
```

### With Condition

```python
# Syntax: {expression for item in iterable if condition}

# Even numbers
numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
evens = {x for x in numbers if x % 2 == 0}
print(evens)  # {2, 4, 6, 8, 10}

# Long words
words = ["cat", "elephant", "dog", "butterfly", "ant"]
long_words = {word for word in words if len(word) > 5}
print(long_words)  # {'elephant', 'butterfly'}

# Positive numbers
numbers = {-2, -1, 0, 1, 2, 3}
positives = {x for x in numbers if x > 0}
print(positives)  # {1, 2, 3}
```

### With Transformation

```python
# Absolute values
numbers = {-2, -1, 0, 1, 2}
absolutes = {abs(x) for x in numbers}
print(absolutes)  # {0, 1, 2}

# First characters
words = ["apple", "banana", "cherry", "apricot"]
first_chars = {word[0] for word in words}
print(first_chars)  # {'a', 'b', 'c'}

# Lengths
words = ["cat", "elephant", "dog"]
lengths = {len(word) for word in words}
print(lengths)  # {3, 8}
```

---

## Common Set Operations

### Remove Duplicates from List

```python
numbers = [1, 2, 2, 3, 4, 4, 5]
unique = list(set(numbers))
print(unique)  # [1, 2, 3, 4, 5] (order may vary)

# Preserve order (Python 3.7+)
unique_ordered = list(dict.fromkeys(numbers))
print(unique_ordered)  # [1, 2, 3, 4, 5]
```

### Find Common Elements

```python
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]
common = list(set(list1) & set(list2))
print(common)  # [4, 5]
```

### Find Unique Elements

```python
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]
unique_to_list1 = list(set(list1) - set(list2))
print(unique_to_list1)  # [1, 2, 3]
```

### Check Membership (Fast)

```python
# Sets are much faster for membership testing
large_list = list(range(1000000))
large_set = set(large_list)

# This is slow
# 999999 in large_list  # O(n)

# This is fast
# 999999 in large_set  # O(1)
```

---

## Practice Questions

### Beginner Level

**Question 1:** Remove duplicates from a list using sets.

```python
# Solution
numbers = [1, 2, 2, 3, 4, 4, 5, 5, 5]
unique = list(set(numbers))
print(unique)
```

**Question 2:** Find common elements between two lists.

```python
# Solution
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]
common = list(set(list1) & set(list2))
print(common)  # [4, 5]
```

**Question 3:** Check if two sets have any common elements.

```python
# Solution
set1 = {1, 2, 3}
set2 = {4, 5, 6}
has_common = not set1.isdisjoint(set2)
print(has_common)  # False
```

### Intermediate Level

**Question 4:** Find elements that are in either set but not in both.

```python
# Solution
set1 = {1, 2, 3, 4, 5}
set2 = {4, 5, 6, 7, 8}
symmetric_diff = set1 ^ set2
print(symmetric_diff)  # {1, 2, 3, 6, 7, 8}
```

**Question 5:** Count unique words in a sentence.

```python
# Solution
sentence = "the quick brown fox jumps over the lazy dog"
words = sentence.split()
unique_words = set(words)
print(f"Unique words: {len(unique_words)}")  # 8
```

**Question 6:** Find all unique characters in a string.

```python
# Solution
text = "hello world"
unique_chars = set(text.replace(" ", ""))
print(unique_chars)  # {'h', 'e', 'l', 'o', 'w', 'r', 'd'}
print(f"Count: {len(unique_chars)}")  # 7
```

### Advanced Level

**Question 7:** Implement a simple spell checker.

```python
# Solution
dictionary = {"hello", "world", "python", "programming", "code"}

def spell_check(text):
    words = text.lower().split()
    misspelled = []
    
    for word in words:
        # Remove punctuation
        clean_word = ''.join(c for c in word if c.isalnum())
        if clean_word and clean_word not in dictionary:
            misspelled.append(clean_word)
    
    return set(misspelled)  # Return unique misspelled words

text = "hello wrld python programing code test"
errors = spell_check(text)
print(f"Misspelled: {errors}")  # {'wrld', 'programing', 'test'}
```

**Question 8:** Find all subsets of a set (power set).

```python
# Solution
def power_set(s):
    s = list(s)
    result = []
    
    # Generate all combinations
    for i in range(2 ** len(s)):
        subset = []
        for j in range(len(s)):
            if i & (1 << j):
                subset.append(s[j])
        result.append(frozenset(subset))
    
    return set(result)

original = {1, 2, 3}
subsets = power_set(original)
print(f"Number of subsets: {len(subsets)}")  # 8
for subset in sorted(subsets, key=len):
    print(subset)
```

**Question 9:** Find students enrolled in multiple courses.

```python
# Solution
math_students = {"Alice", "Bob", "Charlie", "David"}
science_students = {"Bob", "David", "Eve", "Frank"}
english_students = {"Alice", "David", "Frank", "Grace"}

# Students in all three courses
all_three = math_students & science_students & english_students
print(f"All three courses: {all_three}")  # {'David'}

# Students in at least two courses
in_math_and_science = math_students & science_students
in_math_and_english = math_students & english_students
in_science_and_english = science_students & english_students

at_least_two = in_math_and_science | in_math_and_english | in_science_and_english
print(f"At least two courses: {at_least_two}")

# Students in only one course
all_students = math_students | science_students | english_students
only_one = all_students - at_least_two
print(f"Only one course: {only_one}")
```

**Question 10:** Implement set-based data deduplication.

```python
# Solution
class DataDeduplicator:
    def __init__(self):
        self.seen = set()
        self.duplicates = []
    
    def add_record(self, record):
        # Convert record to hashable type
        record_hash = frozenset(record.items()) if isinstance(record, dict) else record
        
        if record_hash in self.seen:
            self.duplicates.append(record)
            return False  # Duplicate
        else:
            self.seen.add(record_hash)
            return True  # New record
    
    def get_stats(self):
        return {
            "unique": len(self.seen),
            "duplicates": len(self.duplicates)
        }

# Usage
dedup = DataDeduplicator()
records = [
    {"id": 1, "name": "Alice"},
    {"id": 2, "name": "Bob"},
    {"id": 1, "name": "Alice"},  # Duplicate
    {"id": 3, "name": "Charlie"},
    {"id": 2, "name": "Bob"},  # Duplicate
]

for record in records:
    is_new = dedup.add_record(record)
    print(f"{record}: {'New' if is_new else 'Duplicate'}")

print(f"\nStats: {dedup.get_stats()}")
```

---

## All Set Methods Summary

```python
# Adding elements
set.add(x)              # Add single element
set.update(iterable)    # Add multiple elements

# Removing elements
set.remove(x)           # Remove x (raises KeyError if not found)
set.discard(x)          # Remove x (no error if not found)
set.pop()               # Remove and return random element
set.clear()             # Remove all elements

# Set operations
set.union(other)                    # Return union
set.intersection(other)             # Return intersection
set.difference(other)               # Return difference
set.symmetric_difference(other)     # Return symmetric difference

# In-place operations
set.update(other)                   # Add elements from other
set.intersection_update(other)      # Keep only common elements
set.difference_update(other)        # Remove elements in other
set.symmetric_difference_update(other)  # Keep elements in either but not both

# Relationships
set.issubset(other)     # Check if subset
set.issuperset(other)   # Check if superset
set.isdisjoint(other)   # Check if no common elements

# Copying
set.copy()              # Return shallow copy
```

---

## Key Takeaways

1. **Sets are unordered** - no indexing or slicing
2. **Sets contain unique elements** - duplicates automatically removed
3. **Sets are mutable** - can add/remove elements
4. **Set elements must be hashable** - immutable types only
5. **Fast membership testing** - O(1) average case
6. **Set operations** - union, intersection, difference, symmetric difference
7. **Frozen sets are immutable** - can be dictionary keys
8. **Set comprehensions** - concise set creation
9. **Use sets** for uniqueness, membership testing, and mathematical operations
10. **Empty set** must use set(), not {}

---

## Common Mistakes

```python
# Mistake 1: Creating empty set with {}
empty = {}  # This is a dictionary!
empty = set()  # Correct

# Mistake 2: Trying to index a set
numbers = {1, 2, 3}
# print(numbers[0])  # TypeError

# Mistake 3: Adding unhashable elements
# my_set = {[1, 2], [3, 4]}  # TypeError: unhashable type: 'list'
my_set = {(1, 2), (3, 4)}  # Correct (tuples are hashable)

# Mistake 4: Expecting order
numbers = {3, 1, 4, 1, 5, 9, 2}
print(numbers)  # Order is not guaranteed

# Mistake 5: Modifying set during iteration
numbers = {1, 2, 3, 4, 5}
# for num in numbers:
#     numbers.remove(num)  # RuntimeError
```

---

## Next Steps

Now that you understand sets, move on to:
- **Dictionaries** (key-value pairs)
- **Functions** (code reusability)
- **Advanced data structures**
- **Algorithm optimization with sets**

Keep practicing with set operations and mathematical problems!
