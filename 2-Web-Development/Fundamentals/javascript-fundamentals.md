# JavaScript Fundamentals - Complete Learning Guide

Master JavaScript from basic syntax to advanced concepts. This comprehensive guide covers everything you need to add interactivity and dynamic behavior to web pages.

## Table of Contents

1. [What is JavaScript?](#what-is-javascript)
2. [Variables and Data Types](#variables-and-data-types)
3. [Operators and Expressions](#operators-and-expressions)
4. [Control Flow](#control-flow)
5. [Functions](#functions)
6. [Objects and Arrays](#objects-and-arrays)
7. [DOM Manipulation](#dom-manipulation)
8. [Event Handling](#event-handling)
9. [Asynchronous JavaScript](#asynchronous-javascript)
10. [Error Handling](#error-handling)
11. [Modern JavaScript (ES6+)](#modern-javascript-es6)
12. [Best Practices](#best-practices)
13. [Practice Projects](#practice-projects)

---

## What is JavaScript?

JavaScript is a high-level, interpreted programming language that adds interactivity and dynamic behavior to web pages. It's the only programming language that runs natively in web browsers.

### Key Concepts:
- **Client-side scripting**: Runs in the user's browser
- **Dynamic typing**: Variables can hold different types of values
- **Event-driven**: Responds to user interactions
- **Interpreted**: No compilation step required
- **Multi-paradigm**: Supports procedural, object-oriented, and functional programming

### Why JavaScript Matters:
- **Interactivity**: Makes websites responsive to user actions
- **Dynamic Content**: Updates page content without reloading
- **Form Validation**: Provides immediate feedback to users
- **Animations**: Creates smooth visual effects
- **API Integration**: Fetches and displays data from servers

### Adding JavaScript to HTML
```html
<!-- Inline JavaScript -->
<button onclick="alert('Hello World!')">Click me</button>

<!-- Internal JavaScript -->
<script>
    console.log('Hello from internal script!');
    
    function greet() {
        alert('Hello World!');
    }
</script>

<!-- External JavaScript -->
<script src="script.js"></script>

<!-- Modern script loading -->
<script src="script.js" defer></script> <!-- Load after HTML parsing -->
<script src="script.js" async></script> <!-- Load asynchronously -->
```

---

## Variables and Data Types

### Variable Declarations
```javascript
// var (function-scoped, can be redeclared)
var name = 'John';
var name = 'Jane'; // Redeclaration allowed
console.log(name); // 'Jane'

// let (block-scoped, cannot be redeclared)
let age = 25;
age = 26; // Reassignment allowed
// let age = 27; // Error: Cannot redeclare

// const (block-scoped, cannot be reassigned)
const PI = 3.14159;
// PI = 3.14; // Error: Cannot reassign

// const with objects (object properties can be modified)
const person = { name: 'John', age: 30 };
person.age = 31; // This is allowed
person.city = 'New York'; // This is allowed
// person = {}; // Error: Cannot reassign the object itself
```

### Data Types

#### Primitive Data Types
```javascript
// String
let firstName = 'John';
let lastName = "Doe";
let fullName = `${firstName} ${lastName}`; // Template literal
let multiLine = `This is a
multi-line string`;

// Number
let integer = 42;
let decimal = 3.14159;
let negative = -10;
let scientific = 2.5e6; // 2,500,000
let infinity = Infinity;
let notANumber = NaN;

// Boolean
let isActive = true;
let isComplete = false;

// Undefined
let undefinedVar;
console.log(undefinedVar); // undefined

// Null
let emptyValue = null;

// Symbol (ES6+)
let symbol1 = Symbol('description');
let symbol2 = Symbol('description');
console.log(symbol1 === symbol2); // false (symbols are unique)

// BigInt (ES2020+)
let bigNumber = 1234567890123456789012345678901234567890n;
let anotherBig = BigInt('1234567890123456789012345678901234567890');
```

#### Non-Primitive Data Types
```javascript
// Object
let person = {
    name: 'John',
    age: 30,
    isStudent: false,
    address: {
        street: '123 Main St',
        city: 'New York',
        zipCode: '10001'
    },
    hobbies: ['reading', 'coding', 'hiking']
};

// Array
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, 'hello', true, null, { name: 'John' }];
let empty = [];

// Function
function greet(name) {
    return `Hello, ${name}!`;
}

// Function as variable
let sayHello = function(name) {
    return `Hello, ${name}!`;
};

// Arrow function
let sayHi = (name) => `Hi, ${name}!`;
```

### Type Checking and Conversion
```javascript
// Type checking
console.log(typeof 'hello'); // 'string'
console.log(typeof 42); // 'number'
console.log(typeof true); // 'boolean'
console.log(typeof undefined); // 'undefined'
console.log(typeof null); // 'object' (this is a known quirk)
console.log(typeof {}); // 'object'
console.log(typeof []); // 'object'
console.log(typeof function() {}); // 'function'

// More specific type checking
console.log(Array.isArray([])); // true
console.log(Array.isArray({})); // false

// Type conversion (explicit)
let str = '123';
let num = Number(str); // 123
let bool = Boolean(str); // true

let number = 456;
let string = String(number); // '456'
let stringAlt = number.toString(); // '456'

// Type coercion (implicit)
console.log('5' + 3); // '53' (string concatenation)
console.log('5' - 3); // 2 (numeric subtraction)
console.log('5' * 3); // 15 (numeric multiplication)
console.log(true + 1); // 2 (boolean to number)
console.log(false + 1); // 1 (boolean to number)
```

---

## Operators and Expressions

### Arithmetic Operators
```javascript
let a = 10;
let b = 3;

console.log(a + b); // 13 (addition)
console.log(a - b); // 7 (subtraction)
console.log(a * b); // 30 (multiplication)
console.log(a / b); // 3.333... (division)
console.log(a % b); // 1 (modulus/remainder)
console.log(a ** b); // 1000 (exponentiation)

// Increment and decrement
let count = 5;
console.log(count++); // 5 (post-increment: use then increment)
console.log(count); // 6

console.log(++count); // 7 (pre-increment: increment then use)
console.log(count--); // 7 (post-decrement: use then decrement)
console.log(--count); // 5 (pre-decrement: decrement then use)
```

### Assignment Operators
```javascript
let x = 10;

x += 5; // x = x + 5; (15)
x -= 3; // x = x - 3; (12)
x *= 2; // x = x * 2; (24)
x /= 4; // x = x / 4; (6)
x %= 4; // x = x % 4; (2)
x **= 3; // x = x ** 3; (8)

// Logical assignment (ES2021+)
let value = null;
value ??= 'default'; // Assign if nullish (null or undefined)
console.log(value); // 'default'

let isEnabled = false;
isEnabled ||= true; // Assign if falsy
console.log(isEnabled); // true

let isActive = true;
isActive &&= false; // Assign if truthy
console.log(isActive); // false
```

### Comparison Operators
```javascript
let a = 5;
let b = '5';

// Equality (loose - with type coercion)
console.log(a == b); // true (5 == '5')
console.log(a != b); // false

// Strict equality (no type coercion)
console.log(a === b); // false (5 !== '5')
console.log(a !== b); // true

// Relational operators
console.log(10 > 5); // true
console.log(10 < 5); // false
console.log(10 >= 10); // true
console.log(10 <= 5); // false

// String comparison
console.log('apple' < 'banana'); // true (lexicographic order)
console.log('Apple' < 'apple'); // true (uppercase comes first)
```

### Logical Operators
```javascript
let a = true;
let b = false;

// Logical AND
console.log(a && b); // false
console.log(true && true); // true

// Logical OR
console.log(a || b); // true
console.log(false || false); // false

// Logical NOT
console.log(!a); // false
console.log(!b); // true
console.log(!!a); // true (double negation)

// Short-circuit evaluation
let user = { name: 'John' };
let name = user && user.name; // 'John' (if user exists, get name)
let defaultName = name || 'Anonymous'; // Use name or default

// Nullish coalescing (ES2020+)
let value = null;
let result = value ?? 'default'; // 'default' (only for null/undefined)

let zero = 0;
let resultOr = zero || 'default'; // 'default' (0 is falsy)
let resultNullish = zero ?? 'default'; // 0 (0 is not nullish)
```

### Ternary Operator
```javascript
// Basic ternary
let age = 18;
let status = age >= 18 ? 'adult' : 'minor';
console.log(status); // 'adult'

// Nested ternary (use sparingly)
let score = 85;
let grade = score >= 90 ? 'A' : 
            score >= 80 ? 'B' : 
            score >= 70 ? 'C' : 
            score >= 60 ? 'D' : 'F';
console.log(grade); // 'B'

// Ternary with function calls
let isLoggedIn = true;
let message = isLoggedIn ? getWelcomeMessage() : getLoginPrompt();

function getWelcomeMessage() {
    return 'Welcome back!';
}

function getLoginPrompt() {
    return 'Please log in';
}
```

---

## Control Flow

### Conditional Statements
```javascript
// if statement
let temperature = 25;

if (temperature > 30) {
    console.log('It\'s hot!');
} else if (temperature > 20) {
    console.log('It\'s warm');
} else if (temperature > 10) {
    console.log('It\'s cool');
} else {
    console.log('It\'s cold!');
}

// Switch statement
let day = 'Monday';

switch (day) {
    case 'Monday':
        console.log('Start of the work week');
        break;
    case 'Tuesday':
    case 'Wednesday':
    case 'Thursday':
        console.log('Midweek');
        break;
    case 'Friday':
        console.log('TGIF!');
        break;
    case 'Saturday':
    case 'Sunday':
        console.log('Weekend!');
        break;
    default:
        console.log('Invalid day');
}

// Switch with expressions (modern approach)
let dayType = (() => {
    switch (day) {
        case 'Monday':
        case 'Tuesday':
        case 'Wednesday':
        case 'Thursday':
        case 'Friday':
            return 'weekday';
        case 'Saturday':
        case 'Sunday':
            return 'weekend';
        default:
            return 'unknown';
    }
})();
```

### Loops
```javascript
// for loop
for (let i = 0; i < 5; i++) {
    console.log(`Count: ${i}`);
}

// for loop with different increments
for (let i = 10; i > 0; i -= 2) {
    console.log(`Countdown: ${i}`);
}

// while loop
let count = 0;
while (count < 3) {
    console.log(`While count: ${count}`);
    count++;
}

// do-while loop (executes at least once)
let number = 0;
do {
    console.log(`Number: ${number}`);
    number++;
} while (number < 3);

// for...in loop (for object properties)
let person = { name: 'John', age: 30, city: 'New York' };
for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}

// for...of loop (for iterable values)
let fruits = ['apple', 'banana', 'orange'];
for (let fruit of fruits) {
    console.log(fruit);
}

// for...of with index
for (let [index, fruit] of fruits.entries()) {
    console.log(`${index}: ${fruit}`);
}

// Break and continue
for (let i = 0; i < 10; i++) {
    if (i === 3) {
        continue; // Skip this iteration
    }
    if (i === 7) {
        break; // Exit the loop
    }
    console.log(i);
}

// Labeled statements (rarely used)
outer: for (let i = 0; i < 3; i++) {
    inner: for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            break outer; // Break out of outer loop
        }
        console.log(`i: ${i}, j: ${j}`);
    }
}
```

---

## Functions

### Function Declarations and Expressions
```javascript
// Function declaration (hoisted)
function greet(name) {
    return `Hello, ${name}!`;
}

// Function expression (not hoisted)
const sayHello = function(name) {
    return `Hello, ${name}!`;
};

// Arrow function (ES6+)
const sayHi = (name) => {
    return `Hi, ${name}!`;
};

// Arrow function (concise syntax)
const sayHey = name => `Hey, ${name}!`;

// Arrow function with multiple parameters
const add = (a, b) => a + b;

// Arrow function with no parameters
const getRandomNumber = () => Math.random();

// Arrow function with object return (parentheses needed)
const createPerson = (name, age) => ({ name, age });
```

### Function Parameters
```javascript
// Default parameters
function greet(name = 'World', greeting = 'Hello') {
    return `${greeting}, ${name}!`;
}

console.log(greet()); // 'Hello, World!'
console.log(greet('John')); // 'Hello, John!'
console.log(greet('John', 'Hi')); // 'Hi, John!'

// Rest parameters
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15

function introduce(firstName, lastName, ...hobbies) {
    console.log(`Name: ${firstName} ${lastName}`);
    console.log(`Hobbies: ${hobbies.join(', ')}`);
}

introduce('John', 'Doe', 'reading', 'coding', 'hiking');

// Destructuring parameters
function displayUser({ name, age, email = 'Not provided' }) {
    console.log(`Name: ${name}`);
    console.log(`Age: ${age}`);
    console.log(`Email: ${email}`);
}

displayUser({ name: 'John', age: 30 });

// Array destructuring parameters
function getCoordinates([x, y, z = 0]) {
    return { x, y, z };
}

console.log(getCoordinates([10, 20])); // { x: 10, y: 20, z: 0 }
```

### Function Scope and Closures
```javascript
// Global scope
let globalVar = 'I am global';

function outerFunction(x) {
    // Function scope
    let outerVar = 'I am outer';
    
    function innerFunction(y) {
        // Inner function has access to outer variables (closure)
        let innerVar = 'I am inner';
        console.log(globalVar); // Accessible
        console.log(outerVar);  // Accessible
        console.log(innerVar);  // Accessible
        return x + y;
    }
    
    return innerFunction;
}

let closure = outerFunction(10);
console.log(closure(5)); // 15

// Practical closure example
function createCounter() {
    let count = 0;
    
    return {
        increment: () => ++count,
        decrement: () => --count,
        getCount: () => count
    };
}

let counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2

// Module pattern with closure
const calculator = (function() {
    let result = 0;
    
    return {
        add: (x) => { result += x; return calculator; },
        subtract: (x) => { result -= x; return calculator; },
        multiply: (x) => { result *= x; return calculator; },
        divide: (x) => { result /= x; return calculator; },
        getResult: () => result,
        reset: () => { result = 0; return calculator; }
    };
})();

calculator.add(10).multiply(2).subtract(5).getResult(); // 15
```

### Higher-Order Functions
```javascript
// Functions that take other functions as parameters
function processArray(arr, callback) {
    let result = [];
    for (let item of arr) {
        result.push(callback(item));
    }
    return result;
}

let numbers = [1, 2, 3, 4, 5];
let doubled = processArray(numbers, x => x * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// Functions that return other functions
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

let double = createMultiplier(2);
let triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Function composition
const compose = (f, g) => (x) => f(g(x));

const addOne = x => x + 1;
const multiplyByTwo = x => x * 2;

const addOneThenDouble = compose(multiplyByTwo, addOne);
console.log(addOneThenDouble(3)); // 8 (3 + 1 = 4, 4 * 2 = 8)
```

### Immediately Invoked Function Expressions (IIFE)
```javascript
// Basic IIFE
(function() {
    console.log('This runs immediately!');
})();

// IIFE with parameters
(function(name) {
    console.log(`Hello, ${name}!`);
})('World');

// IIFE with return value
let result = (function(a, b) {
    return a + b;
})(5, 3);

console.log(result); // 8

// Arrow function IIFE
((name) => {
    console.log(`Hi, ${name}!`);
})('JavaScript');

// Module pattern with IIFE
const myModule = (function() {
    let privateVar = 'I am private';
    
    function privateFunction() {
        console.log('This is private');
    }
    
    return {
        publicMethod: function() {
            console.log('This is public');
            privateFunction();
        },
        getPrivateVar: function() {
            return privateVar;
        }
    };
})();

myModule.publicMethod();
console.log(myModule.getPrivateVar());
```

---

## Objects and Arrays

### Working with Objects
```javascript
// Object creation
let person = {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,
    isEmployed: true,
    address: {
        street: '123 Main St',
        city: 'New York',
        zipCode: '10001'
    },
    hobbies: ['reading', 'coding', 'hiking'],
    
    // Methods
    getFullName: function() {
        return `${this.firstName} ${this.lastName}`;
    },
    
    // ES6 method shorthand
    introduce() {
        return `Hi, I'm ${this.getFullName()}`;
    },
    
    // Arrow function (doesn't have its own 'this')
    getAge: () => {
        // 'this' refers to global object, not person
        return this.age; // undefined in strict mode
    }
};

// Accessing properties
console.log(person.firstName); // 'John'
console.log(person['lastName']); // 'Doe'
console.log(person.address.city); // 'New York'

// Dynamic property access
let propertyName = 'age';
console.log(person[propertyName]); // 30

// Adding properties
person.email = 'john@example.com';
person['phone'] = '555-1234';

// Deleting properties
delete person.isEmployed;

// Checking if property exists
console.log('email' in person); // true
console.log(person.hasOwnProperty('email')); // true
console.log(person.salary !== undefined); // false
```

### Object Methods and Iteration
```javascript
let user = {
    name: 'Alice',
    age: 25,
    city: 'Boston'
};

// Object.keys() - get property names
let keys = Object.keys(user);
console.log(keys); // ['name', 'age', 'city']

// Object.values() - get property values
let values = Object.values(user);
console.log(values); // ['Alice', 25, 'Boston']

// Object.entries() - get key-value pairs
let entries = Object.entries(user);
console.log(entries); // [['name', 'Alice'], ['age', 25], ['city', 'Boston']]

// Iterating over objects
for (let key in user) {
    console.log(`${key}: ${user[key]}`);
}

// Using Object.entries() with for...of
for (let [key, value] of Object.entries(user)) {
    console.log(`${key}: ${value}`);
}

// Object.assign() - copying/merging objects
let defaults = { theme: 'light', language: 'en' };
let userPrefs = { theme: 'dark' };
let settings = Object.assign({}, defaults, userPrefs);
console.log(settings); // { theme: 'dark', language: 'en' }

// Spread operator (ES6+) - modern way to copy/merge
let newSettings = { ...defaults, ...userPrefs };
console.log(newSettings); // { theme: 'dark', language: 'en' }
```

### Working with Arrays
```javascript
// Array creation
let fruits = ['apple', 'banana', 'orange'];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, 'hello', true, { name: 'John' }, [1, 2, 3]];
let empty = [];

// Array constructor
let arr1 = new Array(5); // Creates array with 5 empty slots
let arr2 = new Array(1, 2, 3); // Creates [1, 2, 3]

// Array.from() - create array from iterable
let str = 'hello';
let chars = Array.from(str); // ['h', 'e', 'l', 'l', 'o']

// Array.of() - create array from arguments
let nums = Array.of(1, 2, 3); // [1, 2, 3]
```
### Array Methods
```javascript
let fruits = ['apple', 'banana', 'orange'];

// Adding elements
fruits.push('grape'); // Add to end: ['apple', 'banana', 'orange', 'grape']
fruits.unshift('mango'); // Add to beginning: ['mango', 'apple', 'banana', 'orange', 'grape']

// Removing elements
let lastFruit = fruits.pop(); // Remove from end: 'grape'
let firstFruit = fruits.shift(); // Remove from beginning: 'mango'

// Finding elements
let index = fruits.indexOf('banana'); // 1
let found = fruits.includes('apple'); // true
let item = fruits.find(fruit => fruit.startsWith('o')); // 'orange'
let itemIndex = fruits.findIndex(fruit => fruit.startsWith('o')); // 2

// Slicing and splicing
let citrus = fruits.slice(1, 3); // ['banana', 'orange'] (doesn't modify original)
fruits.splice(1, 1, 'kiwi', 'pear'); // Remove 1 item at index 1, add 'kiwi', 'pear'

// Joining and splitting
let fruitString = fruits.join(', '); // 'apple, kiwi, pear, orange'
let words = 'hello world javascript'.split(' '); // ['hello', 'world', 'javascript']

// Reversing and sorting
let numbers = [3, 1, 4, 1, 5, 9];
numbers.reverse(); // [9, 5, 1, 4, 1, 3] (modifies original)
numbers.sort(); // [1, 1, 3, 4, 5, 9] (lexicographic sort)
numbers.sort((a, b) => b - a); // [9, 5, 4, 3, 1, 1] (descending numeric sort)
```

### Array Iteration Methods
```javascript
let numbers = [1, 2, 3, 4, 5];

// forEach - execute function for each element
numbers.forEach((num, index) => {
    console.log(`Index ${index}: ${num}`);
});

// map - create new array with transformed elements
let doubled = numbers.map(num => num * 2); // [2, 4, 6, 8, 10]

// filter - create new array with elements that pass test
let evens = numbers.filter(num => num % 2 === 0); // [2, 4]

// reduce - reduce array to single value
let sum = numbers.reduce((total, num) => total + num, 0); // 15
let product = numbers.reduce((total, num) => total * num, 1); // 120

// some - test if at least one element passes test
let hasEven = numbers.some(num => num % 2 === 0); // true

// every - test if all elements pass test
let allPositive = numbers.every(num => num > 0); // true

// find and findIndex
let firstEven = numbers.find(num => num % 2 === 0); // 2
let firstEvenIndex = numbers.findIndex(num => num % 2 === 0); // 1

// Chaining methods
let result = numbers
    .filter(num => num > 2)
    .map(num => num * 2)
    .reduce((sum, num) => sum + num, 0);
console.log(result); // 24 (3*2 + 4*2 + 5*2 = 6 + 8 + 10 = 24)
```

---

## DOM Manipulation

### Selecting Elements
```javascript
// Select by ID
let header = document.getElementById('main-header');

// Select by class name
let buttons = document.getElementsByClassName('btn');
let firstButton = buttons[0];

// Select by tag name
let paragraphs = document.getElementsByTagName('p');

// Modern selectors (recommended)
let element = document.querySelector('#main-header'); // First match
let allButtons = document.querySelectorAll('.btn'); // All matches

// CSS selectors
let navLinks = document.querySelectorAll('nav a');
let activeItems = document.querySelectorAll('.active');
let firstChild = document.querySelector('ul > li:first-child');
let lastParagraph = document.querySelector('p:last-of-type');
```

### Modifying Content and Attributes
```javascript
// Text content
let heading = document.querySelector('h1');
heading.textContent = 'New Heading'; // Safe - escapes HTML
heading.innerText = 'Another Heading'; // Respects styling (hidden elements)

// HTML content
let container = document.querySelector('.container');
container.innerHTML = '<p>New <strong>HTML</strong> content</p>'; // Can be unsafe

// Attributes
let link = document.querySelector('a');
link.setAttribute('href', 'https://example.com');
link.setAttribute('target', '_blank');
let url = link.getAttribute('href');
link.removeAttribute('target');

// Properties (preferred for standard attributes)
let input = document.querySelector('input');
input.value = 'New value';
input.disabled = true;
input.checked = false;

// Data attributes
let element = document.querySelector('.data-element');
element.dataset.userId = '123'; // Sets data-user-id="123"
let userId = element.dataset.userId; // Gets data-user-id value
```
### Styling Elements
```javascript
let element = document.querySelector('.box');

// Inline styles
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.fontSize = '20px';
element.style.marginTop = '10px';

// CSS properties with hyphens become camelCase
element.style.borderRadius = '5px'; // border-radius
element.style.textAlign = 'center'; // text-align

// Getting computed styles
let computedStyle = window.getComputedStyle(element);
let currentColor = computedStyle.color;
let currentWidth = computedStyle.width;

// CSS classes (preferred method)
element.classList.add('active');
element.classList.remove('inactive');
element.classList.toggle('highlighted');
element.classList.contains('active'); // true/false

// Multiple classes
element.classList.add('class1', 'class2', 'class3');
element.classList.remove('class1', 'class2');

// Replace class
element.classList.replace('old-class', 'new-class');

// className property (less preferred)
element.className = 'new-class another-class';
element.className += ' additional-class';
```

### Creating and Modifying Elements
```javascript
// Creating elements
let newDiv = document.createElement('div');
let newParagraph = document.createElement('p');
let newImage = document.createElement('img');

// Setting up new elements
newDiv.textContent = 'This is a new div';
newDiv.classList.add('dynamic-content');
newDiv.id = 'new-element';

newParagraph.innerHTML = 'This is a <strong>new paragraph</strong>';

newImage.src = 'image.jpg';
newImage.alt = 'Description';
newImage.width = 300;

// Appending elements
let container = document.querySelector('.container');
container.appendChild(newDiv);
container.appendChild(newParagraph);

// Insert at specific position
container.insertBefore(newImage, newParagraph);

// Modern insertion methods
container.prepend(newDiv); // Add as first child
container.append(newParagraph); // Add as last child
newDiv.before(newImage); // Insert before newDiv
newDiv.after(newParagraph); // Insert after newDiv

// Removing elements
let elementToRemove = document.querySelector('.remove-me');
elementToRemove.remove(); // Modern way
// elementToRemove.parentNode.removeChild(elementToRemove); // Old way

// Cloning elements
let original = document.querySelector('.original');
let clone = original.cloneNode(true); // true = deep clone (includes children)
container.appendChild(clone);
```

### Document Fragments (Performance Optimization)
```javascript
// When adding many elements, use DocumentFragment for better performance
let fragment = document.createDocumentFragment();

for (let i = 0; i < 1000; i++) {
    let listItem = document.createElement('li');
    listItem.textContent = `Item ${i + 1}`;
    fragment.appendChild(listItem);
}

// Single DOM operation instead of 1000
let list = document.querySelector('ul');
list.appendChild(fragment);

// Template element (HTML5)
let template = document.querySelector('#item-template');
let clone = template.content.cloneNode(true);
clone.querySelector('.item-name').textContent = 'New Item';
document.querySelector('.items').appendChild(clone);
```

---

## Event Handling

### Adding Event Listeners
```javascript
// Basic event listener
let button = document.querySelector('#my-button');
button.addEventListener('click', function() {
    console.log('Button clicked!');
});

// Arrow function
button.addEventListener('click', () => {
    console.log('Button clicked with arrow function!');
});

// Named function (can be removed later)
function handleClick() {
    console.log('Button clicked with named function!');
}
button.addEventListener('click', handleClick);

// Removing event listener
button.removeEventListener('click', handleClick);

// Event listener with options
button.addEventListener('click', handleClick, {
    once: true,      // Execute only once
    passive: true,   // Never calls preventDefault()
    capture: true    // Capture phase instead of bubble phase
});
```

### Event Object and Properties
```javascript
button.addEventListener('click', function(event) {
    console.log('Event type:', event.type); // 'click'
    console.log('Target element:', event.target); // Element that triggered event
    console.log('Current target:', event.currentTarget); // Element with listener
    console.log('Mouse coordinates:', event.clientX, event.clientY);
    console.log('Timestamp:', event.timeStamp);
    
    // Prevent default behavior
    event.preventDefault();
    
    // Stop event propagation
    event.stopPropagation();
    
    // Stop immediate propagation (prevents other listeners on same element)
    event.stopImmediatePropagation();
});

// Form events
let form = document.querySelector('form');
form.addEventListener('submit', function(event) {
    event.preventDefault(); // Prevent form submission
    
    let formData = new FormData(form);
    for (let [key, value] of formData.entries()) {
        console.log(`${key}: ${value}`);
    }
});

// Input events
let input = document.querySelector('input');
input.addEventListener('input', function(event) {
    console.log('Current value:', event.target.value);
});

input.addEventListener('change', function(event) {
    console.log('Value changed to:', event.target.value);
});
```
### Common Event Types
```javascript
// Mouse events
element.addEventListener('click', handleClick);
element.addEventListener('dblclick', handleDoubleClick);
element.addEventListener('mousedown', handleMouseDown);
element.addEventListener('mouseup', handleMouseUp);
element.addEventListener('mouseover', handleMouseOver);
element.addEventListener('mouseout', handleMouseOut);
element.addEventListener('mousemove', handleMouseMove);

// Keyboard events
document.addEventListener('keydown', function(event) {
    console.log('Key pressed:', event.key);
    console.log('Key code:', event.code);
    console.log('Ctrl pressed:', event.ctrlKey);
    console.log('Shift pressed:', event.shiftKey);
    console.log('Alt pressed:', event.altKey);
    
    if (event.key === 'Enter') {
        console.log('Enter key pressed');
    }
    
    if (event.ctrlKey && event.key === 's') {
        event.preventDefault(); // Prevent browser save
        console.log('Ctrl+S pressed');
    }
});

// Window events
window.addEventListener('load', function() {
    console.log('Page fully loaded');
});

window.addEventListener('resize', function() {
    console.log('Window resized:', window.innerWidth, window.innerHeight);
});

window.addEventListener('scroll', function() {
    console.log('Page scrolled:', window.scrollY);
});

// Focus events
input.addEventListener('focus', function() {
    console.log('Input focused');
});

input.addEventListener('blur', function() {
    console.log('Input lost focus');
});
```

### Event Delegation
```javascript
// Instead of adding listeners to each item
let list = document.querySelector('ul');

list.addEventListener('click', function(event) {
    // Check if clicked element is a list item
    if (event.target.tagName === 'LI') {
        console.log('List item clicked:', event.target.textContent);
        event.target.classList.toggle('selected');
    }
    
    // Check for specific classes
    if (event.target.classList.contains('delete-btn')) {
        event.target.closest('li').remove();
    }
});

// This works for dynamically added elements too
function addListItem(text) {
    let li = document.createElement('li');
    li.innerHTML = `${text} <button class="delete-btn">Delete</button>`;
    list.appendChild(li);
}

// Custom events
let customEvent = new CustomEvent('userLogin', {
    detail: { username: 'john_doe', timestamp: Date.now() }
});

document.addEventListener('userLogin', function(event) {
    console.log('User logged in:', event.detail.username);
});

// Dispatch the custom event
document.dispatchEvent(customEvent);
```

---

## Asynchronous JavaScript

### Callbacks
```javascript
// Basic callback
function fetchData(callback) {
    setTimeout(() => {
        let data = { id: 1, name: 'John Doe' };
        callback(data);
    }, 1000);
}

fetchData(function(data) {
    console.log('Data received:', data);
});

// Callback with error handling
function fetchDataWithError(callback) {
    setTimeout(() => {
        let success = Math.random() > 0.5;
        if (success) {
            callback(null, { id: 1, name: 'John Doe' });
        } else {
            callback(new Error('Failed to fetch data'), null);
        }
    }, 1000);
}

fetchDataWithError(function(error, data) {
    if (error) {
        console.error('Error:', error.message);
    } else {
        console.log('Data:', data);
    }
});

// Callback hell example (avoid this)
getData(function(a) {
    getMoreData(a, function(b) {
        getEvenMoreData(b, function(c) {
            getFinalData(c, function(d) {
                // Finally do something with d
                console.log(d);
            });
        });
    });
});
```

### Promises
```javascript
// Creating a Promise
let myPromise = new Promise(function(resolve, reject) {
    let success = Math.random() > 0.5;
    
    setTimeout(() => {
        if (success) {
            resolve({ message: 'Success!', data: [1, 2, 3] });
        } else {
            reject(new Error('Something went wrong'));
        }
    }, 1000);
});

// Using Promises
myPromise
    .then(function(result) {
        console.log('Success:', result);
        return result.data;
    })
    .then(function(data) {
        console.log('Data:', data);
    })
    .catch(function(error) {
        console.error('Error:', error.message);
    })
    .finally(function() {
        console.log('Promise completed');
    });

// Promise chaining
function fetchUser(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id > 0) {
                resolve({ id, name: `User ${id}` });
            } else {
                reject(new Error('Invalid user ID'));
            }
        }, 500);
    });
}

function fetchUserPosts(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve([
                { id: 1, title: 'Post 1', userId },
                { id: 2, title: 'Post 2', userId }
            ]);
        }, 300);
    });
}

fetchUser(1)
    .then(user => {
        console.log('User:', user);
        return fetchUserPosts(user.id);
    })
    .then(posts => {
        console.log('Posts:', posts);
    })
    .catch(error => {
        console.error('Error:', error.message);
    });
```
### Async/Await
```javascript
// Basic async/await
async function fetchData() {
    try {
        let response = await fetch('https://api.example.com/data');
        let data = await response.json();
        return data;
    } catch (error) {
        console.error('Error fetching data:', error);
        throw error;
    }
}

// Using async function
async function main() {
    try {
        let data = await fetchData();
        console.log('Data:', data);
    } catch (error) {
        console.error('Failed to get data:', error);
    }
}

main();

// Async/await with multiple operations
async function getUserData(userId) {
    try {
        let user = await fetchUser(userId);
        let posts = await fetchUserPosts(userId);
        let comments = await fetchUserComments(userId);
        
        return {
            user,
            posts,
            comments
        };
    } catch (error) {
        console.error('Error getting user data:', error);
        return null;
    }
}

// Parallel execution with Promise.all
async function getAllUserData(userId) {
    try {
        let [user, posts, comments] = await Promise.all([
            fetchUser(userId),
            fetchUserPosts(userId),
            fetchUserComments(userId)
        ]);
        
        return { user, posts, comments };
    } catch (error) {
        console.error('Error getting user data:', error);
        return null;
    }
}

// Error handling in async/await
async function robustFetch(url) {
    try {
        let response = await fetch(url);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        let data = await response.json();
        return data;
    } catch (error) {
        if (error instanceof TypeError) {
            console.error('Network error:', error.message);
        } else {
            console.error('Fetch error:', error.message);
        }
        return null;
    }
}
```

### Promise Utility Methods
```javascript
// Promise.all - wait for all promises to resolve
let promises = [
    fetch('/api/users'),
    fetch('/api/posts'),
    fetch('/api/comments')
];

Promise.all(promises)
    .then(responses => {
        console.log('All requests completed');
        return Promise.all(responses.map(r => r.json()));
    })
    .then(data => {
        let [users, posts, comments] = data;
        console.log('All data:', { users, posts, comments });
    })
    .catch(error => {
        console.error('One or more requests failed:', error);
    });

// Promise.allSettled - wait for all promises to settle (resolve or reject)
Promise.allSettled(promises)
    .then(results => {
        results.forEach((result, index) => {
            if (result.status === 'fulfilled') {
                console.log(`Promise ${index} succeeded:`, result.value);
            } else {
                console.log(`Promise ${index} failed:`, result.reason);
            }
        });
    });

// Promise.race - resolve with first promise that settles
Promise.race([
    fetch('/api/fast-server'),
    fetch('/api/slow-server')
])
    .then(response => {
        console.log('First server responded');
        return response.json();
    })
    .catch(error => {
        console.error('All servers failed:', error);
    });

// Promise.any - resolve with first promise that fulfills
Promise.any([
    fetch('/api/server1'),
    fetch('/api/server2'),
    fetch('/api/server3')
])
    .then(response => {
        console.log('First successful response');
        return response.json();
    })
    .catch(error => {
        console.error('All requests failed:', error);
    });
```

---

## Error Handling

### Try-Catch-Finally
```javascript
// Basic try-catch
try {
    let result = riskyOperation();
    console.log('Success:', result);
} catch (error) {
    console.error('Error occurred:', error.message);
} finally {
    console.log('This always runs');
}

// Catching specific error types
try {
    JSON.parse('invalid json');
} catch (error) {
    if (error instanceof SyntaxError) {
        console.error('JSON syntax error:', error.message);
    } else if (error instanceof ReferenceError) {
        console.error('Reference error:', error.message);
    } else {
        console.error('Unknown error:', error.message);
    }
}

// Nested try-catch
try {
    try {
        let data = JSON.parse('{"name": "John"}');
        let result = processData(data);
    } catch (parseError) {
        console.error('Parse error:', parseError.message);
        throw new Error('Data processing failed');
    }
} catch (processingError) {
    console.error('Processing error:', processingError.message);
}
```

### Custom Errors
```javascript
// Creating custom error classes
class ValidationError extends Error {
    constructor(message, field) {
        super(message);
        this.name = 'ValidationError';
        this.field = field;
    }
}

class NetworkError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.name = 'NetworkError';
        this.statusCode = statusCode;
    }
}

// Using custom errors
function validateUser(user) {
    if (!user.name) {
        throw new ValidationError('Name is required', 'name');
    }
    if (!user.email) {
        throw new ValidationError('Email is required', 'email');
    }
    if (user.age < 0) {
        throw new ValidationError('Age must be positive', 'age');
    }
}

try {
    validateUser({ name: '', email: 'john@example.com', age: -5 });
} catch (error) {
    if (error instanceof ValidationError) {
        console.error(`Validation failed for ${error.field}: ${error.message}`);
    } else {
        console.error('Unexpected error:', error.message);
    }
}
```
### Error Handling Best Practices
```javascript
// Graceful error handling with fallbacks
function safeParseJSON(jsonString, fallback = null) {
    try {
        return JSON.parse(jsonString);
    } catch (error) {
        console.warn('Failed to parse JSON:', error.message);
        return fallback;
    }
}

// Error boundaries for async operations
async function safeAsyncOperation(operation) {
    try {
        return await operation();
    } catch (error) {
        console.error('Async operation failed:', error.message);
        return { error: true, message: error.message };
    }
}

// Global error handling
window.addEventListener('error', function(event) {
    console.error('Global error:', event.error);
    // Log to error reporting service
});

window.addEventListener('unhandledrejection', function(event) {
    console.error('Unhandled promise rejection:', event.reason);
    event.preventDefault(); // Prevent default browser behavior
});

// Defensive programming
function divide(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
        throw new TypeError('Both arguments must be numbers');
    }
    if (b === 0) {
        throw new Error('Division by zero is not allowed');
    }
    return a / b;
}

function safeDivide(a, b) {
    try {
        return divide(a, b);
    } catch (error) {
        console.error('Division failed:', error.message);
        return NaN;
    }
}
```

---

## Modern JavaScript (ES6+)

### Destructuring
```javascript
// Array destructuring
let colors = ['red', 'green', 'blue'];
let [first, second, third] = colors;
console.log(first); // 'red'

// Skip elements
let [primary, , tertiary] = colors;
console.log(primary, tertiary); // 'red', 'blue'

// Default values
let [a, b, c, d = 'yellow'] = colors;
console.log(d); // 'yellow'

// Rest operator
let [head, ...tail] = colors;
console.log(head); // 'red'
console.log(tail); // ['green', 'blue']

// Object destructuring
let person = { name: 'John', age: 30, city: 'New York' };
let { name, age } = person;
console.log(name, age); // 'John', 30

// Rename variables
let { name: fullName, age: years } = person;
console.log(fullName, years); // 'John', 30

// Default values
let { name: personName, country = 'USA' } = person;
console.log(personName, country); // 'John', 'USA'

// Nested destructuring
let user = {
    id: 1,
    profile: {
        name: 'Alice',
        settings: {
            theme: 'dark',
            notifications: true
        }
    }
};

let { profile: { name, settings: { theme } } } = user;
console.log(name, theme); // 'Alice', 'dark'

// Function parameter destructuring
function greetUser({ name, age = 'unknown' }) {
    console.log(`Hello ${name}, you are ${age} years old`);
}

greetUser({ name: 'Bob', age: 25 });
greetUser({ name: 'Charlie' });
```

### Template Literals
```javascript
let name = 'World';
let greeting = `Hello, ${name}!`;

// Multi-line strings
let html = `
    <div class="card">
        <h2>${name}</h2>
        <p>Welcome to our site!</p>
    </div>
`;

// Expression evaluation
let a = 5;
let b = 10;
let result = `The sum of ${a} and ${b} is ${a + b}`;

// Function calls in templates
function formatDate(date) {
    return date.toLocaleDateString();
}

let message = `Today is ${formatDate(new Date())}`;

// Tagged template literals
function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
        let value = values[i] ? `<mark>${values[i]}</mark>` : '';
        return result + string + value;
    }, '');
}

let searchTerm = 'JavaScript';
let text = highlight`Learn ${searchTerm} programming with ${name}`;
```

### Spread and Rest Operators
```javascript
// Spread with arrays
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]

// Copy array
let original = [1, 2, 3];
let copy = [...original];

// Spread with objects
let obj1 = { a: 1, b: 2 };
let obj2 = { c: 3, d: 4 };
let merged = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }

// Override properties
let defaults = { theme: 'light', size: 'medium' };
let userPrefs = { theme: 'dark' };
let settings = { ...defaults, ...userPrefs }; // { theme: 'dark', size: 'medium' }

// Spread in function calls
function sum(a, b, c) {
    return a + b + c;
}

let numbers = [1, 2, 3];
let result = sum(...numbers); // Same as sum(1, 2, 3)

// Rest parameters
function collectArgs(first, ...rest) {
    console.log('First:', first);
    console.log('Rest:', rest);
}

collectArgs(1, 2, 3, 4, 5); // First: 1, Rest: [2, 3, 4, 5]

// Rest in destructuring
let [head, ...tail] = [1, 2, 3, 4, 5];
console.log(head); // 1
console.log(tail); // [2, 3, 4, 5]

let { name, ...otherProps } = { name: 'John', age: 30, city: 'NYC' };
console.log(name); // 'John'
console.log(otherProps); // { age: 30, city: 'NYC' }
```
### Classes and Modules
```javascript
// ES6 Classes
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    // Method
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    // Getter
    get info() {
        return `${this.name} (${this.age} years old)`;
    }
    
    // Setter
    set age(value) {
        if (value < 0) {
            throw new Error('Age cannot be negative');
        }
        this._age = value;
    }
    
    get age() {
        return this._age;
    }
    
    // Static method
    static createAnonymous() {
        return new Person('Anonymous', 0);
    }
}

// Inheritance
class Student extends Person {
    constructor(name, age, grade) {
        super(name, age); // Call parent constructor
        this.grade = grade;
    }
    
    // Override method
    greet() {
        return `${super.greet()}, I'm a student in grade ${this.grade}`;
    }
    
    // New method
    study(subject) {
        return `${this.name} is studying ${subject}`;
    }
}

let student = new Student('Alice', 16, 10);
console.log(student.greet());
console.log(student.study('Math'));

// Private fields (ES2022+)
class BankAccount {
    #balance = 0; // Private field
    
    constructor(initialBalance) {
        this.#balance = initialBalance;
    }
    
    deposit(amount) {
        this.#balance += amount;
    }
    
    getBalance() {
        return this.#balance;
    }
    
    #validateAmount(amount) { // Private method
        return amount > 0;
    }
}

// ES6 Modules (export)
// math.js
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

export const PI = 3.14159;

export default function multiply(a, b) {
    return a * b;
}

// ES6 Modules (import)
// main.js
import multiply, { add, subtract, PI } from './math.js';
import * as math from './math.js';

console.log(add(5, 3)); // 8
console.log(multiply(4, 2)); // 8
console.log(math.PI); // 3.14159
```

---

## Best Practices

### Code Organization
```javascript
// Use meaningful variable names
// Bad
let d = new Date();
let u = users.filter(x => x.a);

// Good
let currentDate = new Date();
let activeUsers = users.filter(user => user.isActive);

// Use constants for magic numbers
// Bad
if (user.age >= 18) {
    // ...
}

// Good
const LEGAL_AGE = 18;
if (user.age >= LEGAL_AGE) {
    // ...
}

// Function should do one thing
// Bad
function processUserData(users) {
    // Validate users
    let validUsers = users.filter(user => user.email && user.name);
    
    // Sort users
    validUsers.sort((a, b) => a.name.localeCompare(b.name));
    
    // Send emails
    validUsers.forEach(user => sendEmail(user.email));
    
    return validUsers;
}

// Good
function validateUsers(users) {
    return users.filter(user => user.email && user.name);
}

function sortUsersByName(users) {
    return users.sort((a, b) => a.name.localeCompare(b.name));
}

function sendWelcomeEmails(users) {
    users.forEach(user => sendEmail(user.email));
}

function processUserData(users) {
    let validUsers = validateUsers(users);
    let sortedUsers = sortUsersByName(validUsers);
    sendWelcomeEmails(sortedUsers);
    return sortedUsers;
}
```

### Performance Tips
```javascript
// Use efficient array methods
// Bad - O(n²)
let found = false;
for (let i = 0; i < users.length; i++) {
    for (let j = 0; j < activeUsers.length; j++) {
        if (users[i].id === activeUsers[j].id) {
            found = true;
            break;
        }
    }
}

// Good - O(n)
let activeUserIds = new Set(activeUsers.map(user => user.id));
let found = users.some(user => activeUserIds.has(user.id));

// Debounce expensive operations
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

let searchInput = document.querySelector('#search');
let debouncedSearch = debounce(function(event) {
    performSearch(event.target.value);
}, 300);

searchInput.addEventListener('input', debouncedSearch);

// Use object lookup instead of switch/if chains
// Bad
function getStatusMessage(status) {
    if (status === 'pending') return 'Processing...';
    if (status === 'approved') return 'Approved!';
    if (status === 'rejected') return 'Rejected';
    return 'Unknown status';
}

// Good
const STATUS_MESSAGES = {
    pending: 'Processing...',
    approved: 'Approved!',
    rejected: 'Rejected'
};

function getStatusMessage(status) {
    return STATUS_MESSAGES[status] || 'Unknown status';
}
```

### Security Best Practices
```javascript
// Sanitize user input
function sanitizeHTML(str) {
    let div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
}

// Use textContent instead of innerHTML when possible
element.textContent = userInput; // Safe
// element.innerHTML = userInput; // Potentially unsafe

// Validate data types
function processAge(age) {
    if (typeof age !== 'number' || age < 0 || age > 150) {
        throw new Error('Invalid age');
    }
    return age;
}

// Use strict equality
if (userInput === 'admin') { // Good
    // ...
}

if (userInput == 'admin') { // Avoid - can be bypassed
    // ...
}
```

---

## Practice Projects

### Project 1: Interactive To-Do List
```javascript
// HTML structure needed:
// <input id="taskInput" type="text" placeholder="Enter a task">
// <button id="addBtn">Add Task</button>
// <ul id="taskList"></ul>

class TodoApp {
    constructor() {
        this.tasks = JSON.parse(localStorage.getItem('tasks')) || [];
        this.taskInput = document.getElementById('taskInput');
        this.addBtn = document.getElementById('addBtn');
        this.taskList = document.getElementById('taskList');
        
        this.init();
    }
    
    init() {
        this.renderTasks();
        this.addBtn.addEventListener('click', () => this.addTask());
        this.taskInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') this.addTask();
        });
        this.taskList.addEventListener('click', (e) => this.handleTaskClick(e));
    }
    
    addTask() {
        let text = this.taskInput.value.trim();
        if (!text) return;
        
        let task = {
            id: Date.now(),
            text: text,
            completed: false,
            createdAt: new Date().toISOString()
        };
        
        this.tasks.push(task);
        this.saveTasks();
        this.renderTasks();
        this.taskInput.value = '';
    }
    
    toggleTask(id) {
        let task = this.tasks.find(t => t.id === id);
        if (task) {
            task.completed = !task.completed;
            this.saveTasks();
            this.renderTasks();
        }
    }
    
    deleteTask(id) {
        this.tasks = this.tasks.filter(t => t.id !== id);
        this.saveTasks();
        this.renderTasks();
    }
    
    handleTaskClick(event) {
        let taskId = parseInt(event.target.dataset.taskId);
        
        if (event.target.classList.contains('toggle-btn')) {
            this.toggleTask(taskId);
        } else if (event.target.classList.contains('delete-btn')) {
            this.deleteTask(taskId);
        }
    }
    
    renderTasks() {
        this.taskList.innerHTML = '';
        
        this.tasks.forEach(task => {
            let li = document.createElement('li');
            li.className = task.completed ? 'completed' : '';
            li.innerHTML = `
                <span class="task-text">${task.text}</span>
                <button class="toggle-btn" data-task-id="${task.id}">
                    ${task.completed ? 'Undo' : 'Complete'}
                </button>
                <button class="delete-btn" data-task-id="${task.id}">Delete</button>
            `;
            this.taskList.appendChild(li);
        });
    }
    
    saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
}

// Initialize the app
let todoApp = new TodoApp();
```

### Project 2: Weather App with API
```javascript
class WeatherApp {
    constructor(apiKey) {
        this.apiKey = apiKey;
        this.searchInput = document.getElementById('cityInput');
        this.searchBtn = document.getElementById('searchBtn');
        this.weatherDisplay = document.getElementById('weatherDisplay');
        
        this.init();
    }
    
    init() {
        this.searchBtn.addEventListener('click', () => this.searchWeather());
        this.searchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') this.searchWeather();
        });
        
        // Get user's location on load
        this.getCurrentLocationWeather();
    }
    
    async searchWeather() {
        let city = this.searchInput.value.trim();
        if (!city) return;
        
        try {
            this.showLoading();
            let weatherData = await this.fetchWeatherByCity(city);
            this.displayWeather(weatherData);
        } catch (error) {
            this.showError('City not found or API error');
        }
    }
    
    async getCurrentLocationWeather() {
        if (!navigator.geolocation) return;
        
        navigator.geolocation.getCurrentPosition(
            async (position) => {
                try {
                    let { latitude, longitude } = position.coords;
                    let weatherData = await this.fetchWeatherByCoords(latitude, longitude);
                    this.displayWeather(weatherData);
                } catch (error) {
                    console.error('Error getting location weather:', error);
                }
            },
            (error) => {
                console.error('Geolocation error:', error);
            }
        );
    }
    
    async fetchWeatherByCity(city) {
        let url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${this.apiKey}&units=metric`;
        let response = await fetch(url);
        
        if (!response.ok) {
            throw new Error('Weather data not found');
        }
        
        return await response.json();
    }
    
    async fetchWeatherByCoords(lat, lon) {
        let url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=metric`;
        let response = await fetch(url);
        
        if (!response.ok) {
            throw new Error('Weather data not found');
        }
        
        return await response.json();
    }
    
    displayWeather(data) {
        let html = `
            <div class="weather-card">
                <h2>${data.name}, ${data.sys.country}</h2>
                <div class="temperature">${Math.round(data.main.temp)}°C</div>
                <div class="description">${data.weather[0].description}</div>
                <div class="details">
                    <p>Feels like: ${Math.round(data.main.feels_like)}°C</p>
                    <p>Humidity: ${data.main.humidity}%</p>
                    <p>Wind: ${data.wind.speed} m/s</p>
                </div>
            </div>
        `;
        
        this.weatherDisplay.innerHTML = html;
    }
    
    showLoading() {
        this.weatherDisplay.innerHTML = '<div class="loading">Loading...</div>';
    }
    
    showError(message) {
        this.weatherDisplay.innerHTML = `<div class="error">${message}</div>`;
    }
}

// Initialize with your API key
// let weatherApp = new WeatherApp('YOUR_API_KEY');
```

This completes the comprehensive JavaScript fundamentals guide. The file now covers all essential JavaScript concepts from basic syntax to advanced topics like async/await, modern ES6+ features, and practical projects. Each section includes detailed explanations, code examples, and best practices to help learners master JavaScript effectively.