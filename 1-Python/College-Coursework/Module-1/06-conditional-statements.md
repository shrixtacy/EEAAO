# Conditional Statements

## 📚 Theoretical Understanding

### What are Conditional Statements?

Conditional statements are the decision-making structures in programming that allow your code to execute different actions based on whether certain conditions are true or false. They are fundamental to creating programs that can respond to different situations, user inputs, or data values. Without conditional statements, programs would only execute instructions in a fixed, linear sequence, unable to adapt to changing circumstances or make intelligent decisions.

In Python, conditional statements use the keywords `if`, `elif` (else if), and `else` to create branching logic. These statements evaluate boolean expressions (conditions that result in True or False) and execute different code blocks based on the results. This capability transforms programs from simple calculators into intelligent systems that can handle complex scenarios, validate user input, implement business logic, and respond appropriately to different situations.

The power of conditional statements lies in their ability to model real-world decision-making processes. Just as humans make decisions based on conditions ("If it's raining, take an umbrella"), programs use conditional statements to implement logic ("If the password is correct, grant access"). Understanding conditional statements is essential for writing any non-trivial program, as they form the backbone of program control flow.

### The if Statement

The `if` statement is the most basic conditional structure. It evaluates a condition, and if that condition is True, it executes the indented code block that follows. If the condition is False, the code block is skipped entirely. The syntax is straightforward: the keyword `if`, followed by a condition, followed by a colon, with the code to execute indented on the following lines.

Python's use of indentation to define code blocks is unique and important. Unlike languages that use braces `{}` or keywords like `begin`/`end`, Python uses consistent indentation (typically 4 spaces) to show which statements belong to the conditional block. This enforced indentation makes Python code highly readable and reduces errors from mismatched braces, but it also means you must be careful with your indentation - mixing tabs and spaces or inconsistent indentation will cause errors.

### The elif Statement

The `elif` (else if) statement allows you to check multiple conditions in sequence. After an `if` statement, you can have one or more `elif` statements, each with its own condition. Python evaluates these conditions in order from top to bottom, and as soon as it finds a True condition, it executes that block and skips all remaining `elif` and `else` blocks. This sequential evaluation is crucial for understanding how your program will behave.

Using `elif` is more efficient and clearer than using multiple separate `if` statements when you have mutually exclusive conditions. With `elif`, once one condition is met, the rest are skipped. With separate `if` statements, every condition is checked regardless of previous results. This difference affects both performance and logic - `elif` ensures only one block executes, while multiple `if` statements could execute multiple blocks if multiple conditions are true.

### The else Statement

The `else` statement provides a default action when none of the previous conditions are true. It's the catch-all that handles any case not explicitly covered by the `if` and `elif` conditions. The `else` block is optional - you only need it when you want to handle the "all other cases" scenario. It must come last in the conditional structure and doesn't have its own condition.

Using `else` effectively makes your code more robust by ensuring all possible cases are handled. Without an `else`, if all conditions are false, nothing happens - which might be intentional, but could also be an oversight. An `else` block makes it explicit that you've considered what should happen when none of the specific conditions are met, improving code clarity and reducing bugs.


### Nested Conditional Statements

Nested conditionals are conditional statements placed inside other conditional statements. They allow you to create complex decision trees where the evaluation of inner conditions depends on outer conditions being true. While powerful, nested conditionals can quickly become difficult to read and maintain if overused. Generally, if you find yourself nesting more than 2-3 levels deep, consider refactoring your code using functions, early returns, or boolean logic to simplify the structure.

Nested conditionals are useful when you have hierarchical decision-making: first check a broad category, then check specific details within that category. For example, first check if a user is logged in, then check their permission level, then check the specific action they're trying to perform. Each level of nesting adds another layer of specificity to your decision-making logic.

---

## 💻 Practical Examples

### Basic if Statement

```python
# Simple if statement
age = 18
if age >= 18:
    print("You are an adult")
    print("You can vote")

# if statement with no action when false
temperature = 35
if temperature > 30:
    print("It's hot outside!")

# Multiple statements in if block
score = 85
if score >= 80:
    print("Excellent work!")
    grade = "A"
    print(f"Your grade is: {grade}")
```

### if-else Statement

```python
# Basic if-else
age = 15
if age >= 18:
    print("You can vote")
else:
    print("You cannot vote yet")

# if-else with calculations
number = 7
if number % 2 == 0:
    print(f"{number} is even")
else:
    print(f"{number} is odd")

# if-else with multiple statements
password = "secret123"
if password == "admin123":
    print("Access granted")
    print("Welcome, administrator!")
else:
    print("Access denied")
    print("Invalid password")
```

### if-elif-else Statement

```python
# Grade calculator
score = 75

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Score: {score}, Grade: {grade}")

# Day of week
day = 3

if day == 1:
    print("Monday")
elif day == 2:
    print("Tuesday")
elif day == 3:
    print("Wednesday")
elif day == 4:
    print("Thursday")
elif day == 5:
    print("Friday")
elif day == 6:
    print("Saturday")
elif day == 7:
    print("Sunday")
else:
    print("Invalid day number")

# Temperature advisory
temp = 25

if temp < 0:
    print("Freezing! Stay indoors.")
elif temp < 10:
    print("Very cold. Wear a heavy coat.")
elif temp < 20:
    print("Cool. Wear a jacket.")
elif temp < 30:
    print("Pleasant weather.")
else:
    print("Hot! Stay hydrated.")
```

### Nested Conditional Statements

```python
# Login system with nested conditions
username = "admin"
password = "secret"
is_active = True

if username == "admin":
    if password == "secret":
        if is_active:
            print("Login successful")
            print("Welcome, administrator!")
        else:
            print("Account is deactivated")
    else:
        print("Incorrect password")
else:
    print("Username not found")

# Age and ticket pricing
age = 25
is_student = True

if age < 12:
    price = 5
    print("Child ticket")
elif age < 18:
    price = 10
    print("Teen ticket")
else:
    if is_student:
        price = 12
        print("Student discount applied")
    else:
        price = 15
        print("Adult ticket")

print(f"Ticket price: ${price}")

# Grade with attendance check
score = 85
attendance = 75

if score >= 80:
    if attendance >= 75:
        print("Excellent! You pass with distinction.")
    else:
        print("Good score, but attendance is low.")
        print("You pass, but improve attendance.")
else:
    if attendance >= 75:
        print("Score is low, but attendance is good.")
        print("You pass, but study harder.")
    else:
        print("Both score and attendance are low.")
        print("You need to improve.")
```

### Complex Conditions with Logical Operators

```python
# Using 'and' operator
age = 25
has_license = True

if age >= 18 and has_license:
    print("You can drive")
else:
    print("You cannot drive")

# Using 'or' operator
day = "Saturday"

if day == "Saturday" or day == "Sunday":
    print("It's the weekend!")
else:
    print("It's a weekday")

# Using 'not' operator
is_raining = False

if not is_raining:
    print("You don't need an umbrella")
else:
    print("Take an umbrella")

# Complex condition
age = 20
income = 30000
credit_score = 700

if age >= 18 and income >= 25000 and credit_score >= 650:
    print("Loan approved")
else:
    print("Loan denied")
    if age < 18:
        print("Reason: Age requirement not met")
    if income < 25000:
        print("Reason: Income requirement not met")
    if credit_score < 650:
        print("Reason: Credit score too low")
```

### Ternary Conditional Expression

```python
# Ternary operator (conditional expression)
age = 20
status = "Adult" if age >= 18 else "Minor"
print(status)

# Ternary with calculation
number = 7
result = "Even" if number % 2 == 0 else "Odd"
print(f"{number} is {result}")

# Nested ternary (use sparingly - can be hard to read)
score = 85
grade = "A" if score >= 90 else "B" if score >= 80 else "C" if score >= 70 else "F"
print(f"Grade: {grade}")

# Ternary in function call
def get_discount(is_member):
    return 20 if is_member else 0

discount = get_discount(True)
print(f"Discount: {discount}%")
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain the difference between using multiple `if` statements versus using `if-elif-else`. When should you use each approach? Provide examples demonstrating scenarios where each is appropriate.

**Detailed Answer:**

Understanding the difference between multiple `if` statements and `if-elif-else` chains is crucial for writing correct and efficient code. These structures behave differently and are suited for different scenarios.

**Multiple if Statements - Independent Conditions**

When you use multiple separate `if` statements, each condition is evaluated independently, regardless of whether previous conditions were true or false. Every `if` statement's code block can potentially execute.

```python
# Multiple if statements - ALL conditions are checked
score = 85
attendance = 90

if score >= 80:
    print("Excellent score!")  # This executes

if attendance >= 85:
    print("Great attendance!")  # This also executes

if score >= 80 and attendance >= 85:
    print("Perfect student!")  # This also executes

# Output:
# Excellent score!
# Great attendance!
# Perfect student!
```

In this example, all three conditions are true, so all three messages print. Each `if` is independent - the evaluation of one doesn't affect the others.

**if-elif-else Chain - Mutually Exclusive Conditions**

With `if-elif-else`, Python evaluates conditions sequentially from top to bottom. As soon as one condition is true, that block executes and the entire chain is exited. No further conditions are checked.

```python
# if-elif-else chain - ONLY ONE block executes
score = 85

if score >= 90:
    grade = "A"
    print("Excellent!")
elif score >= 80:
    grade = "B"
    print("Very good!")  # This executes
elif score >= 70:
    grade = "C"
    print("Good!")
else:
    grade = "F"
    print("Needs improvement")

# Output:
# Very good!
# Only the first true condition executes
```

Even though `score >= 70` is also true, that block doesn't execute because `score >= 80` was true first.

**When to Use Multiple if Statements:**

Use multiple `if` statements when conditions are independent and multiple actions might need to occur:

```python
# Validation checks - multiple issues might exist
username = "ab"
password = "123"

if len(username) < 3:
    print("Error: Username too short")  # Executes

if len(password) < 8:
    print("Error: Password too short")  # Also executes

if not username.isalnum():
    print("Error: Username must be alphanumeric")

# Output:
# Error: Username too short
# Error: Password too short
# Both errors are reported
```

```python
# Feature flags - multiple features can be enabled
has_premium = True
has_beta_access = True

if has_premium:
    print("Premium features enabled")  # Executes

if has_beta_access:
    print("Beta features enabled")  # Also executes

# Both features are enabled independently
```

```python
# Multiple conditions that should all be checked
temperature = 35
humidity = 80
wind_speed = 50

if temperature > 30:
    print("Heat warning")

if humidity > 70:
    print("High humidity warning")

if wind_speed > 40:
    print("Wind warning")

# All three warnings should be displayed
```

**When to Use if-elif-else:**

Use `if-elif-else` when conditions are mutually exclusive and only one action should occur:

```python
# Grade assignment - student gets ONE grade
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"  # This executes
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"Your grade is: {grade}")
# Output: Your grade is: B
```

```python
# Traffic light - only one state at a time
light = "yellow"

if light == "red":
    print("Stop")
elif light == "yellow":
    print("Slow down")  # This executes
elif light == "green":
    print("Go")
else:
    print("Invalid light color")

# Only one action occurs
```

```python
# User role - user has ONE primary role
role = "admin"

if role == "admin":
    print("Full access granted")  # This executes
    permissions = ["read", "write", "delete", "admin"]
elif role == "editor":
    print("Editor access granted")
    permissions = ["read", "write"]
elif role == "viewer":
    print("Viewer access granted")
    permissions = ["read"]
else:
    print("No access")
    permissions = []

# User gets permissions for their ONE role
```

**Performance Difference:**

```python
# Multiple if statements - ALL conditions checked (slower)
number = 5

if number > 0:
    print("Positive")  # Executes
if number < 10:
    print("Less than 10")  # Also checked and executes
if number != 0:
    print("Not zero")  # Also checked and executes

# All three conditions are evaluated

# if-elif-else - Stops at first true condition (faster)
number = 5

if number > 0:
    print("Positive")  # Executes, then exits
elif number < 10:
    print("Less than 10")  # Not checked
elif number != 0:
    print("Not zero")  # Not checked

# Only first condition is evaluated
```

**Common Mistake - Using Multiple ifs When elif is Needed:**

```python
# WRONG - Using multiple ifs for mutually exclusive conditions
score = 85

if score >= 90:
    grade = "A"
if score >= 80:  # This will execute even though we already assigned a grade!
    grade = "B"
if score >= 70:
    grade = "C"

print(grade)  # Output: C (WRONG! Should be B)
# Problem: All conditions are checked, last true one wins

# CORRECT - Using elif for mutually exclusive conditions
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:  # Only checked if previous was false
    grade = "B"
elif score >= 70:
    grade = "C"

print(grade)  # Output: B (CORRECT!)
```

**Decision Tree:**

```
Need to check multiple conditions?
│
├─ Are conditions independent? (multiple can be true)
│  └─ Use multiple if statements
│
└─ Are conditions mutually exclusive? (only one should be true)
   └─ Use if-elif-else chain
```

**Best Practices:**

1. **Use `if-elif-else` for categories**: When assigning to categories (grades, sizes, priorities)
2. **Use multiple `if` for validations**: When checking multiple independent requirements
3. **Use `if-elif-else` for efficiency**: When conditions are expensive to evaluate
4. **Use multiple `if` for clarity**: When each condition represents a distinct check
5. **Order matters in `if-elif-else`**: Put most specific or most likely conditions first

**Conclusion:**

Multiple `if` statements check all conditions independently and can execute multiple blocks. `if-elif-else` chains check conditions sequentially and execute only the first true block. Choose based on whether your conditions are independent (multiple `if`) or mutually exclusive (`if-elif-else`). Understanding this difference prevents logic errors and improves code efficiency.

---

### Question 2: Explain nested conditional statements. What are the advantages and disadvantages of nesting? How can you refactor deeply nested conditionals to improve code readability? Provide examples.

**Detailed Answer:**

Nested conditional statements are `if` statements placed inside other `if` statements, creating hierarchical decision-making structures. While powerful, they can quickly become complex and difficult to maintain. Understanding when to use nesting and when to refactor is essential for writing clean, maintainable code.

**What are Nested Conditionals?**

Nested conditionals occur when you place one conditional statement inside another. The inner conditional is only evaluated if the outer conditional's condition is true.

```python
# Simple nested conditional
age = 25
has_license = True

if age >= 18:
    print("Age requirement met")
    if has_license:
        print("You can drive")  # Inner condition
    else:
        print("You need a license")
else:
    print("Too young to drive")
```

**Advantages of Nested Conditionals:**

**1. Hierarchical Logic**

Nested conditionals naturally represent hierarchical decision-making:

```python
# User authentication system
username = "admin"
password = "secret123"
is_active = True
has_2fa = True

if username in database:
    if password_matches(password):
        if is_active:
            if has_2fa:
                if verify_2fa_code():
                    print("Login successful")
                else:
                    print("Invalid 2FA code")
            else:
                print("2FA required")
        else:
            print("Account deactivated")
    else:
        print("Wrong password")
else:
    print("User not found")
```

**2. Avoiding Redundant Checks**

Nesting can prevent unnecessary evaluations:

```python
# Without nesting - redundant checks
age = 25
income = 50000

if age >= 18 and income >= 30000:
    print("Eligible for loan")

if age >= 18 and income >= 30000 and credit_score >= 700:
    print("Eligible for premium loan")

# With nesting - more efficient
if age >= 18:
    if income >= 30000:
        print("Eligible for loan")
        if credit_score >= 700:
            print("Eligible for premium loan")
```

**3. Clear Dependency Relationships**

Nesting makes dependencies explicit:

```python
# File processing
if file_exists:
    if file_readable:
        if file_not_empty:
            process_file()
        else:
            print("File is empty")
    else:
        print("Cannot read file")
else:
    print("File not found")
```

**Disadvantages of Nested Conditionals:**

**1. Reduced Readability**

Deep nesting is hard to read and understand:

```python
# Hard to read - too much nesting
if condition1:
    if condition2:
        if condition3:
            if condition4:
                if condition5:
                    if condition6:
                        # What are we even checking anymore?
                        do_something()
```

**2. Difficult to Maintain**

Adding or modifying conditions in deeply nested code is error-prone:

```python
# Hard to modify - where does this else belong?
if a:
    if b:
        if c:
            if d:
                action1()
            else:  # Belongs to d
                action2()
        else:  # Belongs to c
            action3()
    else:  # Belongs to b
        action4()
```

**3. Increased Cognitive Load**

Developers must track multiple levels of context:

```python
# Mental overhead - tracking multiple conditions
if user_logged_in:
    if has_permission:
        if resource_available:
            if quota_not_exceeded:
                # Must remember: logged in AND has permission 
                # AND resource available AND quota OK
                perform_action()
```

**Refactoring Techniques:**

**Technique 1: Early Returns (Guard Clauses)**

Replace nested conditions with early returns:

```python
# BEFORE - Nested
def process_order(order):
    if order is not None:
        if order.is_valid():
            if order.has_items():
                if order.payment_confirmed():
                    return "Order processed"
                else:
                    return "Payment not confirmed"
            else:
                return "No items in order"
        else:
            return "Invalid order"
    else:
        return "Order is None"

# AFTER - Early returns (Guard clauses)
def process_order(order):
    if order is None:
        return "Order is None"
    
    if not order.is_valid():
        return "Invalid order"
    
    if not order.has_items():
        return "No items in order"
    
    if not order.payment_confirmed():
        return "Payment not confirmed"
    
    return "Order processed"

# Much more readable - flat structure
```

**Technique 2: Boolean Logic**

Combine conditions using `and`, `or`, `not`:

```python
# BEFORE - Nested
age = 25
income = 50000
credit_score = 720

if age >= 18:
    if income >= 30000:
        if credit_score >= 700:
            print("Loan approved")

# AFTER - Combined with 'and'
if age >= 18 and income >= 30000 and credit_score >= 700:
    print("Loan approved")

# Even better - extract to variable
is_eligible = age >= 18 and income >= 30000 and credit_score >= 700
if is_eligible:
    print("Loan approved")
```

**Technique 3: Extract to Functions**

Break complex conditions into named functions:

```python
# BEFORE - Complex nested logic
def can_access_resource(user, resource):
    if user is not None:
        if user.is_active:
            if user.has_subscription:
                if resource.is_available:
                    if not resource.is_restricted or user.is_admin:
                        return True
    return False

# AFTER - Extract to helper functions
def is_valid_user(user):
    return user is not None and user.is_active and user.has_subscription

def can_access(user, resource):
    if not resource.is_available:
        return False
    if resource.is_restricted and not user.is_admin:
        return False
    return True

def can_access_resource(user, resource):
    if not is_valid_user(user):
        return False
    return can_access(user, resource)

# Much clearer - each function has single responsibility
```

**Technique 4: Dictionary/Lookup Tables**

Replace nested if-elif with dictionaries:

```python
# BEFORE - Nested if-elif
def get_day_name(day_number):
    if day_number == 1:
        return "Monday"
    elif day_number == 2:
        return "Tuesday"
    elif day_number == 3:
        return "Wednesday"
    # ... etc

# AFTER - Dictionary lookup
def get_day_name(day_number):
    days = {
        1: "Monday",
        2: "Tuesday",
        3: "Wednesday",
        4: "Thursday",
        5: "Friday",
        6: "Saturday",
        7: "Sunday"
    }
    return days.get(day_number, "Invalid day")
```

**Technique 5: State Pattern**

For complex state-based logic, use classes:

```python
# BEFORE - Deeply nested state checks
def process_document(doc):
    if doc.state == "draft":
        if doc.has_content:
            if doc.author_approved:
                doc.state = "pending_review"
            else:
                print("Author approval needed")
        else:
            print("Document empty")
    elif doc.state == "pending_review":
        if doc.reviewer_approved:
            doc.state = "published"
        else:
            print("Awaiting review")
    # ... more states

# AFTER - State pattern with classes
class DocumentState:
    def process(self, doc):
        raise NotImplementedError

class DraftState(DocumentState):
    def process(self, doc):
        if not doc.has_content:
            return "Document empty"
        if not doc.author_approved:
            return "Author approval needed"
        doc.state = PendingReviewState()
        return "Moved to review"

class PendingReviewState(DocumentState):
    def process(self, doc):
        if not doc.reviewer_approved:
            return "Awaiting review"
        doc.state = PublishedState()
        return "Published"

# Usage
doc.state.process(doc)
```

**Best Practices:**

1. **Limit nesting to 2-3 levels maximum**
2. **Use early returns to flatten structure**
3. **Extract complex conditions to named variables**
4. **Break complex logic into smaller functions**
5. **Consider alternative patterns (lookup tables, state pattern)**
6. **Prioritize readability over cleverness**

**When Nesting is Acceptable:**

```python
# Acceptable - 2 levels, clear logic
if user.is_authenticated():
    if user.has_permission("write"):
        save_document()
    else:
        show_error("No write permission")
else:
    redirect_to_login()

# Acceptable - clear hierarchical structure
if file_exists(path):
    if is_readable(path):
        content = read_file(path)
        process(content)
```

**Conclusion:**

Nested conditionals are useful for hierarchical logic but can quickly become unreadable. Limit nesting depth, use early returns, extract to functions, and combine conditions with boolean logic. The goal is code that's easy to understand, maintain, and modify. When you find yourself nesting more than 2-3 levels deep, it's time to refactor.

---

## ✅ Key Takeaways

1. Conditional statements enable decision-making in programs
2. `if` executes code when condition is True
3. `elif` checks additional conditions sequentially
4. `else` provides default action when all conditions are False
5. Multiple `if` statements are independent; `if-elif-else` is mutually exclusive
6. Nested conditionals create hierarchical logic but reduce readability
7. Use early returns and guard clauses to flatten nested structures
8. Extract complex conditions to named variables or functions
9. Indentation defines code blocks in Python
10. Choose the right structure based on whether conditions are independent or exclusive

---

**Next Topic**: Loops and Iteration
**Previous Topic**: Conditional Code
**Module**: 1 - Programming for Everybody

Happy Learning! 🐍
