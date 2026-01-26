# Data Structures Fundamentals

## Overview

Data structures are ways to organize and store data so you can access and modify it efficiently. Think of them as different types of containers, each optimized for specific operations.

**Key Principle**: Choose the right data structure based on what operations you need to perform most frequently.

---

## Arrays: The Foundation

### What Are Arrays?
Arrays store elements in contiguous memory locations, accessed by index.

```python
# Python List (Dynamic Array)
numbers = [1, 2, 3, 4, 5]
print(numbers[0])  # Access: O(1)
numbers.append(6)  # Add to end: O(1) amortized
```

```javascript
// JavaScript Array
const numbers = [1, 2, 3, 4, 5];
console.log(numbers[0]); // Access: O(1)
numbers.push(6);         // Add to end: O(1) amortized
```

```java
// Java Array (Fixed Size)
int[] numbers = {1, 2, 3, 4, 5};
System.out.println(numbers[0]); // Access: O(1)

// Java ArrayList (Dynamic)
ArrayList<Integer> list = new ArrayList<>();
list.add(1); // Add: O(1) amortized
```

### Time Complexity
- **Access**: O(1) - Direct index access
- **Search**: O(n) - Must check each element
- **Insertion**: O(n) - May need to shift elements
- **Deletion**: O(n) - May need to shift elements

### When to Use Arrays
✅ **Use When:**
- You need fast random access to elements
- You know the approximate size of your data
- You're doing mathematical operations (matrices, vectors)
- Memory usage is a concern

❌ **Don't Use When:**
- You frequently insert/delete from the middle
- The size varies dramatically
- You need to maintain sorted order with frequent updates

### Real-World Applications
- **Image Processing**: Pixel data in 2D arrays
- **Game Development**: Game boards, sprite animations
- **Data Analysis**: Numerical computations, statistics
- **Buffers**: Audio/video streaming, network packets

---

## Linked Lists: Dynamic Flexibility

### What Are Linked Lists?
Each element (node) contains data and a reference to the next node.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# Creating a linked list: 1 -> 2 -> 3
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)

# Traversal: O(n)
current = head
while current:
    print(current.val)
    current = current.next
```

```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

// Creating a linked list: 1 -> 2 -> 3
const head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
```

### Time Complexity
- **Access**: O(n) - Must traverse from head
- **Search**: O(n) - Must check each node
- **Insertion**: O(1) - If you have the node reference
- **Deletion**: O(1) - If you have the node reference

### When to Use Linked Lists
✅ **Use When:**
- You frequently insert/delete at the beginning
- The size varies significantly
- You don't need random access
- You're implementing other data structures (stacks, queues)

❌ **Don't Use When:**
- You need fast random access
- Memory usage is critical (extra pointer overhead)
- You're doing cache-intensive operations

### Real-World Applications
- **Undo Functionality**: Text editors, image editors
- **Music Playlists**: Next/previous song navigation
- **Browser History**: Forward/back navigation
- **Memory Management**: Free block lists in allocators

---

## Stacks: Last In, First Out (LIFO)

### What Are Stacks?
Elements are added and removed from the same end (top).

```python
# Using Python list as stack
stack = []
stack.append(1)    # Push: O(1)
stack.append(2)    # Push: O(1)
stack.append(3)    # Push: O(1)

top = stack.pop()  # Pop: O(1), returns 3
print(stack)       # [1, 2]

# Check top without removing
if stack:
    top = stack[-1]  # Peek: O(1)
```

```javascript
// Using JavaScript array as stack
const stack = [];
stack.push(1);     // Push: O(1)
stack.push(2);     // Push: O(1)
stack.push(3);     // Push: O(1)

const top = stack.pop(); // Pop: O(1), returns 3
console.log(stack);      // [1, 2]

// Check top without removing
if (stack.length > 0) {
    const top = stack[stack.length - 1]; // Peek: O(1)
}
```

### Time Complexity
- **Push**: O(1) - Add to top
- **Pop**: O(1) - Remove from top
- **Peek**: O(1) - Look at top element
- **Search**: O(n) - Must check each element

### When to Use Stacks
✅ **Use When:**
- You need to reverse the order of operations
- You're parsing nested structures
- You need to track function calls or states
- You're implementing backtracking algorithms

### Real-World Applications
- **Function Calls**: Call stack in programming languages
- **Browser History**: Back button functionality
- **Undo Operations**: Ctrl+Z in applications
- **Expression Evaluation**: Mathematical expressions, syntax parsing
- **Backtracking**: Maze solving, puzzle games

### Stack Implementation Example
```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        raise IndexError("Stack is empty")
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        raise IndexError("Stack is empty")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)

# Usage
stack = Stack()
stack.push("first")
stack.push("second")
print(stack.pop())  # "second"
print(stack.peek()) # "first"
```

---

## Queues: First In, First Out (FIFO)

### What Are Queues?
Elements are added at the rear and removed from the front.

```python
from collections import deque

# Using deque for efficient queue operations
queue = deque()
queue.append(1)      # Enqueue: O(1)
queue.append(2)      # Enqueue: O(1)
queue.append(3)      # Enqueue: O(1)

first = queue.popleft()  # Dequeue: O(1), returns 1
print(queue)             # deque([2, 3])

# Check front without removing
if queue:
    front = queue[0]     # Peek: O(1)
```

```javascript
// JavaScript doesn't have efficient queue, but here's a simple version
class Queue {
    constructor() {
        this.items = [];
        this.front = 0;
    }
    
    enqueue(item) {
        this.items.push(item); // O(1)
    }
    
    dequeue() {
        if (this.isEmpty()) {
            throw new Error("Queue is empty");
        }
        const item = this.items[this.front];
        this.front++;
        return item; // O(1)
    }
    
    peek() {
        if (this.isEmpty()) {
            throw new Error("Queue is empty");
        }
        return this.items[this.front]; // O(1)
    }
    
    isEmpty() {
        return this.front >= this.items.length;
    }
}
```

### Time Complexity
- **Enqueue**: O(1) - Add to rear
- **Dequeue**: O(1) - Remove from front
- **Peek**: O(1) - Look at front element
- **Search**: O(n) - Must check each element

### When to Use Queues
✅ **Use When:**
- You need to process items in order
- You're implementing breadth-first search
- You're handling requests or tasks
- You need buffering between producers and consumers

### Real-World Applications
- **Task Scheduling**: Operating system process queues
- **Print Queues**: Documents waiting to be printed
- **Breadth-First Search**: Graph traversal, shortest path
- **Buffer Management**: Streaming data, network packets
- **Customer Service**: Call centers, support tickets

---

## Hash Tables: Fast Key-Value Access

### What Are Hash Tables?
Use a hash function to map keys to array indices for fast access.

```python
# Python dictionary (hash table)
hash_table = {}
hash_table["name"] = "Alice"      # Insert: O(1) average
hash_table["age"] = 30            # Insert: O(1) average
hash_table["city"] = "New York"   # Insert: O(1) average

print(hash_table["name"])         # Access: O(1) average
del hash_table["age"]             # Delete: O(1) average

# Check if key exists
if "name" in hash_table:          # Search: O(1) average
    print("Name found")
```

```javascript
// JavaScript object (hash table)
const hashTable = {};
hashTable["name"] = "Alice";      // Insert: O(1) average
hashTable["age"] = 30;            // Insert: O(1) average
hashTable["city"] = "New York";   // Insert: O(1) average

console.log(hashTable["name"]);   // Access: O(1) average
delete hashTable["age"];          // Delete: O(1) average

// Check if key exists
if ("name" in hashTable) {        // Search: O(1) average
    console.log("Name found");
}

// Using Map for better hash table behavior
const map = new Map();
map.set("name", "Alice");
map.set("age", 30);
console.log(map.get("name"));     // "Alice"
console.log(map.has("age"));      // true
```

### Time Complexity
- **Insert**: O(1) average, O(n) worst case
- **Delete**: O(1) average, O(n) worst case
- **Search**: O(1) average, O(n) worst case
- **Access**: O(1) average, O(n) worst case

### When to Use Hash Tables
✅ **Use When:**
- You need fast key-based lookups
- You're counting occurrences of items
- You need to check membership quickly
- You're implementing caches or memoization

❌ **Don't Use When:**
- You need to maintain order
- You need to find min/max efficiently
- Memory usage is extremely critical
- You need range queries

### Real-World Applications
- **Caching**: Web browsers, database query results
- **Database Indexing**: Fast record lookups
- **Symbol Tables**: Compilers, interpreters
- **Counting**: Word frequency, analytics
- **Authentication**: Session management, API keys

### Hash Table Implementation Example
```python
class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]  # Chaining for collision resolution
    
    def _hash(self, key):
        return hash(key) % self.size
    
    def insert(self, key, value):
        index = self._hash(key)
        bucket = self.table[index]
        
        # Update existing key
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        
        # Add new key-value pair
        bucket.append((key, value))
    
    def get(self, key):
        index = self._hash(key)
        bucket = self.table[index]
        
        for k, v in bucket:
            if k == key:
                return v
        
        raise KeyError(key)
    
    def delete(self, key):
        index = self._hash(key)
        bucket = self.table[index]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                del bucket[i]
                return
        
        raise KeyError(key)

# Usage
ht = HashTable()
ht.insert("name", "Alice")
ht.insert("age", 30)
print(ht.get("name"))  # "Alice"
```

---

## Advanced Data Structures Overview

### Trees
**Purpose**: Hierarchical data organization
**Common Types**: Binary trees, BST, AVL, Red-Black
**Applications**: File systems, decision trees, parsing

### Graphs
**Purpose**: Represent relationships between entities
**Common Types**: Directed, undirected, weighted
**Applications**: Social networks, maps, dependencies

### Heaps
**Purpose**: Priority-based access to elements
**Common Types**: Min-heap, max-heap
**Applications**: Priority queues, scheduling, algorithms

---

## Choosing the Right Data Structure

### Decision Framework

1. **What operations do you need most frequently?**
   - Random access → Array
   - Insert/delete at ends → Stack/Queue
   - Key-based lookup → Hash Table

2. **What are your performance requirements?**
   - Need O(1) access → Array or Hash Table
   - Need O(1) insertion → Linked List or Stack
   - Need sorted order → Consider trees

3. **What are your memory constraints?**
   - Tight memory → Arrays
   - Dynamic size → Linked structures
   - Fast access → Hash tables (with overhead)

### Common Combinations
- **Array + Hash Table**: Fast access and lookup
- **Stack + Hash Table**: Undo with fast state lookup
- **Queue + Hash Table**: Task processing with deduplication

---

## Practice Problems

### Beginner Level
1. Implement a stack using arrays
2. Reverse a string using a stack
3. Check if parentheses are balanced
4. Implement a queue using two stacks

### Intermediate Level
1. Design a hash table with collision resolution
2. Implement LRU cache using hash table + doubly linked list
3. Find the first non-repeating character in a string
4. Implement a circular queue

### Advanced Level
1. Design a data structure for autocomplete
2. Implement a thread-safe hash table
3. Design a data structure for range sum queries
4. Implement a memory-efficient sparse matrix

---

## Next Steps

1. **Implement Each Structure**: Code them from scratch in your preferred language
2. **Solve Problems**: Use LeetCode, HackerRank for practice
3. **Analyze Trade-offs**: Understand when to use each structure
4. **Build Projects**: Apply data structures to real applications

Remember: The goal isn't to memorize implementations, but to understand when and why to use each data structure.