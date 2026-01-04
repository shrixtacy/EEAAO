# Web Development Fundamentals - Complete Learning Guide

This comprehensive guide covers HTML, CSS, and JavaScript fundamentals with practical examples and mini-projects. Master these core technologies before moving to frameworks.

## Table of Contents

1. [HTML Fundamentals](#html-fundamentals)
2. [CSS Fundamentals](#css-fundamentals)
3. [JavaScript Fundamentals](#javascript-fundamentals)
4. [Integration Projects](#integration-projects)
5. [Next Steps](#next-steps)

---

## HTML Fundamentals

### What is HTML?

HTML (HyperText Markup Language) is the standard markup language for creating web pages. It describes the structure and content of web pages using elements and tags.

### Basic HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Web Page</title>
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>This is my first paragraph.</p>
</body>
</html>
```

### Essential HTML Elements

#### Text Elements
```html
<!-- Headings (h1 is most important, h6 is least) -->
<h1>Main Title</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>

<!-- Paragraphs and text formatting -->
<p>This is a paragraph with <strong>bold text</strong> and <em>italic text</em>.</p>
<p>You can also use <b>bold</b> and <i>italic</i> for styling only.</p>

<!-- Line breaks and horizontal rules -->
<p>First line<br>Second line</p>
<hr>
```
#### Lists
```html
<!-- Unordered (bulleted) list -->
<ul>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>

<!-- Ordered (numbered) list -->
<ol>
    <li>Step one</li>
    <li>Step two</li>
    <li>Step three</li>
</ol>

<!-- Description list -->
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
</dl>
```

#### Links and Images
```html
<!-- Links -->
<a href="https://example.com">External link</a>
<a href="about.html">Internal link</a>
<a href="#section1">Link to section on same page</a>
<a href="mailto:someone@example.com">Email link</a>

<!-- Images -->
<img src="image.jpg" alt="Description of image" width="300" height="200">
<img src="https://example.com/image.png" alt="Remote image">

<!-- Image with link -->
<a href="https://example.com">
    <img src="logo.png" alt="Company logo">
</a>
```

#### Tables
```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
            <th>City</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John Doe</td>
            <td>30</td>
            <td>New York</td>
        </tr>
        <tr>
            <td>Jane Smith</td>
            <td>25</td>
            <td>Los Angeles</td>
        </tr>
    </tbody>
</table>
```
#### Forms
```html
<form action="/submit" method="POST">
    <!-- Text inputs -->
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required>
    
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>
    
    <!-- Textarea -->
    <label for="message">Message:</label>
    <textarea id="message" name="message" rows="4" cols="50"></textarea>
    
    <!-- Select dropdown -->
    <label for="country">Country:</label>
    <select id="country" name="country">
        <option value="us">United States</option>
        <option value="ca">Canada</option>
        <option value="uk">United Kingdom</option>
    </select>
    
    <!-- Radio buttons -->
    <fieldset>
        <legend>Gender:</legend>
        <input type="radio" id="male" name="gender" value="male">
        <label for="male">Male</label>
        
        <input type="radio" id="female" name="gender" value="female">
        <label for="female">Female</label>
    </fieldset>
    
    <!-- Checkboxes -->
    <input type="checkbox" id="newsletter" name="newsletter" value="yes">
    <label for="newsletter">Subscribe to newsletter</label>
    
    <!-- Submit button -->
    <button type="submit">Submit</button>
</form>
```

### Semantic HTML Elements

Semantic elements provide meaning to your content and improve accessibility:

```html
<header>
    <nav>
        <ul>
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>
</header>

<main>
    <article>
        <header>
            <h1>Article Title</h1>
            <time datetime="2024-01-15">January 15, 2024</time>
        </header>
        
        <section>
            <h2>Introduction</h2>
            <p>Article introduction...</p>
        </section>
        
        <section>
            <h2>Main Content</h2>
            <p>Main article content...</p>
        </section>
    </article>
    
    <aside>
        <h3>Related Articles</h3>
        <ul>
            <li><a href="#">Related Article 1</a></li>
            <li><a href="#">Related Article 2</a></li>
        </ul>
    </aside>
</main>

<footer>
    <p>&copy; 2024 My Website. All rights reserved.</p>
</footer>
```
### HTML Attributes

Common attributes that enhance HTML elements:

```html
<!-- Global attributes (work on any element) -->
<div id="unique-identifier" class="css-class-name" title="Tooltip text">
    Content with global attributes
</div>

<!-- Data attributes for JavaScript -->
<button data-action="save" data-id="123">Save</button>

<!-- Accessibility attributes -->
<img src="chart.png" alt="Sales increased 25% this quarter" role="img">
<button aria-label="Close dialog" aria-expanded="false">×</button>

<!-- Link attributes -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    External link (opens in new tab)
</a>
```

### Mini-Project 1: Personal Profile Page

Create a complete HTML page with your personal information:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>John Doe - Personal Profile</title>
</head>
<body>
    <header>
        <h1>John Doe</h1>
        <p>Web Developer & Designer</p>
    </header>
    
    <nav>
        <ul>
            <li><a href="#about">About</a></li>
            <li><a href="#skills">Skills</a></li>
            <li><a href="#projects">Projects</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>
    
    <main>
        <section id="about">
            <h2>About Me</h2>
            <img src="profile.jpg" alt="John Doe profile picture" width="200">
            <p>I'm a passionate web developer with 3 years of experience...</p>
        </section>
        
        <section id="skills">
            <h2>Skills</h2>
            <ul>
                <li>HTML5 & CSS3</li>
                <li>JavaScript</li>
                <li>React</li>
                <li>Node.js</li>
            </ul>
        </section>
        
        <section id="projects">
            <h2>Projects</h2>
            <article>
                <h3>E-commerce Website</h3>
                <p>Built a full-stack e-commerce platform...</p>
                <a href="https://github.com/johndoe/ecommerce">View on GitHub</a>
            </article>
        </section>
        
        <section id="contact">
            <h2>Contact Me</h2>
            <form>
                <label for="name">Name:</label>
                <input type="text" id="name" name="name" required>
                
                <label for="email">Email:</label>
                <input type="email" id="email" name="email" required>
                
                <label for="message">Message:</label>
                <textarea id="message" name="message" required></textarea>
                
                <button type="submit">Send Message</button>
            </form>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2024 John Doe. All rights reserved.</p>
    </footer>
</body>
</html>
```

---

## CSS Fundamentals

### What is CSS?

CSS (Cascading Style Sheets) controls the presentation and layout of HTML elements. It separates content from design, making websites more maintainable and flexible.
### CSS Syntax and Selectors

```css
/* Basic syntax: selector { property: value; } */
h1 {
    color: blue;
    font-size: 24px;
}

/* Element selector */
p {
    line-height: 1.6;
}

/* Class selector */
.highlight {
    background-color: yellow;
    padding: 10px;
}

/* ID selector */
#header {
    background-color: #333;
    color: white;
}

/* Descendant selector */
nav ul li {
    list-style: none;
}

/* Child selector */
nav > ul {
    margin: 0;
}

/* Pseudo-classes */
a:hover {
    color: red;
}

a:visited {
    color: purple;
}

button:focus {
    outline: 2px solid blue;
}

/* Pseudo-elements */
p::first-line {
    font-weight: bold;
}

p::before {
    content: "→ ";
    color: blue;
}
```

### CSS Box Model

Every HTML element is a rectangular box with four areas:

```css
.box-example {
    /* Content area */
    width: 200px;
    height: 100px;
    
    /* Padding (space inside the element) */
    padding: 20px;
    
    /* Border */
    border: 2px solid #333;
    
    /* Margin (space outside the element) */
    margin: 10px;
    
    /* Box-sizing controls how width/height are calculated */
    box-sizing: border-box; /* Includes padding and border in width/height */
}

/* Visual example */
.box-model-demo {
    width: 300px;
    height: 200px;
    padding: 20px;
    border: 5px solid red;
    margin: 15px;
    background-color: lightblue;
}
```
### Typography and Text Styling

```css
/* Font properties */
.text-styling {
    font-family: 'Arial', 'Helvetica', sans-serif;
    font-size: 16px;
    font-weight: bold; /* or 100-900 */
    font-style: italic;
    line-height: 1.5;
    letter-spacing: 1px;
    word-spacing: 2px;
}

/* Text alignment and decoration */
.text-alignment {
    text-align: center; /* left, right, center, justify */
    text-decoration: underline; /* none, underline, line-through */
    text-transform: uppercase; /* lowercase, capitalize, uppercase */
}

/* Colors */
.colors {
    color: #333; /* Hex color */
    color: rgb(51, 51, 51); /* RGB */
    color: rgba(51, 51, 51, 0.8); /* RGBA with transparency */
    color: hsl(0, 0%, 20%); /* HSL */
    background-color: #f0f0f0;
}
```

### Layout with Flexbox

Flexbox is perfect for one-dimensional layouts:

```css
/* Flex container */
.flex-container {
    display: flex;
    
    /* Direction */
    flex-direction: row; /* row, column, row-reverse, column-reverse */
    
    /* Main axis alignment */
    justify-content: center; /* flex-start, flex-end, center, space-between, space-around */
    
    /* Cross axis alignment */
    align-items: center; /* flex-start, flex-end, center, stretch, baseline */
    
    /* Wrap */
    flex-wrap: wrap; /* nowrap, wrap, wrap-reverse */
    
    /* Gap between items */
    gap: 20px;
}

/* Flex items */
.flex-item {
    flex: 1; /* flex-grow: 1, flex-shrink: 1, flex-basis: 0% */
    
    /* Or specify individually */
    flex-grow: 1;
    flex-shrink: 0;
    flex-basis: 200px;
}

/* Practical flexbox examples */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
}

.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

.card {
    flex: 1 1 300px; /* grow, shrink, basis */
    min-width: 250px;
}
```
### Layout with CSS Grid

CSS Grid is perfect for two-dimensional layouts:

```css
/* Grid container */
.grid-container {
    display: grid;
    
    /* Define columns and rows */
    grid-template-columns: 1fr 2fr 1fr; /* 3 columns: 1 part, 2 parts, 1 part */
    grid-template-rows: auto 1fr auto; /* 3 rows: auto height, flexible, auto height */
    
    /* Or use repeat */
    grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Responsive columns */
    
    /* Gap between grid items */
    gap: 1rem;
    
    /* Grid areas (optional) */
    grid-template-areas: 
        "header header header"
        "sidebar main main"
        "footer footer footer";
}

/* Grid items */
.grid-item {
    /* Position by line numbers */
    grid-column: 1 / 3; /* Start at line 1, end at line 3 */
    grid-row: 2 / 4;
    
    /* Or use span */
    grid-column: span 2; /* Span 2 columns */
    
    /* Or use grid areas */
    grid-area: header;
}

/* Practical grid example */
.page-layout {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    min-height: 100vh;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }
```

### Responsive Design

Make your website work on all devices:

```css
/* Mobile-first approach */
.responsive-container {
    width: 100%;
    padding: 1rem;
}

/* Tablet styles */
@media (min-width: 768px) {
    .responsive-container {
        max-width: 750px;
        margin: 0 auto;
        padding: 2rem;
    }
}

/* Desktop styles */
@media (min-width: 1024px) {
    .responsive-container {
        max-width: 1200px;
        padding: 3rem;
    }
}

/* Common breakpoints */
/* Mobile: up to 767px */
/* Tablet: 768px to 1023px */
/* Desktop: 1024px and up */

/* Responsive images */
.responsive-image {
    max-width: 100%;
    height: auto;
}

/* Responsive typography */
.responsive-text {
    font-size: clamp(1rem, 2.5vw, 2rem); /* min, preferred, max */
}
```
### Mini-Project 2: Responsive Card Layout

Create a responsive card layout using Flexbox and Grid:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Card Layout</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }
        
        .header {
            text-align: center;
            margin-bottom: 3rem;
        }
        
        .card-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
        }
        
        .card {
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            overflow: hidden;
            transition: transform 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
        }
        
        .card-image {
            width: 100%;
            height: 200px;
            background: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
        }
        
        .card-content {
            padding: 1.5rem;
        }
        
        .card-title {
            font-size: 1.25rem;
            margin-bottom: 0.5rem;
            color: #2c3e50;
        }
        
        .card-description {
            color: #7f8c8d;
            margin-bottom: 1rem;
        }
        
        .card-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .btn {
            background: #3498db;
            color: white;
            padding: 0.5rem 1rem;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            text-decoration: none;
            display: inline-block;
        }
        
        .btn:hover {
            background: #2980b9;
        }
        
        @media (max-width: 768px) {
            .container {
                padding: 1rem;
            }
            
            .card-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="header">
            <h1>Our Services</h1>
            <p>Discover what we can do for you</p>
        </header>
        
        <div class="card-grid">
            <div class="card">
                <div class="card-image"></div>
                <div class="card-content">
                    <h3 class="card-title">Web Development</h3>
                    <p class="card-description">Custom websites and web applications built with modern technologies.</p>
                    <div class="card-footer">
                        <span class="price">$2,500+</span>
                        <a href="#" class="btn">Learn More</a>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <div class="card-image"></div>
                <div class="card-content">
                    <h3 class="card-title">Mobile Apps</h3>
                    <p class="card-description">Native and cross-platform mobile applications for iOS and Android.</p>
                    <div class="card-footer">
                        <span class="price">$5,000+</span>
                        <a href="#" class="btn">Learn More</a>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <div class="card-image"></div>
                <div class="card-content">
                    <h3 class="card-title">UI/UX Design</h3>
                    <p class="card-description">Beautiful and user-friendly interface designs that convert.</p>
                    <div class="card-footer">
                        <span class="price">$1,500+</span>
                        <a href="#" class="btn">Learn More</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

---

## JavaScript Fundamentals

### What is JavaScript?

JavaScript is a programming language that adds interactivity and dynamic behavior to web pages. It runs in the browser and can manipulate HTML and CSS in real-time.
### Variables and Data Types

```javascript
// Variable declarations
let name = "John"; // Can be reassigned
const age = 30; // Cannot be reassigned
var city = "New York"; // Older syntax, avoid in modern code

// Data types
let string = "Hello World";
let number = 42;
let boolean = true;
let array = [1, 2, 3, "four", true];
let object = {
    name: "John",
    age: 30,
    isStudent: false
};
let nullValue = null;
let undefinedValue = undefined;

// Type checking
console.log(typeof string); // "string"
console.log(typeof number); // "number"
console.log(typeof boolean); // "boolean"
console.log(typeof array); // "object"
console.log(typeof object); // "object"
console.log(Array.isArray(array)); // true
```

### Functions

```javascript
// Function declaration
function greet(name) {
    return `Hello, ${name}!`;
}

// Function expression
const greetExpression = function(name) {
    return `Hello, ${name}!`;
};

// Arrow function (ES6+)
const greetArrow = (name) => {
    return `Hello, ${name}!`;
};

// Short arrow function
const greetShort = name => `Hello, ${name}!`;

// Function with multiple parameters
const add = (a, b) => a + b;

// Function with default parameters
const greetWithDefault = (name = "World") => `Hello, ${name}!`;

// Using functions
console.log(greet("Alice")); // "Hello, Alice!"
console.log(add(5, 3)); // 8
console.log(greetWithDefault()); // "Hello, World!"
```

### Control Structures

```javascript
// If statements
const score = 85;

if (score >= 90) {
    console.log("A grade");
} else if (score >= 80) {
    console.log("B grade");
} else if (score >= 70) {
    console.log("C grade");
} else {
    console.log("Need improvement");
}

// Ternary operator
const result = score >= 70 ? "Pass" : "Fail";

// Switch statement
const day = "Monday";

switch (day) {
    case "Monday":
        console.log("Start of work week");
        break;
    case "Friday":
        console.log("TGIF!");
        break;
    case "Saturday":
    case "Sunday":
        console.log("Weekend!");
        break;
    default:
        console.log("Regular day");
}

// Loops
// For loop
for (let i = 0; i < 5; i++) {
    console.log(`Count: ${i}`);
}

// While loop
let count = 0;
while (count < 3) {
    console.log(`While count: ${count}`);
    count++;
}

// For...of loop (arrays)
const fruits = ["apple", "banana", "orange"];
for (const fruit of fruits) {
    console.log(fruit);
}

// For...in loop (objects)
const person = { name: "John", age: 30, city: "NYC" };
for (const key in person) {
    console.log(`${key}: ${person[key]}`);
}
```
### Arrays and Array Methods

```javascript
// Creating arrays
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, "hello", true, { name: "John" }];

// Array methods
const fruits = ["apple", "banana", "orange"];

// Add/remove elements
fruits.push("grape"); // Add to end
fruits.unshift("mango"); // Add to beginning
const lastFruit = fruits.pop(); // Remove from end
const firstFruit = fruits.shift(); // Remove from beginning

// Find elements
const index = fruits.indexOf("banana"); // Find index
const exists = fruits.includes("apple"); // Check if exists

// Transform arrays
const upperFruits = fruits.map(fruit => fruit.toUpperCase());
const longFruits = fruits.filter(fruit => fruit.length > 5);
const totalLength = fruits.reduce((sum, fruit) => sum + fruit.length, 0);

// Iterate arrays
fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});

// Array destructuring
const [first, second, ...rest] = fruits;
console.log(first); // First element
console.log(rest); // Remaining elements
```

### Objects and Object Methods

```javascript
// Creating objects
const person = {
    name: "John Doe",
    age: 30,
    city: "New York",
    hobbies: ["reading", "coding", "hiking"],
    
    // Methods
    greet() {
        return `Hello, I'm ${this.name}`;
    },
    
    getAge: function() {
        return this.age;
    },
    
    // Arrow functions don't have their own 'this'
    getBirthYear: () => {
        return new Date().getFullYear() - this.age; // 'this' won't work here
    }
};

// Accessing properties
console.log(person.name); // Dot notation
console.log(person["age"]); // Bracket notation
console.log(person.greet()); // Call method

// Adding/modifying properties
person.email = "john@example.com";
person.age = 31;

// Object destructuring
const { name, age, city } = person;
console.log(name); // "John Doe"

// Object methods
const keys = Object.keys(person); // Get all keys
const values = Object.values(person); // Get all values
const entries = Object.entries(person); // Get key-value pairs

// Copying objects
const personCopy = { ...person }; // Shallow copy
const personCopy2 = Object.assign({}, person); // Alternative shallow copy
```
### DOM Manipulation

The Document Object Model (DOM) represents the HTML structure as JavaScript objects:

```javascript
// Selecting elements
const elementById = document.getElementById("myId");
const elementsByClass = document.getElementsByClassName("myClass");
const elementsByTag = document.getElementsByTagName("p");

// Modern selectors (preferred)
const element = document.querySelector("#myId"); // First match
const elements = document.querySelectorAll(".myClass"); // All matches

// Modifying content
element.textContent = "New text content";
element.innerHTML = "<strong>Bold text</strong>";

// Modifying attributes
element.setAttribute("class", "new-class");
element.getAttribute("id");
element.removeAttribute("title");

// Modifying styles
element.style.color = "red";
element.style.backgroundColor = "yellow";
element.style.fontSize = "20px";

// Adding/removing CSS classes
element.classList.add("active");
element.classList.remove("inactive");
element.classList.toggle("highlight");
element.classList.contains("active"); // Returns boolean

// Creating and adding elements
const newElement = document.createElement("div");
newElement.textContent = "I'm a new element";
newElement.className = "new-element";

const parent = document.querySelector("#parent");
parent.appendChild(newElement);
parent.insertBefore(newElement, parent.firstChild);

// Removing elements
element.remove(); // Modern way
parent.removeChild(element); // Older way
```

### Event Handling

```javascript
// Adding event listeners
const button = document.querySelector("#myButton");

// Click event
button.addEventListener("click", function(event) {
    console.log("Button clicked!");
    console.log(event.target); // The clicked element
});

// Arrow function event handler
button.addEventListener("click", (event) => {
    event.preventDefault(); // Prevent default behavior
    console.log("Button clicked with arrow function!");
});

// Multiple event types
const input = document.querySelector("#myInput");

input.addEventListener("focus", () => {
    console.log("Input focused");
});

input.addEventListener("blur", () => {
    console.log("Input lost focus");
});

input.addEventListener("input", (event) => {
    console.log("Input value:", event.target.value);
});

// Form handling
const form = document.querySelector("#myForm");

form.addEventListener("submit", (event) => {
    event.preventDefault(); // Prevent form submission
    
    const formData = new FormData(form);
    const data = Object.fromEntries(formData);
    console.log("Form data:", data);
});

// Event delegation (for dynamic content)
document.addEventListener("click", (event) => {
    if (event.target.matches(".dynamic-button")) {
        console.log("Dynamic button clicked!");
    }
});
```
### Asynchronous JavaScript

```javascript
// Promises
const fetchData = () => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const success = Math.random() > 0.5;
            if (success) {
                resolve("Data fetched successfully!");
            } else {
                reject("Failed to fetch data");
            }
        }, 1000);
    });
};

// Using promises
fetchData()
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error(error);
    });

// Async/await (modern approach)
const getData = async () => {
    try {
        const data = await fetchData();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
};

// Fetch API for HTTP requests
const fetchUserData = async (userId) => {
    try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const user = await response.json();
        return user;
    } catch (error) {
        console.error("Error fetching user:", error);
        throw error;
    }
};

// Using the fetch function
fetchUserData(1)
    .then(user => {
        console.log("User:", user);
    })
    .catch(error => {
        console.error("Failed to get user:", error);
    });
```

### Mini-Project 3: Interactive Todo List

Create a fully functional todo list with JavaScript:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Todo List</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 2rem;
            background-color: #f5f5f5;
        }
        
        .container {
            background: white;
            padding: 2rem;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        
        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 2rem;
        }
        
        .input-section {
            display: flex;
            gap: 1rem;
            margin-bottom: 2rem;
        }
        
        #todoInput {
            flex: 1;
            padding: 0.75rem;
            border: 2px solid #ddd;
            border-radius: 4px;
            font-size: 1rem;
        }
        
        #todoInput:focus {
            outline: none;
            border-color: #007bff;
        }
        
        #addBtn {
            padding: 0.75rem 1.5rem;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 1rem;
        }
        
        #addBtn:hover {
            background: #0056b3;
        }
        
        .filters {
            display: flex;
            gap: 1rem;
            margin-bottom: 1rem;
        }
        
        .filter-btn {
            padding: 0.5rem 1rem;
            border: 1px solid #ddd;
            background: white;
            cursor: pointer;
            border-radius: 4px;
        }
        
        .filter-btn.active {
            background: #007bff;
            color: white;
        }
        
        #todoList {
            list-style: none;
        }
        
        .todo-item {
            display: flex;
            align-items: center;
            padding: 1rem;
            border: 1px solid #eee;
            margin-bottom: 0.5rem;
            border-radius: 4px;
            background: white;
        }
        
        .todo-item.completed {
            opacity: 0.6;
            text-decoration: line-through;
        }
        
        .todo-checkbox {
            margin-right: 1rem;
        }
        
        .todo-text {
            flex: 1;
        }
        
        .delete-btn {
            background: #dc3545;
            color: white;
            border: none;
            padding: 0.25rem 0.5rem;
            border-radius: 4px;
            cursor: pointer;
        }
        
        .delete-btn:hover {
            background: #c82333;
        }
        
        .empty-state {
            text-align: center;
            color: #666;
            font-style: italic;
            padding: 2rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>My Todo List</h1>
        
        <div class="input-section">
            <input type="text" id="todoInput" placeholder="Add a new todo..." maxlength="100">
            <button id="addBtn">Add Todo</button>
        </div>
        
        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>
        
        <ul id="todoList">
            <li class="empty-state">No todos yet. Add one above!</li>
        </ul>
    </div>

    <script>
        class TodoApp {
            constructor() {
                this.todos = JSON.parse(localStorage.getItem('todos')) || [];
                this.currentFilter = 'all';
                this.init();
            }
            
            init() {
                this.bindEvents();
                this.render();
            }
            
            bindEvents() {
                const addBtn = document.getElementById('addBtn');
                const todoInput = document.getElementById('todoInput');
                const filterBtns = document.querySelectorAll('.filter-btn');
                
                addBtn.addEventListener('click', () => this.addTodo());
                todoInput.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') this.addTodo();
                });
                
                filterBtns.forEach(btn => {
                    btn.addEventListener('click', (e) => {
                        this.setFilter(e.target.dataset.filter);
                    });
                });
                
                // Event delegation for todo items
                document.getElementById('todoList').addEventListener('click', (e) => {
                    if (e.target.classList.contains('todo-checkbox')) {
                        this.toggleTodo(parseInt(e.target.dataset.id));
                    } else if (e.target.classList.contains('delete-btn')) {
                        this.deleteTodo(parseInt(e.target.dataset.id));
                    }
                });
            }
            
            addTodo() {
                const input = document.getElementById('todoInput');
                const text = input.value.trim();
                
                if (text === '') {
                    alert('Please enter a todo item!');
                    return;
                }
                
                const todo = {
                    id: Date.now(),
                    text: text,
                    completed: false,
                    createdAt: new Date().toISOString()
                };
                
                this.todos.push(todo);
                input.value = '';
                this.saveTodos();
                this.render();
            }
            
            toggleTodo(id) {
                const todo = this.todos.find(t => t.id === id);
                if (todo) {
                    todo.completed = !todo.completed;
                    this.saveTodos();
                    this.render();
                }
            }
            
            deleteTodo(id) {
                if (confirm('Are you sure you want to delete this todo?')) {
                    this.todos = this.todos.filter(t => t.id !== id);
                    this.saveTodos();
                    this.render();
                }
            }
            
            setFilter(filter) {
                this.currentFilter = filter;
                
                // Update active filter button
                document.querySelectorAll('.filter-btn').forEach(btn => {
                    btn.classList.remove('active');
                });
                document.querySelector(`[data-filter="${filter}"]`).classList.add('active');
                
                this.render();
            }
            
            getFilteredTodos() {
                switch (this.currentFilter) {
                    case 'active':
                        return this.todos.filter(todo => !todo.completed);
                    case 'completed':
                        return this.todos.filter(todo => todo.completed);
                    default:
                        return this.todos;
                }
            }
            
            render() {
                const todoList = document.getElementById('todoList');
                const filteredTodos = this.getFilteredTodos();
                
                if (filteredTodos.length === 0) {
                    todoList.innerHTML = '<li class="empty-state">No todos to show!</li>';
                    return;
                }
                
                todoList.innerHTML = filteredTodos.map(todo => `
                    <li class="todo-item ${todo.completed ? 'completed' : ''}">
                        <input type="checkbox" class="todo-checkbox" 
                               data-id="${todo.id}" ${todo.completed ? 'checked' : ''}>
                        <span class="todo-text">${this.escapeHtml(todo.text)}</span>
                        <button class="delete-btn" data-id="${todo.id}">Delete</button>
                    </li>
                `).join('');
            }
            
            saveTodos() {
                localStorage.setItem('todos', JSON.stringify(this.todos));
            }
            
            escapeHtml(text) {
                const div = document.createElement('div');
                div.textContent = text;
                return div.innerHTML;
            }
        }
        
        // Initialize the app
        new TodoApp();
    </script>
</body>
</html>
```

---

## Integration Projects

Now that you've learned HTML, CSS, and JavaScript fundamentals, here are some projects that combine all three:

### Project 1: Personal Portfolio Website
- **HTML**: Semantic structure with header, nav, main sections, footer
- **CSS**: Responsive design with Flexbox/Grid, animations, mobile-first approach
- **JavaScript**: Smooth scrolling, form validation, dynamic content loading

### Project 2: Weather App
- **HTML**: Form for city input, sections for weather display
- **CSS**: Card-based layout, loading states, responsive design
- **JavaScript**: Fetch weather data from API, DOM manipulation, error handling

### Project 3: E-commerce Product Page
- **HTML**: Product images, descriptions, reviews, shopping cart
- **CSS**: Image gallery, product cards, shopping cart styling
- **JavaScript**: Image carousel, add to cart functionality, local storage

### Project 4: Blog Website
- **HTML**: Article structure, navigation, sidebar
- **CSS**: Typography, reading experience, responsive layout
- **JavaScript**: Search functionality, comment system, reading progress

---

## Next Steps

### 🎯 Immediate Next Steps (Week 1-2)
1. **Practice Daily**: Build one small project each day
2. **Master the Basics**: Ensure you're comfortable with all concepts above
3. **Use Developer Tools**: Learn Chrome DevTools for debugging
4. **Version Control**: Learn Git and GitHub basics

### 🚀 Intermediate Goals (Month 1-2)
1. **Choose a Framework**: Learn React (most popular) or Vue.js
2. **Build Bigger Projects**: Create 2-3 substantial applications
3. **Learn Build Tools**: Understand npm, webpack, or Vite
4. **Responsive Design**: Master mobile-first development

### 🏆 Advanced Goals (Month 3-6)
1. **Backend Integration**: Learn Node.js or connect to APIs
2. **Testing**: Write unit tests for your JavaScript code
3. **Performance**: Optimize loading times and user experience
4. **Deployment**: Deploy projects to production (Netlify, Vercel)

### 📚 Recommended Learning Resources
- **MDN Web Docs**: Comprehensive reference for HTML, CSS, JavaScript
- **JavaScript.info**: In-depth JavaScript tutorial
- **CSS-Tricks**: CSS techniques and best practices
- **FreeCodeCamp**: Interactive coding challenges
- **The Odin Project**: Full curriculum with projects

### 🛠️ Essential Tools to Learn
- **VS Code**: Code editor with extensions
- **Chrome DevTools**: Debugging and performance analysis
- **Git/GitHub**: Version control and collaboration
- **npm**: Package manager for JavaScript
- **Figma**: Design tool for UI/UX mockups

Remember: The key to mastering web development is consistent practice and building real projects. Don't just read about these concepts—implement them in your own projects!