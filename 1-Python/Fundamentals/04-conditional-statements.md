# Python Conditional Statements - Complete Guide

## Table of Contents
1. [If Statements](#if-statements)
2. [If-Else Statements](#if-else-statements)
3. [If-Elif-Else Statements](#if-elif-else-statements)
4. [Logical Operators](#logical-operators)
5. [Comparison Operators](#comparison-operators)
6. [Conditional Expressions (Ternary)](#conditional-expressions-ternary)
7. [Nested Conditionals](#nested-conditionals)
8. [Practice Questions](#practice-questions)

---

## If Statements

### Basic Syntax

```python
if condition:
    # Code block executes if condition is True
    statement1
    statement2
```

### Simple Examples

```python
# Example 1: Check if positive
number = 10
if number > 0:
    print("Number is positive")

# Example 2: Check if even
number = 8
if number % 2 == 0:
    print("Number is even")

# Example 3: Check string
name = "Alice"
if name == "Alice":
    print("Hello, Alice!")

# Example 4: Check if in list
fruits = ["apple", "banana", "cherry"]
if "apple" in fruits:
    print("Apple is in the list")
```

### Truthy and Falsy Values

```python
# Falsy values: False, 0, 0.0, "", [], {}, None
# Everything else is Truthy

# Empty string is False
text = ""
if text:
    print("Has text")
else:
    print("Empty string")  # This executes

# Non-empty string is True
text = "Hello"
if text:
    print("Has text")  # This executes

# Zero is False
number = 0
if number:
    print("Non-zero")
else:
    print("Zero")  # This executes

# Empty list is False
my_list = []
if my_list:
    print("List has items")
else:
    print("Empty list")  # This executes

# None is False
value = None
if value:
    print("Has value")
else:
    print("No value")  # This executes
```

---

## If-Else Statements

### Basic Syntax

```python
if condition:
    # Executes if condition is True
    statement1
else:
    # Executes if condition is False
    statement2
```

### Examples

```python
# Example 1: Even or odd
number = 7
if number % 2 == 0:
    print("Even")
else:
    print("Odd")  # Output: Odd

# Example 2: Age check
age = 16
if age >= 18:
    print("Adult")
else:
    print("Minor")  # Output: Minor

# Example 3: Password validation
password = "secret123"
if len(password) >= 8:
    print("Password is strong")
else:
    print("Password is too short")  # Output: Password is too short

# Example 4: Grade pass/fail
score = 75
if score >= 60:
    print("Pass")  # Output: Pass
else:
    print("Fail")
```

---

## If-Elif-Else Statements

### Basic Syntax

```python
if condition1:
    # Executes if condition1 is True
    statement1
elif condition2:
    # Executes if condition1 is False and condition2 is True
    statement2
elif condition3:
    # Executes if condition1 and condition2 are False and condition3 is True
    statement3
else:
    # Executes if all conditions are False
    statement4
```

### Examples

```python
# Example 1: Grade classification
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"  # This executes
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Grade: {grade}")  # Output: Grade: B

# Example 2: Temperature description
temp = 25

if temp > 30:
    print("Hot")
elif temp > 20:
    print("Warm")  # This executes
elif temp > 10:
    print("Cool")
else:
    print("Cold")

# Example 3: Traffic light
light = "yellow"

if light == "red":
    print("Stop")
elif light == "yellow":
    print("Slow down")  # This executes
elif light == "green":
    print("Go")
else:
    print("Invalid light color")

# Example 4: Age categories
age = 35

if age < 13:
    category = "Child"
elif age < 20:
    category = "Teenager"
elif age < 60:
    category = "Adult"  # This executes
else:
    category = "Senior"

print(f"Category: {category}")
```

---

## Logical Operators

### AND Operator

Both conditions must be True.

```python
# Basic AND
age = 25
has_license = True

if age >= 18 and has_license:
    print("Can drive")  # This executes

# Multiple AND conditions
score = 85
attendance = 90

if score >= 80 and attendance >= 85:
    print("Excellent performance")  # This executes

# AND with different types
username = "admin"
password = "secret"

if username == "admin" and password == "secret":
    print("Login successful")  # This executes

# Short-circuit evaluation
# If first condition is False, second is not evaluated
x = 0
if x != 0 and 10 / x > 1:  # Second part not evaluated (no division by zero error)
    print("This won't print")
```

### OR Operator

At least one condition must be True.

```python
# Basic OR
day = "Saturday"

if day == "Saturday" or day == "Sunday":
    print("Weekend!")  # This executes

# Multiple OR conditions
age = 70

if age < 18 or age > 65:
    print("Discounted ticket")  # This executes

# OR with different conditions
is_raining = False
is_snowing = True

if is_raining or is_snowing:
    print("Take an umbrella")  # This executes

# Short-circuit evaluation
# If first condition is True, second is not evaluated
x = 5
if x > 0 or 10 / x > 1:  # Second part not evaluated
    print("Positive number")  # This executes
```

### NOT Operator

Inverts the boolean value.

```python
# Basic NOT
is_logged_in = False

if not is_logged_in:
    print("Please log in")  # This executes

# NOT with comparison
age = 15

if not age >= 18:
    print("Minor")  # This executes

# NOT with membership
fruits = ["apple", "banana"]

if "cherry" not in fruits:
    print("Cherry not found")  # This executes

# Double negative (avoid this - confusing)
is_valid = True
if not not is_valid:  # Same as: if is_valid:
    print("Valid")
```

### Combining Logical Operators

```python
# AND and OR
age = 25
has_license = True
has_car = False

if (age >= 18 and has_license) or has_car:
    print("Can drive")  # This executes

# Complex condition
score = 85
attendance = 90
has_project = True

if (score >= 80 and attendance >= 85) or has_project:
    print("Pass")  # This executes

# Using parentheses for clarity
x = 10
if (x > 5 and x < 15) or (x > 20 and x < 25):
    print("In range")  # This executes

# NOT with AND/OR
is_weekend = True
is_holiday = False

if is_weekend and not is_holiday:
    print("Regular weekend")  # This executes
```

---

## Comparison Operators

### All Comparison Operators

```python
# Equal to (==)
print(5 == 5)    # True
print(5 == 3)    # False

# Not equal to (!=)
print(5 != 3)    # True
print(5 != 5)    # False

# Greater than (>)
print(5 > 3)     # True
print(3 > 5)     # False

# Less than (<)
print(3 < 5)     # True
print(5 < 3)     # False

# Greater than or equal to (>=)
print(5 >= 5)    # True
print(5 >= 3)    # True
print(3 >= 5)    # False

# Less than or equal to (<=)
print(3 <= 5)    # True
print(5 <= 5)    # True
print(5 <= 3)    # False
```

### Chaining Comparisons

```python
# Chained comparisons
x = 10

# Traditional way
if x > 5 and x < 15:
    print("Between 5 and 15")

# Pythonic way (chained)
if 5 < x < 15:
    print("Between 5 and 15")  # This executes

# Multiple chains
age = 25
if 18 <= age <= 65:
    print("Working age")  # This executes

# Complex chains
score = 85
if 0 <= score <= 100:
    print("Valid score")  # This executes
```

### String Comparisons

```python
# Lexicographic comparison (alphabetical order)
print("apple" < "banana")   # True
print("apple" == "Apple")   # False (case-sensitive)

# Case-insensitive comparison
name1 = "Alice"
name2 = "alice"
if name1.lower() == name2.lower():
    print("Same name")  # This executes

# String length comparison
if len("hello") > len("hi"):
    print("First string is longer")  # This executes
```

### Membership Operators

```python
# in operator
fruits = ["apple", "banana", "cherry"]
if "apple" in fruits:
    print("Apple found")  # This executes

# not in operator
if "mango" not in fruits:
    print("Mango not found")  # This executes

# String membership
text = "Hello, World!"
if "World" in text:
    print("Found")  # This executes

# Dictionary membership (checks keys)
person = {"name": "John", "age": 30}
if "name" in person:
    print("Has name key")  # This executes
```

### Identity Operators

```python
# is operator (checks if same object)
x = [1, 2, 3]
y = [1, 2, 3]
z = x

print(x == y)   # True (same values)
print(x is y)   # False (different objects)
print(x is z)   # True (same object)

# is not operator
if x is not y:
    print("Different objects")  # This executes

# Common use with None
value = None
if value is None:
    print("No value")  # This executes

# Don't use 'is' for value comparison
# Bad practice
if x is 5:  # Don't do this
    pass

# Good practice
if x == 5:  # Do this
    pass
```

---

## Conditional Expressions (Ternary)

### Basic Syntax

```python
# Syntax: value_if_true if condition else value_if_false
result = "Even" if 10 % 2 == 0 else "Odd"
print(result)  # Output: Even
```

### Examples

```python
# Example 1: Assign based on condition
age = 20
status = "Adult" if age >= 18 else "Minor"
print(status)  # Output: Adult

# Example 2: Max of two numbers
a, b = 10, 20
max_value = a if a > b else b
print(max_value)  # Output: 20

# Example 3: Absolute value
number = -5
absolute = number if number >= 0 else -number
print(absolute)  # Output: 5

# Example 4: Default value
name = ""
display_name = name if name else "Guest"
print(display_name)  # Output: Guest

# Example 5: In function return
def get_grade(score):
    return "Pass" if score >= 60 else "Fail"

print(get_grade(75))  # Output: Pass
print(get_grade(45))  # Output: Fail

# Example 6: In list comprehension
numbers = [1, 2, 3, 4, 5]
labels = ["Even" if n % 2 == 0 else "Odd" for n in numbers]
print(labels)  # Output: ['Odd', 'Even', 'Odd', 'Even', 'Odd']

# Example 7: Nested ternary (avoid if too complex)
score = 85
grade = "A" if score >= 90 else ("B" if score >= 80 else "C")
print(grade)  # Output: B
```

---

## Nested Conditionals

### Basic Nesting

```python
# Example 1: Age and license check
age = 20
has_license = True

if age >= 18:
    if has_license:
        print("Can drive")  # This executes
    else:
        print("Need license")
else:
    print("Too young")

# Example 2: Grade with bonus
score = 85
has_bonus = True

if score >= 60:
    if has_bonus:
        final_score = score + 5
        print(f"Pass with bonus: {final_score}")  # This executes
    else:
        print(f"Pass: {score}")
else:
    print("Fail")

# Example 3: Login validation
username = "admin"
password = "secret"

if username == "admin":
    if password == "secret":
        print("Login successful")  # This executes
    else:
        print("Wrong password")
else:
    print("User not found")
```

### Avoiding Deep Nesting

```python
# Bad: Deep nesting (hard to read)
def process_order(has_stock, has_payment, has_address):
    if has_stock:
        if has_payment:
            if has_address:
                return "Order processed"
            else:
                return "Missing address"
        else:
            return "Payment required"
    else:
        return "Out of stock"

# Good: Early returns (flat structure)
def process_order_better(has_stock, has_payment, has_address):
    if not has_stock:
        return "Out of stock"
    if not has_payment:
        return "Payment required"
    if not has_address:
        return "Missing address"
    return "Order processed"

# Good: Using logical operators
def process_order_best(has_stock, has_payment, has_address):
    if has_stock and has_payment and has_address:
        return "Order processed"
    if not has_stock:
        return "Out of stock"
    if not has_payment:
        return "Payment required"
    return "Missing address"
```

---

## Practice Questions

### Beginner Level

**Question 1:** Check if a number is positive, negative, or zero.

```python
# Solution
number = int(input("Enter a number: "))

if number > 0:
    print("Positive")
elif number < 0:
    print("Negative")
else:
    print("Zero")
```

**Question 2:** Check if a year is a leap year.

```python
# Solution
year = int(input("Enter a year: "))

if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
    print(f"{year} is a leap year")
else:
    print(f"{year} is not a leap year")
```

**Question 3:** Find the largest of three numbers.

```python
# Solution
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))
c = int(input("Enter third number: "))

if a >= b and a >= c:
    largest = a
elif b >= a and b >= c:
    largest = b
else:
    largest = c

print(f"Largest: {largest}")
```

### Intermediate Level

**Question 4:** Calculate electricity bill based on units consumed.
- First 100 units: $0.50/unit
- Next 100 units: $0.75/unit
- Above 200 units: $1.20/unit

```python
# Solution
units = int(input("Enter units consumed: "))

if units <= 100:
    bill = units * 0.50
elif units <= 200:
    bill = (100 * 0.50) + ((units - 100) * 0.75)
else:
    bill = (100 * 0.50) + (100 * 0.75) + ((units - 200) * 1.20)

print(f"Bill: ${bill:.2f}")
```

**Question 5:** Determine if a triangle is valid and its type.

```python
# Solution
a = float(input("Enter side a: "))
b = float(input("Enter side b: "))
c = float(input("Enter side c: "))

# Check if valid triangle
if a + b > c and b + c > a and a + c > b:
    # Determine type
    if a == b == c:
        print("Equilateral triangle")
    elif a == b or b == c or a == c:
        print("Isosceles triangle")
    else:
        print("Scalene triangle")
else:
    print("Not a valid triangle")
```

**Question 6:** BMI Calculator with categories.

```python
# Solution
weight = float(input("Enter weight (kg): "))
height = float(input("Enter height (m): "))

bmi = weight / (height ** 2)

print(f"BMI: {bmi:.2f}")

if bmi < 18.5:
    category = "Underweight"
elif bmi < 25:
    category = "Normal weight"
elif bmi < 30:
    category = "Overweight"
else:
    category = "Obese"

print(f"Category: {category}")
```

### Advanced Level

**Question 7:** Validate password strength.
Requirements: At least 8 characters, contains uppercase, lowercase, digit, and special character.

```python
# Solution
password = input("Enter password: ")

has_upper = any(c.isupper() for c in password)
has_lower = any(c.islower() for c in password)
has_digit = any(c.isdigit() for c in password)
has_special = any(not c.isalnum() for c in password)
is_long_enough = len(password) >= 8

if is_long_enough and has_upper and has_lower and has_digit and has_special:
    print("Strong password")
elif is_long_enough and (has_upper or has_lower) and has_digit:
    print("Medium password")
else:
    print("Weak password")
    
    # Provide feedback
    if not is_long_enough:
        print("- Password must be at least 8 characters")
    if not has_upper:
        print("- Add uppercase letters")
    if not has_lower:
        print("- Add lowercase letters")
    if not has_digit:
        print("- Add digits")
    if not has_special:
        print("- Add special characters")
```

**Question 8:** Rock, Paper, Scissors game.

```python
# Solution
import random

choices = ["rock", "paper", "scissors"]
computer = random.choice(choices)
player = input("Enter your choice (rock/paper/scissors): ").lower()

if player not in choices:
    print("Invalid choice!")
elif player == computer:
    print(f"Tie! Both chose {player}")
elif (player == "rock" and computer == "scissors") or \
     (player == "paper" and computer == "rock") or \
     (player == "scissors" and computer == "paper"):
    print(f"You win! {player} beats {computer}")
else:
    print(f"You lose! {computer} beats {player}")
```

**Question 9:** Tax calculator with multiple brackets.

```python
# Solution
income = float(input("Enter annual income: $"))

# Tax brackets
if income <= 10000:
    tax = 0
elif income <= 40000:
    tax = (income - 10000) * 0.10
elif income <= 85000:
    tax = (30000 * 0.10) + ((income - 40000) * 0.20)
else:
    tax = (30000 * 0.10) + (45000 * 0.20) + ((income - 85000) * 0.30)

net_income = income - tax
tax_rate = (tax / income * 100) if income > 0 else 0

print(f"\nIncome: ${income:,.2f}")
print(f"Tax: ${tax:,.2f}")
print(f"Net Income: ${net_income:,.2f}")
print(f"Effective Tax Rate: {tax_rate:.2f}%")
```

**Question 10:** Date validator (check if valid date).

```python
# Solution
day = int(input("Enter day: "))
month = int(input("Enter month: "))
year = int(input("Enter year: "))

# Check month validity
if month < 1 or month > 12:
    print("Invalid month")
else:
    # Days in each month
    if month in [1, 3, 5, 7, 8, 10, 12]:
        max_days = 31
    elif month in [4, 6, 9, 11]:
        max_days = 30
    else:  # February
        # Check leap year
        if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
            max_days = 29
        else:
            max_days = 28
    
    # Check day validity
    if day < 1 or day > max_days:
        print(f"Invalid day for month {month}")
    else:
        print(f"{day}/{month}/{year} is a valid date")
```

---

## Key Takeaways

1. **if** executes code block when condition is True
2. **elif** provides alternative conditions (checked in order)
3. **else** executes when all conditions are False
4. **Logical operators**: and, or, not
5. **Comparison operators**: ==, !=, <, >, <=, >=
6. **Membership operators**: in, not in
7. **Identity operators**: is, is not
8. **Ternary operator**: value_if_true if condition else value_if_false
9. **Avoid deep nesting** - use early returns or logical operators
10. **Indentation matters** - Python uses indentation to define code blocks

---

## Common Mistakes

```python
# Mistake 1: Using = instead of ==
x = 5
# if x = 5:  # SyntaxError
if x == 5:  # Correct

# Mistake 2: Forgetting colon
# if x > 0  # SyntaxError
if x > 0:  # Correct
    print("Positive")

# Mistake 3: Wrong indentation
if x > 0:
# print("Positive")  # IndentationError
    print("Positive")  # Correct

# Mistake 4: Using 'is' for value comparison
x = 1000
# if x is 1000:  # May not work (use ==)
if x == 1000:  # Correct

# Mistake 5: Confusing and/or
age = 25
# if age > 18 or < 65:  # SyntaxError
if age > 18 and age < 65:  # Correct
if 18 < age < 65:  # Better (Pythonic)
```

---

## Next Steps

Now that you understand conditional statements, move on to:
- **Loops (while and for)**
- **Nested Loops**
- **Loop Control (break, continue, pass)**
- **Collections (Lists, Tuples, Sets)**

Practice with real-world decision-making problems!
