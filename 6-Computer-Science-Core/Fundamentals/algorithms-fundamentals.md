# Algorithms Fundamentals

## Overview

Algorithms are step-by-step procedures for solving problems. Think of them as recipes - they take inputs, perform operations, and produce outputs. The key is choosing the right algorithm for your specific problem and constraints.

**Key Principle**: There's rarely one "best" algorithm. The optimal choice depends on your data size, performance requirements, and resource constraints.

---

## Big O Notation: Understanding Performance

### What Is Big O?
Big O describes how an algorithm's performance scales with input size.

```python
# O(1) - Constant Time
def get_first_element(arr):
    return arr[0]  # Always takes the same time

# O(n) - Linear Time  
def find_element(arr, target):
    for item in arr:  # Time grows with array size
        if item == target:
            return True
    return False

# O(n²) - Quadratic Time
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):      # Outer loop: n times
        for j in range(n-1): # Inner loop: n times
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```

### Common Time Complexities (Best to Worst)
- **O(1)**: Hash table lookup, array access
- **O(log n)**: Binary search, balanced tree operations
- **O(n)**: Linear search, single loop
- **O(n log n)**: Efficient sorting (merge sort, quick sort)
- **O(n²)**: Nested loops, bubble sort
- **O(2ⁿ)**: Recursive fibonacci, brute force solutions

### Space Complexity
How much extra memory an algorithm uses.

```python
# O(1) Space - Constant extra memory
def reverse_array_in_place(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1

# O(n) Space - Linear extra memory
def reverse_array_new(arr):
    return arr[::-1]  # Creates new array
```

---

## Sorting Algorithms: Organizing Data

### Bubble Sort: The Teaching Algorithm
**Time**: O(n²), **Space**: O(1)
**Use**: Never in production, good for learning

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        # Flag to optimize - if no swaps, array is sorted
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr

# Example
numbers = [64, 34, 25, 12, 22, 11, 90]
print(bubble_sort(numbers))  # [11, 12, 22, 25, 34, 64, 90]
```

### Quick Sort: The Practical Choice
**Time**: O(n log n) average, O(n²) worst, **Space**: O(log n)
**Use**: General-purpose sorting, built into most languages

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)

# Example
numbers = [3, 6, 8, 10, 1, 2, 1]
print(quick_sort(numbers))  # [1, 1, 2, 3, 6, 8, 10]
```

### Merge Sort: The Stable Choice
**Time**: O(n log n) always, **Space**: O(n)
**Use**: When you need stable sorting or guaranteed performance

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# Example
numbers = [38, 27, 43, 3, 9, 82, 10]
print(merge_sort(numbers))  # [3, 9, 10, 27, 38, 43, 82]
```

### When to Use Each Sorting Algorithm

✅ **Quick Sort When:**
- General-purpose sorting
- Memory is limited
- Average case performance matters most

✅ **Merge Sort When:**
- You need stable sorting (equal elements keep relative order)
- Worst-case performance matters
- You're sorting linked lists

✅ **Built-in Sort When:**
- Production code (use `sorted()` in Python, `Array.sort()` in JavaScript)
- You don't need to implement sorting yourself

---

## Searching Algorithms: Finding Data

### Linear Search: The Simple Approach
**Time**: O(n), **Space**: O(1)
**Use**: Unsorted data, small datasets

```python
def linear_search(arr, target):
    for i, value in enumerate(arr):
        if value == target:
            return i  # Return index
    return -1  # Not found

# Example
numbers = [2, 3, 4, 10, 40]
print(linear_search(numbers, 10))  # 3
print(linear_search(numbers, 5))   # -1
```

### Binary Search: The Efficient Approach
**Time**: O(log n), **Space**: O(1)
**Use**: Sorted data, large datasets

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1  # Not found

# Recursive version
def binary_search_recursive(arr, target, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    
    if left > right:
        return -1
    
    mid = (left + right) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)

# Example
numbers = [2, 3, 4, 10, 40]
print(binary_search(numbers, 10))  # 3
print(binary_search(numbers, 5))   # -1
```

### Real-World Applications
- **Database Indexing**: B-tree searches for database records
- **Git Bisect**: Finding the commit that introduced a bug
- **Load Balancing**: Finding the right server based on capacity
- **Game Development**: Collision detection, pathfinding

---

## Recursion: Solving Problems by Breaking Them Down

### Understanding Recursion
A function that calls itself with a smaller version of the problem.

```python
# Classic example: Factorial
def factorial(n):
    # Base case - stops the recursion
    if n <= 1:
        return 1
    # Recursive case - function calls itself
    return n * factorial(n - 1)

print(factorial(5))  # 5 * 4 * 3 * 2 * 1 = 120
```

### Fibonacci: The Classic Problem
```python
# Naive recursive approach - VERY SLOW
def fibonacci_naive(n):
    if n <= 1:
        return n
    return fibonacci_naive(n - 1) + fibonacci_naive(n - 2)

# Optimized with memoization
def fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fibonacci_memo(n - 1, memo) + fibonacci_memo(n - 2, memo)
    return memo[n]

# Iterative approach - FASTEST
def fibonacci_iterative(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b

print(fibonacci_iterative(10))  # 55
```

### When to Use Recursion
✅ **Use When:**
- Problem has a recursive structure (trees, fractals)
- You can break the problem into smaller subproblems
- The recursive solution is clearer than iterative

❌ **Don't Use When:**
- Simple iteration would work better
- You're getting stack overflow errors
- Performance is critical and you haven't optimized

---

## Dynamic Programming: Optimizing Recursive Solutions

### What Is Dynamic Programming?
Solving complex problems by breaking them into simpler subproblems and storing results to avoid redundant calculations.

### The Coin Change Problem
```python
def coin_change(coins, amount):
    # dp[i] = minimum coins needed to make amount i
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0  # 0 coins needed to make amount 0
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1

# Example: coins = [1, 3, 4], amount = 6
# Answer: 2 coins (3 + 3)
print(coin_change([1, 3, 4], 6))  # 2
```

### Longest Common Subsequence
```python
def longest_common_subsequence(text1, text2):
    m, n = len(text1), len(text2)
    # dp[i][j] = LCS length of text1[:i] and text2[:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

# Example
print(longest_common_subsequence("abcde", "ace"))  # 3 ("ace")
```

### DP Problem-Solving Steps
1. **Identify subproblems**: What smaller problems do you need to solve?
2. **Define state**: What parameters uniquely identify a subproblem?
3. **Write recurrence**: How do subproblems relate to each other?
4. **Implement**: Bottom-up (iterative) or top-down (memoization)

---

## Graph Algorithms: Navigating Relationships

### Graph Representation
```python
# Adjacency List (most common)
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

# Adjacency Matrix (for dense graphs)
# graph[i][j] = 1 if edge exists, 0 otherwise
```

### Breadth-First Search (BFS)
**Use**: Shortest path in unweighted graphs, level-order traversal

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    result = []
    
    while queue:
        node = queue.popleft()
        if node not in visited:
            visited.add(node)
            result.append(node)
            
            # Add unvisited neighbors to queue
            for neighbor in graph[node]:
                if neighbor not in visited:
                    queue.append(neighbor)
    
    return result

# Example
print(bfs(graph, 'A'))  # ['A', 'B', 'C', 'D', 'E', 'F']
```

### Depth-First Search (DFS)
**Use**: Detecting cycles, topological sorting, maze solving

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    result = [start]
    
    for neighbor in graph[start]:
        if neighbor not in visited:
            result.extend(dfs(graph, neighbor, visited))
    
    return result

# Iterative version
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            result.append(node)
            
            # Add unvisited neighbors to stack
            for neighbor in graph[node]:
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return result

print(dfs(graph, 'A'))  # ['A', 'B', 'D', 'E', 'F', 'C']
```

### Dijkstra's Algorithm: Shortest Path
**Use**: Finding shortest path in weighted graphs

```python
import heapq

def dijkstra(graph, start):
    # graph = {node: [(neighbor, weight), ...]}
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]  # (distance, node)
    
    while pq:
        current_distance, current_node = heapq.heappop(pq)
        
        if current_distance > distances[current_node]:
            continue
        
        for neighbor, weight in graph[current_node]:
            distance = current_distance + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances

# Example weighted graph
weighted_graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('C', 1), ('D', 5)],
    'C': [('D', 8), ('E', 10)],
    'D': [('E', 2)],
    'E': []
}

print(dijkstra(weighted_graph, 'A'))
# {'A': 0, 'B': 4, 'C': 2, 'D': 9, 'E': 11}
```

---

## Algorithm Optimization Techniques

### 1. Two Pointers Technique
**Use**: Array problems, finding pairs, palindromes

```python
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    
    return []

# Example
print(two_sum_sorted([2, 7, 11, 15], 9))  # [0, 1]
```

### 2. Sliding Window Technique
**Use**: Subarray problems, string matching

```python
def max_sum_subarray(arr, k):
    if len(arr) < k:
        return None
    
    # Calculate sum of first window
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    # Slide the window
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i - k] + arr[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum

# Example
print(max_sum_subarray([2, 1, 5, 1, 3, 2], 3))  # 9 (5+1+3)
```

### 3. Divide and Conquer
**Use**: Large problems that can be split into smaller ones

```python
def max_subarray_sum(arr, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    
    if left == right:
        return arr[left]
    
    mid = (left + right) // 2
    
    # Maximum sum in left half
    left_sum = max_subarray_sum(arr, left, mid)
    
    # Maximum sum in right half
    right_sum = max_subarray_sum(arr, mid + 1, right)
    
    # Maximum sum crossing the middle
    left_cross_sum = float('-inf')
    temp_sum = 0
    for i in range(mid, left - 1, -1):
        temp_sum += arr[i]
        left_cross_sum = max(left_cross_sum, temp_sum)
    
    right_cross_sum = float('-inf')
    temp_sum = 0
    for i in range(mid + 1, right + 1):
        temp_sum += arr[i]
        right_cross_sum = max(right_cross_sum, temp_sum)
    
    cross_sum = left_cross_sum + right_cross_sum
    
    return max(left_sum, right_sum, cross_sum)

# Example
print(max_subarray_sum([-2, 1, -3, 4, -1, 2, 1, -5, 4]))  # 6
```

---

## Real-World Algorithm Applications

### Web Development
- **Sorting**: Search results ranking, data table sorting
- **Searching**: Autocomplete, filtering
- **Graph Algorithms**: Social network features, recommendation systems
- **Dynamic Programming**: Caching strategies, optimization problems

### Game Development
- **Pathfinding**: A* algorithm for NPC movement
- **Collision Detection**: Spatial partitioning algorithms
- **AI**: Minimax for game AI, decision trees

### Data Science
- **Sorting**: Data preprocessing, statistical analysis
- **Graph Algorithms**: Network analysis, clustering
- **Dynamic Programming**: Sequence alignment, optimization

### System Design
- **Load Balancing**: Consistent hashing algorithms
- **Caching**: LRU, LFU replacement algorithms
- **Distributed Systems**: Consensus algorithms, replication

---

## Common Algorithm Patterns

### 1. Brute Force → Optimization
```python
# Brute force: Check all pairs
def two_sum_brute_force(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []

# Optimized: Use hash table
def two_sum_optimized(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```

### 2. Recursive → Iterative
```python
# Recursive tree traversal
def inorder_recursive(root):
    if not root:
        return []
    return (inorder_recursive(root.left) + 
            [root.val] + 
            inorder_recursive(root.right))

# Iterative tree traversal
def inorder_iterative(root):
    result = []
    stack = []
    current = root
    
    while stack or current:
        while current:
            stack.append(current)
            current = current.left
        
        current = stack.pop()
        result.append(current.val)
        current = current.right
    
    return result
```

---

## Practice Strategy

### Beginner Problems
1. **Two Sum**: Hash table usage
2. **Reverse String**: Two pointers
3. **Valid Parentheses**: Stack usage
4. **Maximum Subarray**: Kadane's algorithm

### Intermediate Problems
1. **Merge Intervals**: Sorting + greedy
2. **Longest Substring Without Repeating Characters**: Sliding window
3. **Number of Islands**: DFS/BFS on grid
4. **Coin Change**: Dynamic programming

### Advanced Problems
1. **Word Ladder**: BFS with transformations
2. **Serialize and Deserialize Binary Tree**: Tree traversal
3. **Alien Dictionary**: Topological sorting
4. **Median of Two Sorted Arrays**: Binary search

### Problem-Solving Framework
1. **Understand**: What exactly is the problem asking?
2. **Examples**: Work through examples manually
3. **Approach**: What algorithm pattern fits?
4. **Code**: Implement step by step
5. **Test**: Verify with examples and edge cases
6. **Optimize**: Can you improve time/space complexity?

---

## Next Steps

1. **Master the Basics**: Ensure you understand sorting, searching, and basic graph algorithms
2. **Practice Regularly**: Solve 2-3 problems per week consistently
3. **Learn Patterns**: Recognize common problem patterns and solutions
4. **Build Projects**: Apply algorithms to real applications
5. **Study Trade-offs**: Understand when to use each algorithm

Remember: The goal isn't to memorize every algorithm, but to develop problem-solving skills and recognize patterns.