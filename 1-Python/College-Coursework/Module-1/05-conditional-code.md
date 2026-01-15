# Conditional Code

## 📚 Theoretical Understanding

### What is Conditional Code?

Conditional code is the foundation of decision-making in programming. It allows programs to execute different actions based on whether certain conditions are true or false. Without conditional logic, programs would be linear sequences of instructions that always execute the same way, regardless of input or circumstances. Conditional code gives programs the ability to respond dynamically to different situations, making them intelligent and adaptive.

Think of conditional code as the "if-then" reasoning we use in everyday life: "If it's raining, take an umbrella." "If the traffic light is red, stop." "If I'm hungry, eat something." Programming conditionals work the same way - they evaluate a condition (a statement that can be true or false) and execute different code blocks based on the result. This simple concept is incredibly powerful and forms the basis of all program logic.

In Python, conditional code is implemented primarily through `if`, `elif` (else-if), and `else` statements. These keywords allow you to create branching logic where the program can take different paths through the code. The condition being evaluated must be a boolean expression - something that evaluates to either `True` or `False`. Python's clean syntax makes conditional code highly readable, with indentation clearly showing which code belongs to which condition.

### Boolean Logic and Truth Values

Understanding boolean logic is essential for writing effective conditional code. Boolean logic deals with truth values - statements that are either true or false, with no middle ground. In Python, these are represented by the keywords `True` and `False` (note the capital letters - Python is case-sensitive).

Boolean expressions are created using comparison operators and logical operators. Comparison operators (`==`, `!=`, `<`, `>`, `<=`, `>=`) compare two values and return a boolean result. For example, `5 > 3` evaluates to `True`, while `10 == 20` evaluates to `False`. These comparisons form the conditions that control program flow.

Logical operators (`and`, `or`, `not`) combine multiple boolean expressions into more complex conditions. The `and` operator returns `True` only if both conditions are true. The `or` operator returns `True` if at least one condition is true. The `not` operator inverts a boolean value, turning `True` into `False` and vice versa. These operators allow you to express sophisticated logic: "If the user is logged in AND has admin privileges, show the admin panel."

Python also has the concept of "truthiness" - values that aren't explicitly `True` or `False` but are treated as such in boolean contexts. Empty collections (empty lists, empty strings, empty dictionaries) are "falsy" (treated as `False`), while non-empty collections are "truthy" (treated as `True`). The number `0` is falsy, while any non-zero number is truthy. `None` (Python's null value) is falsy. This truthiness concept makes conditional code more concise and Pythonic.

### Control Flow and Program Execution

Control flow refers to the order in which individual statements, instructions, or function calls are executed in a program. Without conditionals, control flow is sequential - the program executes line by line from top to bottom. Conditional statements introduce branching - the ability for the program to skip certain sections of code or choose between alternative paths.

When Python encounters an `if` statement, it evaluates the condition. If the condition is `True`, Python executes the indented code block beneath the `if`. If the condition is `False`, Python skips that block and moves to the next part of the program (either an `elif`, an `else`, or whatever comes after the conditional structure). This decision-making happens at runtime, meaning the path taken through the code can change each time the program runs, depending on the conditions.

Understanding control flow is crucial for debugging and reasoning about program behavior. When a program doesn't behave as expected, tracing the control flow - determining which conditions were true or false and which code blocks were executed - is often the key to finding the bug. Good programmers develop a mental model of how control flows through their code under different conditions.

### The Importance of Indentation in Python

Python uses indentation (whitespace at the beginning of lines) to define code blocks, unlike many languages that use curly braces `{}` or keywords like `begin`/`end`. This design choice is controversial but serves Python's philosophy of readability. The indentation visually shows the structure of the code - which statements belong to which conditional block.

Indentation in Python isn't just for aesthetics - it's syntactically significant. The indented code after an `if` statement is the code that executes when the condition is true. When the indentation returns to the previous level, that marks the end of the conditional block. This forced consistency means all Python code has a similar visual structure, making it easier to read code written by others.

The standard indentation in Python is 4 spaces (not tabs, though many editors convert tabs to spaces automatically). Mixing tabs and spaces can cause errors. Inconsistent indentation will cause Python to raise an `IndentationError`. While this strictness can frustrate beginners, it enforces good coding practices and ensures that code structure is always clear and unambiguous.

### Comparison Operators and Their Behavior

Comparison operators are the building blocks of conditional expressions. Python provides six main comparison operators, each serving a specific purpose:

- `==` (equal to): Checks if two values are equal
- `!=` (not equal to): Checks if two values are different
- `<` (less than): Checks if left value is smaller than right value
- `>` (greater than): Checks if left value is larger than right value
- `<=` (less than or equal to): Checks if left value is smaller than or equal to right value
- `>=` (greater than or equal to): Checks if left value is larger than or equal to right value

These operators work with numbers, strings, and other comparable types. When comparing strings, Python uses lexicographic (dictionary) order based on Unicode values. When comparing numbers, Python handles mixed types intelligently - `5 == 5.0` is `True` even though one is an integer and one is a float, because their numeric values are equal.

Python also supports comparison chaining, a feature not found in many languages. Instead of writing `x > 0 and x < 10`, you can write `0 < x < 10`, which is more readable and closer to mathematical notation. This chaining works with any comparison operators and can include multiple comparisons: `a < b <= c < d`.

---

## 💻 Practical Examples

### Basic If Statements

```python
# Example 1: Simple if statement
age = 18

if age >= 18:
    print("You are an adult")
    print("You can vote")

print("This always executes")

# Example 2: If with else
temperature = 25

if temperature > 30:
    print("It's hot outside")
else:
    print("Temperature is comfortable")

# Example 3: Multiple conditions with elif
score = 85

if score >= 90:
    print("Grade: A")
elif score >= 80:
    print("Grade: B")
elif score >= 70:
    print("Grade: C")
elif score >= 60:
    print("Grade: D")
else:
    print("Grade: F")
```

### Comparison Operators

```python
# Numeric comparisons
x = 10
y = 20

print(x == y)   # False (equal to)
print(x != y)   # True (not equal to)
print(x < y)    # True (less than)
print(x > y)    # False (greater than)
print(x <= 10)  # True (less than or equal)
print(y >= 20)  # True (greater than or equal)

# String comparisons
name1 = "Alice"
name2 = "Bob"

print(name1 == name2)  # False
print(name1 < name2)   # True (lexicographic order)

# Comparison chaining
age = 25
if 18 <= age < 65:
    print("Working age")

# Equivalent to:
if age >= 18 and age < 65:
    print("Working age")
```

### Logical Operators

```python
# AND operator - both conditions must be true
age = 25
has_license = True

if age >= 18 and has_license:
    print("You can drive")

# OR operator - at least one condition must be true
is_weekend = True
is_holiday = False

if is_weekend or is_holiday:
    print("No work today!")

# NOT operator - inverts boolean value
is_raining = False

if not is_raining:
    print("You don't need an umbrella")

# Complex conditions
temperature = 28
is_sunny = True
has_sunscreen = False

if temperature > 25 and is_sunny and not has_sunscreen:
    print("Warning: Apply sunscreen!")
```

### Truthiness and Falsy Values

```python
# Empty collections are falsy
empty_list = []
if not empty_list:
    print("List is empty")

# Non-empty collections are truthy
numbers = [1, 2, 3]
if numbers:
    print("List has items")

# Empty string is falsy
name = ""
if not name:
    print("Name is empty")

# Zero is falsy
count = 0
if not count:
    print("Count is zero")

# None is falsy
result = None
if result is None:
    print("No result")

# Practical example: checking user input
user_input = input("Enter your name: ")

if user_input:  # Truthy if not empty
    print(f"Hello, {user_input}!")
else:
    print("You didn't enter a name")
```

### Nested Conditionals

```python
# Example: Login system
username = "admin"
password = "secret123"
is_active = True

if username == "admin":
    if password == "secret123":
        if is_active:
            print("Login successful")
        else:
            print("Account is deactivated")
    else:
        print("Incorrect password")
else:
    print("User not found")

# Better approach: flatten with logical operators
if username == "admin" and password == "secret123" and is_active:
    print("Login successful")
elif username == "admin" and password == "secret123":
    print("Account is deactivated")
elif username == "admin":
    print("Incorrect password")
else:
    print("User not found")
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain the difference between `if-elif-else` and multiple separate `if` statements. When should you use each approach? Provide examples demonstrating scenarios where the choice matters.

**Detailed Answer:**

The difference between `if-elif-else` chains and multiple separate `if` statements is fundamental to understanding control flow in Python. While they might seem similar at first glance, they behave very differently and are suited for different scenarios.

**If-Elif-Else Chain: Mutually Exclusive Conditions**

An `if-elif-else` chain evaluates conditions sequentially and executes only the first block whose condition is true. Once a condition is satisfied, Python skips all remaining conditions in the chain, even if they would also be true.

```python
# Example 1: Grade calculation (mutually exclusive)
score = 85

if score >= 90:
    grade = "A"
    print("Excellent!")
elif score >= 80:  # Only checked if score < 90
    grade = "B"
    print("Good job!")
elif score >= 70:  # Only checked if score < 80
    grade = "C"
    print("Satisfactory")
elif score >= 60:  # Only checked if score < 70
    grade = "D"
    print("Needs improvement")
else:  # Only executed if all above are false
    grade = "F"
    print("Failed")

print(f"Your grade is: {grade}")

# With score = 85:
# - First condition (>= 90) is False, skip
# - Second condition (>= 80) is True, execute this block
# - All remaining conditions are skipped
# Output: "Good job!" and "Your grade is: B"
```

**Multiple Separate If Statements: Independent Conditions**

Multiple separate `if` statements evaluate each condition independently. All conditions are checked, and multiple blocks can execute if multiple conditions are true.

```python
# Example 2: Multiple independent checks
score = 85

if score >= 90:
    print("Excellent!")

if score >= 80:  # Always checked, regardless of above
    print("Good job!")

if score >= 70:  # Always checked
    print("Satisfactory")

if score >= 60:  # Always checked
    print("Needs improvement")

# With score = 85:
# - First condition (>= 90) is False, skip
# - Second condition (>= 80) is True, execute
# - Third condition (>= 70) is True, execute
# - Fourth condition (>= 60) is True, execute
# Output: "Good job!", "Satisfactory", "Needs improvement"
# This is probably NOT what we want for grades!
```

**When to Use If-Elif-Else:**

Use `if-elif-else` when conditions are mutually exclusive - only one should be true and execute:

```python
# Scenario 1: Categorization (only one category applies)
age = 25

if age < 13:
    category = "Child"
elif age < 20:
    category = "Teenager"
elif age < 65:
    category = "Adult"
else:
    category = "Senior"

print(f"Category: {category}")

# Scenario 2: Menu selection (only one option chosen)
choice = input("Choose (1) Save, (2) Load, (3) Exit: ")

if choice == "1":
    print("Saving...")
    save_game()
elif choice == "2":
    print("Loading...")
    load_game()
elif choice == "3":
    print("Exiting...")
    exit_game()
else:
    print("Invalid choice")

# Scenario 3: Priority-based decisions
balance = 1500
withdrawal = 2000

if balance >= withdrawal:
    print("Withdrawal approved")
    balance -= withdrawal
elif balance > 0:
    print(f"Insufficient funds. Available: ${balance}")
else:
    print("Account is empty")
```

**When to Use Multiple If Statements:**

Use multiple separate `if` statements when conditions are independent - multiple can be true simultaneously:

```python
# Scenario 1: Multiple validations (all should be checked)
password = "abc123"

if len(password) < 8:
    print("Error: Password must be at least 8 characters")

if not any(c.isupper() for c in password):
    print("Error: Password must contain uppercase letter")

if not any(c.isdigit() for c in password):
    print("Error: Password must contain a digit")

if not any(c in "!@#$%^&*" for c in password):
    print("Error: Password must contain special character")

# All conditions are checked, user sees all errors at once

# Scenario 2: Multiple features/flags
user_permissions = {
    'can_read': True,
    'can_write': True,
    'can_delete': False,
    'is_admin': False
}

if user_permissions['can_read']:
    print("✓ Read access granted")

if user_permissions['can_write']:
    print("✓ Write access granted")

if user_permissions['can_delete']:
    print("✓ Delete access granted")

if user_permissions['is_admin']:
    print("✓ Admin privileges granted")

# Each permission is independent

# Scenario 3: Multiple alerts/warnings
temperature = 35
humidity = 80
air_quality = 150

if temperature > 30:
    print("⚠ Heat warning")

if humidity > 70:
    print("⚠ High humidity")

if air_quality > 100:
    print("⚠ Poor air quality")

# All conditions should be checked and reported
```

**Performance Considerations:**

```python
# If-elif-else is more efficient when conditions are mutually exclusive
import time

# Scenario: Checking a value against ranges
value = 95

# Method 1: If-elif-else (efficient)
start = time.time()
for _ in range(1000000):
    if value >= 90:
        result = "A"
    elif value >= 80:
        result = "B"
    elif value >= 70:
        result = "C"
    else:
        result = "F"
end = time.time()
print(f"If-elif-else: {end - start:.4f} seconds")

# Method 2: Multiple ifs (inefficient for this case)
start = time.time()
for _ in range(1000000):
    if value >= 90:
        result = "A"
    if value >= 80:  # Checked even though we know value >= 90
        result = "B"
    if value >= 70:  # Checked even though we know value >= 90
        result = "C"
    if value < 70:
        result = "F"
end = time.time()
print(f"Multiple ifs: {end - start:.4f} seconds")

# If-elif-else is faster because it stops after first match
```

**Common Mistake: Using Multiple Ifs When Elif is Needed**

```python
# WRONG: Multiple ifs for mutually exclusive conditions
score = 85
grade = ""

if score >= 90:
    grade = "A"

if score >= 80:  # This executes even though we want only one grade
    grade = "B"  # Overwrites previous grade!

if score >= 70:
    grade = "C"  # Overwrites again!

print(f"Grade: {grade}")  # Output: C (WRONG! Should be B)

# CORRECT: If-elif-else for mutually exclusive conditions
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

print(f"Grade: {grade}")  # Output: B (CORRECT!)
```

**Decision Tree:**

```python
# Use this decision tree to choose:

def choose_conditional_structure(conditions):
    """
    Decision tree for choosing conditional structure
    """
    if conditions_are_mutually_exclusive():
        return "Use if-elif-else"
    elif need_to_execute_multiple_blocks():
        return "Use multiple if statements"
    elif need_default_action():
        return "Use if-elif-else with else clause"
    else:
        return "Use multiple if statements"

# Examples:
# - Categorizing age groups → if-elif-else (mutually exclusive)
# - Checking multiple permissions → multiple ifs (independent)
# - Menu selection → if-elif-else (only one choice)
# - Form validation → multiple ifs (show all errors)
# - Priority-based decisions → if-elif-else (first match wins)
```

**Key Takeaways:**

1. **If-elif-else**: Use when only one condition should execute (mutually exclusive)
2. **Multiple ifs**: Use when multiple conditions can be true simultaneously (independent)
3. **Performance**: If-elif-else is more efficient for mutually exclusive conditions
4. **Readability**: If-elif-else clearly shows mutual exclusivity
5. **Debugging**: Multiple ifs can lead to unexpected behavior if used incorrectly

---

### Question 2: Explain Python's concept of "truthiness" and "falsy" values. Why is `if value:` different from `if value == True:`? Provide examples showing when truthiness is useful and when explicit comparison is necessary.

**Detailed Answer:**

Python's concept of truthiness is one of its most powerful and Pythonic features, but it can also be confusing for beginners. Understanding truthiness is essential for writing idiomatic Python code and avoiding subtle bugs.

**What is Truthiness?**

In Python, every value has an inherent boolean interpretation - it's either "truthy" (treated as `True` in boolean contexts) or "falsy" (treated as `False` in boolean contexts). This means you can use any value in a conditional statement, not just explicit `True` or `False` values.

**Falsy Values (Treated as False):**

```python
# These values are falsy:
falsy_values = [
    False,          # Boolean False
    None,           # None type
    0,              # Zero integer
    0.0,            # Zero float
    0j,             # Zero complex number
    '',             # Empty string
    "",             # Empty string (double quotes)
    [],             # Empty list
    (),             # Empty tuple
    {},             # Empty dictionary
    set(),          # Empty set
    range(0),       # Empty range
]

# Testing falsy values
for value in falsy_values:
    if not value:
        print(f"{repr(value)} is falsy")

# All print as falsy
```

**Truthy Values (Treated as True):**

```python
# Everything else is truthy:
truthy_values = [
    True,           # Boolean True
    1,              # Non-zero integer
    -1,             # Negative numbers
    3.14,           # Non-zero float
    "hello",        # Non-empty string
    " ",            # String with space (not empty!)
    [1, 2, 3],      # Non-empty list
    [0],            # List with zero (list itself is not empty!)
    (1,),           # Non-empty tuple
    {'key': 'value'}, # Non-empty dictionary
    {0},            # Set with zero (set itself is not empty!)
]

# Testing truthy values
for value in truthy_values:
    if value:
        print(f"{repr(value)} is truthy")

# All print as truthy
```

**Why `if value:` is Different from `if value == True:`**

This is a crucial distinction that trips up many beginners:

```python
# Example 1: The difference
value = "hello"

# Method 1: Truthiness check
if value:
    print("value is truthy")  # This executes

# Method 2: Explicit comparison to True
if value == True:
    print("value equals True")  # This does NOT execute!

# Why? "hello" is truthy, but it's not equal to True
print(value == True)  # False
print(bool(value))    # True (truthy)

# Example 2: With numbers
number = 1

if number:
    print("number is truthy")  # Executes

if number == True:
    print("number equals True")  # Also executes! (1 == True is True)

# But:
number = 2

if number:
    print("number is truthy")  # Executes

if number == True:
    print("number equals True")  # Does NOT execute! (2 != True)

# Example 3: With lists
my_list = [1, 2, 3]

if my_list:
    print("list is truthy")  # Executes

if my_list == True:
    print("list equals True")  # Does NOT execute!
```

**When to Use Truthiness:**

**1. Checking if Collections are Empty**

```python
# PYTHONIC: Use truthiness
items = []

if not items:
    print("No items")

# NON-PYTHONIC: Explicit length check
if len(items) == 0:
    print("No items")

# Truthiness is clearer and more Pythonic

# Example: Processing list
def process_items(items):
    if not items:
        print("Nothing to process")
        return
    
    for item in items:
        print(f"Processing: {item}")

process_items([])  # "Nothing to process"
process_items([1, 2, 3])  # Processes items
```

**2. Checking if String is Empty**

```python
# PYTHONIC: Use truthiness
name = input("Enter your name: ")

if not name:
    print("Name cannot be empty")
else:
    print(f"Hello, {name}!")

# NON-PYTHONIC: Explicit comparison
if name == "":
    print("Name cannot be empty")

# Truthiness is more readable
```

**3. Checking if Value Exists (Not None)**

```python
# PYTHONIC: Use truthiness
result = find_user("alice")

if result:
    print(f"Found user: {result}")
else:
    print("User not found")

# This works when find_user returns None for not found
# and a user object (truthy) when found
```

**4. Default Values and Optional Parameters**

```python
def greet(name=None):
    # PYTHONIC: Use truthiness
    if not name:
        name = "Guest"
    
    print(f"Hello, {name}!")

greet()  # "Hello, Guest!"
greet("Alice")  # "Hello, Alice!"

# Example: Configuration with defaults
def configure(settings=None):
    if not settings:
        settings = {'theme': 'dark', 'language': 'en'}
    
    return settings
```

**When to Use Explicit Comparison:**

**1. When Zero is a Valid Value**

```python
# WRONG: Using truthiness when zero is valid
count = 0

if not count:
    print("No items")  # This executes, but zero might be valid!

# CORRECT: Explicit comparison to None
count = 0

if count is None:
    print("Count not set")
else:
    print(f"Count: {count}")  # "Count: 0"

# Example: Age validation
age = 0  # Baby's age

# WRONG
if not age:
    print("Age not provided")  # Wrong! Age is 0, which is valid

# CORRECT
if age is None:
    print("Age not provided")
else:
    print(f"Age: {age}")  # "Age: 0"
```

**2. When Distinguishing Between False and None**

```python
# Example: API response
def check_permission(user):
    # Returns: True (allowed), False (denied), None (user not found)
    if user == "admin":
        return True
    elif user == "guest":
        return False
    else:
        return None

# WRONG: Using truthiness
result = check_permission("guest")
if result:
    print("Access granted")
else:
    print("Access denied or user not found")
# Can't distinguish between False and None!

# CORRECT: Explicit checks
result = check_permission("guest")
if result is None:
    print("User not found")
elif result:
    print("Access granted")
else:
    print("Access denied")
```

**3. When Checking Boolean Flags Explicitly**

```python
# Example: Feature flags
class Settings:
    def __init__(self):
        self.dark_mode = False  # Explicitly False, not just falsy
        self.notifications = True

settings = Settings()

# WRONG: Truthiness might be confusing
if settings.dark_mode:
    apply_dark_theme()
# This doesn't execute, which is correct, but...

# BETTER: Explicit for boolean flags (more readable)
if settings.dark_mode is True:
    apply_dark_theme()

# Or even better: descriptive variable names
if settings.dark_mode:  # OK if variable name is clearly boolean
    apply_dark_theme()
```

**4. When Empty String is Different from None**

```python
# Example: Form input
def process_form(name, email):
    # name="" (empty string) vs name=None (not provided)
    
    # WRONG: Treats empty string and None the same
    if not name:
        print("Name is missing")
    
    # CORRECT: Distinguish between empty and missing
    if name is None:
        print("Name field not provided")
    elif name == "":
        print("Name cannot be empty")
    else:
        print(f"Name: {name}")

process_form("", "email@example.com")  # Empty string
process_form(None, "email@example.com")  # Not provided
```

**Best Practices:**

```python
# 1. Use truthiness for collections
if items:  # Good
    process(items)

if len(items) > 0:  # Avoid
    process(items)

# 2. Use truthiness for strings
if name:  # Good
    greet(name)

if name != "":  # Avoid
    greet(name)

# 3. Use explicit comparison for None
if value is None:  # Good
    handle_missing()

if not value:  # Avoid (could be 0, "", [], etc.)
    handle_missing()

# 4. Use explicit comparison when zero is valid
if count is not None:  # Good
    print(f"Count: {count}")

if count:  # Avoid (fails for count=0)
    print(f"Count: {count}")

# 5. Use `is` for None, True, False
if value is None:  # Good
if value == None:  # Avoid

if flag is True:  # OK, but usually unnecessary
if flag:  # Better (if flag is clearly boolean)
```

**Common Pitfalls:**

```python
# Pitfall 1: Empty list vs None
def get_results():
    return []  # Empty results

results = get_results()

if not results:
    print("No results")  # This executes
# But did the function fail (None) or just return empty results ([])? 
# Can't tell with truthiness!

# Pitfall 2: Zero vs None
def calculate_discount(price):
    if price < 100:
        return 0  # No discount
    return price * 0.1

discount = calculate_discount(50)

if not discount:
    print("No discount available")  # Executes, but discount IS available (it's 0)

# Pitfall 3: False vs None
def is_valid(data):
    if not data:
        return None  # Invalid input
    # ... validation logic
    return False  # Valid but failed validation

result = is_valid("")

if not result:
    print("Validation failed")  # Can't distinguish None from False!
```

**Conclusion:**

Truthiness is a powerful Python feature that makes code more concise and readable when used appropriately. Use truthiness for checking if collections are empty or strings are blank. Use explicit comparisons when you need to distinguish between different falsy values (like `0` vs `None` vs `False`) or when the distinction matters for your logic. Understanding when to use each approach is a mark of experienced Python programmers.

---

## ✅ Key Takeaways

1. Conditional code enables programs to make decisions and branch execution
2. Boolean expressions evaluate to `True` or `False`
3. Comparison operators (`==`, `!=`, `<`, `>`, `<=`, `>=`) create boolean expressions
4. Logical operators (`and`, `or`, `not`) combine boolean expressions
5. Python uses indentation to define code blocks (4 spaces is standard)
6. `if-elif-else` chains execute only the first true condition (mutually exclusive)
7. Multiple `if` statements check all conditions independently
8. Truthiness: empty collections, empty strings, `0`, and `None` are falsy
9. Use truthiness for checking empty collections/strings
10. Use explicit comparison when distinguishing between falsy values matters

---

**Next Topic**: Conditional Statements
**Previous Topic**: Variables and Expressions
**Module**: 1 - Programming for Everybody

Happy Learning! 🐍
