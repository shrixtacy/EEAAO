# Python File Detection and Operations - Complete Guide

## 📚 Table of Contents
1. [os Module](#os-module)
2. [pathlib Module](#pathlib-module)
3. [Checking File Existence](#checking-file-existence)
4. [File and Directory Operations](#file-and-directory-operations)
5. [Practice Questions](#practice-questions)

---

## 1. os Module

```python
import os

# Current directory
print(os.getcwd())

# List files
print(os.listdir('.'))

# Check if file exists
print(os.path.exists('file.txt'))

# Check if path is file
print(os.path.isfile('file.txt'))

# Check if path is directory
print(os.path.isdir('folder'))

# Get file size
print(os.path.getsize('file.txt'))

# Join paths
path = os.path.join('folder', 'subfolder', 'file.txt')
print(path)

# Split path
print(os.path.split('/path/to/file.txt'))

# Get filename
print(os.path.basename('/path/to/file.txt'))

# Get directory
print(os.path.dirname('/path/to/file.txt'))
```

---

## 2. pathlib Module

```python
from pathlib import Path

# Current directory
print(Path.cwd())

# Home directory
print(Path.home())

# Create Path object
path = Path('folder/file.txt')

# Check if exists
print(path.exists())

# Check if file
print(path.is_file())

# Check if directory
print(path.is_dir())

# Get file size
print(path.stat().st_size)

# Join paths
path = Path('folder') / 'subfolder' / 'file.txt'

# Get parts
print(path.parts)
print(path.name)      # filename
print(path.stem)      # filename without extension
print(path.suffix)    # extension
print(path.parent)    # parent directory
```

---

## 3. Checking File Existence

```python
import os
from pathlib import Path

# Using os
if os.path.exists('file.txt'):
    print("File exists")

# Using pathlib
path = Path('file.txt')
if path.exists():
    print("File exists")

# Check multiple files
files = ['file1.txt', 'file2.txt', 'file3.txt']
for file in files:
    if Path(file).exists():
        print(f"{file} exists")
    else:
        print(f"{file} not found")
```

---

## 4. File and Directory Operations

```python
import os
from pathlib import Path

# Create directory
os.mkdir('new_folder')
Path('new_folder').mkdir(exist_ok=True)

# Create nested directories
os.makedirs('folder/subfolder/subsubfolder')
Path('folder/subfolder').mkdir(parents=True, exist_ok=True)

# Remove file
os.remove('file.txt')
Path('file.txt').unlink()

# Remove directory
os.rmdir('folder')
Path('folder').rmdir()

# Rename file
os.rename('old.txt', 'new.txt')
Path('old.txt').rename('new.txt')

# List all files in directory
for file in os.listdir('.'):
    print(file)

# List with pathlib
for file in Path('.').iterdir():
    print(file)

# Find all .txt files
for file in Path('.').glob('*.txt'):
    print(file)

# Find recursively
for file in Path('.').rglob('*.txt'):
    print(file)
```

---

## 5. Practice Questions

**Question 1**: Write a function to check if a file exists and print its size.
**Question 2**: Create a function to list all files in a directory with a specific extension.
**Question 3**: Write a program to organize files by extension into folders.

---

## 🎯 Key Takeaways

1. Use `os` module for file operations
2. Use `pathlib` for modern path handling
3. Check file existence before operations
4. Use `glob()` for pattern matching
5. Always handle exceptions for file operations

---

**Next Topic**: Writing Files
**Previous Topic**: Decorators

Happy Coding! 🐍
