# Python Variables and Data Types - Complete Guide

## Table of Contents
1. [Variables](#variables)
2. [Data Types](#data-types)
3. [Type Casting](#type-casting)
4. [User Input](#user-input)
5. [Practice Questions](#practice-questions)

---

## Variables

### What are Variables?
Variables are containers for storing data values. In Python, you don't need to declare the type of a variable.

### Variable Naming Rules
- Must start with a letter or underscore
- Cannot start with a number
- Can only contain alphanumeric characters and underscores (A-z, 0-9, and _)
- Case-sensitive (age, Age, and AGE are different variables)
- Cannot use Python keywords

### Examples

```python
# Valid variable names
name = "John"
age = 25
_private = "hidden"
user_name = "john_doe"
userName = "johnDoe"
user1 = "first user"

# Invalid variable names (will cause errors)
# 2name = "John"  # Cannot start with number
# user-name = "John"  # Cannot use hyphen
# for = "loop"  # Cannot use Python keywords
```

### Multiple Assignment

```python
# Assign same value to multiple variables
x = y = z = 100
print(x, y, z)  # Output: 100 100 100

# Assign different values to multiple variables
a, b, c = 1, 2, 3
print(a, b, c)  # Output: 1 2 3

# Swap variables
x, y = 10, 20
x, y = y, x
print(x, y)  # Output: 20 10
```

---

## Data Types

### 1. Numeric Types

#### Integer (int)
Whole numbers without decimal points.

```python
age = 25
year = 2024
negative = -10
large_number = 1_000_000  # Underscores for readability

print(type(age))  # Output: <class 'int'>
```

#### Float
Numbers with decimal points.

```python
price = 19.99
temperature = -5.5
pi = 3.14159
scientific = 2.5e3  # 2500.0

print(type(price))  # Output: <class 'float'>
```

#### Complex
Numbers with real and imaginary parts.

```python
complex_num = 3 + 4j
print(complex_num.real)  # Output: 3.0
print(complex_num.imag)  # Output: 4.0
print(type(complex_num))  # Output: <class 'complex'>
```

### 2. String (str)
Text data enclosed in quotes.

```python
# Single quotes
name = 'John'

# Double quotes
message = "Hello, World!"

# Triple quotes (multiline strings)
paragraph = """This is a
multiline
string"""

# String with quotes inside
quote = "He said, 'Hello!'"
quote2 = 'She said, "Hi!"'

print(type(name))  # Output: <class 'str'>
```

### 3. Boolean (bool)
True or False values.

```python
is_active = True
is_logged_in = False

print(type(is_active))  # Output: <class 'bool'>

# Boolean from comparisons
result = 5 > 3
print(result)  # Output: True
```

### 4. NoneType
Represents absence of value.

```python
value = None
print(type(value))  # Output: <class 'NoneType'>
```

---

## Type Casting

### Converting Between Types

#### To Integer - int()

```python
# Float to int (truncates decimal)
float_num = 3.14
int_num = int(float_num)
print(int_num)  # Output: 3

# String to int
str_num = "100"
int_num = int(str_num)
print(int_num)  # Output: 100

# Boolean to int
bool_val = True
int_val = int(bool_val)
print(int_val)  # Output: 1 (True=1, False=0)

# Invalid conversion (will raise error)
# int("hello")  # ValueError
```

#### To Float - float()

```python
# Int to float
int_num = 5
float_num = float(int_num)
print(float_num)  # Output: 5.0

# String to float
str_num = "3.14"
float_num = float(str_num)
print(float_num)  # Output: 3.14

# Boolean to float
bool_val = False
float_val = float(bool_val)
print(float_val)  # Output: 0.0
```

#### To String - str()

```python
# Int to string
num = 42
str_num = str(num)
print(str_num)  # Output: "42"
print(type(str_num))  # Output: <class 'str'>

# Float to string
pi = 3.14
str_pi = str(pi)
print(str_pi)  # Output: "3.14"

# Boolean to string
bool_val = True
str_bool = str(bool_val)
print(str_bool)  # Output: "True"

# List to string
my_list = [1, 2, 3]
str_list = str(my_list)
print(str_list)  # Output: "[1, 2, 3]"
```

#### To Boolean - bool()

```python
# Numbers to boolean (0 is False, everything else is True)
print(bool(0))      # Output: False
print(bool(1))      # Output: True
print(bool(-5))     # Output: True
print(bool(3.14))   # Output: True

# Strings to boolean (empty string is False)
print(bool(""))     # Output: False
print(bool("Hello")) # Output: True

# None to boolean
print(bool(None))   # Output: False

# Collections to boolean (empty is False)
print(bool([]))     # Output: False
print(bool([1, 2])) # Output: True
```

### Practical Type Casting Examples

```python
# Example 1: Calculate age from birth year
birth_year = input("Enter your birth year: ")  # Returns string
birth_year = int(birth_year)  # Convert to int
current_year = 2024
age = current_year - birth_year
print(f"You are {age} years old")

# Example 2: Calculate total price
price = input("Enter price: ")  # Returns string
quantity = input("Enter quantity: ")  # Returns string
total = float(price) * int(quantity)
print(f"Total: ${total:.2f}")

# Example 3: Concatenation requires same types
age = 25
message = "I am " + str(age) + " years old"
print(message)  # Output: I am 25 years old
```

---

## User Input

### input() Function
Gets input from user as a string.

```python
# Basic input
name = input("Enter your name: ")
print(f"Hello, {name}!")

# Input with type conversion
age = int(input("Enter your age: "))
print(f"You are {age} years old")

# Multiple inputs
first_name = input("First name: ")
last_name = input("Last name: ")
full_name = first_name + " " + last_name
print(f"Full name: {full_name}")
```

### Input Validation Example

```python
# Get valid integer input
while True:
    try:
        age = int(input("Enter your age: "))
        if age > 0:
            break
        else:
            print("Age must be positive!")
    except ValueError:
        print("Please enter a valid number!")

print(f"Your age is {age}")
```

### Practical Input Examples

```python
# Example 1: Simple calculator
num1 = float(input("Enter first number: "))
num2 = float(input("Enter second number: "))
result = num1 + num2
print(f"Sum: {result}")

# Example 2: Temperature converter
celsius = float(input("Enter temperature in Celsius: "))
fahrenheit = (celsius * 9/5) + 32
print(f"{celsius}°C = {fahrenheit}°F")

# Example 3: Personal info collector
print("=== Personal Information ===")
name = input("Name: ")
age = int(input("Age: "))
city = input("City: ")
is_student = input("Are you a student? (yes/no): ").lower() == "yes"

print(f"\nSummary:")
print(f"Name: {name}")
print(f"Age: {age}")
print(f"City: {city}")
print(f"Student: {is_student}")
```

---

## Practice Questions

### Beginner Level

**Question 1:** Create variables for your name, age, and favorite color. Print them.

```python
# Solution
name = "Alice"
age = 25
favorite_color = "blue"
print(f"Name: {name}, Age: {age}, Favorite Color: {favorite_color}")
```

**Question 2:** Swap two variables without using a third variable.

```python
# Solution
a = 10
b = 20
a, b = b, a
print(f"a = {a}, b = {b}")  # Output: a = 20, b = 10
```

**Question 3:** Convert a string "123" to an integer and add 77 to it.

```python
# Solution
str_num = "123"
result = int(str_num) + 77
print(result)  # Output: 200
```

### Intermediate Level

**Question 4:** Get user's first name and last name, then print full name in uppercase.

```python
# Solution
first_name = input("Enter first name: ")
last_name = input("Enter last name: ")
full_name = (first_name + " " + last_name).upper()
print(f"Full name: {full_name}")
```

**Question 5:** Calculate the area of a rectangle using user input.

```python
# Solution
length = float(input("Enter length: "))
width = float(input("Enter width: "))
area = length * width
print(f"Area: {area} square units")
```

**Question 6:** Check if a number entered by user is even or odd (using boolean).

```python
# Solution
number = int(input("Enter a number: "))
is_even = (number % 2 == 0)
print(f"Is {number} even? {is_even}")
```

### Advanced Level

**Question 7:** Create a program that converts hours, minutes, and seconds to total seconds.

```python
# Solution
hours = int(input("Enter hours: "))
minutes = int(input("Enter minutes: "))
seconds = int(input("Enter seconds: "))

total_seconds = (hours * 3600) + (minutes * 60) + seconds
print(f"Total seconds: {total_seconds}")
```

**Question 8:** Calculate BMI (Body Mass Index) from user input.
Formula: BMI = weight(kg) / (height(m))²

```python
# Solution
weight = float(input("Enter weight in kg: "))
height = float(input("Enter height in meters: "))

bmi = weight / (height ** 2)
print(f"Your BMI is: {bmi:.2f}")

# Bonus: Categorize BMI
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

**Question 9:** Create a simple interest calculator.
Formula: Simple Interest = (Principal × Rate × Time) / 100

```python
# Solution
principal = float(input("Enter principal amount: "))
rate = float(input("Enter interest rate (%): "))
time = float(input("Enter time (years): "))

simple_interest = (principal * rate * time) / 100
total_amount = principal + simple_interest

print(f"Simple Interest: ${simple_interest:.2f}")
print(f"Total Amount: ${total_amount:.2f}")
```

**Question 10:** Type conversion challenge - handle multiple data types.

```python
# Solution
print("=== Data Type Converter ===")
value = input("Enter a value: ")

print(f"\nOriginal: {value} (type: {type(value).__name__})")
print(f"As integer: {int(float(value))}")  # Handle decimals
print(f"As float: {float(value)}")
print(f"As boolean: {bool(value)}")
print(f"Length as string: {len(value)}")
```

---

## Key Takeaways

1. **Variables** store data and don't require type declaration in Python
2. **Data types** include int, float, str, bool, complex, and None
3. **Type casting** converts data from one type to another using int(), float(), str(), bool()
4. **input()** always returns a string - convert it to needed type
5. **Multiple assignment** allows assigning values to multiple variables at once
6. **Type checking** can be done using type() function
7. **Underscores** in numbers improve readability (1_000_000)
8. **Boolean conversion**: 0, empty strings, None, empty collections are False

---

## Common Mistakes to Avoid

```python
# Mistake 1: Forgetting to convert input
age = input("Enter age: ")  # This is a string!
# age + 1  # TypeError: can only concatenate str to str

# Correct way
age = int(input("Enter age: "))
age = age + 1

# Mistake 2: Invalid type conversion
# int("hello")  # ValueError
# int("3.14")   # ValueError (use float first)

# Mistake 3: Using reserved keywords as variables
# for = 5  # SyntaxError
# class = "Python"  # SyntaxError

# Mistake 4: Incorrect variable naming
# 2name = "John"  # SyntaxError
# user-name = "John"  # SyntaxError (use user_name)
```

---

## Next Steps

Now that you understand variables and data types, you're ready to move on to:
- **Arithmetic Operations and Math**
- **String Methods and Formatting**
- **Conditional Statements (if/else)**
- **Loops (for/while)**

Practice these concepts thoroughly before moving forward!
