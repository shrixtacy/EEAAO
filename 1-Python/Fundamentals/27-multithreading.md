# Python Multithreading - Complete Guide

## 📚 Table of Contents
1. [Introduction to Threading](#introduction-to-threading)
2. [Creating Threads](#creating-threads)
3. [Thread Synchronization](#thread-synchronization)
4. [Locks and Semaphores](#locks-and-semaphores)
5. [Practice Questions](#practice-questions)

---

## 1. Introduction to Threading

Threading allows multiple operations to run concurrently.

```python
import threading
import time

def task():
    print(f"Thread {threading.current_thread().name} starting")
    time.sleep(2)
    print(f"Thread {threading.current_thread().name} finishing")

# Create and start thread
thread = threading.Thread(target=task)
thread.start()
thread.join()  # Wait for thread to complete
```

---

## 2. Creating Threads

```python
import threading

def print_numbers():
    for i in range(5):
        print(f"Number: {i}")

def print_letters():
    for letter in 'ABCDE':
        print(f"Letter: {letter}")

# Create threads
t1 = threading.Thread(target=print_numbers)
t2 = threading.Thread(target=print_letters)

# Start threads
t1.start()
t2.start()

# Wait for completion
t1.join()
t2.join()

print("Done!")
```

---

## 3. Thread Synchronization

```python
import threading

counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(100000):
        with lock:
            counter += 1

threads = []
for _ in range(10):
    t = threading.Thread(target=increment)
    t.start()
    threads.append(t)

for t in threads:
    t.join()

print(f"Counter: {counter}")  # 1000000
```

---

## 4. Locks and Semaphores

```python
import threading
import time

# Lock example
lock = threading.Lock()

def critical_section(thread_id):
    lock.acquire()
    try:
        print(f"Thread {thread_id} in critical section")
        time.sleep(1)
    finally:
        lock.release()

# Semaphore example
semaphore = threading.Semaphore(3)  # Max 3 threads

def limited_access(thread_id):
    semaphore.acquire()
    try:
        print(f"Thread {thread_id} accessing resource")
        time.sleep(2)
    finally:
        semaphore.release()
```

---

## 5. Practice Questions

**Question 1**: Create a program that downloads multiple files concurrently.
**Question 2**: Build a thread pool for processing tasks.
**Question 3**: Implement a producer-consumer pattern with threads.

---

## 🎯 Key Takeaways

1. Use `threading.Thread()` to create threads
2. Call `start()` to begin execution
3. Call `join()` to wait for completion
4. Use locks for thread synchronization
5. Semaphores limit concurrent access

---

**Next Topic**: Request API Data
**Previous Topic**: Random Numbers

Happy Coding! 🐍
