# Regular Expressions

## 📚 Theoretical Understanding

### What are Regular Expressions?

Regular expressions (regex or regexp) are powerful patterns used for matching, searching, and manipulating text. Think of them as a specialized mini-language for describing text patterns. Instead of searching for exact strings like "hello", you can search for patterns like "any word that starts with 'h' and ends with 'o'" or "any sequence of digits" or "email addresses in this format."

Regular expressions are not unique to Python - they're a universal concept used across programming languages, text editors, command-line tools, and databases. Learning regex in Python means you're learning a skill that transfers to JavaScript, Java, Perl, and many other environments. The syntax might vary slightly between implementations, but the core concepts remain the same.

The power of regular expressions lies in their ability to express complex patterns concisely. What might take dozens of lines of string manipulation code can often be expressed in a single regex pattern. For example, validating an email address format, extracting phone numbers from text, or finding all URLs in a document becomes straightforward with regex. However, this power comes with complexity - regex patterns can be cryptic and hard to read, especially for beginners.

### Why Regular Expressions Matter

In real-world programming, you constantly work with text data: user input, log files, web scraping, data cleaning, configuration files, and more. Regular expressions are the Swiss Army knife for text processing. They enable you to:

- **Validate input**: Check if user input matches expected patterns (email, phone number, password strength)
- **Extract data**: Pull specific information from unstructured text (dates, prices, names)
- **Transform text**: Find and replace patterns (reformatting dates, cleaning data)
- **Parse logs**: Extract relevant information from log files
- **Web scraping**: Extract data from HTML (though libraries like BeautifulSoup are often better)

Without regex, these tasks require writing complex string manipulation code with loops, conditionals, and multiple string methods. With regex, you describe the pattern you're looking for, and the regex engine handles the searching. This makes your code more concise, often more efficient, and easier to maintain (once you understand regex syntax).

### The re Module in Python

Python provides regular expression functionality through the `re` module, which is part of the standard library. You don't need to install anything - just import it. The `re` module provides functions for pattern matching, searching, and text manipulation using regular expressions.

The most commonly used functions in the `re` module are:
- `re.search()`: Searches for a pattern anywhere in the string
- `re.match()`: Checks if the pattern matches at the beginning of the string
- `re.findall()`: Returns all non-overlapping matches as a list
- `re.sub()`: Replaces matches with a new string
- `re.split()`: Splits string by pattern matches

Understanding when to use each function is crucial. `search()` is most versatile - it finds the pattern anywhere in the string. `match()` is stricter - it only succeeds if the pattern matches at the start. `findall()` is perfect when you need all occurrences, not just the first. `sub()` is your find-and-replace tool.

### Regular Expression Syntax Basics

Regular expressions use special characters (metacharacters) that have special meanings:

- `.` (dot): Matches any single character except newline
- `^`: Matches the start of the string
- `$`: Matches the end of the string
- `*`: Matches 0 or more repetitions of the preceding pattern
- `+`: Matches 1 or more repetitions
- `?`: Matches 0 or 1 repetition (makes preceding pattern optional)
- `[]`: Character class - matches any one character inside brackets
- `|`: OR operator - matches either pattern on left or right
- `()`: Groups patterns together
- `\`: Escapes special characters or introduces special sequences

These metacharacters can be combined to create complex patterns. For example, `\d+` means "one or more digits", `[a-z]*` means "zero or more lowercase letters", and `^[A-Z]` means "starts with an uppercase letter".

### Character Classes and Special Sequences

Character classes let you specify a set of characters to match. `[abc]` matches 'a', 'b', or 'c'. `[a-z]` matches any lowercase letter. `[0-9]` matches any digit. You can negate a character class with `^`: `[^0-9]` matches anything that's NOT a digit.

Python's regex also provides shorthand character classes:
- `\d`: Matches any digit (equivalent to `[0-9]`)
- `\D`: Matches any non-digit
- `\w`: Matches any word character (letters, digits, underscore)
- `\W`: Matches any non-word character
- `\s`: Matches any whitespace (space, tab, newline)
- `\S`: Matches any non-whitespace

These shorthands make patterns more readable. Instead of `[0-9][0-9][0-9]`, you can write `\d{3}` (three digits). Instead of `[a-zA-Z0-9_]+`, you can write `\w+` (one or more word characters).

---

## 💻 Practical Examples

### Basic Pattern Matching

```python
import re

# Simple search
text = "The quick brown fox jumps over the lazy dog"

# Search for a word
match = re.search(r'fox', text)
if match:
    print("Found 'fox' at position:", match.start())  # Position 16
    print("Matched text:", match.group())  # 'fox'

# Match at beginning
match = re.match(r'The', text)
print("Matches at start:", match is not None)  # True

match = re.match(r'fox', text)
print("Matches at start:", match is not None)  # False (fox is not at start)

# Find all occurrences
text = "The cat and the dog and the bird"
matches = re.findall(r'the', text, re.IGNORECASE)
print("Found 'the':", len(matches), "times")  # 3 times
print("Matches:", matches)  # ['The', 'the', 'the']
```

### Character Classes and Quantifiers

```python
import re

# Match digits
text = "My phone number is 555-1234"
digits = re.findall(r'\d', text)
print("All digits:", digits)  # ['5', '5', '5', '1', '2', '3', '4']

# Match sequences of digits
phone = re.findall(r'\d+', text)
print("Number sequences:", phone)  # ['555', '1234']

# Match specific pattern
phone_pattern = r'\d{3}-\d{4}'
match = re.search(phone_pattern, text)
if match:
    print("Phone number:", match.group())  # 555-1234

# Match email pattern
text = "Contact us at support@example.com or sales@company.org"
email_pattern = r'\w+@\w+\.\w+'
emails = re.findall(email_pattern, text)
print("Emails found:", emails)  # ['support@example.com', 'sales@company.org']

# Match words
text = "Hello, World! How are you?"
words = re.findall(r'\w+', text)
print("Words:", words)  # ['Hello', 'World', 'How', 'are', 'you']
```

### Groups and Capturing

```python
import re

# Extract parts of a pattern
text = "John Doe, age 30, email: john@example.com"

# Capture name and age
pattern = r'(\w+ \w+), age (\d+)'
match = re.search(pattern, text)
if match:
    name = match.group(1)  # First group
    age = match.group(2)   # Second group
    print(f"Name: {name}, Age: {age}")  # Name: John Doe, Age: 30

# Extract email components
email_pattern = r'(\w+)@(\w+)\.(\w+)'
match = re.search(email_pattern, text)
if match:
    username = match.group(1)
    domain = match.group(2)
    tld = match.group(3)
    print(f"Username: {username}")  # john
    print(f"Domain: {domain}")      # example
    print(f"TLD: {tld}")            # com

# Named groups (more readable)
pattern = r'(?P<name>\w+ \w+), age (?P<age>\d+)'
match = re.search(pattern, text)
if match:
    print("Name:", match.group('name'))  # John Doe
    print("Age:", match.group('age'))    # 30
```

### Find and Replace

```python
import re

# Simple replacement
text = "The color is red. I like red."
new_text = re.sub(r'red', 'blue', text)
print(new_text)  # The color is blue. I like blue.

# Replace with limit
new_text = re.sub(r'red', 'blue', text, count=1)
print(new_text)  # The color is blue. I like red.

# Replace using groups
text = "Date: 2024-01-15"
# Convert YYYY-MM-DD to DD/MM/YYYY
new_text = re.sub(r'(\d{4})-(\d{2})-(\d{2})', r'\3/\2/\1', text)
print(new_text)  # Date: 15/01/2024

# Replace with function
def uppercase_match(match):
    return match.group(0).upper()

text = "hello world"
new_text = re.sub(r'\w+', uppercase_match, text)
print(new_text)  # HELLO WORLD
```

### Splitting Strings

```python
import re

# Split by whitespace
text = "apple banana cherry date"
words = re.split(r'\s+', text)
print(words)  # ['apple', 'banana', 'cherry', 'date']

# Split by multiple delimiters
text = "apple,banana;cherry:date"
fruits = re.split(r'[,;:]', text)
print(fruits)  # ['apple', 'banana', 'cherry', 'date']

# Split by pattern
text = "one1two2three3four"
parts = re.split(r'\d+', text)
print(parts)  # ['one', 'two', 'three', 'four']
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain the difference between `re.search()`, `re.match()`, and `re.findall()`. When should you use each function? Provide examples demonstrating their different behaviors.

**Detailed Answer:**

These three functions are the most commonly used in the `re` module, but they behave differently and are suited for different tasks. Understanding their differences is crucial for effective regex usage in Python.

**`re.search()` - Find First Match Anywhere**

`re.search()` scans through the entire string looking for the first location where the pattern matches. It returns a Match object if found, or None if no match exists. This is the most flexible and commonly used function.

```python
import re

text = "The price is $25.99 and the tax is $3.50"

# Search finds the first match anywhere in the string
match = re.search(r'\$\d+\.\d+', text)
if match:
    print("Found:", match.group())  # Found: $25.99
    print("Position:", match.start())  # Position: 13

# Even if pattern is at the end
text = "Hello World Python"
match = re.search(r'Python', text)
if match:
    print("Found Python")  # This works!

# Returns None if not found
match = re.search(r'Java', text)
print(match)  # None
```

**When to use `re.search()`:**
- When you need to find if a pattern exists anywhere in the string
- When you only need the first occurrence
- When you need the position of the match
- Most general-purpose pattern matching

**`re.match()` - Match Only at Beginning**

`re.match()` checks if the pattern matches at the START of the string. If the pattern appears later in the string, `match()` returns None. This is more restrictive than `search()`.

```python
import re

text = "Python is awesome"

# match() succeeds - pattern at start
match = re.match(r'Python', text)
if match:
    print("Matched:", match.group())  # Matched: Python

# match() fails - pattern not at start
match = re.match(r'awesome', text)
print(match)  # None

# Even though 'awesome' is in the string, match() doesn't find it
# because it's not at the beginning

# To match entire string from start
text = "Python123"
match = re.match(r'Python\d+', text)
if match:
    print("Valid format")  # Valid format

# Fails if anything before the pattern
text = " Python123"  # Note the space
match = re.match(r'Python\d+', text)
print(match)  # None (space at start)
```

**When to use `re.match()`:**
- When validating input format (must match from the start)
- When checking if a string starts with a specific pattern
- When parsing structured data that must follow a specific format
- When you want to ensure nothing comes before your pattern

**`re.findall()` - Find All Matches**

`re.findall()` returns a list of all non-overlapping matches in the string. Unlike `search()` which returns only the first match, `findall()` returns all matches. It returns strings (not Match objects), or tuples if the pattern has groups.

```python
import re

text = "The prices are $25.99, $10.50, and $99.99"

# findall() returns all matches as a list
prices = re.findall(r'\$\d+\.\d+', text)
print("All prices:", prices)  
# All prices: ['$25.99', '$10.50', '$99.99']

# With groups, returns tuples
text = "Emails: john@example.com, jane@test.org, bob@company.net"
pattern = r'(\w+)@(\w+\.\w+)'
matches = re.findall(pattern, text)
print("Matches:", matches)
# Matches: [('john', 'example.com'), ('jane', 'test.org'), ('bob', 'company.net')]

# Extract just usernames
usernames = [match[0] for match in matches]
print("Usernames:", usernames)  # ['john', 'jane', 'bob']

# Count occurrences
text = "The cat and the dog and the bird"
count = len(re.findall(r'the', text, re.IGNORECASE))
print(f"'the' appears {count} times")  # 3 times
```

**When to use `re.findall()`:**
- When you need all occurrences, not just the first
- When extracting multiple pieces of data from text
- When counting pattern occurrences
- When you don't need Match object features (position, etc.)

**Comparison Example:**

```python
import re

text = "Python 3.9, Python 3.10, Python 3.11"

# search() - finds first match
match = re.search(r'Python \d+\.\d+', text)
print("search():", match.group() if match else None)
# Output: Python 3.9

# match() - only matches at start
match = re.match(r'Python \d+\.\d+', text)
print("match():", match.group() if match else None)
# Output: Python 3.9 (works because pattern is at start)

# findall() - finds all matches
matches = re.findall(r'Python \d+\.\d+', text)
print("findall():", matches)
# Output: ['Python 3.9', 'Python 3.10', 'Python 3.11']

# Now with pattern not at start
text = "I love Python 3.9, Python 3.10, Python 3.11"

# search() - still finds first match
match = re.search(r'Python \d+\.\d+', text)
print("search():", match.group() if match else None)
# Output: Python 3.9

# match() - fails because pattern not at start
match = re.match(r'Python \d+\.\d+', text)
print("match():", match)
# Output: None

# findall() - still finds all matches
matches = re.findall(r'Python \d+\.\d+', text)
print("findall():", matches)
# Output: ['Python 3.9', 'Python 3.10', 'Python 3.11']
```

**Decision Tree:**

```
Need to find pattern in string?
├─ Only care if it exists? → Use search()
├─ Must be at start? → Use match()
├─ Need all occurrences? → Use findall()
└─ Need to replace? → Use sub()
```

**Key Differences Summary:**

| Function | Returns | Finds | Use Case |
|----------|---------|-------|----------|
| `search()` | Match object or None | First match anywhere | Check if pattern exists |
| `match()` | Match object or None | Match at start only | Validate format |
| `findall()` | List of strings/tuples | All matches | Extract all occurrences |

---

### Question 2: Write a regular expression to validate email addresses and explain each component of the pattern. What are the limitations of using regex for email validation, and when should you use alternative approaches?

**Detailed Answer:**

Email validation is one of the most common uses of regular expressions, but it's also one of the most misunderstood. Let's build an email validator step by step and understand its limitations.

**Basic Email Pattern:**

```python
import re

# Simple email pattern
pattern = r'\w+@\w+\.\w+'

# Test emails
emails = [
    "john@example.com",      # Valid
    "jane.doe@company.org",  # Valid
    "invalid.email",         # Invalid (no @)
    "@example.com",          # Invalid (no username)
    "user@",                 # Invalid (no domain)
]

for email in emails:
    if re.match(pattern, email):
        print(f"✓ {email} is valid")
    else:
        print(f"✗ {email} is invalid")
```

**Pattern Breakdown:**
- `\w+`: One or more word characters (letters, digits, underscore) for username
- `@`: Literal @ symbol
- `\w+`: One or more word characters for domain name
- `\.`: Literal dot (escaped because . is a special character)
- `\w+`: One or more word characters for top-level domain

**Problem:** This pattern is too simple. It doesn't handle:
- Dots in username (jane.doe@example.com)
- Hyphens in domain (user@my-company.com)
- Multiple dots in domain (user@mail.example.com)
- Plus signs in username (user+tag@example.com)

**Improved Email Pattern:**

```python
import re

# More comprehensive pattern
pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'

def validate_email(email):
    """Validate email address using regex"""
    return re.match(pattern, email) is not None

# Test cases
test_emails = [
    ("john@example.com", True),
    ("jane.doe@company.org", True),
    ("user+tag@mail.example.com", True),
    ("user@my-company.co.uk", True),
    ("invalid.email", False),
    ("@example.com", False),
    ("user@", False),
    ("user @example.com", False),  # Space not allowed
    ("user@.com", False),  # Domain can't start with dot
]

print("Email Validation Results:")
print("-" * 50)
for email, expected in test_emails:
    result = validate_email(email)
    status = "✓" if result == expected else "✗"
    print(f"{status} {email:30} → {result}")
```

**Detailed Pattern Breakdown:**

```python
pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
```

Let's break this down component by component:

1. **`^`** - Start of string anchor
   - Ensures pattern matches from the beginning
   - Prevents partial matches

2. **`[a-zA-Z0-9._%+-]+`** - Username part
   - `[...]`: Character class (match any character inside)
   - `a-zA-Z`: Any letter (uppercase or lowercase)
   - `0-9`: Any digit
   - `._%+-`: Literal characters allowed in email usernames
   - `+`: One or more of the preceding characters
   - Matches: "john", "jane.doe", "user+tag", "user_name"

3. **`@`** - Literal @ symbol
   - Must appear exactly once
   - Separates username from domain

4. **`[a-zA-Z0-9.-]+`** - Domain name part
   - Similar to username but allows hyphens
   - Matches: "example", "my-company", "mail.example"

5. **`\.`** - Literal dot
   - Escaped because `.` is a special regex character
   - Separates domain from TLD

6. **`[a-zA-Z]{2,}`** - Top-level domain (TLD)
   - `[a-zA-Z]`: Only letters (no numbers in TLD)
   - `{2,}`: At least 2 characters (for .uk, .com, etc.)
   - Matches: "com", "org", "co.uk", "info"

7. **`$`** - End of string anchor
   - Ensures pattern matches to the end
   - Prevents trailing characters

**Even Better Pattern (More Realistic):**

```python
import re

# RFC 5322 compliant (simplified)
pattern = r'^[a-zA-Z0-9][a-zA-Z0-9._%+-]*@[a-zA-Z0-9][a-zA-Z0-9.-]*\.[a-zA-Z]{2,}$'

def validate_email_advanced(email):
    """
    Advanced email validation with additional checks
    """
    # Basic pattern check
    if not re.match(pattern, email):
        return False, "Invalid email format"
    
    # Additional validation rules
    username, domain = email.split('@')
    
    # Username checks
    if username.startswith('.') or username.endswith('.'):
        return False, "Username cannot start or end with dot"
    
    if '..' in username:
        return False, "Username cannot have consecutive dots"
    
    # Domain checks
    if domain.startswith('-') or domain.endswith('-'):
        return False, "Domain cannot start or end with hyphen"
    
    if '..' in domain:
        return False, "Domain cannot have consecutive dots"
    
    # Length checks
    if len(email) > 254:  # RFC 5321
        return False, "Email too long (max 254 characters)"
    
    if len(username) > 64:  # RFC 5321
        return False, "Username too long (max 64 characters)"
    
    return True, "Valid email"

# Test with edge cases
test_cases = [
    "john@example.com",
    ".john@example.com",      # Invalid: starts with dot
    "john.@example.com",      # Invalid: ends with dot
    "john..doe@example.com",  # Invalid: consecutive dots
    "john@-example.com",      # Invalid: domain starts with hyphen
    "john@example-.com",      # Invalid: domain ends with hyphen
    "a" * 65 + "@example.com", # Invalid: username too long
]

print("\nAdvanced Email Validation:")
print("-" * 60)
for email in test_cases:
    valid, message = validate_email_advanced(email)
    status = "✓" if valid else "✗"
    print(f"{status} {email[:30]:30} → {message}")
```

**Limitations of Regex Email Validation:**

1. **RFC Compliance is Complex**
   - The official email specification (RFC 5322) is extremely complex
   - A fully compliant regex would be hundreds of characters long
   - Some valid emails according to RFC are rarely used in practice

2. **Cannot Verify Email Exists**
   - Regex only checks format, not if the email actually exists
   - "fake@nonexistent-domain-12345.com" passes regex but doesn't exist

3. **International Characters**
   - Modern emails can use Unicode characters (internationalized domain names)
   - Simple regex patterns don't handle these well

4. **Overly Restrictive or Permissive**
   - Too strict: Rejects valid emails
   - Too loose: Accepts invalid emails
   - Finding the right balance is difficult

**When to Use Alternative Approaches:**

```python
# 1. Use email validation libraries
from email_validator import validate_email, EmailNotValidError

def validate_with_library(email):
    try:
        # Validate and get normalized form
        valid = validate_email(email)
        return True, valid.email
    except EmailNotValidError as e:
        return False, str(e)

# 2. Send verification email (best practice)
def send_verification_email(email):
    """
    Best way to validate email: send a verification link
    Only confirms email when user clicks the link
    """
    # Generate verification token
    # Send email with verification link
    # Wait for user to click link
    pass

# 3. Simple validation for user experience
def simple_email_check(email):
    """
    Quick check for obvious errors
    Use before sending verification email
    """
    if '@' not in email:
        return False, "Email must contain @"
    
    if email.count('@') != 1:
        return False, "Email must contain exactly one @"
    
    username, domain = email.split('@')
    
    if not username or not domain:
        return False, "Email must have username and domain"
    
    if '.' not in domain:
        return False, "Domain must contain a dot"
    
    return True, "Format looks valid"
```

**Best Practice Approach:**

```python
import re

def validate_email_practical(email):
    """
    Practical email validation for real-world use
    
    Strategy:
    1. Basic regex for format check
    2. Additional logical checks
    3. Recommend sending verification email
    """
    # Step 1: Basic format check
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    
    if not re.match(pattern, email):
        return {
            'valid': False,
            'message': 'Invalid email format',
            'recommendation': 'Check for typos'
        }
    
    # Step 2: Length check
    if len(email) > 254:
        return {
            'valid': False,
            'message': 'Email too long',
            'recommendation': 'Maximum 254 characters'
        }
    
    # Step 3: Basic structure check
    username, domain = email.split('@')
    
    if len(username) > 64:
        return {
            'valid': False,
            'message': 'Username part too long',
            'recommendation': 'Maximum 64 characters before @'
        }
    
    # Step 4: Format looks valid
    return {
        'valid': True,
        'message': 'Format is valid',
        'recommendation': 'Send verification email to confirm'
    }

# Usage
email = "user@example.com"
result = validate_email_practical(email)
print(f"Valid: {result['valid']}")
print(f"Message: {result['message']}")
print(f"Recommendation: {result['recommendation']}")
```

**Key Takeaways:**

1. **Regex is good for format checking**, not for verifying email existence
2. **Keep patterns reasonable** - don't try to be 100% RFC compliant
3. **Always send verification emails** - it's the only way to confirm validity
4. **Use libraries** for production code - they handle edge cases better
5. **Regex is first line of defense** - catches obvious errors before sending verification

The best email validation strategy combines regex for quick format checking with email verification for actual confirmation. Regex alone is never sufficient for production systems.

---

## 🔍 Common Regular Expression Patterns

### Phone Numbers
```python
# US phone number: (123) 456-7890 or 123-456-7890
pattern = r'\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}'

# International: +1-234-567-8900
pattern = r'\+?\d{1,3}[-.\s]?\(?\d{1,4}\)?[-.\s]?\d{1,4}[-.\s]?\d{1,9}'
```

### URLs
```python
# Basic URL
pattern = r'https?://[^\s]+'

# More specific
pattern = r'https?://(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b'
```

### Dates
```python
# YYYY-MM-DD
pattern = r'\d{4}-\d{2}-\d{2}'

# MM/DD/YYYY or DD/MM/YYYY
pattern = r'\d{1,2}/\d{1,2}/\d{4}'
```

### Credit Cards
```python
# Visa, MasterCard, etc. (basic)
pattern = r'\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}'
```

---

## ✅ Key Takeaways

1. Regular expressions are patterns for matching text
2. Use `re.search()` for finding patterns anywhere
3. Use `re.match()` for validating format from start
4. Use `re.findall()` for extracting all matches
5. Metacharacters like `.`, `*`, `+`, `?` have special meanings
6. Character classes `[]` match any character inside
7. Groups `()` capture parts of matches
8. Regex is powerful but can be complex - start simple
9. Always test regex patterns with various inputs
10. For production, consider using specialized libraries

---

**Next Topic**: Networks and Sockets
**Previous Topic**: Module 2 - Python Data Structures
**Module**: 3 - Using Python to Access Web Data

Happy Learning! 🐍
