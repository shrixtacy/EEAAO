# Python Writing Files - Complete Guide

## 📚 Table of Contents
1. [Opening Files for Writing](#opening-files-for-writing)
2. [write() and writelines()](#write-and-writelines)
3. [Context Managers](#context-managers)
4. [File Modes](#file-modes)
5. [Practice Questions](#practice-questions)

---

## 1. Opening Files for Writing

```python
# Basic file writing
file = open('output.txt', 'w')
file.write('Hello, World!')
file.close()
```

---

## 2. write() and writelines()

```python
# write() - writes a string
with open('output.txt', 'w') as file:
    file.write('Line 1\n')
    file.write('Line 2\n')

# writelines() - writes a list of strings
lines = ['Line 1\n', 'Line 2\n', 'Line 3\n']
with open('output.txt', 'w') as file:
    file.writelines(lines)
```

---

## 3. Context Managers

```python
# Using 'with' statement (recommended)
with open('output.txt', 'w') as file:
    file.write('Hello, World!')
# File automatically closed
```

---

## 4. File Modes

```python
# 'w' - Write (overwrites)
with open('file.txt', 'w') as file:
    file.write('New content')

# 'a' - Append
with open('file.txt', 'a') as file:
    file.write('Appended content')

# 'x' - Exclusive creation (fails if exists)
with open('newfile.txt', 'x') as file:
    file.write('Content')
```

---

## 5. Practice Questions

**Question 1**: Write a program to create a file and write 10 lines to it.
**Question 2**: Create a function to append text to an existing file.
**Question 3**: Write a program to save a list of dictionaries to a file.

---

## 🎯 Key Takeaways

1. Use `with` statement for automatic file closing
2. `'w'` mode overwrites, `'a'` mode appends
3. Always close files or use context managers
4. Use `write()` for strings, `writelines()` for lists

---

**Next Topic**: Reading Files
**Previous Topic**: File Detection

Happy Coding! 🐍
