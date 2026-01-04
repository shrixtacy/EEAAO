# CSS Fundamentals - Complete Learning Guide

Master CSS (Cascading Style Sheets) from basic styling to advanced layout techniques. This comprehensive guide covers everything you need to create beautiful, responsive web designs.

## Table of Contents

1. [What is CSS?](#what-is-css)
2. [CSS Syntax and Selectors](#css-syntax-and-selectors)
3. [The Box Model](#the-box-model)
4. [Colors and Typography](#colors-and-typography)
5. [Layout Fundamentals](#layout-fundamentals)
6. [Flexbox Layout](#flexbox-layout)
7. [CSS Grid Layout](#css-grid-layout)
8. [Responsive Design](#responsive-design)
9. [Animations and Transitions](#animations-and-transitions)
10. [CSS Best Practices](#css-best-practices)
11. [Practice Projects](#practice-projects)

---

## What is CSS?

CSS (Cascading Style Sheets) controls the presentation and layout of HTML elements. It separates content from design, making websites more maintainable and flexible.

### Key Concepts:
- **Selectors**: Target HTML elements to style
- **Properties**: Define what aspect to style (color, font-size, etc.)
- **Values**: Specify how to style the property
- **Cascade**: How styles are applied when multiple rules conflict
- **Inheritance**: How child elements inherit parent styles
- **Specificity**: Which styles take precedence

### Why CSS Matters:
- **Visual Design**: Makes websites beautiful and engaging
- **User Experience**: Improves readability and usability
- **Responsive Design**: Adapts to different screen sizes
- **Performance**: Efficient styling reduces load times
- **Maintainability**: Separates design from content

---

## CSS Syntax and Selectors

### Basic Syntax
```css
/* Basic syntax: selector { property: value; } */
h1 {
    color: blue;
    font-size: 24px;
    margin-bottom: 16px;
}

/* Multiple selectors */
h1, h2, h3 {
    font-family: Arial, sans-serif;
    color: #333;
}

/* Comments */
/* This is a single-line comment */

/*
This is a
multi-line comment
*/
```

### Element Selectors
```css
/* Type selectors */
p {
    line-height: 1.6;
    margin-bottom: 1em;
}

h1 {
    font-size: 2.5rem;
    font-weight: bold;
}

/* Universal selector */
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
```
### Class and ID Selectors
```css
/* Class selectors (can be used multiple times) */
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

.button {
    display: inline-block;
    padding: 12px 24px;
    background-color: #007bff;
    color: white;
    text-decoration: none;
    border-radius: 4px;
    transition: background-color 0.3s ease;
}

.button:hover {
    background-color: #0056b3;
}

/* ID selectors (should be unique) */
#header {
    background-color: #f8f9fa;
    padding: 20px 0;
    border-bottom: 1px solid #dee2e6;
}

#navigation {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
```

### Attribute Selectors
```css
/* Attribute exists */
input[required] {
    border-color: #dc3545;
}

/* Exact attribute value */
input[type="email"] {
    background-image: url('email-icon.svg');
}

/* Attribute contains value */
a[href*="mailto"] {
    color: #28a745;
}

/* Attribute starts with value */
a[href^="https"] {
    padding-left: 20px;
    background: url('external-link.svg') no-repeat left center;
}

/* Attribute ends with value */
a[href$=".pdf"] {
    padding-left: 20px;
    background: url('pdf-icon.svg') no-repeat left center;
}

/* Attribute contains word */
img[alt~="logo"] {
    max-width: 200px;
}
```

### Pseudo-classes and Pseudo-elements
```css
/* Link states */
a:link { color: #007bff; }
a:visited { color: #6f42c1; }
a:hover { color: #0056b3; text-decoration: underline; }
a:active { color: #004085; }
a:focus { outline: 2px solid #007bff; }

/* Form states */
input:focus {
    border-color: #007bff;
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

input:valid {
    border-color: #28a745;
}

input:invalid {
    border-color: #dc3545;
}

input:disabled {
    background-color: #e9ecef;
    opacity: 0.6;
}

/* Structural pseudo-classes */
li:first-child {
    margin-top: 0;
}

li:last-child {
    margin-bottom: 0;
}

tr:nth-child(even) {
    background-color: #f8f9fa;
}

tr:nth-child(odd) {
    background-color: white;
}

p:nth-of-type(2) {
    font-weight: bold;
}

/* Pseudo-elements */
p::first-line {
    font-weight: bold;
    font-size: 1.1em;
}

p::first-letter {
    font-size: 3em;
    float: left;
    line-height: 1;
    margin-right: 8px;
}

.quote::before {
    content: """;
    font-size: 2em;
    color: #6c757d;
}

.quote::after {
    content: """;
    font-size: 2em;
    color: #6c757d;
}

/* Selection styling */
::selection {
    background-color: #007bff;
    color: white;
}
```

### Combinators
```css
/* Descendant combinator (space) */
nav ul {
    list-style: none;
    margin: 0;
    padding: 0;
}

nav ul li {
    display: inline-block;
    margin-right: 20px;
}

/* Child combinator (>) */
nav > ul {
    display: flex;
}

nav > ul > li {
    position: relative;
}

/* Adjacent sibling combinator (+) */
h1 + p {
    font-size: 1.2em;
    color: #6c757d;
}

/* General sibling combinator (~) */
h2 ~ p {
    margin-left: 20px;
}
```

---

## The Box Model

### Understanding the Box Model
```css
/* Every element is a rectangular box */
.box-example {
    /* Content area */
    width: 300px;
    height: 200px;
    
    /* Padding (space inside the element) */
    padding: 20px;
    
    /* Border */
    border: 2px solid #333;
    
    /* Margin (space outside the element) */
    margin: 10px;
    
    /* Background (covers content + padding) */
    background-color: lightblue;
}

/* Box-sizing property */
.content-box {
    box-sizing: content-box; /* Default: width/height = content only */
    width: 300px;
    padding: 20px;
    border: 2px solid black;
    /* Total width = 300 + 40 + 4 = 344px */
}

.border-box {
    box-sizing: border-box; /* width/height includes padding and border */
    width: 300px;
    padding: 20px;
    border: 2px solid black;
    /* Total width = 300px (padding and border included) */
}

/* Global box-sizing reset */
*, *::before, *::after {
    box-sizing: border-box;
}
```

### Margins and Padding
```css
/* Margin shorthand */
.margin-examples {
    margin: 10px; /* All sides */
    margin: 10px 20px; /* Top/bottom, left/right */
    margin: 10px 20px 30px; /* Top, left/right, bottom */
    margin: 10px 20px 30px 40px; /* Top, right, bottom, left (clockwise) */
}

/* Individual margin properties */
.margin-individual {
    margin-top: 10px;
    margin-right: 20px;
    margin-bottom: 30px;
    margin-left: 40px;
}

/* Padding shorthand (same pattern as margin) */
.padding-examples {
    padding: 15px;
    padding: 10px 20px;
    padding: 10px 20px 30px;
    padding: 10px 20px 30px 40px;
}

/* Centering with margin */
.center-block {
    width: 800px;
    margin: 0 auto; /* Centers horizontally */
}

/* Margin collapse */
.margin-collapse-example {
    margin-bottom: 20px;
}

.margin-collapse-example + .margin-collapse-example {
    margin-top: 30px;
    /* Actual space between elements: 30px (larger margin wins) */
}
```

### Borders
```css
/* Border shorthand */
.border-examples {
    border: 2px solid #333; /* width style color */
    border: 1px dashed red;
    border: 3px dotted blue;
}

/* Individual border properties */
.border-detailed {
    border-width: 2px;
    border-style: solid;
    border-color: #333;
}

/* Individual sides */
.border-sides {
    border-top: 2px solid red;
    border-right: 1px dashed blue;
    border-bottom: 3px dotted green;
    border-left: 2px solid purple;
}

/* Border radius */
.border-radius {
    border-radius: 8px; /* All corners */
    border-radius: 10px 20px; /* Top-left/bottom-right, top-right/bottom-left */
    border-radius: 10px 20px 30px; /* Top-left, top-right/bottom-left, bottom-right */
    border-radius: 10px 20px 30px 40px; /* Top-left, top-right, bottom-right, bottom-left */
}

/* Circular elements */
.circle {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background-color: #007bff;
}

/* Pill-shaped buttons */
.pill-button {
    padding: 12px 24px;
    border-radius: 50px;
    background-color: #28a745;
    color: white;
    border: none;
}
```

---

## Colors and Typography

### Color Values
```css
/* Named colors */
.named-colors {
    color: red;
    background-color: blue;
    border-color: green;
}

/* Hexadecimal colors */
.hex-colors {
    color: #ff0000; /* Red */
    background-color: #00ff00; /* Green */
    border-color: #0000ff; /* Blue */
    
    /* Shorthand hex */
    color: #f00; /* Same as #ff0000 */
    background-color: #0f0; /* Same as #00ff00 */
}

/* RGB colors */
.rgb-colors {
    color: rgb(255, 0, 0); /* Red */
    background-color: rgb(0, 255, 0); /* Green */
    border-color: rgb(0, 0, 255); /* Blue */
}

/* RGBA colors (with transparency) */
.rgba-colors {
    background-color: rgba(255, 0, 0, 0.5); /* 50% transparent red */
    border-color: rgba(0, 0, 0, 0.1); /* 10% transparent black */
}

/* HSL colors */
.hsl-colors {
    color: hsl(0, 100%, 50%); /* Red */
    background-color: hsl(120, 100%, 50%); /* Green */
    border-color: hsl(240, 100%, 50%); /* Blue */
}

/* HSLA colors (with transparency) */
.hsla-colors {
    background-color: hsla(0, 100%, 50%, 0.5); /* 50% transparent red */
}

/* CSS Custom Properties (Variables) */
:root {
    --primary-color: #007bff;
    --secondary-color: #6c757d;
    --success-color: #28a745;
    --danger-color: #dc3545;
    --warning-color: #ffc107;
    --info-color: #17a2b8;
}

.using-variables {
    color: var(--primary-color);
    background-color: var(--secondary-color);
    border-color: var(--success-color);
}
```

### Typography
```css
/* Font families */
.font-families {
    font-family: Arial, sans-serif; /* Fallback fonts */
    font-family: "Times New Roman", serif;
    font-family: "Courier New", monospace;
    font-family: Georgia, "Times New Roman", serif;
}

/* Web fonts */
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap');

.web-fonts {
    font-family: 'Roboto', sans-serif;
}

/* Font properties */
.font-properties {
    font-size: 16px; /* Absolute size */
    font-size: 1.2em; /* Relative to parent */
    font-size: 1.2rem; /* Relative to root */
    font-size: 120%; /* Percentage */
    
    font-weight: normal; /* 400 */
    font-weight: bold; /* 700 */
    font-weight: 300; /* Light */
    font-weight: 600; /* Semi-bold */
    
    font-style: normal;
    font-style: italic;
    font-style: oblique;
    
    font-variant: normal;
    font-variant: small-caps;
}

/* Text properties */
.text-properties {
    text-align: left;
    text-align: center;
    text-align: right;
    text-align: justify;
    
    text-decoration: none;
    text-decoration: underline;
    text-decoration: line-through;
    text-decoration: overline;
    
    text-transform: none;
    text-transform: uppercase;
    text-transform: lowercase;
    text-transform: capitalize;
    
    line-height: 1.5; /* Recommended for readability */
    line-height: 24px;
    line-height: 150%;
    
    letter-spacing: normal;
    letter-spacing: 1px;
    letter-spacing: 0.1em;
    
    word-spacing: normal;
    word-spacing: 2px;
    
    text-indent: 0;
    text-indent: 2em; /* Indent first line */
}

/* Advanced typography */
.advanced-typography {
    /* Text shadow */
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    text-shadow: 1px 1px 2px #000, 0 0 1em blue;
    
    /* White space handling */
    white-space: normal;
    white-space: nowrap; /* Prevents wrapping */
    white-space: pre; /* Preserves spaces and line breaks */
    white-space: pre-wrap; /* Preserves spaces, allows wrapping */
    
    /* Text overflow */
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    
    /* Word breaking */
    word-break: normal;
    word-break: break-all;
    word-break: keep-all;
    
    word-wrap: normal;
    word-wrap: break-word;
}
```

---

## Layout Fundamentals

### Display Property
```css
/* Block elements */
.block-element {
    display: block;
    width: 100%; /* Takes full width by default */
    margin: 10px 0; /* Vertical margins work */
}

/* Inline elements */
.inline-element {
    display: inline;
    /* Width and height have no effect */
    /* Vertical margins and padding don't work properly */
}

/* Inline-block elements */
.inline-block-element {
    display: inline-block;
    width: 200px; /* Width works */
    height: 100px; /* Height works */
    margin: 10px; /* All margins work */
    vertical-align: top; /* Can be aligned */
}

/* None - hides element completely */
.hidden-element {
    display: none; /* Element is not rendered */
}

/* Visibility hidden - hides but keeps space */
.invisible-element {
    visibility: hidden; /* Element is invisible but takes space */
}
```

### Positioning
```css
/* Static positioning (default) */
.static-position {
    position: static; /* Normal document flow */
}

/* Relative positioning */
.relative-position {
    position: relative;
    top: 10px; /* Moves down 10px from normal position */
    left: 20px; /* Moves right 20px from normal position */
}

/* Absolute positioning */
.absolute-position {
    position: absolute;
    top: 50px; /* 50px from top of nearest positioned ancestor */
    right: 20px; /* 20px from right of nearest positioned ancestor */
}

/* Fixed positioning */
.fixed-position {
    position: fixed;
    top: 0; /* Sticks to top of viewport */
    left: 0;
    width: 100%;
    background-color: white;
    z-index: 1000; /* Appears above other elements */
}

/* Sticky positioning */
.sticky-position {
    position: sticky;
    top: 20px; /* Sticks when 20px from top during scroll */
}

/* Z-index stacking */
.stacking-context {
    position: relative;
    z-index: 1; /* Higher numbers appear on top */
}
```

### Float and Clear
```css
/* Float (legacy layout method) */
.float-left {
    float: left;
    width: 200px;
    margin-right: 20px;
}

.float-right {
    float: right;
    width: 200px;
    margin-left: 20px;
}

/* Clearing floats */
.clear-both {
    clear: both; /* Clears both left and right floats */
}

/* Clearfix hack */
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

---

## Flexbox Layout

### Flex Container Properties
```css
/* Basic flex container */
.flex-container {
    display: flex; /* or inline-flex */
    
    /* Main axis direction */
    flex-direction: row; /* Default: left to right */
    flex-direction: row-reverse; /* Right to left */
    flex-direction: column; /* Top to bottom */
    flex-direction: column-reverse; /* Bottom to top */
    
    /* Wrapping */
    flex-wrap: nowrap; /* Default: single line */
    flex-wrap: wrap; /* Multi-line */
    flex-wrap: wrap-reverse; /* Multi-line, reversed */
    
    /* Shorthand for direction and wrap */
    flex-flow: row wrap;
    
    /* Main axis alignment */
    justify-content: flex-start; /* Default: start of main axis */
    justify-content: flex-end; /* End of main axis */
    justify-content: center; /* Center of main axis */
    justify-content: space-between; /* Equal space between items */
    justify-content: space-around; /* Equal space around items */
    justify-content: space-evenly; /* Equal space everywhere */
    
    /* Cross axis alignment */
    align-items: stretch; /* Default: stretch to fill container */
    align-items: flex-start; /* Start of cross axis */
    align-items: flex-end; /* End of cross axis */
    align-items: center; /* Center of cross axis */
    align-items: baseline; /* Align baselines */
    
    /* Multi-line cross axis alignment */
    align-content: stretch; /* Default */
    align-content: flex-start;
    align-content: flex-end;
    align-content: center;
    align-content: space-between;
    align-content: space-around;
    
    /* Gap between items */
    gap: 20px; /* Same gap for rows and columns */
    row-gap: 20px; /* Gap between rows */
    column-gap: 30px; /* Gap between columns */
}
```

### Flex Item Properties
```css
.flex-item {
    /* Flex grow */
    flex-grow: 0; /* Default: don't grow */
    flex-grow: 1; /* Grow to fill available space */
    flex-grow: 2; /* Grow twice as much as items with flex-grow: 1 */
    
    /* Flex shrink */
    flex-shrink: 1; /* Default: can shrink */
    flex-shrink: 0; /* Don't shrink */
    flex-shrink: 2; /* Shrink twice as much */
    
    /* Flex basis */
    flex-basis: auto; /* Default: based on content */
    flex-basis: 200px; /* Initial size before growing/shrinking */
    flex-basis: 25%; /* Percentage of container */
    
    /* Flex shorthand */
    flex: 1; /* flex-grow: 1, flex-shrink: 1, flex-basis: 0% */
    flex: 0 1 auto; /* Default values */
    flex: 2 1 200px; /* grow: 2, shrink: 1, basis: 200px */
    
    /* Individual alignment */
    align-self: auto; /* Default: inherit from container */
    align-self: flex-start;
    align-self: flex-end;
    align-self: center;
    align-self: stretch;
    align-self: baseline;
    
    /* Order */
    order: 0; /* Default: source order */
    order: 1; /* Appears after items with order: 0 */
    order: -1; /* Appears before items with order: 0 */
}
```

### Common Flexbox Patterns
```css
/* Centering content */
.center-content {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

/* Navigation bar */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 2rem;
}

.navbar-brand {
    font-size: 1.5rem;
    font-weight: bold;
}

.navbar-nav {
    display: flex;
    list-style: none;
    gap: 2rem;
}

/* Card layout */
.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    padding: 2rem;
}

.card {
    flex: 1 1 300px; /* Grow, shrink, min-width */
    min-width: 250px;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    padding: 1.5rem;
}

/* Sidebar layout */
.sidebar-layout {
    display: flex;
    min-height: 100vh;
}

.sidebar {
    flex: 0 0 250px; /* Don't grow/shrink, fixed width */
    background-color: #f8f9fa;
    padding: 2rem;
}

.main-content {
    flex: 1; /* Take remaining space */
    padding: 2rem;
}

/* Footer that sticks to bottom */
.page-container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.page-header,
.page-footer {
    flex: 0 0 auto; /* Don't grow/shrink */
}

.page-main {
    flex: 1 0 auto; /* Grow to fill space */
}
```
---

## CSS Grid Layout

### Grid Container Properties
```css
/* Basic grid container */
.grid-container {
    display: grid; /* or inline-grid */
    
    /* Define columns */
    grid-template-columns: 200px 1fr 100px; /* Fixed, flexible, fixed */
    grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Responsive columns */
    grid-template-columns: 1fr 2fr 1fr; /* Proportional columns */
    
    /* Define rows */
    grid-template-rows: 100px auto 50px; /* Fixed, content-based, fixed */
    grid-template-rows: repeat(3, 200px); /* 3 rows of 200px each */
    grid-template-rows: minmax(100px, auto); /* Minimum 100px, grow as needed */
    
    /* Grid areas */
    grid-template-areas: 
        "header header header"
        "sidebar main main"
        "footer footer footer";
    
    /* Gaps */
    gap: 20px; /* Same gap for rows and columns */
    row-gap: 20px; /* Gap between rows */
    column-gap: 30px; /* Gap between columns */
    
    /* Alignment */
    justify-items: stretch; /* Default: stretch items horizontally */
    justify-items: start; /* Align items to start of cell */
    justify-items: end; /* Align items to end of cell */
    justify-items: center; /* Center items in cell */
    
    align-items: stretch; /* Default: stretch items vertically */
    align-items: start; /* Align items to top of cell */
    align-items: end; /* Align items to bottom of cell */
    align-items: center; /* Center items in cell */
    
    /* Content alignment */
    justify-content: start; /* Default: align grid to start */
    justify-content: end; /* Align grid to end */
    justify-content: center; /* Center grid */
    justify-content: stretch; /* Stretch grid to fill container */
    justify-content: space-around; /* Equal space around columns */
    justify-content: space-between; /* Equal space between columns */
    justify-content: space-evenly; /* Equal space everywhere */
    
    align-content: start; /* Align grid to top */
    align-content: end; /* Align grid to bottom */
    align-content: center; /* Center grid vertically */
    align-content: stretch; /* Stretch grid to fill container */
    align-content: space-around; /* Equal space around rows */
    align-content: space-between; /* Equal space between rows */
    align-content: space-evenly; /* Equal space everywhere */
}
```

### Grid Item Properties
```css
.grid-item {
    /* Position by line numbers */
    grid-column-start: 1;
    grid-column-end: 3; /* Spans from line 1 to line 3 */
    grid-row-start: 2;
    grid-row-end: 4;
    
    /* Shorthand */
    grid-column: 1 / 3; /* Start / end */
    grid-row: 2 / 4;
    
    /* Span syntax */
    grid-column: span 2; /* Span 2 columns */
    grid-row: span 3; /* Span 3 rows */
    
    /* Grid area shorthand */
    grid-area: 2 / 1 / 4 / 3; /* row-start / col-start / row-end / col-end */
    
    /* Named grid areas */
    grid-area: header; /* Use named area from grid-template-areas */
    
    /* Individual alignment */
    justify-self: start; /* Align item horizontally in its cell */
    justify-self: end;
    justify-self: center;
    justify-self: stretch; /* Default */
    
    align-self: start; /* Align item vertically in its cell */
    align-self: end;
    align-self: center;
    align-self: stretch; /* Default */
}
```

### Common Grid Patterns
```css
/* Basic page layout */
.page-grid {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header"
        "main"
        "footer";
    min-height: 100vh;
}

.page-header { grid-area: header; }
.page-main { grid-area: main; }
.page-footer { grid-area: footer; }

/* Sidebar layout */
.sidebar-grid {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "sidebar header"
        "sidebar main"
        "sidebar footer";
    min-height: 100vh;
}

/* Card grid */
.card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    padding: 2rem;
}

/* Magazine layout */
.magazine-grid {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: repeat(6, 100px);
    gap: 1rem;
}

.featured-article {
    grid-column: 1 / 8; /* Spans 7 columns */
    grid-row: 1 / 4; /* Spans 3 rows */
}

.sidebar-article {
    grid-column: 8 / 13; /* Spans 5 columns */
    grid-row: 1 / 3; /* Spans 2 rows */
}

/* Image gallery */
.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    grid-auto-rows: 200px;
    gap: 1rem;
}

.gallery-item:nth-child(3n) {
    grid-column: span 2;
}

.gallery-item:nth-child(5n) {
    grid-row: span 2;
}
```

---

## Responsive Design

### Media Queries
```css
/* Mobile-first approach */
/* Base styles for mobile */
.container {
    width: 100%;
    padding: 1rem;
}

/* Tablet styles */
@media (min-width: 768px) {
    .container {
        max-width: 750px;
        margin: 0 auto;
        padding: 2rem;
    }
}

/* Desktop styles */
@media (min-width: 1024px) {
    .container {
        max-width: 1200px;
        padding: 3rem;
    }
}

/* Large desktop styles */
@media (min-width: 1440px) {
    .container {
        max-width: 1400px;
    }
}

/* Different media query types */
@media screen and (max-width: 767px) {
    /* Styles for screens smaller than 768px */
}

@media print {
    /* Styles for printing */
    body { font-size: 12pt; }
    .no-print { display: none; }
}

@media (orientation: landscape) {
    /* Styles for landscape orientation */
}

@media (orientation: portrait) {
    /* Styles for portrait orientation */
}

@media (prefers-color-scheme: dark) {
    /* Dark mode styles */
    body {
        background-color: #1a1a1a;
        color: #ffffff;
    }
}

@media (prefers-reduced-motion: reduce) {
    /* Reduced motion for accessibility */
    * {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

### Responsive Units
```css
/* Relative units */
.responsive-text {
    /* Relative to parent element */
    font-size: 1.2em;
    margin: 0.5em;
    
    /* Relative to root element */
    font-size: 1.2rem;
    padding: 1rem;
    
    /* Viewport units */
    width: 50vw; /* 50% of viewport width */
    height: 100vh; /* 100% of viewport height */
    font-size: 4vw; /* 4% of viewport width */
    
    /* Minimum and maximum viewport units */
    font-size: 2vmin; /* 2% of smaller viewport dimension */
    font-size: 2vmax; /* 2% of larger viewport dimension */
    
    /* Percentage */
    width: 80%; /* 80% of parent width */
    
    /* Calc function */
    width: calc(100% - 40px); /* Full width minus 40px */
    margin: calc(1rem + 2px);
    font-size: calc(1rem + 0.5vw); /* Responsive typography */
}

/* Responsive images */
.responsive-image {
    max-width: 100%;
    height: auto;
}

/* Responsive typography */
.responsive-heading {
    font-size: clamp(1.5rem, 4vw, 3rem); /* min, preferred, max */
}
```

### Container Queries (Modern CSS)
```css
/* Container queries for component-based responsive design */
.card-container {
    container-type: inline-size;
    container-name: card;
}

@container card (min-width: 400px) {
    .card {
        display: flex;
        flex-direction: row;
    }
    
    .card-image {
        flex: 0 0 200px;
    }
    
    .card-content {
        flex: 1;
        padding-left: 1rem;
    }
}
```

---

## Animations and Transitions

### CSS Transitions
```css
/* Basic transition */
.button {
    background-color: #007bff;
    color: white;
    padding: 12px 24px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    
    /* Transition property */
    transition: background-color 0.3s ease;
}

.button:hover {
    background-color: #0056b3;
}

/* Multiple transitions */
.card {
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    
    transition: 
        transform 0.3s ease,
        box-shadow 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

/* Transition shorthand */
.element {
    transition: all 0.3s ease-in-out;
    /* property duration timing-function delay */
}

/* Different timing functions */
.timing-examples {
    transition-timing-function: ease; /* Default */
    transition-timing-function: linear;
    transition-timing-function: ease-in;
    transition-timing-function: ease-out;
    transition-timing-function: ease-in-out;
    transition-timing-function: cubic-bezier(0.25, 0.1, 0.25, 1);
}
```

### CSS Transforms
```css
/* 2D Transforms */
.transform-examples {
    /* Translate (move) */
    transform: translateX(50px); /* Move right 50px */
    transform: translateY(-20px); /* Move up 20px */
    transform: translate(50px, -20px); /* Move right 50px, up 20px */
    
    /* Scale */
    transform: scaleX(1.5); /* Scale width by 1.5x */
    transform: scaleY(0.8); /* Scale height by 0.8x */
    transform: scale(1.2); /* Scale both dimensions by 1.2x */
    transform: scale(1.5, 0.8); /* Scale width 1.5x, height 0.8x */
    
    /* Rotate */
    transform: rotate(45deg); /* Rotate 45 degrees clockwise */
    transform: rotate(-90deg); /* Rotate 90 degrees counter-clockwise */
    
    /* Skew */
    transform: skewX(15deg); /* Skew horizontally */
    transform: skewY(10deg); /* Skew vertically */
    transform: skew(15deg, 10deg); /* Skew both directions */
    
    /* Multiple transforms */
    transform: translate(50px, 100px) rotate(45deg) scale(1.2);
    
    /* Transform origin */
    transform-origin: center; /* Default */
    transform-origin: top left;
    transform-origin: 50% 50%; /* Same as center */
    transform-origin: 100px 50px; /* Specific point */
}

/* 3D Transforms */
.transform-3d {
    /* 3D translations */
    transform: translateZ(50px); /* Move forward 50px */
    transform: translate3d(50px, 100px, 25px); /* X, Y, Z */
    
    /* 3D rotations */
    transform: rotateX(45deg); /* Rotate around X-axis */
    transform: rotateY(45deg); /* Rotate around Y-axis */
    transform: rotateZ(45deg); /* Same as rotate(45deg) */
    transform: rotate3d(1, 1, 0, 45deg); /* Rotate around custom axis */
    
    /* 3D scaling */
    transform: scaleZ(2); /* Scale depth */
    transform: scale3d(1.5, 1.2, 2); /* X, Y, Z scaling */
    
    /* Perspective */
    perspective: 1000px; /* Apply to parent element */
    transform: perspective(1000px) rotateY(45deg); /* Apply to element */
    
    /* Transform style */
    transform-style: preserve-3d; /* Preserve 3D for children */
}
```

### CSS Animations
```css
/* Define keyframes */
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes slideIn {
    0% {
        transform: translateX(-100%);
    }
    50% {
        transform: translateX(10px);
    }
    100% {
        transform: translateX(0);
    }
}

@keyframes bounce {
    0%, 20%, 53%, 80%, 100% {
        transform: translate3d(0, 0, 0);
    }
    40%, 43% {
        transform: translate3d(0, -30px, 0);
    }
    70% {
        transform: translate3d(0, -15px, 0);
    }
    90% {
        transform: translate3d(0, -4px, 0);
    }
}

/* Apply animations */
.fade-in {
    animation: fadeIn 0.5s ease-out;
}

.slide-in {
    animation: slideIn 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.bounce-element {
    animation: bounce 2s infinite;
}

/* Animation properties */
.animation-properties {
    animation-name: fadeIn;
    animation-duration: 1s;
    animation-timing-function: ease-in-out;
    animation-delay: 0.5s;
    animation-iteration-count: infinite; /* or number */
    animation-direction: normal; /* normal, reverse, alternate, alternate-reverse */
    animation-fill-mode: forwards; /* none, forwards, backwards, both */
    animation-play-state: running; /* running, paused */
    
    /* Shorthand */
    animation: fadeIn 1s ease-in-out 0.5s infinite alternate forwards;
}

/* Loading spinner */
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #007bff;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

/* Pulse effect */
@keyframes pulse {
    0% {
        transform: scale(1);
        opacity: 1;
    }
    50% {
        transform: scale(1.05);
        opacity: 0.8;
    }
    100% {
        transform: scale(1);
        opacity: 1;
    }
}

.pulse-button {
    animation: pulse 2s ease-in-out infinite;
}
```

---

## CSS Best Practices

### Organization and Structure
```css
/* CSS Reset/Normalize */
*, *::before, *::after {
    box-sizing: border-box;
}

body {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    line-height: 1.6;
    color: #333;
}

/* CSS Custom Properties */
:root {
    /* Colors */
    --primary-color: #007bff;
    --secondary-color: #6c757d;
    --success-color: #28a745;
    --danger-color: #dc3545;
    --warning-color: #ffc107;
    --info-color: #17a2b8;
    --light-color: #f8f9fa;
    --dark-color: #343a40;
    
    /* Typography */
    --font-family-base: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    --font-family-monospace: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
    --font-size-base: 1rem;
    --line-height-base: 1.6;
    
    /* Spacing */
    --spacing-xs: 0.25rem;
    --spacing-sm: 0.5rem;
    --spacing-md: 1rem;
    --spacing-lg: 1.5rem;
    --spacing-xl: 3rem;
    
    /* Breakpoints */
    --breakpoint-sm: 576px;
    --breakpoint-md: 768px;
    --breakpoint-lg: 992px;
    --breakpoint-xl: 1200px;
}

/* BEM Methodology */
.card {
    /* Block */
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    padding: var(--spacing-lg);
}

.card__header {
    /* Element */
    border-bottom: 1px solid #dee2e6;
    margin-bottom: var(--spacing-md);
    padding-bottom: var(--spacing-md);
}

.card__title {
    /* Element */
    font-size: 1.25rem;
    font-weight: 600;
    margin: 0;
}

.card__content {
    /* Element */
    color: var(--secondary-color);
}

.card--featured {
    /* Modifier */
    border: 2px solid var(--primary-color);
    box-shadow: 0 4px 20px rgba(0, 123, 255, 0.15);
}

.card__title--large {
    /* Element modifier */
    font-size: 1.5rem;
}
```

### Performance Optimization
```css
/* Efficient selectors */
/* ✅ Good: Specific and efficient */
.navigation-item {
    color: var(--primary-color);
}

/* ❌ Bad: Overly specific */
body div.container ul.navigation li.navigation-item a {
    color: var(--primary-color);
}

/* ❌ Bad: Universal selector */
* {
    border: 1px solid red; /* Only for debugging */
}

/* Hardware acceleration */
.animated-element {
    /* Use transform and opacity for smooth animations */
    transform: translateZ(0); /* Force hardware acceleration */
    will-change: transform; /* Hint to browser for optimization */
}

/* Efficient animations */
@keyframes slideIn {
    from {
        transform: translateX(-100%); /* Use transform, not left */
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

/* Critical CSS inlining */
/* Inline critical above-the-fold CSS in HTML head */
/* Load non-critical CSS asynchronously */

/* Font loading optimization */
@font-face {
    font-family: 'CustomFont';
    src: url('font.woff2') format('woff2'),
         url('font.woff') format('woff');
    font-display: swap; /* Show fallback font while loading */
}
```

### Maintainable CSS
```css
/* Consistent naming conventions */
.btn { /* Base component */ }
.btn-primary { /* Variant */ }
.btn-large { /* Size modifier */ }
.btn-disabled { /* State */ }

/* Utility classes */
.text-center { text-align: center; }
.text-left { text-align: left; }
.text-right { text-align: right; }

.mb-0 { margin-bottom: 0; }
.mb-1 { margin-bottom: var(--spacing-xs); }
.mb-2 { margin-bottom: var(--spacing-sm); }
.mb-3 { margin-bottom: var(--spacing-md); }

.d-none { display: none; }
.d-block { display: block; }
.d-flex { display: flex; }

/* Component-based architecture */
/* Button component */
.btn {
    display: inline-block;
    padding: var(--spacing-sm) var(--spacing-md);
    border: 1px solid transparent;
    border-radius: 4px;
    font-size: var(--font-size-base);
    line-height: 1.5;
    text-align: center;
    text-decoration: none;
    cursor: pointer;
    transition: all 0.15s ease-in-out;
}

.btn:hover {
    text-decoration: none;
}

.btn:focus {
    outline: 0;
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

.btn:disabled {
    opacity: 0.65;
    cursor: not-allowed;
}

/* Button variants */
.btn-primary {
    color: white;
    background-color: var(--primary-color);
    border-color: var(--primary-color);
}

.btn-primary:hover {
    background-color: #0056b3;
    border-color: #004085;
}

.btn-secondary {
    color: white;
    background-color: var(--secondary-color);
    border-color: var(--secondary-color);
}
```

---

## Practice Projects

### Project 1: Personal Portfolio Layout
Create a responsive portfolio with:
- CSS Grid for overall layout
- Flexbox for navigation and card layouts
- Custom CSS animations
- Dark/light mode toggle
- Responsive typography

### Project 2: E-commerce Product Grid
Build a product showcase featuring:
- CSS Grid for product layout
- Hover effects and transitions
- Responsive image handling
- Filter and sort animations
- Shopping cart UI

### Project 3: Dashboard Interface
Design a admin dashboard with:
- Complex CSS Grid layout
- Sidebar navigation
- Data visualization styling
- Form styling and validation states
- Responsive breakpoints

### Project 4: Blog Layout
Create a blog design with:
- Typography-focused design
- Reading-optimized layouts
- Comment system styling
- Tag and category styling
- Print stylesheet

### Project 5: Landing Page
Build a marketing landing page with:
- Hero section with animations
- Feature sections with icons
- Testimonial carousel styling
- Call-to-action buttons
- Mobile-first responsive design

---

## Next Steps

After mastering CSS fundamentals:

1. **CSS Preprocessors**: Learn Sass or Less for advanced features
2. **CSS Frameworks**: Explore Tailwind CSS or Bootstrap
3. **CSS-in-JS**: Understand styled-components for React
4. **CSS Architecture**: Learn methodologies like BEM, SMACSS, or ITCSS
5. **Advanced CSS**: Explore CSS Houdini, Container Queries, and modern features
6. **Performance**: Learn CSS optimization and critical rendering path

Remember: CSS is both an art and a science. Practice regularly, experiment with different techniques, and always consider the user experience when making design decisions.