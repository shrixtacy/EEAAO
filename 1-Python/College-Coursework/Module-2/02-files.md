# Files

## 📚 Theoretical Understanding

### What is File I/O?

File Input/Output (I/O) is the process of reading data from files and writing data to files on your computer's storage system. Files are persistent storage - unlike variables in memory that disappear when your program ends, files remain on disk and can be accessed later. Understanding file I/O is essential because most real-world programs need to save data, load configuration, process documents, or generate reports.

Python provides built-in functions and methods for file operations, making file handling straightforward and Pythonic. The fundamental operations are opening files, reading or writing data, and closing files. Python's file handling is designed to be safe and efficient, with features like automatic resource management through context managers.

Files can contain different types of data: text files store human-readable characters (like .txt, .csv, .json), while binary files store raw bytes (like images, videos, executables). Python handles both types, but the approach differs. Text files are processed as strings with encoding (usually UTF-8), while binary files are processed as raw bytes.

### File Modes and Operations

When opening a file, you specify a mode that determines what operations are allowed. The main modes are read (`'r'`), write (`'w'`), and append (`'a'`). Read mode opens an existing file for reading - attempting to read a non-existent file raises an error. Write mode creates a new file or overwrites an existing file. Append mode adds data to the end of an existing file or creates a new file if it doesn't exist.

Additional mode modifiers include `'b'` for binary mode and `'+'` for read-write mode. For example, `'rb'` opens a file for reading in binary mode, while `'w+'` opens a file for both reading and writing. Understanding modes prevents common errors like accidentally overwriting important files or trying to write to read-only files.

### Context Managers and the `with` Statement

The `with` statement is the Pythonic way to handle files. It automatically closes the file when the block ends, even if an exception occurs. This prevents resource leaks and ensures data is properly flushed to disk. Without `with`, you must manually call `close()` and handle exceptions carefully.

Context managers implement the context management protocol (`__enter__` and `__exit__` methods), making them work with `with`. For files, this means the file is opened when entering the block and closed when exiting. This pattern is so useful that it's considered best practice for all file operations in Python.

### File Paths and the OS Module

File paths specify the location of files on your system. Paths can be absolute (full path from root directory) or relative (path from current directory). Different operating systems use different path separators: Windows uses backslashes (`\`), while Unix/Linux/Mac use forward slashes (`/`).

Python's `os` and `pathlib` modules provide cross-platform path handling. They abstract away OS differences, letting you write portable code. The `os.path` module offers functions for joining paths, checking existence, and getting file information. The newer `pathlib` module provides an object-oriented interface that's more intuitive and Pythonic.

### Text Encoding

Text files are stored as bytes, but we read them as characters. Encoding is the mapping between bytes and characters. UTF-8 is the most common encoding, supporting all Unicode characters. Other encodings include ASCII (English only), Latin-1 (Western European), and various language-specific encodings.

When opening text files, Python uses the system's default encoding (usually UTF-8). You can specify encoding explicitly with the `encoding` parameter. Encoding errors occur when bytes don't match the expected encoding - Python can handle these with error handlers like `'ignore'`, `'replace'`, or `'strict'` (default, raises exception).

---

## 💻 Practical Examples

### Basic File Reading

```python
# Method 1: Read entire file as string
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)

# Method 2: Read file line by line
with open('example.txt', 'r') as file:
    for line in file:
        print(line.strip())  # strip() removes newline

# Method 3: Read all lines into a list
with open('example.txt', 'r') as file:
    lines = file.readlines()
    print(lines)

# Method 4: Read specific number of characters
with open('example.txt', 'r') as file:
    chunk = file.read(100)  # Read first 100 characters
    print(chunk)
```

### Basic File Writing

```python
# Write to file (overwrites existing content)
with open('output.txt', 'w') as file:
    file.write("Hello, World!\n")
    file.write("This is a new line.\n")

# Write multiple lines at once
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open('output.txt', 'w') as file:
    file.writelines(lines)

# Append to file (adds to end)
with open('output.txt', 'a') as file:
    file.write("This line is appended.\n")

# Write with print function
with open('output.txt', 'w') as file:
    print("Hello, World!", file=file)
    print("Another line", file=file)
```

### Working with CSV Files

```python
import csv

# Reading CSV
with open('data.csv', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)  # Each row is a list

# Reading CSV with headers
with open('data.csv', 'r') as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row)  # Each row is a dictionary

# Writing CSV
data = [
    ['Name', 'Age', 'City'],
    ['Alice', '25', 'New York'],
    ['Bob', '30', 'London']
]

with open('output.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerows(data)

# Writing CSV with dictionaries
data = [
    {'Name': 'Alice', 'Age': 25, 'City': 'New York'},
    {'Name': 'Bob', 'Age': 30, 'City': 'London'}
]

with open('output.csv', 'w', newline='') as file:
    fieldnames = ['Name', 'Age', 'City']
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(data)
```

### Working with JSON Files

```python
import json

# Reading JSON
with open('data.json', 'r') as file:
    data = json.load(file)
    print(data)

# Writing JSON
data = {
    'name': 'Alice',
    'age': 25,
    'city': 'New York',
    'hobbies': ['reading', 'coding', 'gaming']
}

with open('output.json', 'w') as file:
    json.dump(data, file, indent=4)

# Pretty printing JSON
print(json.dumps(data, indent=2))
```

### File Path Operations

```python
import os
from pathlib import Path

# Using os.path
file_path = os.path.join('folder', 'subfolder', 'file.txt')
print(file_path)

# Check if file exists
if os.path.exists('example.txt'):
    print("File exists")

# Get file size
size = os.path.getsize('example.txt')
print(f"File size: {size} bytes")

# Get absolute path
abs_path = os.path.abspath('example.txt')
print(abs_path)

# Using pathlib (modern approach)
path = Path('folder') / 'subfolder' / 'file.txt'
print(path)

# Check existence
if path.exists():
    print("File exists")

# Read file using pathlib
content = path.read_text()

# Write file using pathlib
path.write_text("Hello, World!")
```

### Binary File Operations

```python
# Reading binary file
with open('image.jpg', 'rb') as file:
    data = file.read()
    print(f"File size: {len(data)} bytes")

# Writing binary file
with open('copy.jpg', 'wb') as file:
    file.write(data)

# Copying files
def copy_file(source, destination):
    with open(source, 'rb') as src:
        with open(destination, 'wb') as dst:
            dst.write(src.read())

copy_file('original.jpg', 'backup.jpg')
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain the importance of using context managers (`with` statement) for file operations. What problems does it solve, and what happens if you don't use it? Provide examples showing proper and improper file handling.

**Detailed Answer:**

The `with` statement and context managers are fundamental to proper file handling in Python. Understanding why they're important prevents resource leaks, data corruption, and hard-to-debug errors.

**What is a Context Manager?**

A context manager is an object that defines methods for entering and exiting a runtime context. For files, this means:
- `__enter__`: Opens the file and returns the file object
- `__exit__`: Closes the file, even if an exception occurs

```python
# How 'with' works internally
# This:
with open('file.txt', 'r') as file:
    content = file.read()

# Is equivalent to:
file = open('file.txt', 'r')
try:
    content = file.read()
finally:
    file.close()
```

**Problems Without Context Managers:**

**1. Resource Leaks**

```python
# BAD: File not closed properly
def read_file_bad(filename):
    file = open(filename, 'r')
    content = file.read()
    # Forgot to close!
    return content

# If this function is called many times:
for i in range(1000):
    data = read_file_bad('data.txt')
# Eventually runs out of file handles!

# GOOD: Using context manager
def read_file_good(filename):
    with open(filename, 'r') as file:
        content = file.read()
    # File automatically closed here
    return content
```

**2. Data Not Flushed to Disk**

```python
# BAD: Data might not be written
def write_data_bad(filename, data):
    file = open(filename, 'w')
    file.write(data)
    # If program crashes here, data might be lost!
    # File buffer not flushed to disk

# GOOD: Context manager ensures flush
def write_data_good(filename, data):
    with open(filename, 'w') as file:
        file.write(data)
    # Data guaranteed to be written to disk
```

**3. Exception Handling Issues**

```python
# BAD: File not closed if exception occurs
def process_file_bad(filename):
    file = open(filename, 'r')
    data = file.read()
    result = process_data(data)  # Might raise exception
    file.close()  # Never reached if exception occurs!
    return result

# GOOD: File closed even with exception
def process_file_good(filename):
    with open(filename, 'r') as file:
        data = file.read()
        result = process_data(data)  # Exception is fine
    # File closed automatically
    return result
```

**Real-World Example: Log File Writing**

```python
# BAD: Potential issues
class Logger:
    def __init__(self, filename):
        self.file = open(filename, 'a')
    
    def log(self, message):
        self.file.write(f"{message}\n")
    
    def close(self):
        self.file.close()

# Problems:
# 1. Must remember to call close()
# 2. If exception occurs, file never closed
# 3. If program crashes, logs might be lost

logger = Logger('app.log')
logger.log("Application started")
# ... program runs ...
logger.close()  # Easy to forget!

# GOOD: Using context manager
class Logger:
    def __init__(self, filename):
        self.filename = filename
    
    def __enter__(self):
        self.file = open(self.filename, 'a')
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
        return False
    
    def log(self, message):
        self.file.write(f"{message}\n")

# Usage:
with Logger('app.log') as logger:
    logger.log("Application started")
    # ... program runs ...
# File automatically closed, even if exception occurs
```

**Multiple File Operations**

```python
# BAD: Nested try-finally blocks
def copy_file_bad(source, dest):
    src = open(source, 'rb')
    try:
        dst = open(dest, 'wb')
        try:
            dst.write(src.read())
        finally:
            dst.close()
    finally:
        src.close()

# GOOD: Multiple context managers
def copy_file_good(source, dest):
    with open(source, 'rb') as src:
        with open(dest, 'wb') as dst:
            dst.write(src.read())

# EVEN BETTER: Single with statement (Python 3.1+)
def copy_file_better(source, dest):
    with open(source, 'rb') as src, open(dest, 'wb') as dst:
        dst.write(src.read())
```

**Performance Implications**

```python
import time

# BAD: Opening/closing in loop without context manager
def process_lines_bad(filename):
    for i in range(100):
        file = open(filename, 'r')
        lines = file.readlines()
        # Process lines...
        file.close()  # Might forget!

# GOOD: Context manager ensures proper cleanup
def process_lines_good(filename):
    for i in range(100):
        with open(filename, 'r') as file:
            lines = file.readlines()
            # Process lines...
        # File properly closed each iteration

# BEST: Open file once
def process_lines_best(filename):
    with open(filename, 'r') as file:
        for i in range(100):
            file.seek(0)  # Reset to beginning
            lines = file.readlines()
            # Process lines...
```

**Custom Context Managers**

```python
from contextlib import contextmanager

# Creating custom context manager for timing
@contextmanager
def timer(name):
    start = time.time()
    print(f"{name} started")
    try:
        yield
    finally:
        end = time.time()
        print(f"{name} took {end - start:.2f} seconds")

# Usage:
with timer("File processing"):
    with open('large_file.txt', 'r') as file:
        data = file.read()
        # Process data...

# Creating context manager for temporary files
@contextmanager
def temporary_file(filename):
    # Setup: create file
    with open(filename, 'w') as f:
        f.write("Temporary data")
    
    try:
        yield filename
    finally:
        # Cleanup: delete file
        import os
        if os.path.exists(filename):
            os.remove(filename)

# Usage:
with temporary_file('temp.txt') as temp:
    # Use temporary file
    with open(temp, 'r') as f:
        print(f.read())
# File automatically deleted after block
```

**Best Practices:**

```python
# 1. Always use 'with' for file operations
with open('file.txt', 'r') as file:
    data = file.read()

# 2. Multiple files in one statement
with open('input.txt', 'r') as infile, open('output.txt', 'w') as outfile:
    outfile.write(infile.read())

# 3. Handle exceptions explicitly if needed
try:
    with open('file.txt', 'r') as file:
        data = file.read()
except FileNotFoundError:
    print("File not found")
except PermissionError:
    print("Permission denied")

# 4. Use pathlib for modern code
from pathlib import Path
path = Path('file.txt')
if path.exists():
    content = path.read_text()  # Uses context manager internally
```

**Conclusion:**

Context managers (`with` statement) are essential for proper file handling. They guarantee files are closed, data is flushed, and resources are released - even when exceptions occur. Always use `with` for file operations. It's not just best practice; it's the Pythonic way and prevents subtle bugs that are hard to debug.

---

### Question 2: Explain the difference between text mode and binary mode for file operations. When should you use each mode? Provide examples showing how encoding affects text files and why binary mode is necessary for certain file types.

**Detailed Answer:**

Understanding the difference between text and binary modes is crucial for correct file handling. Using the wrong mode can corrupt data, cause encoding errors, or produce unexpected results.

**Text Mode vs Binary Mode:**

**Text Mode (`'r'`, `'w'`, `'a'`)**
- Reads/writes strings (str objects)
- Applies encoding/decoding (default: UTF-8)
- Handles platform-specific line endings
- Suitable for human-readable text files

**Binary Mode (`'rb'`, `'wb'`, `'ab'`)**
- Reads/writes bytes (bytes objects)
- No encoding/decoding applied
- Preserves exact byte content
- Suitable for non-text files (images, videos, executables)

```python
# Text mode - returns strings
with open('text.txt', 'r') as file:
    content = file.read()
    print(type(content))  # <class 'str'>
    print(repr(content))  # 'Hello\nWorld'

# Binary mode - returns bytes
with open('text.txt', 'rb') as file:
    content = file.read()
    print(type(content))  # <class 'bytes'>
    print(repr(content))  # b'Hello\nWorld' or b'Hello\r\nWorld' on Windows
```

**Encoding in Text Mode:**

```python
# Writing with different encodings
text = "Hello, 世界"  # Contains Chinese characters

# UTF-8 encoding (default, supports all Unicode)
with open('utf8.txt', 'w', encoding='utf-8') as file:
    file.write(text)

# ASCII encoding (will fail with non-ASCII characters)
try:
    with open('ascii.txt', 'w', encoding='ascii') as file:
        file.write(text)  # UnicodeEncodeError!
except UnicodeEncodeError as e:
    print(f"Error: {e}")

# ASCII with error handling
with open('ascii.txt', 'w', encoding='ascii', errors='ignore') as file:
    file.write(text)  # Writes "Hello, " (ignores Chinese characters)

# Reading with wrong encoding
with open('utf8.txt', 'w', encoding='utf-8') as file:
    file.write("Café")

# Correct encoding
with open('utf8.txt', 'r', encoding='utf-8') as file:
    print(file.read())  # "Café"

# Wrong encoding
with open('utf8.txt', 'r', encoding='ascii', errors='ignore') as file:
    print(file.read())  # "Caf" (é is lost)
```

**Line Ending Handling:**

```python
# Text mode handles platform differences
# Windows: \r\n (CRLF)
# Unix/Mac: \n (LF)

# Writing in text mode
with open('lines.txt', 'w') as file:
    file.write("Line 1\n")
    file.write("Line 2\n")
# On Windows, Python converts \n to \r\n automatically

# Reading in text mode
with open('lines.txt', 'r') as file:
    content = file.read()
    print(repr(content))  # 'Line 1\nLine 2\n' (normalized to \n)

# Binary mode preserves exact bytes
with open('lines.txt', 'rb') as file:
    content = file.read()
    print(repr(content))  # b'Line 1\r\nLine 2\r\n' on Windows
```

**When to Use Text Mode:**

**1. Plain Text Files**

```python
# Reading configuration files
with open('config.txt', 'r') as file:
    for line in file:
        key, value = line.strip().split('=')
        print(f"{key}: {value}")

# Writing log files
with open('app.log', 'a') as file:
    file.write(f"{datetime.now()}: Application started\n")
```

**2. CSV Files**

```python
import csv

# CSV files are text files
with open('data.csv', 'r') as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)
```

**3. JSON Files**

```python
import json

# JSON files are text files
with open('data.json', 'r') as file:
    data = json.load(file)
```

**4. Source Code Files**

```python
# Reading Python source code
with open('script.py', 'r') as file:
    code = file.read()
    print(code)
```

**When to Use Binary Mode:**

**1. Image Files**

```python
# Copying an image
with open('original.jpg', 'rb') as src:
    with open('copy.jpg', 'wb') as dst:
        dst.write(src.read())

# Reading image for processing
from PIL import Image
import io

with open('image.jpg', 'rb') as file:
    image_data = file.read()
    image = Image.open(io.BytesIO(image_data))
```

**2. Binary Data Files**

```python
import struct

# Writing binary data
data = struct.pack('i', 42)  # Pack integer as bytes
with open('data.bin', 'wb') as file:
    file.write(data)

# Reading binary data
with open('data.bin', 'rb') as file:
    data = file.read()
    number = struct.unpack('i', data)[0]
    print(number)  # 42
```

**3. Executable Files**

```python
# Copying executable
with open('program.exe', 'rb') as src:
    with open('backup.exe', 'wb') as dst:
        dst.write(src.read())
```

**4. Network Protocols**

```python
# Reading binary protocol data
with open('packet.dat', 'rb') as file:
    header = file.read(4)  # Read 4-byte header
    payload = file.read()  # Read rest
```

**Problems with Wrong Mode:**

```python
# WRONG: Using text mode for binary data
# This corrupts the image!
with open('image.jpg', 'r') as src:  # Should be 'rb'
    with open('corrupted.jpg', 'w') as dst:  # Should be 'wb'
        dst.write(src.read())
# Result: Corrupted image file

# WRONG: Using binary mode for text with encoding
with open('utf8.txt', 'wb') as file:
    file.write("Hello")  # TypeError: a bytes-like object is required
    # Must encode first:
    file.write("Hello".encode('utf-8'))  # Correct
```

**Practical Example: File Type Detection**

```python
def detect_file_type(filename):
    """Detect if file is text or binary"""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            file.read()
        return "text"
    except UnicodeDecodeError:
        return "binary"

print(detect_file_type('document.txt'))  # "text"
print(detect_file_type('image.jpg'))     # "binary"
```

**Best Practices:**

```python
# 1. Use text mode for human-readable files
with open('readme.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# 2. Use binary mode for non-text files
with open('data.bin', 'rb') as file:
    data = file.read()

# 3. Always specify encoding for text files
with open('file.txt', 'r', encoding='utf-8') as file:
    content = file.read()

# 4. Use binary mode when exact bytes matter
with open('file.dat', 'rb') as file:
    checksum = hashlib.md5(file.read()).hexdigest()

# 5. Convert between text and bytes explicitly
text = "Hello"
bytes_data = text.encode('utf-8')  # str -> bytes
text_again = bytes_data.decode('utf-8')  # bytes -> str
```

**Conclusion:**

Use text mode for human-readable text files (txt, csv, json, source code) and binary mode for everything else (images, videos, executables, binary data). Text mode handles encoding and line endings automatically, while binary mode preserves exact byte content. Always specify encoding explicitly for text files to avoid platform-dependent issues. Using the wrong mode can corrupt data or cause encoding errors.

---

## ✅ Key Takeaways

1. Always use `with` statement for file operations (context manager)
2. Text mode for human-readable files, binary mode for everything else
3. Specify encoding explicitly for text files (UTF-8 recommended)
4. Files are automatically closed when `with` block ends
5. Use `pathlib` for modern, cross-platform path handling
6. CSV and JSON modules handle their respective formats
7. Binary mode preserves exact byte content
8. Text mode handles platform-specific line endings
9. Context managers prevent resource leaks and data loss
10. Check file existence before operations to avoid errors

---

**Next Topic**: Lists
**Previous Topic**: Strings
**Module**: 2 - Python Data Structures

Happy Learning! 🐍
