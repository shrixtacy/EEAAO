# Python String Methods - Complete Reference Guide

## Table of Contents
1. [String Basics](#string-basics)
2. [String Indexing and Slicing](#string-indexing-and-slicing)
3. [String Methods - Case Conversion](#case-conversion-methods)
4. [String Methods - Searching](#searching-methods)
5. [String Methods - Validation](#validation-methods)
6. [String Methods - Modification](#modification-methods)
7. [Format Specifiers](#format-specifiers)
8. [Practice Questions](#practice-questions)

---

## String Basics

### Creating Strings

```python
# Single quotes
name = 'John'

# Double quotes
message = "Hello, World!"

# Triple quotes (multiline)
paragraph = """This is a
multiline
string"""

# Escape characters
text = "He said, \"Hello!\""
path = "C:\\Users\\Documents"
newline = "Line 1\nLine 2"
tab = "Column1\tColumn2"
```

### String Properties

```python
# Strings are immutable
text = "Hello"
# text[0] = 'h'  # TypeError: 'str' object does not support item assignment

# Strings are iterable
for char in "Python":
    print(char)  # P y t h o n

# String length
text = "Hello"
print(len(text))  # Output: 5
```

---

## String Indexing and Slicing

### Indexing

```python
text = "Python"

# Positive indexing (0-based)
print(text[0])   # Output: P
print(text[1])   # Output: y
print(text[5])   # Output: n

# Negative indexing (from end)
print(text[-1])  # Output: n
print(text[-2])  # Output: o
print(text[-6])  # Output: P
```

### Slicing

```python
text = "Python Programming"

# Basic slicing [start:end]
print(text[0:6])    # Output: Python
print(text[7:18])   # Output: Programming

# Omitting start (starts from beginning)
print(text[:6])     # Output: Python

# Omitting end (goes to end)
print(text[7:])     # Output: Programming

# Negative indices
print(text[-11:])   # Output: Programming
print(text[:-12])   # Output: Python

# Step parameter [start:end:step]
print(text[::2])    # Output: Pto rgamn (every 2nd char)
print(text[::3])    # Output: Ph ormn (every 3rd char)

# Reverse string
print(text[::-1])   # Output: gnimmargorP nohtyP

# Reverse with step
print(text[::-2])   # Output: gimrr oty
```

### Slicing Examples

```python
text = "Hello, World!"

# Get first 5 characters
print(text[:5])     # Output: Hello

# Get last 6 characters
print(text[-6:])    # Output: World!

# Get middle portion
print(text[7:12])   # Output: World

# Remove first and last character
print(text[1:-1])   # Output: ello, World

# Get every other character
print(text[::2])    # Output: Hlo ol!

# Reverse the string
print(text[::-1])   # Output: !dlroW ,olleH
```

---

## Case Conversion Methods

### upper() - Convert to Uppercase

```python
text = "hello world"
result = text.upper()
print(result)  # Output: HELLO WORLD

# Original string unchanged (immutable)
print(text)    # Output: hello world

# Practical use
email = input("Enter email: ").upper()
```

### lower() - Convert to Lowercase

```python
text = "HELLO WORLD"
result = text.lower()
print(result)  # Output: hello world

# Case-insensitive comparison
user_input = "YES"
if user_input.lower() == "yes":
    print("Confirmed!")
```

### capitalize() - Capitalize First Letter

```python
text = "hello world"
result = text.capitalize()
print(result)  # Output: Hello world

# Only first letter of entire string
text = "hello WORLD"
print(text.capitalize())  # Output: Hello world
```

### title() - Title Case (Each Word Capitalized)

```python
text = "hello world from python"
result = text.title()
print(result)  # Output: Hello World From Python

# Useful for names
name = "john doe"
print(name.title())  # Output: John Doe
```

### swapcase() - Swap Case

```python
text = "Hello World"
result = text.swapcase()
print(result)  # Output: hELLO wORLD

text = "PyThOn"
print(text.swapcase())  # Output: pYtHoN
```

### casefold() - Aggressive Lowercase

```python
# Similar to lower() but more aggressive
text = "HELLO"
print(text.casefold())  # Output: hello

# Handles special characters better
german = "ß"
print(german.lower())     # Output: ß
print(german.casefold())  # Output: ss
```

---

## Searching Methods

### find() - Find Substring (Returns Index or -1)

```python
text = "Hello, World!"

# Find substring
print(text.find("World"))   # Output: 7
print(text.find("Python"))  # Output: -1 (not found)

# Find with start position
print(text.find("o"))       # Output: 4 (first 'o')
print(text.find("o", 5))    # Output: 8 (second 'o')

# Find with start and end
print(text.find("o", 5, 10))  # Output: 8
```

### rfind() - Find from Right

```python
text = "Hello, World!"

# Find last occurrence
print(text.rfind("o"))      # Output: 8
print(text.rfind("l"))      # Output: 10
```

### index() - Like find() but Raises Error

```python
text = "Hello, World!"

# Find substring
print(text.index("World"))  # Output: 7

# Not found raises ValueError
# print(text.index("Python"))  # ValueError
```

### rindex() - Index from Right

```python
text = "Hello, World!"
print(text.rindex("o"))     # Output: 8
```

### count() - Count Occurrences

```python
text = "Hello, World!"

# Count character
print(text.count("l"))      # Output: 3
print(text.count("o"))      # Output: 2

# Count substring
print(text.count("ll"))     # Output: 1

# Count with range
print(text.count("o", 0, 5))  # Output: 1
```

### startswith() - Check if Starts With

```python
text = "Hello, World!"

# Check prefix
print(text.startswith("Hello"))   # Output: True
print(text.startswith("World"))   # Output: False

# Check with start position
print(text.startswith("World", 7))  # Output: True

# Multiple prefixes (tuple)
print(text.startswith(("Hello", "Hi")))  # Output: True
```

### endswith() - Check if Ends With

```python
text = "Hello, World!"

# Check suffix
print(text.endswith("!"))       # Output: True
print(text.endswith("World!"))  # Output: True
print(text.endswith("Hello"))   # Output: False

# Check with range
print(text.endswith("World", 0, 12))  # Output: True

# Multiple suffixes
filename = "document.pdf"
print(filename.endswith((".pdf", ".doc", ".txt")))  # Output: True
```

---

## Validation Methods

### isalpha() - Check if All Alphabetic

```python
print("Hello".isalpha())      # Output: True
print("Hello123".isalpha())   # Output: False
print("Hello World".isalpha()) # Output: False (space)
print("".isalpha())           # Output: False (empty)
```

### isdigit() - Check if All Digits

```python
print("123".isdigit())        # Output: True
print("123.45".isdigit())     # Output: False (decimal point)
print("12a3".isdigit())       # Output: False
print("".isdigit())           # Output: False
```

### isalnum() - Check if Alphanumeric

```python
print("Hello123".isalnum())   # Output: True
print("Hello 123".isalnum())  # Output: False (space)
print("Hello!".isalnum())     # Output: False (special char)
```

### isspace() - Check if All Whitespace

```python
print("   ".isspace())        # Output: True
print("\t\n".isspace())       # Output: True
print("Hello ".isspace())     # Output: False
print("".isspace())           # Output: False
```

### islower() - Check if All Lowercase

```python
print("hello".islower())      # Output: True
print("Hello".islower())      # Output: False
print("hello123".islower())   # Output: True (numbers ignored)
print("hello world".islower()) # Output: True
```

### isupper() - Check if All Uppercase

```python
print("HELLO".isupper())      # Output: True
print("Hello".isupper())      # Output: False
print("HELLO123".isupper())   # Output: True
```

### istitle() - Check if Title Case

```python
print("Hello World".istitle())  # Output: True
print("Hello world".istitle())  # Output: False
print("HELLO WORLD".istitle())  # Output: False
```

### isdecimal() - Check if Decimal

```python
print("123".isdecimal())      # Output: True
print("12.3".isdecimal())     # Output: False
print("½".isdecimal())        # Output: False
```

### isnumeric() - Check if Numeric

```python
print("123".isnumeric())      # Output: True
print("½".isnumeric())        # Output: True
print("Ⅳ".isnumeric())        # Output: True (Roman numeral)
```

### isidentifier() - Check if Valid Identifier

```python
print("variable".isidentifier())   # Output: True
print("_private".isidentifier())   # Output: True
print("2variable".isidentifier())  # Output: False
print("my-var".isidentifier())     # Output: False
```

### isprintable() - Check if Printable

```python
print("Hello".isprintable())  # Output: True
print("Hello\n".isprintable()) # Output: False (newline)
print("Hello\t".isprintable()) # Output: False (tab)
```

---

## Modification Methods

### strip() - Remove Leading/Trailing Whitespace

```python
text = "   Hello, World!   "
print(text.strip())           # Output: "Hello, World!"

# Remove specific characters
text = "***Hello***"
print(text.strip("*"))        # Output: "Hello"

# Original unchanged
print(text)                   # Output: "***Hello***"
```

### lstrip() - Remove Leading Whitespace

```python
text = "   Hello, World!   "
print(text.lstrip())          # Output: "Hello, World!   "

text = "***Hello***"
print(text.lstrip("*"))       # Output: "Hello***"
```

### rstrip() - Remove Trailing Whitespace

```python
text = "   Hello, World!   "
print(text.rstrip())          # Output: "   Hello, World!"

text = "***Hello***"
print(text.rstrip("*"))       # Output: "***Hello"
```

### replace() - Replace Substring

```python
text = "Hello, World!"

# Replace substring
print(text.replace("World", "Python"))  # Output: Hello, Python!

# Replace with count limit
text = "one one one"
print(text.replace("one", "two", 2))    # Output: two two one

# Remove substring (replace with empty)
text = "Hello, World!"
print(text.replace(",", ""))            # Output: Hello World!
```

### split() - Split into List

```python
text = "Hello World Python"

# Split by whitespace (default)
print(text.split())           # Output: ['Hello', 'World', 'Python']

# Split by specific delimiter
text = "apple,banana,cherry"
print(text.split(","))        # Output: ['apple', 'banana', 'cherry']

# Split with max splits
text = "one two three four"
print(text.split(" ", 2))     # Output: ['one', 'two', 'three four']

# Split lines
text = "Line 1\nLine 2\nLine 3"
print(text.split("\n"))       # Output: ['Line 1', 'Line 2', 'Line 3']
```

### rsplit() - Split from Right

```python
text = "one two three four"
print(text.rsplit(" ", 2))    # Output: ['one two', 'three', 'four']
```

### splitlines() - Split by Line Breaks

```python
text = "Line 1\nLine 2\r\nLine 3"
print(text.splitlines())      # Output: ['Line 1', 'Line 2', 'Line 3']

# Keep line breaks
print(text.splitlines(True))  # Output: ['Line 1\n', 'Line 2\r\n', 'Line 3']
```

### join() - Join List into String

```python
# Join with space
words = ["Hello", "World", "Python"]
print(" ".join(words))        # Output: Hello World Python

# Join with comma
print(",".join(words))        # Output: Hello,World,Python

# Join with newline
print("\n".join(words))       # Output: Hello\nWorld\nPython

# Join numbers (must convert to strings)
numbers = [1, 2, 3]
print("-".join(str(n) for n in numbers))  # Output: 1-2-3
```

### center() - Center String

```python
text = "Hello"

# Center with width
print(text.center(20))        # Output: "       Hello        "

# Center with fill character
print(text.center(20, "*"))   # Output: "*******Hello********"
```

### ljust() - Left Justify

```python
text = "Hello"
print(text.ljust(20))         # Output: "Hello               "
print(text.ljust(20, "-"))    # Output: "Hello---------------"
```

### rjust() - Right Justify

```python
text = "Hello"
print(text.rjust(20))         # Output: "               Hello"
print(text.rjust(20, "-"))    # Output: "---------------Hello"
```

### zfill() - Pad with Zeros

```python
# Useful for numbers
print("42".zfill(5))          # Output: 00042
print("-42".zfill(5))         # Output: -0042
print("3.14".zfill(6))        # Output: 003.14
```

### expandtabs() - Expand Tabs

```python
text = "Hello\tWorld"
print(text.expandtabs())      # Output: "Hello   World" (default 8 spaces)
print(text.expandtabs(4))     # Output: "Hello   World" (4 spaces)
```

---

## Format Specifiers

### Old Style (%) Formatting

```python
# String
name = "John"
print("Hello, %s!" % name)    # Output: Hello, John!

# Integer
age = 25
print("Age: %d" % age)        # Output: Age: 25

# Float
price = 19.99
print("Price: %.2f" % price)  # Output: Price: 19.99

# Multiple values
print("%s is %d years old" % (name, age))  # Output: John is 25 years old
```

### str.format() Method

```python
# Positional arguments
print("Hello, {}!".format("John"))  # Output: Hello, John!

# Multiple arguments
print("{} is {} years old".format("John", 25))  # Output: John is 25 years old

# Indexed arguments
print("{1} {0}".format("World", "Hello"))  # Output: Hello World

# Named arguments
print("{name} is {age} years old".format(name="John", age=25))

# Number formatting
print("Price: {:.2f}".format(19.99))  # Output: Price: 19.99
print("Number: {:,}".format(1000000))  # Output: Number: 1,000,000
```

### f-strings (Python 3.6+) - RECOMMENDED

```python
# Basic usage
name = "John"
age = 25
print(f"Hello, {name}!")      # Output: Hello, John!
print(f"{name} is {age} years old")  # Output: John is 25 years old

# Expressions
print(f"Next year: {age + 1}")  # Output: Next year: 26

# Number formatting
price = 19.99
print(f"Price: {price:.2f}")  # Output: Price: 19.99

# Alignment
print(f"{name:>10}")          # Output: "      John" (right align)
print(f"{name:<10}")          # Output: "John      " (left align)
print(f"{name:^10}")          # Output: "   John   " (center)

# Padding
number = 42
print(f"{number:05d}")        # Output: 00042

# Thousands separator
big_num = 1000000
print(f"{big_num:,}")         # Output: 1,000,000

# Percentage
ratio = 0.75
print(f"{ratio:.2%}")         # Output: 75.00%

# Scientific notation
large = 1000000
print(f"{large:e}")           # Output: 1.000000e+06
```

### Format Specification Mini-Language

```python
value = 123.456

# General format: {value:[[fill]align][sign][#][0][width][,][.precision][type]}

# Fill and align
print(f"{value:*>10}")        # Output: ***123.456
print(f"{value:*<10}")        # Output: 123.456***
print(f"{value:*^10}")        # Output: *123.456**

# Sign
print(f"{value:+}")           # Output: +123.456
print(f"{-value:+}")          # Output: -123.456
print(f"{value: }")           # Output:  123.456 (space for positive)

# Width and precision
print(f"{value:10.2f}")       # Output: "    123.46"

# Type specifiers
print(f"{42:b}")              # Output: 101010 (binary)
print(f"{42:o}")              # Output: 52 (octal)
print(f"{42:x}")              # Output: 2a (hexadecimal)
print(f"{42:X}")              # Output: 2A (uppercase hex)
```

---

## Practice Questions

### Beginner Level

**Question 1:** Extract the domain from an email address.

```python
# Solution
email = "user@example.com"
domain = email.split("@")[1]
print(f"Domain: {domain}")  # Output: Domain: example.com
```

**Question 2:** Count vowels in a string.

```python
# Solution
text = input("Enter a string: ").lower()
vowels = "aeiou"
count = sum(1 for char in text if char in vowels)
print(f"Vowels: {count}")
```

**Question 3:** Reverse each word in a sentence.

```python
# Solution
sentence = "Hello World Python"
reversed_words = " ".join(word[::-1] for word in sentence.split())
print(reversed_words)  # Output: olleH dlroW nohtyP
```

### Intermediate Level

**Question 4:** Check if a string is a palindrome.

```python
# Solution
text = input("Enter a string: ").lower().replace(" ", "")
is_palindrome = text == text[::-1]
print(f"Is palindrome: {is_palindrome}")
```

**Question 5:** Remove all digits from a string.

```python
# Solution
text = "Hello123World456"
result = ''.join(char for char in text if not char.isdigit())
print(result)  # Output: HelloWorld
```

**Question 6:** Title case a name properly (handle "McDonald", "O'Brien").

```python
# Solution
def title_case_name(name):
    # Handle special cases
    special_prefixes = ["Mc", "Mac", "O'"]
    words = name.split()
    result = []
    
    for word in words:
        if word.startswith("Mc") and len(word) > 2:
            result.append("Mc" + word[2:].capitalize())
        elif word.startswith("Mac") and len(word) > 3:
            result.append("Mac" + word[3:].capitalize())
        elif word.startswith("O'") and len(word) > 2:
            result.append("O'" + word[2:].capitalize())
        else:
            result.append(word.capitalize())
    
    return " ".join(result)

print(title_case_name("john mcdonald"))  # Output: John McDonald
print(title_case_name("mary o'brien"))   # Output: Mary O'Brien
```

### Advanced Level

**Question 7:** Implement a simple text encryption (Caesar cipher).

```python
# Solution
def caesar_cipher(text, shift):
    result = ""
    for char in text:
        if char.isalpha():
            start = ord('A') if char.isupper() else ord('a')
            shifted = (ord(char) - start + shift) % 26 + start
            result += chr(shifted)
        else:
            result += char
    return result

text = "Hello, World!"
encrypted = caesar_cipher(text, 3)
print(f"Encrypted: {encrypted}")  # Output: Khoor, Zruog!

decrypted = caesar_cipher(encrypted, -3)
print(f"Decrypted: {decrypted}")  # Output: Hello, World!
```

**Question 8:** Parse and format a phone number.

```python
# Solution
def format_phone(phone):
    # Remove all non-digits
    digits = ''.join(char for char in phone if char.isdigit())
    
    # Format based on length
    if len(digits) == 10:
        return f"({digits[:3]}) {digits[3:6]}-{digits[6:]}"
    elif len(digits) == 11 and digits[0] == '1':
        return f"+1 ({digits[1:4]}) {digits[4:7]}-{digits[7:]}"
    else:
        return "Invalid phone number"

print(format_phone("1234567890"))      # Output: (123) 456-7890
print(format_phone("11234567890"))     # Output: +1 (123) 456-7890
print(format_phone("(123) 456-7890"))  # Output: (123) 456-7890
```

**Question 9:** Extract all URLs from text.

```python
# Solution
import re

def extract_urls(text):
    # Simple URL pattern
    pattern = r'https?://[^\s]+'
    urls = re.findall(pattern, text)
    return urls

text = "Visit https://example.com and http://test.org for more info"
urls = extract_urls(text)
print(urls)  # Output: ['https://example.com', 'http://test.org']
```

**Question 10:** Create a text alignment function.

```python
# Solution
def align_text(text, width, alignment='left'):
    lines = text.split('\n')
    result = []
    
    for line in lines:
        if alignment == 'left':
            result.append(line.ljust(width))
        elif alignment == 'right':
            result.append(line.rjust(width))
        elif alignment == 'center':
            result.append(line.center(width))
    
    return '\n'.join(result)

text = "Hello\nWorld\nPython"
print(align_text(text, 20, 'center'))
```

---

## All String Methods Summary

```python
# Case conversion
str.upper(), str.lower(), str.capitalize(), str.title(), str.swapcase(), str.casefold()

# Searching
str.find(), str.rfind(), str.index(), str.rindex(), str.count()
str.startswith(), str.endswith()

# Validation
str.isalpha(), str.isdigit(), str.isalnum(), str.isspace()
str.islower(), str.isupper(), str.istitle()
str.isdecimal(), str.isnumeric(), str.isidentifier(), str.isprintable()

# Modification
str.strip(), str.lstrip(), str.rstrip()
str.replace(), str.split(), str.rsplit(), str.splitlines(), str.join()
str.center(), str.ljust(), str.rjust(), str.zfill()
str.expandtabs()

# Formatting
str.format(), f-strings
```

---

## Key Takeaways

1. **Strings are immutable** - methods return new strings
2. **Indexing starts at 0**, negative indices count from end
3. **Slicing syntax**: [start:end:step]
4. **Use f-strings** for modern string formatting
5. **strip()** removes whitespace, useful for user input
6. **split() and join()** are inverses of each other
7. **Validation methods** (is*) return boolean values
8. **Case methods** don't modify original string

---

## Next Steps

Now that you master strings, move on to:
- **Conditional Statements (if/elif/else)**
- **Logical Operators**
- **Loops (for/while)**
- **Lists, Tuples, and Sets**

Keep practicing with real-world text processing!
