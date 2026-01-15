# Variables and Expressions

## 📚 Theoretical Understanding

### What are Variables?

Variables are fundamental building blocks in programming - they are named containers that store data values in your computer's memory. Think of a variable as a labeled box where you can put information, retrieve it later, and even change what's inside. The name "variable" comes from the fact that the value stored can vary (change) during program execution.

In Python, variables are more accurately called "name bindings" or "references." When you create a variable, you're creating a name that refers to an object in memory. This is different from languages like C or Java where variables are fixed memory locations. Python's approach is more flexible - the same variable name can refer to different types of objects at different times, and multiple variables can refer to the same object.

Understanding variables is crucial because they allow programs to be dynamic and reusable. Instead of hardcoding values like `print(5 + 3)`, you can use variables: `x = 5; y = 3; print(x + y)`. This makes your code more flexible, readable, and maintainable. Variables let you store user input, calculation results, and any data your program needs to remember and manipulate.

### Variable Naming Rules and Conventions

Python has strict rules about variable names, and following these rules is not optional - your code won't run if you break them. Variable names must start with a letter (a-z, A-Z) or underscore (_), followed by any combination of letters, numbers, or underscores. They cannot start with a number, cannot contain spaces, and cannot be Python keywords (reserved words like `if`, `for`, `while`, etc.).

Beyond the rules, Python has conventions - guidelines that aren't enforced by the language but are followed by the community. The most important convention is using snake_case for variable names: lowercase words separated by underscores (like `user_name`, `total_price`, `is_valid`). This makes code more readable and is part of PEP 8, Python's official style guide. Constants (values that shouldn't change) are written in UPPERCASE (like `MAX_SIZE`, `PI`, `DEFAULT_COLOR`).

Good variable names are descriptive and meaningful. Compare `x = 25` with `student_age = 25`. The second immediately tells you what the variable represents. While single-letter variables like `i`, `j`, `k` are acceptable for loop counters, most variables should have names that explain their purpose. This self-documenting code is easier to read, debug, and maintain.

### What are Expressions?

An expression is any combination of values, variables, operators, and function calls that Python can evaluate to produce a result. Expressions are the building blocks of statements - they represent computations that produce values. For example, `2 + 3` is an expression that evaluates to `5`, and `x * 2 + y` is an expression that combines variables and operators to produce a result.

Expressions can be simple or complex. A single number like `42` is an expression (it evaluates to itself). A variable name like `age` is an expression (it evaluates to the value stored in that variable). More complex expressions combine multiple elements: `(price * quantity) * (1 + tax_rate)` combines variables, operators, and parentheses to calculate a final price with tax.

Understanding the difference between expressions and statements is important. An expression produces a value, while a statement performs an action. `x + 5` is an expression (produces a value), but `x = x + 5` is a statement (performs assignment). However, statements often contain expressions - the assignment statement contains the expression `x + 5`.

### Operators and Operator Precedence

Operators are special symbols that perform operations on values and variables. Python has several categories of operators: arithmetic (+, -, *, /, //, %, **), comparison (==, !=, <, >, <=, >=), logical (and, or, not), assignment (=, +=, -=, etc.), and more. Each operator has a specific purpose and follows specific rules.

Operator precedence determines the order in which operations are performed in complex expressions. Just like in mathematics where multiplication happens before addition, Python has a defined order: parentheses first, then exponentiation, then multiplication/division, then addition/subtraction. For example, `2 + 3 * 4` evaluates to `14` (not `20`) because multiplication has higher precedence than addition.

When operators have the same precedence, associativity determines the order. Most operators are left-associative (evaluated left to right), but exponentiation is right-associative. Understanding precedence prevents bugs and makes your code predictable. When in doubt, use parentheses to make your intentions explicit: `(2 + 3) * 4` clearly shows you want addition first.

### Dynamic Typing in Python

Python is dynamically typed, meaning you don't need to declare a variable's type before using it. The type is determined automatically based on the value you assign. You can assign an integer to a variable, then later assign a string to the same variable - Python handles this seamlessly. This flexibility makes Python code shorter and faster to write, but it also requires careful attention to avoid type-related bugs.

Dynamic typing doesn't mean Python is untyped or loosely typed - every value has a specific type, and Python enforces type rules. You can't add a string to an integer without explicit conversion. The difference is that types are associated with values, not variables. A variable is just a name that can refer to any type of value.

This dynamic nature is both a strength and a potential weakness. It makes Python very flexible and great for rapid development, but it can also lead to runtime errors that statically-typed languages would catch at compile time. Modern Python addresses this with optional type hints, allowing you to annotate your code with expected types while maintaining the dynamic nature of the language.

---

## 💻 Practical Examples

### Variable Declaration and Assignment

```python
# Simple variable assignment
name = "Alice"
age = 25
height = 5.6
is_student = True

# Multiple assignment
x, y, z = 10, 20, 30

# Same value to multiple variables
a = b = c = 100

# Swapping variables
x, y = 5, 10
x, y = y, x  # Now x=10, y=5

# Variable reassignment (dynamic typing)
value = 42        # integer
value = "hello"   # now a string
value = [1, 2, 3] # now a list
```

### Expressions and Operators

```python
# Arithmetic expressions
result = 10 + 5 * 2        # 20 (multiplication first)
result = (10 + 5) * 2      # 30 (parentheses first)
result = 10 / 3            # 3.333... (true division)
result = 10 // 3           # 3 (integer division)
result = 10 % 3            # 1 (remainder/modulo)
result = 2 ** 3            # 8 (exponentiation)

# Compound expressions
x = 5
y = 10
z = (x + y) * 2 - x ** 2   # (15 * 2) - 25 = 5

# Comparison expressions (return True/False)
is_equal = (5 == 5)        # True
is_greater = (10 > 5)      # True
is_not_equal = (3 != 4)    # True

# Logical expressions
result = (5 > 3) and (10 < 20)  # True (both conditions true)
result = (5 > 10) or (3 < 5)    # True (at least one true)
result = not (5 > 10)           # True (negation)

# String expressions
full_name = "John" + " " + "Doe"  # Concatenation
repeated = "Ha" * 3               # "HaHaHa"
```

### Augmented Assignment Operators

```python
# Instead of: x = x + 5
x = 10
x += 5   # x = 15 (add and assign)
x -= 3   # x = 12 (subtract and assign)
x *= 2   # x = 24 (multiply and assign)
x /= 4   # x = 6.0 (divide and assign)
x //= 2  # x = 3.0 (integer divide and assign)
x %= 2   # x = 1.0 (modulo and assign)
x **= 3  # x = 1.0 (exponentiate and assign)

# Works with strings too
message = "Hello"
message += " World"  # "Hello World"
```

### Type Checking and Conversion

```python
# Check variable type
x = 42
print(type(x))  # <class 'int'>

y = "Hello"
print(type(y))  # <class 'str'>

# Type conversion (casting)
num_str = "123"
num_int = int(num_str)      # Convert string to integer
num_float = float(num_str)  # Convert string to float

age = 25
age_str = str(age)          # Convert integer to string

# Be careful with conversions
# int("hello")  # ValueError: invalid literal
# int("12.5")   # ValueError: invalid literal
# int(12.5)     # 12 (truncates decimal)
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain the difference between `=`, `==`, and `is` operators in Python with examples. Why is understanding this difference crucial for writing correct Python code?

**Detailed Answer:**

These three operators look similar but serve completely different purposes, and confusing them is one of the most common mistakes beginners make. Understanding their differences is fundamental to writing correct Python code.

**The `=` Operator (Assignment)**

The single equals sign `=` is the assignment operator. It assigns a value to a variable. It does NOT compare values - it stores a value in a variable name.

```python
# Assignment - storing values
x = 10          # Assign 10 to x
name = "Alice"  # Assign "Alice" to name
result = x + 5  # Assign the result of x + 5 to result

# Common mistake:
if x = 10:  # SyntaxError! Cannot use = in conditions
    print("x is 10")
```

**The `==` Operator (Equality Comparison)**

The double equals `==` is the equality comparison operator. It compares two values and returns `True` if they're equal, `False` otherwise. It checks if the VALUES are the same.

```python
# Comparison - checking if values are equal
x = 10
y = 10
z = 20

print(x == y)  # True (values are equal)
print(x == z)  # False (values are different)

# Correct usage in conditions
if x == 10:
    print("x is 10")  # This works!

# Works with different types
print(10 == 10.0)  # True (values are equal, even though types differ)
print("hello" == "hello")  # True
print([1, 2] == [1, 2])    # True (lists with same content)
```

**The `is` Operator (Identity Comparison)**

The `is` operator checks if two variables refer to the SAME OBJECT in memory, not just equal values. It checks identity, not equality.

```python
# Identity comparison - checking if same object
x = [1, 2, 3]
y = [1, 2, 3]
z = x

print(x == y)  # True (same values)
print(x is y)  # False (different objects in memory)
print(x is z)  # True (z refers to same object as x)

# With immutable objects (integers, strings)
a = 10
b = 10
print(a is b)  # True (Python caches small integers)

# But be careful:
a = 1000
b = 1000
print(a is b)  # Might be False (large integers not always cached)

# Best practice: use 'is' only for None, True, False
if value is None:
    print("Value is None")

if flag is True:  # Though 'if flag:' is more Pythonic
    print("Flag is true")
```

**Why This Matters:**

1. **Correctness**: Using `=` instead of `==` in a condition causes a syntax error. Using `is` instead of `==` for value comparison can cause subtle bugs.

2. **Performance**: `is` is faster than `==` because it just compares memory addresses, not values. But use it only when you actually need identity checking.

3. **Semantics**: They express different intentions:
   - `=` says "store this value"
   - `==` says "are these values equal?"
   - `is` says "are these the same object?"

4. **Common Pitfalls**:
   ```python
   # WRONG: Using 'is' for value comparison
   if user_input is "yes":  # Might not work!
       do_something()
   
   # RIGHT: Use == for value comparison
   if user_input == "yes":
       do_something()
   
   # RIGHT: Use 'is' only for None, True, False
   if result is None:
       handle_error()
   ```

**Memory Diagram Example:**
```python
x = [1, 2, 3]  # Creates list object at memory address 0x1000
y = [1, 2, 3]  # Creates NEW list object at memory address 0x2000
z = x          # z points to SAME object as x (address 0x1000)

# x == y  → True (same content)
# x is y  → False (different memory addresses)
# x is z  → True (same memory address)
```

---

### Question 2: What is operator precedence, and why does `2 + 3 * 4` evaluate to `14` instead of `20`? Explain with examples how to control evaluation order in complex expressions.

**Detailed Answer:**

Operator precedence is the set of rules that determines the order in which operations are performed in an expression with multiple operators. Just like in mathematics, Python follows specific rules about which operations happen first. Understanding precedence is crucial for writing correct expressions and avoiding bugs.

**Why `2 + 3 * 4` = `14`:**

```python
result = 2 + 3 * 4

# Python evaluates this as:
# Step 1: 3 * 4 = 12 (multiplication has higher precedence)
# Step 2: 2 + 12 = 14 (then addition)

# NOT as:
# (2 + 3) * 4 = 5 * 4 = 20

print(result)  # Output: 14
```

Multiplication has higher precedence than addition, so it's performed first, regardless of the order in the expression. This follows the mathematical order of operations (PEMDAS/BODMAS).

**Python Operator Precedence (Highest to Lowest):**

1. **Parentheses** `()`
2. **Exponentiation** `**`
3. **Unary operators** `+x`, `-x`, `~x`
4. **Multiplication, Division, Modulo** `*`, `/`, `//`, `%`
5. **Addition, Subtraction** `+`, `-`
6. **Comparison operators** `<`, `<=`, `>`, `>=`, `==`, `!=`
7. **Logical NOT** `not`
8. **Logical AND** `and`
9. **Logical OR** `or`

**Examples Demonstrating Precedence:**

```python
# Example 1: Arithmetic precedence
result = 10 + 5 * 2
# Evaluation: 10 + (5 * 2) = 10 + 10 = 20
print(result)  # 20

# Example 2: Exponentiation before multiplication
result = 2 * 3 ** 2
# Evaluation: 2 * (3 ** 2) = 2 * 9 = 18
print(result)  # 18

# Example 3: Division and multiplication (same precedence, left-to-right)
result = 20 / 4 * 2
# Evaluation: (20 / 4) * 2 = 5.0 * 2 = 10.0
print(result)  # 10.0

# Example 4: Comparison before logical operators
result = 5 > 3 and 10 < 20
# Evaluation: (5 > 3) and (10 < 20) = True and True = True
print(result)  # True

# Example 5: Complex expression
result = 2 + 3 * 4 ** 2 / 2 - 1
# Step 1: 4 ** 2 = 16 (exponentiation)
# Step 2: 3 * 16 = 48 (multiplication)
# Step 3: 48 / 2 = 24.0 (division)
# Step 4: 2 + 24.0 = 26.0 (addition)
# Step 5: 26.0 - 1 = 25.0 (subtraction)
print(result)  # 25.0
```

**Controlling Evaluation Order with Parentheses:**

Parentheses have the highest precedence and force operations to be performed first:

```python
# Without parentheses
result = 2 + 3 * 4
print(result)  # 14

# With parentheses - force addition first
result = (2 + 3) * 4
print(result)  # 20

# Complex example
result = (10 + 5) * (3 - 1) ** 2
# Step 1: (10 + 5) = 15
# Step 2: (3 - 1) = 2
# Step 3: 2 ** 2 = 4
# Step 4: 15 * 4 = 60
print(result)  # 60

# Nested parentheses (innermost first)
result = ((2 + 3) * 4) - (10 / 2)
# Step 1: (2 + 3) = 5
# Step 2: 5 * 4 = 20
# Step 3: 10 / 2 = 5.0
# Step 4: 20 - 5.0 = 15.0
print(result)  # 15.0
```

**Associativity (When Operators Have Same Precedence):**

When operators have the same precedence, associativity determines the order:

```python
# Left-to-right associativity (most operators)
result = 20 - 10 - 5
# Evaluation: (20 - 10) - 5 = 10 - 5 = 5
print(result)  # 5

# Right-to-left associativity (exponentiation)
result = 2 ** 3 ** 2
# Evaluation: 2 ** (3 ** 2) = 2 ** 9 = 512
print(result)  # 512

# NOT: (2 ** 3) ** 2 = 8 ** 2 = 64
```

**Best Practices:**

1. **Use Parentheses for Clarity**: Even when not required, parentheses make your intentions clear:
   ```python
   # Less clear
   result = a + b * c - d / e
   
   # More clear
   result = a + (b * c) - (d / e)
   ```

2. **Break Complex Expressions**: Split very complex expressions into multiple lines:
   ```python
   # Hard to read
   result = (price * quantity * (1 + tax_rate)) - discount + shipping
   
   # Easier to read
   subtotal = price * quantity
   tax = subtotal * tax_rate
   total = subtotal + tax - discount + shipping
   ```

3. **Don't Rely on Memorizing Precedence**: When in doubt, use parentheses. Code clarity is more important than showing off precedence knowledge.

**Common Mistakes:**

```python
# Mistake 1: Forgetting precedence
if x > 0 and x < 10:  # Correct
if x > 0 and < 10:    # SyntaxError!

# Mistake 2: Comparison chaining confusion
if 0 < x < 10:        # Correct (Python allows this!)
if (0 < x) and (x < 10):  # Also correct, more explicit

# Mistake 3: Logical operator precedence
if not x == 5:        # Evaluated as: not (x == 5)
if x != 5:            # Clearer way to write the same thing
```

Understanding operator precedence prevents bugs, makes your code more predictable, and helps you read and understand other people's code. When in doubt, use parentheses to make your intentions explicit!

---

## 🔍 Additional Important Concepts

### Variable Scope Preview

```python
# Global variable
global_var = 100

def my_function():
    # Local variable
    local_var = 50
    print(global_var)  # Can access global
    print(local_var)   # Can access local

my_function()
# print(local_var)  # Error! local_var doesn't exist here
```

### Constants Convention

```python
# Constants (by convention, not enforced)
PI = 3.14159
MAX_SIZE = 100
DEFAULT_COLOR = "blue"

# Use them like variables, but don't change them
radius = 5
area = PI * radius ** 2
```

---

## ✅ Key Takeaways

1. Variables are named references to values in memory
2. Python is dynamically typed - variables can hold any type
3. Use descriptive variable names in snake_case
4. `=` assigns, `==` compares values, `is` compares identity
5. Operator precedence determines evaluation order
6. Use parentheses to control evaluation order and improve clarity
7. Expressions produce values, statements perform actions
8. Augmented assignment operators (+=, -=, etc.) are shortcuts

---

**Next Topic**: Conditional Code
**Previous Topic**: Eben Upton and the Raspberry Pi
**Module**: 1 - Programming for Everybody

Happy Learning! 🐍
