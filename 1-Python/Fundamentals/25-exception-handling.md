# Python Exception Handling - Complete Guide

## 📚 Table of Contents
1. [try-except Blocks](#try-except-blocks)
2. [Multiple except Clauses](#multiple-except-clauses)
3. [else and finally](#else-and-finally)
4. [Raising Exceptions](#raising-exceptions)
5. [Custom Exceptions](#custom-exceptions)
6. [Practice Questions](#practice-questions)

---

## 1. try-except Blocks

```python
# Basic exception handling
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")

# Catching any exception
try:
    result = int("abc")
except Exception as e:
    print(f"Error: {e}")
```

---

## 2. Multiple except Clauses

```python
try:
    num = int(input("Enter a number: "))
    result = 10 / num
except ValueError:
    print("Invalid input!")
except ZeroDivisionError:
    print("Cannot divide by zero!")
except Exception as e:
    print(f"Unexpected error: {e}")
```

---

## 3. else and finally

```python
try:
    file = open('file.txt', 'r')
    content = file.read()
except FileNotFoundError:
    print("File not found!")
else:
    print("File read successfully")
    print(content)
finally:
    print("Cleanup code runs always")
    if 'file' in locals():
        file.close()
```

---

## 4. Raising Exceptions

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Divisor cannot be zero")
    return a / b

try:
    result = divide(10, 0)
except ValueError as e:
    print(e)
```

---

## 5. Custom Exceptions

```python
class InsufficientFundsError(Exception):
    pass

class BankAccount:
    def __init__(self, balance):
        self.balance = balance
    
    def withdraw(self, amount):
        if amount > self.balance:
            raise InsufficientFundsError("Not enough funds")
        self.balance -= amount

account = BankAccount(100)
try:
    account.withdraw(150)
except InsufficientFundsError as e:
    print(e)
```

---

## 6. Practice Questions

**Question 1**: Write a function that handles file reading errors.
**Question 2**: Create a custom exception for invalid age values.
**Question 3**: Write a program that validates user input with exception handling.

---

## 🎯 Key Takeaways

1. Use `try-except` to handle errors
2. Catch specific exceptions first
3. `else` runs if no exception occurs
4. `finally` always runs
5. Create custom exceptions for specific errors

---

**Next Topic**: Random Numbers
**Previous Topic**: Reading Files

Happy Coding! 🐍
