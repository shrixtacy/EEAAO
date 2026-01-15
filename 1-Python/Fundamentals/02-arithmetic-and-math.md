# Python Arithmetic and Math Operations - Complete Guide

## Table of Contents
1. [Basic Arithmetic Operators](#basic-arithmetic-operators)
2. [Math Module Functions](#math-module-functions)
3. [Built-in Math Functions](#built-in-math-functions)
4. [Operator Precedence](#operator-precedence)
5. [Practical Examples](#practical-examples)
6. [Practice Questions](#practice-questions)

---

## Basic Arithmetic Operators

### Addition (+)

```python
# Integer addition
result = 5 + 3
print(result)  # Output: 8

# Float addition
result = 5.5 + 2.3
print(result)  # Output: 7.8

# Mixed types (int + float = float)
result = 5 + 2.5
print(result)  # Output: 7.5

# String concatenation (not arithmetic, but uses +)
text = "Hello" + " " + "World"
print(text)  # Output: Hello World
```

### Subtraction (-)

```python
# Basic subtraction
result = 10 - 3
print(result)  # Output: 7

# Negative numbers
result = 5 - 10
print(result)  # Output: -5

# Float subtraction
result = 10.5 - 3.2
print(result)  # Output: 7.3
```

### Multiplication (*)

```python
# Basic multiplication
result = 4 * 5
print(result)  # Output: 20

# Float multiplication
result = 2.5 * 4
print(result)  # Output: 10.0

# String repetition
text = "Ha" * 3
print(text)  # Output: HaHaHa

# List repetition
numbers = [1, 2] * 3
print(numbers)  # Output: [1, 2, 1, 2, 1, 2]
```

### Division (/)
Always returns a float.

```python
# Basic division
result = 10 / 2
print(result)  # Output: 5.0 (note: float, not int)

# Division with remainder
result = 10 / 3
print(result)  # Output: 3.3333333333333335

# Division by float
result = 10 / 2.5
print(result)  # Output: 4.0
```

### Floor Division (//)
Returns the quotient without remainder (rounds down).

```python
# Floor division
result = 10 // 3
print(result)  # Output: 3

# With negative numbers
result = -10 // 3
print(result)  # Output: -4 (rounds down, not toward zero)

# Float floor division
result = 10.0 // 3
print(result)  # Output: 3.0
```

### Modulus (%)
Returns the remainder of division.

```python
# Basic modulus
result = 10 % 3
print(result)  # Output: 1

# Check if even or odd
number = 7
is_even = (number % 2 == 0)
print(f"{number} is even: {is_even}")  # Output: 7 is even: False

# With negative numbers
result = -10 % 3
print(result)  # Output: 2

# Float modulus
result = 10.5 % 3
print(result)  # Output: 1.5
```

### Exponentiation (**)
Raises a number to a power.

```python
# Basic exponentiation
result = 2 ** 3
print(result)  # Output: 8 (2 × 2 × 2)

# Square
result = 5 ** 2
print(result)  # Output: 25

# Cube
result = 3 ** 3
print(result)  # Output: 27

# Square root (power of 0.5)
result = 16 ** 0.5
print(result)  # Output: 4.0

# Negative exponent (reciprocal)
result = 2 ** -2
print(result)  # Output: 0.25 (1/4)
```

---

## Math Module Functions

Import the math module to access advanced mathematical functions.

```python
import math
```

### Constants

```python
import math

# Pi
print(math.pi)  # Output: 3.141592653589793

# Euler's number
print(math.e)  # Output: 2.718281828459045

# Infinity
print(math.inf)  # Output: inf

# Tau (2 * pi)
print(math.tau)  # Output: 6.283185307179586
```

### Rounding Functions

```python
import math

# math.ceil() - Round up
print(math.ceil(4.2))   # Output: 5
print(math.ceil(4.8))   # Output: 5
print(math.ceil(-4.2))  # Output: -4

# math.floor() - Round down
print(math.floor(4.2))   # Output: 4
print(math.floor(4.8))   # Output: 4
print(math.floor(-4.2))  # Output: -5

# math.trunc() - Remove decimal part
print(math.trunc(4.8))   # Output: 4
print(math.trunc(-4.8))  # Output: -4
```

### Power and Logarithm Functions

```python
import math

# math.sqrt() - Square root
print(math.sqrt(16))  # Output: 4.0
print(math.sqrt(2))   # Output: 1.4142135623730951

# math.pow() - Power (returns float)
print(math.pow(2, 3))  # Output: 8.0

# math.exp() - e raised to power
print(math.exp(1))  # Output: 2.718281828459045
print(math.exp(2))  # Output: 7.38905609893065

# math.log() - Natural logarithm (base e)
print(math.log(math.e))  # Output: 1.0
print(math.log(10))      # Output: 2.302585092994046

# math.log10() - Base 10 logarithm
print(math.log10(100))  # Output: 2.0
print(math.log10(1000)) # Output: 3.0

# math.log2() - Base 2 logarithm
print(math.log2(8))   # Output: 3.0
print(math.log2(16))  # Output: 4.0

# math.log(x, base) - Custom base logarithm
print(math.log(8, 2))  # Output: 3.0
```

### Trigonometric Functions

```python
import math

# Angles in radians
angle = math.pi / 4  # 45 degrees

# math.sin() - Sine
print(math.sin(angle))  # Output: 0.7071067811865476

# math.cos() - Cosine
print(math.cos(angle))  # Output: 0.7071067811865476

# math.tan() - Tangent
print(math.tan(angle))  # Output: 0.9999999999999999

# Convert degrees to radians
degrees = 90
radians = math.radians(degrees)
print(math.sin(radians))  # Output: 1.0

# Convert radians to degrees
radians = math.pi / 2
degrees = math.degrees(radians)
print(degrees)  # Output: 90.0

# Inverse trigonometric functions
print(math.asin(1))  # Output: 1.5707963267948966 (pi/2)
print(math.acos(0))  # Output: 1.5707963267948966 (pi/2)
print(math.atan(1))  # Output: 0.7853981633974483 (pi/4)
```

### Other Useful Functions

```python
import math

# math.factorial() - Factorial
print(math.factorial(5))  # Output: 120 (5! = 5×4×3×2×1)
print(math.factorial(0))  # Output: 1

# math.gcd() - Greatest Common Divisor
print(math.gcd(48, 18))  # Output: 6
print(math.gcd(100, 50)) # Output: 50

# math.lcm() - Least Common Multiple (Python 3.9+)
print(math.lcm(4, 6))   # Output: 12
print(math.lcm(12, 18)) # Output: 36

# math.fabs() - Absolute value (returns float)
print(math.fabs(-5))    # Output: 5.0
print(math.fabs(-3.14)) # Output: 3.14

# math.copysign() - Copy sign from one number to another
print(math.copysign(5, -1))   # Output: -5.0
print(math.copysign(-5, 1))   # Output: 5.0

# math.isclose() - Check if two floats are close
print(math.isclose(0.1 + 0.2, 0.3))  # Output: True
print(math.isclose(1.0, 1.0001, rel_tol=0.01))  # Output: True
```

---

## Built-in Math Functions

These functions don't require importing math module.

### abs() - Absolute Value

```python
# Integer
print(abs(-5))    # Output: 5
print(abs(5))     # Output: 5

# Float
print(abs(-3.14)) # Output: 3.14

# Complex (returns magnitude)
print(abs(3 + 4j)) # Output: 5.0
```

### round() - Round to Nearest Integer

```python
# Basic rounding
print(round(4.5))    # Output: 4 (banker's rounding)
print(round(5.5))    # Output: 6
print(round(4.6))    # Output: 5

# Round to specific decimal places
print(round(3.14159, 2))  # Output: 3.14
print(round(3.14159, 3))  # Output: 3.142
print(round(1234.5678, 1)) # Output: 1234.6

# Round to tens, hundreds, etc.
print(round(1234, -1))  # Output: 1230
print(round(1234, -2))  # Output: 1200
```

### pow() - Power Function

```python
# Basic power
print(pow(2, 3))  # Output: 8

# With modulo (efficient for large numbers)
print(pow(2, 10, 1000))  # Output: 24 (2^10 % 1000)

# Negative exponent
print(pow(2, -2))  # Output: 0.25
```

### min() and max()

```python
# Find minimum
print(min(5, 2, 8, 1))  # Output: 1
print(min([10, 20, 5, 15]))  # Output: 5

# Find maximum
print(max(5, 2, 8, 1))  # Output: 8
print(max([10, 20, 5, 15]))  # Output: 20

# With strings (lexicographic order)
print(min("apple", "banana", "cherry"))  # Output: apple
print(max("apple", "banana", "cherry"))  # Output: cherry
```

### sum() - Sum of Iterable

```python
# Sum of list
numbers = [1, 2, 3, 4, 5]
print(sum(numbers))  # Output: 15

# Sum with start value
print(sum(numbers, 10))  # Output: 25 (15 + 10)

# Sum of range
print(sum(range(1, 11)))  # Output: 55 (1+2+...+10)
```

### divmod() - Division and Modulus

```python
# Returns (quotient, remainder)
result = divmod(10, 3)
print(result)  # Output: (3, 1)

quotient, remainder = divmod(17, 5)
print(f"17 ÷ 5 = {quotient} remainder {remainder}")
# Output: 17 ÷ 5 = 3 remainder 2
```

---

## Operator Precedence

Order of operations (PEMDAS/BODMAS):

1. **Parentheses** ()
2. **Exponentiation** **
3. **Unary** +x, -x
4. **Multiplication, Division, Floor Division, Modulus** *, /, //, %
5. **Addition, Subtraction** +, -

```python
# Without parentheses
result = 2 + 3 * 4
print(result)  # Output: 14 (not 20)

# With parentheses
result = (2 + 3) * 4
print(result)  # Output: 20

# Complex expression
result = 2 ** 3 + 4 * 5 - 6 / 2
print(result)  # Output: 25.0
# Breakdown: 8 + 20 - 3.0 = 25.0

# Using parentheses for clarity
result = (2 ** 3) + (4 * 5) - (6 / 2)
print(result)  # Output: 25.0
```

---

## Practical Examples

### Example 1: Simple Calculator

```python
print("=== Simple Calculator ===")
num1 = float(input("Enter first number: "))
operator = input("Enter operator (+, -, *, /): ")
num2 = float(input("Enter second number: "))

if operator == "+":
    result = num1 + num2
elif operator == "-":
    result = num1 - num2
elif operator == "*":
    result = num1 * num2
elif operator == "/":
    if num2 != 0:
        result = num1 / num2
    else:
        result = "Error: Division by zero"
else:
    result = "Invalid operator"

print(f"Result: {result}")
```

### Example 2: Circle Calculator

```python
import math

print("=== Circle Calculator ===")
radius = float(input("Enter radius: "))

# Calculate area
area = math.pi * radius ** 2
print(f"Area: {area:.2f}")

# Calculate circumference
circumference = 2 * math.pi * radius
print(f"Circumference: {circumference:.2f}")

# Calculate diameter
diameter = 2 * radius
print(f"Diameter: {diameter:.2f}")
```

### Example 3: Pythagorean Theorem

```python
import math

print("=== Pythagorean Theorem Calculator ===")
print("Find the hypotenuse of a right triangle")

a = float(input("Enter side a: "))
b = float(input("Enter side b: "))

# c² = a² + b²
c = math.sqrt(a ** 2 + b ** 2)
print(f"Hypotenuse c = {c:.2f}")
```

### Example 4: Compound Interest Calculator

```python
import math

print("=== Compound Interest Calculator ===")
principal = float(input("Enter principal amount: $"))
rate = float(input("Enter annual interest rate (%): "))
time = float(input("Enter time (years): "))
n = int(input("Enter number of times interest is compounded per year: "))

# A = P(1 + r/n)^(nt)
amount = principal * math.pow((1 + rate / (100 * n)), n * time)
interest = amount - principal

print(f"\nFinal Amount: ${amount:.2f}")
print(f"Interest Earned: ${interest:.2f}")
```

### Example 5: Distance Between Two Points

```python
import math

print("=== Distance Calculator ===")
x1 = float(input("Enter x1: "))
y1 = float(input("Enter y1: "))
x2 = float(input("Enter x2: "))
y2 = float(input("Enter y2: "))

# Distance = √[(x2-x1)² + (y2-y1)²]
distance = math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
print(f"Distance: {distance:.2f}")
```

---

## Practice Questions

### Beginner Level

**Question 1:** Calculate the average of three numbers.

```python
# Solution
num1 = float(input("Enter first number: "))
num2 = float(input("Enter second number: "))
num3 = float(input("Enter third number: "))

average = (num1 + num2 + num3) / 3
print(f"Average: {average:.2f}")
```

**Question 2:** Convert temperature from Celsius to Fahrenheit.
Formula: F = (C × 9/5) + 32

```python
# Solution
celsius = float(input("Enter temperature in Celsius: "))
fahrenheit = (celsius * 9/5) + 32
print(f"{celsius}°C = {fahrenheit}°F")
```

**Question 3:** Calculate the area of a triangle.
Formula: Area = (base × height) / 2

```python
# Solution
base = float(input("Enter base: "))
height = float(input("Enter height: "))
area = (base * height) / 2
print(f"Area: {area:.2f}")
```

### Intermediate Level

**Question 4:** Check if a year is a leap year.
Rules: Divisible by 4, but not by 100 unless also divisible by 400.

```python
# Solution
year = int(input("Enter a year: "))
is_leap = (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)
print(f"{year} is a leap year: {is_leap}")
```

**Question 5:** Calculate the volume of a sphere.
Formula: V = (4/3) × π × r³

```python
# Solution
import math

radius = float(input("Enter radius: "))
volume = (4/3) * math.pi * (radius ** 3)
print(f"Volume: {volume:.2f}")
```

**Question 6:** Find the nth Fibonacci number using formula.
Formula: Fib(n) = (φⁿ - ψⁿ) / √5, where φ = (1+√5)/2, ψ = (1-√5)/2

```python
# Solution
import math

n = int(input("Enter n: "))
phi = (1 + math.sqrt(5)) / 2
psi = (1 - math.sqrt(5)) / 2
fib = round((phi ** n - psi ** n) / math.sqrt(5))
print(f"Fibonacci({n}) = {fib}")
```

### Advanced Level

**Question 7:** Calculate loan monthly payment.
Formula: M = P × [r(1+r)ⁿ] / [(1+r)ⁿ - 1]
Where: M = monthly payment, P = principal, r = monthly rate, n = number of payments

```python
# Solution
import math

principal = float(input("Enter loan amount: $"))
annual_rate = float(input("Enter annual interest rate (%): "))
years = int(input("Enter loan term (years): "))

monthly_rate = annual_rate / (100 * 12)
num_payments = years * 12

if monthly_rate == 0:
    monthly_payment = principal / num_payments
else:
    monthly_payment = principal * (monthly_rate * (1 + monthly_rate) ** num_payments) / \
                     ((1 + monthly_rate) ** num_payments - 1)

total_payment = monthly_payment * num_payments
total_interest = total_payment - principal

print(f"\nMonthly Payment: ${monthly_payment:.2f}")
print(f"Total Payment: ${total_payment:.2f}")
print(f"Total Interest: ${total_interest:.2f}")
```

**Question 8:** Calculate standard deviation of a list of numbers.
Formula: σ = √[Σ(x - μ)² / N]

```python
# Solution
import math

numbers = [float(x) for x in input("Enter numbers (space-separated): ").split()]

# Calculate mean
mean = sum(numbers) / len(numbers)

# Calculate variance
variance = sum((x - mean) ** 2 for x in numbers) / len(numbers)

# Calculate standard deviation
std_dev = math.sqrt(variance)

print(f"Mean: {mean:.2f}")
print(f"Standard Deviation: {std_dev:.2f}")
```

**Question 9:** Calculate the angle between two vectors.
Formula: θ = arccos[(a·b) / (|a||b|)]

```python
# Solution
import math

print("Enter first vector (x, y):")
a_x = float(input("x: "))
a_y = float(input("y: "))

print("Enter second vector (x, y):")
b_x = float(input("x: "))
b_y = float(input("y: "))

# Dot product
dot_product = a_x * b_x + a_y * b_y

# Magnitudes
mag_a = math.sqrt(a_x ** 2 + a_y ** 2)
mag_b = math.sqrt(b_x ** 2 + b_y ** 2)

# Angle in radians
angle_rad = math.acos(dot_product / (mag_a * mag_b))

# Convert to degrees
angle_deg = math.degrees(angle_rad)

print(f"Angle: {angle_deg:.2f}°")
```

**Question 10:** Scientific calculator with multiple operations.

```python
# Solution
import math

print("=== Scientific Calculator ===")
print("1. Square Root")
print("2. Power")
print("3. Logarithm (base 10)")
print("4. Sine")
print("5. Cosine")
print("6. Factorial")

choice = int(input("Enter choice (1-6): "))

if choice == 1:
    num = float(input("Enter number: "))
    result = math.sqrt(num)
elif choice == 2:
    base = float(input("Enter base: "))
    exp = float(input("Enter exponent: "))
    result = base ** exp
elif choice == 3:
    num = float(input("Enter number: "))
    result = math.log10(num)
elif choice == 4:
    angle = float(input("Enter angle in degrees: "))
    result = math.sin(math.radians(angle))
elif choice == 5:
    angle = float(input("Enter angle in degrees: "))
    result = math.cos(math.radians(angle))
elif choice == 6:
    num = int(input("Enter number: "))
    result = math.factorial(num)
else:
    result = "Invalid choice"

print(f"Result: {result}")
```

---

## Key Takeaways

1. **Basic operators**: +, -, *, /, //, %, **
2. **Division** (/) always returns float, **floor division** (//) returns integer
3. **Modulus** (%) returns remainder, useful for checking divisibility
4. **Exponentiation** (**) is more efficient than math.pow()
5. **math module** provides advanced functions (trigonometry, logarithms, etc.)
6. **Built-in functions**: abs(), round(), min(), max(), sum(), divmod()
7. **Operator precedence**: Parentheses → Exponentiation → Multiplication/Division → Addition/Subtraction
8. **Always use parentheses** for clarity in complex expressions

---

## Common Mistakes to Avoid

```python
# Mistake 1: Integer division in Python 2 style
# In Python 3, / always returns float
result = 5 / 2  # 2.5, not 2

# Mistake 2: Forgetting operator precedence
result = 2 + 3 * 4  # 14, not 20
result = (2 + 3) * 4  # 20 (use parentheses)

# Mistake 3: Division by zero
# result = 10 / 0  # ZeroDivisionError
# Always check before dividing

# Mistake 4: Using degrees in trig functions
# math.sin(90)  # Wrong! Use radians
math.sin(math.radians(90))  # Correct

# Mistake 5: Floating point precision
result = 0.1 + 0.2
print(result == 0.3)  # False!
print(math.isclose(result, 0.3))  # True (use isclose)
```

---

## Next Steps

Now that you understand arithmetic and math operations, you're ready to learn:
- **String Methods and Formatting**
- **Conditional Statements (if/elif/else)**
- **Logical Operators**
- **Loops (for/while)**

Keep practicing with real-world problems!
