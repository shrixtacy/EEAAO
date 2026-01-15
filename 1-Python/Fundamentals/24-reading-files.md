# Python Reading Files - Complete Guide

## 📚 Table of Contents
1. [Opening Files for Reading](#opening-files-for-reading)
2. [read(), readline(), readlines()](#read-readline-readlines)
3. [Iterating Over Files](#iterating-over-files)
4. [File Modes](#file-modes)
5. [Practice Questions](#practice-questions)

---

## 1. Opening Files for Reading

```python
# Basic file reading
file = open('input.txt', 'r')
content = file.read()
print(content)
file.close()
```

---

## 2. read(), readline(), readlines()

```python
# read() - reads entire file
with open('file.txt', 'r') as file:
    content = file.read()
    print(content)

# readline() - reads one line
with open('file.txt', 'r') as file:
    line = file.readline()
    print(line)

# readlines() - reads all lines into list
with open('file.txt', 'r') as file:
    lines = file.readlines()
    for line in lines:
        print(line.strip())
```

---

## 3. Iterating Over Files

```python
# Best way to read large files
with open('file.txt', 'r') as file:
    for line in file:
        print(line.strip())
```

---

## 4. File Modes

```python
# 'r' - Read (default)
with open('file.txt', 'r') as file:
    content = file.read()

# 'rb' - Read binary
with open('image.png', 'rb') as file:
    data = file.read()
```

---

## 5. Practice Questions

**Question 1**: Read a file and count the number of lines.
**Question 2**: Read a file and find all lines containing a specific word.
**Question 3**: Read a CSV file and process the data.

---

## 🎯 Key Takeaways

1. Use `with` statement for safe file handling
2. `read()` reads entire file
3. `readline()` reads one line at a time
4. `readlines()` reads all lines into a list
5. Iterate over file object for memory efficiency

---

**Next Topic**: Exception Handling
**Previous Topic**: Writing Files

Happy Coding! 🐍
