# HTML Fundamentals - Complete Learning Guide

Master HTML (HyperText Markup Language) from absolute basics to advanced semantic markup. This comprehensive guide covers everything you need to build well-structured, accessible web pages.

## Table of Contents

1. [What is HTML?](#what-is-html)
2. [HTML Document Structure](#html-document-structure)
3. [Essential HTML Elements](#essential-html-elements)
4. [Text Content and Formatting](#text-content-and-formatting)
5. [Lists and Navigation](#lists-and-navigation)
6. [Links and Navigation](#links-and-navigation)
7. [Images and Media](#images-and-media)
8. [Tables](#tables)
9. [Forms and Input Elements](#forms-and-input-elements)
10. [Semantic HTML Elements](#semantic-html-elements)
11. [HTML Attributes](#html-attributes)
12. [Accessibility in HTML](#accessibility-in-html)
13. [HTML Best Practices](#html-best-practices)
14. [Practice Projects](#practice-projects)

---

## What is HTML?

HTML (HyperText Markup Language) is the standard markup language for creating web pages. It describes the structure and content of web pages using elements and tags.

### Key Concepts:
- **Elements**: Building blocks of HTML (e.g., `<h1>`, `<p>`, `<div>`)
- **Tags**: Markup that defines elements (opening `<tag>` and closing `</tag>`)
- **Attributes**: Additional information about elements (e.g., `class`, `id`, `src`)
- **Content**: Text, images, and other media between opening and closing tags

### Why HTML Matters:
- **Foundation**: Every website starts with HTML
- **SEO**: Search engines read HTML structure
- **Accessibility**: Screen readers rely on proper HTML
- **Semantics**: Meaningful markup improves user experience

---

## HTML Document Structure

Every HTML document follows a standard structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="A brief description of your page">
    <meta name="keywords" content="html, web development, tutorial">
    <meta name="author" content="Your Name">
    <title>Page Title - Appears in Browser Tab</title>
    
    <!-- External CSS -->
    <link rel="stylesheet" href="styles.css">
    
    <!-- Favicon -->
    <link rel="icon" type="image/x-icon" href="/favicon.ico">
    
    <!-- Open Graph for Social Media -->
    <meta property="og:title" content="Page Title">
    <meta property="og:description" content="Page description">
    <meta property="og:image" content="image-url.jpg">
    <meta property="og:url" content="https://yoursite.com">
</head>
<body>
    <!-- Page content goes here -->
    <h1>Welcome to My Website</h1>
    <p>This is the main content area.</p>
    
    <!-- JavaScript -->
    <script src="script.js"></script>
</body>
</html>
```

### Document Structure Breakdown:

#### DOCTYPE Declaration
```html
<!DOCTYPE html>
```
- Tells the browser this is an HTML5 document
- Must be the first line in your HTML file
- Not case-sensitive but conventionally written in uppercase

#### HTML Element
```html
<html lang="en">
```
- Root element that contains all other elements
- `lang` attribute specifies the language (important for accessibility)
- Other common values: `lang="es"` (Spanish), `lang="fr"` (French)

#### Head Section
The `<head>` contains metadata about the document:

```html
<head>
    <!-- Character encoding - always include this first -->
    <meta charset="UTF-8">
    
    <!-- Responsive design viewport -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- SEO meta tags -->
    <meta name="description" content="Concise page description (150-160 characters)">
    <meta name="keywords" content="relevant, keywords, separated, by, commas">
    <meta name="author" content="Your Name or Company">
    
    <!-- Page title (appears in browser tab and search results) -->
    <title>Descriptive Page Title | Your Site Name</title>
    
    <!-- External resources -->
    <link rel="stylesheet" href="css/styles.css">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="icon" href="favicon.ico" type="image/x-icon">
</head>
```

#### Body Section
The `<body>` contains all visible content:

```html
<body>
    <!-- All visible page content -->
    <header>
        <nav><!-- Navigation --></nav>
    </header>
    
    <main>
        <!-- Main content -->
    </main>
    
    <footer>
        <!-- Footer content -->
    </footer>
    
    <!-- JavaScript files (best practice: before closing body tag) -->
    <script src="js/script.js"></script>
</body>
```

---

## Essential HTML Elements

### Heading Elements
Headings create a hierarchical structure for your content:

```html
<h1>Main Page Title (Only one per page)</h1>
<h2>Major Section Heading</h2>
<h3>Subsection Heading</h3>
<h4>Sub-subsection Heading</h4>
<h5>Minor Heading</h5>
<h6>Smallest Heading</h6>

<!-- Example of proper heading hierarchy -->
<h1>Complete Guide to Web Development</h1>
    <h2>Frontend Development</h2>
        <h3>HTML Fundamentals</h3>
            <h4>Document Structure</h4>
            <h4>Semantic Elements</h4>
        <h3>CSS Styling</h3>
    <h2>Backend Development</h2>
        <h3>Server-Side Languages</h3>
```

**Best Practices:**
- Use only one `<h1>` per page
- Don't skip heading levels (don't go from `<h2>` to `<h4>`)
- Use headings for structure, not styling
- Keep headings descriptive and concise

### Paragraph and Text Elements
```html
<!-- Paragraphs -->
<p>This is a paragraph of text. Paragraphs are block-level elements that create natural breaks between sections of content.</p>

<p>This is another paragraph. Notice the space between paragraphs is automatically created by the browser.</p>

<!-- Line breaks and horizontal rules -->
<p>This is the first line.<br>
This is the second line on a new line within the same paragraph.</p>

<hr> <!-- Horizontal rule - creates a thematic break -->

<!-- Preformatted text -->
<pre>
    This text preserves    spaces
    and line breaks
    exactly as written
</pre>

<!-- Code elements -->
<p>To create a paragraph, use the <code>&lt;p&gt;</code> element.</p>

<pre><code>
function greet(name) {
    return `Hello, ${name}!`;
}
</code></pre>

<!-- Blockquotes -->
<blockquote cite="https://example.com/quote-source">
    <p>"The best way to learn web development is by building projects and practicing consistently."</p>
    <footer>— <cite>Web Development Expert</cite></footer>
</blockquote>
```

---

## Text Content and Formatting

### Inline Text Elements
```html
<!-- Emphasis and importance -->
<p>This is <em>emphasized text</em> (usually italic).</p>
<p>This is <strong>important text</strong> (usually bold).</p>
<p>This is <mark>highlighted text</mark> (usually yellow background).</p>

<!-- Visual formatting (use CSS instead when possible) -->
<p>This is <b>bold text</b> without semantic meaning.</p>
<p>This is <i>italic text</i> without semantic meaning.</p>
<p>This is <u>underlined text</u>.</p>
<p>This is <s>strikethrough text</s>.</p>

<!-- Subscript and superscript -->
<p>Water formula: H<sub>2</sub>O</p>
<p>Einstein's equation: E=mc<sup>2</sup></p>

<!-- Small text -->
<p>Regular text <small>(fine print or side comments)</small></p>

<!-- Abbreviations and definitions -->
<p>The <abbr title="World Wide Web">WWW</abbr> was invented by Tim Berners-Lee.</p>
<p><dfn>HTML</dfn> stands for HyperText Markup Language.</p>

<!-- Insertions and deletions -->
<p>The price is <del>$100</del> <ins>$80</ins>.</p>

<!-- Time element -->
<p>The meeting is scheduled for <time datetime="2024-01-15T14:00">January 15, 2024 at 2:00 PM</time>.</p>
```

### Address and Contact Information
```html
<address>
    <p>Contact us:</p>
    <p>
        <strong>John Doe</strong><br>
        123 Web Street<br>
        Internet City, IC 12345<br>
        <a href="mailto:john@example.com">john@example.com</a><br>
        <a href="tel:+1234567890">+1 (234) 567-890</a>
    </p>
</address>
```

---

## Lists and Navigation

### Unordered Lists
```html
<!-- Basic unordered list -->
<ul>
    <li>HTML - Structure</li>
    <li>CSS - Styling</li>
    <li>JavaScript - Interactivity</li>
</ul>

<!-- Nested lists -->
<ul>
    <li>Frontend Technologies
        <ul>
            <li>HTML5</li>
            <li>CSS3
                <ul>
                    <li>Flexbox</li>
                    <li>Grid</li>
                    <li>Animations</li>
                </ul>
            </li>
            <li>JavaScript ES6+</li>
        </ul>
    </li>
    <li>Backend Technologies
        <ul>
            <li>Node.js</li>
            <li>Python</li>
            <li>PHP</li>
        </ul>
    </li>
</ul>
```

### Ordered Lists
```html
<!-- Basic ordered list -->
<ol>
    <li>Learn HTML fundamentals</li>
    <li>Master CSS styling</li>
    <li>Add JavaScript interactivity</li>
    <li>Build your first project</li>
</ol>

<!-- Different numbering types -->
<ol type="A">
    <li>First item (A)</li>
    <li>Second item (B)</li>
    <li>Third item (C)</li>
</ol>

<ol type="I">
    <li>Roman numeral I</li>
    <li>Roman numeral II</li>
    <li>Roman numeral III</li>
</ol>

<!-- Starting from a specific number -->
<ol start="5">
    <li>This is item 5</li>
    <li>This is item 6</li>
    <li>This is item 7</li>
</ol>

<!-- Reversed order -->
<ol reversed>
    <li>Third step</li>
    <li>Second step</li>
    <li>First step</li>
</ol>
```

### Description Lists
```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language - the structure of web pages</dd>
    
    <dt>CSS</dt>
    <dd>Cascading Style Sheets - the presentation of web pages</dd>
    
    <dt>JavaScript</dt>
    <dd>Programming language for web interactivity</dd>
    <dd>Can also be used for server-side development</dd>
    
    <dt>Responsive Design</dt>
    <dt>Mobile-First Design</dt>
    <dd>Design approaches that ensure websites work on all devices</dd>
</dl>
```

---

## Links and Navigation

### Basic Links
```html
<!-- External links -->
<a href="https://www.example.com">Visit Example.com</a>
<a href="https://www.example.com" target="_blank" rel="noopener noreferrer">
    Open Example.com in new tab
</a>

<!-- Internal links (same site) -->
<a href="about.html">About Us</a>
<a href="/contact">Contact Page</a>
<a href="../index.html">Back to Home</a>

<!-- Links to sections on the same page -->
<a href="#section1">Go to Section 1</a>
<a href="#top">Back to Top</a>

<!-- Email and phone links -->
<a href="mailto:someone@example.com">Send Email</a>
<a href="mailto:someone@example.com?subject=Hello&body=Hi there!">
    Send Email with Subject and Body
</a>
<a href="tel:+1234567890">Call Us</a>
<a href="sms:+1234567890">Send SMS</a>

<!-- Download links -->
<a href="document.pdf" download>Download PDF</a>
<a href="image.jpg" download="my-image.jpg">Download Image</a>
```

### Navigation Structures
```html
<!-- Basic navigation -->
<nav>
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/services">Services</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>

<!-- Navigation with current page indicator -->
<nav>
    <ul>
        <li><a href="/" aria-current="page">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/services">Services</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>

<!-- Dropdown navigation -->
<nav>
    <ul>
        <li><a href="/">Home</a></li>
        <li>
            <a href="/services">Services</a>
            <ul>
                <li><a href="/services/web-design">Web Design</a></li>
                <li><a href="/services/development">Development</a></li>
                <li><a href="/services/seo">SEO</a></li>
            </ul>
        </li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>

<!-- Breadcrumb navigation -->
<nav aria-label="Breadcrumb">
    <ol>
        <li><a href="/">Home</a></li>
        <li><a href="/products">Products</a></li>
        <li><a href="/products/laptops">Laptops</a></li>
        <li aria-current="page">Gaming Laptops</li>
    </ol>
</nav>
```

---

## Images and Media

### Images
```html
<!-- Basic image -->
<img src="image.jpg" alt="Description of the image">

<!-- Image with all attributes -->
<img src="hero-image.jpg" 
     alt="A beautiful sunset over the mountains" 
     width="800" 
     height="400"
     loading="lazy"
     decoding="async">

<!-- Responsive images -->
<img src="small-image.jpg"
     srcset="small-image.jpg 300w,
             medium-image.jpg 600w,
             large-image.jpg 1200w"
     sizes="(max-width: 300px) 300px,
            (max-width: 600px) 600px,
            1200px"
     alt="Responsive image example">

<!-- Picture element for art direction -->
<picture>
    <source media="(min-width: 800px)" srcset="desktop-image.jpg">
    <source media="(min-width: 400px)" srcset="tablet-image.jpg">
    <img src="mobile-image.jpg" alt="Responsive image with art direction">
</picture>

<!-- Figure with caption -->
<figure>
    <img src="chart.png" alt="Sales data showing 25% increase">
    <figcaption>
        Sales increased by 25% in the last quarter
    </figcaption>
</figure>
```

### Audio and Video
```html
<!-- Audio element -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    Your browser does not support the audio element.
</audio>

<!-- Audio with additional attributes -->
<audio controls autoplay muted loop>
    <source src="background-music.mp3" type="audio/mpeg">
    <p>Your browser doesn't support HTML5 audio.</p>
</audio>

<!-- Video element -->
<video controls width="640" height="360">
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    <track kind="subtitles" src="subtitles-en.vtt" srclang="en" label="English">
    <track kind="subtitles" src="subtitles-es.vtt" srclang="es" label="Spanish">
    Your browser does not support the video element.
</video>

<!-- Video with poster image -->
<video controls poster="video-thumbnail.jpg" width="640" height="360">
    <source src="presentation.mp4" type="video/mp4">
    <p>Your browser doesn't support HTML5 video.</p>
</video>

<!-- Embedded content -->
<iframe src="https://www.youtube.com/embed/VIDEO_ID" 
        width="560" 
        height="315" 
        frameborder="0" 
        allowfullscreen
        title="YouTube video player">
</iframe>
```

---

## Tables

### Basic Table Structure
```html
<table>
    <caption>Monthly Sales Report</caption>
    <thead>
        <tr>
            <th scope="col">Month</th>
            <th scope="col">Sales ($)</th>
            <th scope="col">Growth (%)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>January</td>
            <td>$50,000</td>
            <td>+5%</td>
        </tr>
        <tr>
            <td>February</td>
            <td>$55,000</td>
            <td>+10%</td>
        </tr>
        <tr>
            <td>March</td>
            <td>$60,000</td>
            <td>+9%</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <th scope="row">Total</th>
            <td>$165,000</td>
            <td>+8%</td>
        </tr>
    </tfoot>
</table>
```

### Complex Table with Spanning Cells
```html
<table>
    <caption>Employee Information</caption>
    <thead>
        <tr>
            <th rowspan="2">Name</th>
            <th colspan="2">Contact</th>
            <th rowspan="2">Department</th>
        </tr>
        <tr>
            <th>Email</th>
            <th>Phone</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John Doe</td>
            <td>john@company.com</td>
            <td>555-0123</td>
            <td>Engineering</td>
        </tr>
        <tr>
            <td>Jane Smith</td>
            <td>jane@company.com</td>
            <td>555-0124</td>
            <td>Marketing</td>
        </tr>
    </tbody>
</table>
```

---

## Forms and Input Elements

### Basic Form Structure
```html
<form action="/submit" method="POST" enctype="multipart/form-data">
    <fieldset>
        <legend>Personal Information</legend>
        
        <!-- Text inputs -->
        <div>
            <label for="firstName">First Name:</label>
            <input type="text" id="firstName" name="firstName" required>
        </div>
        
        <div>
            <label for="lastName">Last Name:</label>
            <input type="text" id="lastName" name="lastName" required>
        </div>
        
        <div>
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
        </div>
        
        <div>
            <label for="phone">Phone:</label>
            <input type="tel" id="phone" name="phone">
        </div>
    </fieldset>
    
    <fieldset>
        <legend>Preferences</legend>
        
        <!-- Radio buttons -->
        <div>
            <p>Preferred contact method:</p>
            <input type="radio" id="contactEmail" name="contactMethod" value="email" checked>
            <label for="contactEmail">Email</label>
            
            <input type="radio" id="contactPhone" name="contactMethod" value="phone">
            <label for="contactPhone">Phone</label>
            
            <input type="radio" id="contactMail" name="contactMethod" value="mail">
            <label for="contactMail">Mail</label>
        </div>
        
        <!-- Checkboxes -->
        <div>
            <p>Interests:</p>
            <input type="checkbox" id="webDev" name="interests" value="web-development">
            <label for="webDev">Web Development</label>
            
            <input type="checkbox" id="design" name="interests" value="design">
            <label for="design">Design</label>
            
            <input type="checkbox" id="marketing" name="interests" value="marketing">
            <label for="marketing">Marketing</label>
        </div>
    </fieldset>
    
    <div>
        <label for="message">Message:</label>
        <textarea id="message" name="message" rows="5" cols="50" placeholder="Enter your message here..."></textarea>
    </div>
    
    <div>
        <button type="submit">Submit Form</button>
        <button type="reset">Reset Form</button>
    </div>
</form>
```

### Advanced Input Types
```html
<form>
    <!-- HTML5 input types -->
    <div>
        <label for="birthdate">Birth Date:</label>
        <input type="date" id="birthdate" name="birthdate">
    </div>
    
    <div>
        <label for="appointmentTime">Appointment Time:</label>
        <input type="datetime-local" id="appointmentTime" name="appointmentTime">
    </div>
    
    <div>
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" min="18" max="100" step="1">
    </div>
    
    <div>
        <label for="salary">Salary Range:</label>
        <input type="range" id="salary" name="salary" min="30000" max="200000" step="5000" value="50000">
        <output for="salary">$50,000</output>
    </div>
    
    <div>
        <label for="website">Website:</label>
        <input type="url" id="website" name="website" placeholder="https://example.com">
    </div>
    
    <div>
        <label for="favoriteColor">Favorite Color:</label>
        <input type="color" id="favoriteColor" name="favoriteColor" value="#ff0000">
    </div>
    
    <div>
        <label for="resume">Upload Resume:</label>
        <input type="file" id="resume" name="resume" accept=".pdf,.doc,.docx">
    </div>
    
    <div>
        <label for="photos">Upload Photos:</label>
        <input type="file" id="photos" name="photos" multiple accept="image/*">
    </div>
    
    <div>
        <label for="country">Country:</label>
        <select id="country" name="country" required>
            <option value="">Select a country</option>
            <option value="us">United States</option>
            <option value="ca">Canada</option>
            <option value="uk">United Kingdom</option>
            <option value="au">Australia</option>
        </select>
    </div>
    
    <div>
        <label for="skills">Skills:</label>
        <select id="skills" name="skills" multiple size="4">
            <option value="html">HTML</option>
            <option value="css">CSS</option>
            <option value="javascript">JavaScript</option>
            <option value="react">React</option>
            <option value="nodejs">Node.js</option>
        </select>
    </div>
    
    <div>
        <label for="browser">Choose your browser:</label>
        <input list="browsers" id="browser" name="browser">
        <datalist id="browsers">
            <option value="Chrome">
            <option value="Firefox">
            <option value="Safari">
            <option value="Edge">
            <option value="Opera">
        </datalist>
    </div>
</form>
```

### Form Validation
```html
<form novalidate>
    <div>
        <label for="username">Username (3-20 characters):</label>
        <input type="text" 
               id="username" 
               name="username" 
               required 
               minlength="3" 
               maxlength="20"
               pattern="[a-zA-Z0-9_]+"
               title="Username must contain only letters, numbers, and underscores">
    </div>
    
    <div>
        <label for="password">Password:</label>
        <input type="password" 
               id="password" 
               name="password" 
               required 
               minlength="8"
               pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}"
               title="Password must contain at least 8 characters with one number, one lowercase and one uppercase letter">
    </div>
    
    <div>
        <label for="confirmPassword">Confirm Password:</label>
        <input type="password" id="confirmPassword" name="confirmPassword" required>
    </div>
    
    <div>
        <input type="checkbox" id="terms" name="terms" required>
        <label for="terms">I agree to the terms and conditions</label>
    </div>
    
    <button type="submit">Create Account</button>
</form>
```

---

## Semantic HTML Elements

### Page Structure Elements
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Semantic HTML Example</title>
</head>
<body>
    <!-- Page header -->
    <header>
        <h1>My Website</h1>
        <nav>
            <ul>
                <li><a href="/">Home</a></li>
                <li><a href="/about">About</a></li>
                <li><a href="/blog">Blog</a></li>
                <li><a href="/contact">Contact</a></li>
            </ul>
        </nav>
    </header>
    
    <!-- Main content area -->
    <main>
        <!-- Individual article -->
        <article>
            <header>
                <h2>Article Title</h2>
                <p>Published on <time datetime="2024-01-15">January 15, 2024</time> by <address>John Doe</address></p>
            </header>
            
            <section>
                <h3>Introduction</h3>
                <p>This is the introduction section of the article...</p>
            </section>
            
            <section>
                <h3>Main Content</h3>
                <p>This is the main content section...</p>
                
                <aside>
                    <h4>Related Information</h4>
                    <p>This sidebar contains related information...</p>
                </aside>
            </section>
            
            <footer>
                <p>Tags: <a href="/tags/html">HTML</a>, <a href="/tags/semantic">Semantic</a></p>
            </footer>
        </article>
        
        <!-- Another article -->
        <article>
            <h2>Another Article</h2>
            <p>Content of another article...</p>
        </article>
    </main>
    
    <!-- Sidebar -->
    <aside>
        <section>
            <h3>Recent Posts</h3>
            <ul>
                <li><a href="/post1">Recent Post 1</a></li>
                <li><a href="/post2">Recent Post 2</a></li>
                <li><a href="/post3">Recent Post 3</a></li>
            </ul>
        </section>
        
        <section>
            <h3>Categories</h3>
            <ul>
                <li><a href="/category/web-dev">Web Development</a></li>
                <li><a href="/category/design">Design</a></li>
                <li><a href="/category/tutorials">Tutorials</a></li>
            </ul>
        </section>
    </aside>
    
    <!-- Page footer -->
    <footer>
        <section>
            <h3>Contact Information</h3>
            <address>
                <p>123 Web Street<br>
                Internet City, IC 12345<br>
                <a href="mailto:contact@example.com">contact@example.com</a></p>
            </address>
        </section>
        
        <section>
            <h3>Follow Us</h3>
            <ul>
                <li><a href="https://twitter.com/example">Twitter</a></li>
                <li><a href="https://facebook.com/example">Facebook</a></li>
                <li><a href="https://linkedin.com/company/example">LinkedIn</a></li>
            </ul>
        </section>
        
        <p>&copy; 2024 My Website. All rights reserved.</p>
    </footer>
</body>
</html>
```

### Content Sectioning Elements
```html
<!-- Details and Summary -->
<details>
    <summary>Click to expand FAQ</summary>
    <p>This is the answer to the frequently asked question. It's hidden by default and shown when the user clicks the summary.</p>
</details>

<!-- Dialog element -->
<dialog id="myDialog">
    <h2>Dialog Title</h2>
    <p>This is a modal dialog.</p>
    <button onclick="document.getElementById('myDialog').close()">Close</button>
</dialog>

<!-- Progress and Meter -->
<p>Download progress:</p>
<progress value="70" max="100">70%</progress>

<p>Disk usage:</p>
<meter value="6" min="0" max="10">6 out of 10</meter>

<p>Score:</p>
<meter value="0.6" optimum="0.8" high="0.6" low="0.4">60%</meter>
```

---

## HTML Attributes

### Global Attributes
```html
<!-- ID and Class -->
<div id="unique-identifier" class="container main-content">Content</div>

<!-- Data attributes -->
<button data-action="save" data-id="123" data-confirm="true">Save</button>

<!-- Title attribute (tooltip) -->
<abbr title="HyperText Markup Language">HTML</abbr>

<!-- Hidden attribute -->
<div hidden>This content is hidden</div>

<!-- Contenteditable -->
<div contenteditable="true">You can edit this text</div>

<!-- Draggable -->
<img src="image.jpg" alt="Draggable image" draggable="true">

<!-- Spellcheck -->
<textarea spellcheck="true">Check spelling in this text area</textarea>

<!-- Translate -->
<p translate="no">This text should not be translated</p>

<!-- Direction -->
<p dir="rtl">This text is right-to-left</p>

<!-- Access key -->
<button accesskey="s">Save (Alt+S)</button>

<!-- Tab index -->
<div tabindex="0">This div can receive focus</div>
<div tabindex="-1">This div can be focused programmatically</div>
```

### Form-Specific Attributes
```html
<!-- Autocomplete -->
<input type="text" name="firstName" autocomplete="given-name">
<input type="email" name="email" autocomplete="email">
<input type="tel" name="phone" autocomplete="tel">

<!-- Placeholder -->
<input type="text" placeholder="Enter your name">

<!-- Required -->
<input type="email" required>

<!-- Readonly and Disabled -->
<input type="text" value="Cannot be changed" readonly>
<input type="text" value="Cannot be used" disabled>

<!-- Multiple -->
<input type="file" multiple>
<select multiple>
    <option>Option 1</option>
    <option>Option 2</option>
</select>

<!-- Autofocus -->
<input type="text" autofocus>

<!-- Form attribute -->
<form id="myForm">
    <input type="text" name="field1">
</form>
<input type="text" name="field2" form="myForm">
```

---

## Accessibility in HTML

### ARIA Attributes
```html
<!-- Roles -->
<div role="button" tabindex="0">Custom Button</div>
<nav role="navigation">
<main role="main">
<aside role="complementary">

<!-- Labels and Descriptions -->
<button aria-label="Close dialog">×</button>
<input type="password" aria-describedby="pwd-help">
<div id="pwd-help">Password must be at least 8 characters</div>

<!-- States -->
<button aria-expanded="false" aria-controls="menu">Menu</button>
<div id="menu" aria-hidden="true">Menu content</div>

<!-- Live regions -->
<div aria-live="polite" id="status"></div>
<div aria-live="assertive" id="errors"></div>

<!-- Landmarks -->
<header role="banner">
<nav role="navigation">
<main role="main">
<aside role="complementary">
<footer role="contentinfo">
```

### Accessible Forms
```html
<form>
    <fieldset>
        <legend>Personal Information</legend>
        
        <div>
            <label for="name">Full Name <span aria-label="required">*</span></label>
            <input type="text" id="name" name="name" required aria-describedby="name-error">
            <div id="name-error" role="alert" aria-live="polite"></div>
        </div>
        
        <div role="group" aria-labelledby="contact-legend">
            <div id="contact-legend">Preferred Contact Method</div>
            <input type="radio" id="email-contact" name="contact" value="email">
            <label for="email-contact">Email</label>
            
            <input type="radio" id="phone-contact" name="contact" value="phone">
            <label for="phone-contact">Phone</label>
        </div>
    </fieldset>
    
    <button type="submit" aria-describedby="submit-help">Submit Form</button>
    <div id="submit-help">Submitting will send your information to our team</div>
</form>
```

### Skip Links and Navigation
```html
<body>
    <a href="#main-content" class="skip-link">Skip to main content</a>
    
    <header>
        <nav aria-label="Main navigation">
            <ul>
                <li><a href="/" aria-current="page">Home</a></li>
                <li><a href="/about">About</a></li>
                <li><a href="/contact">Contact</a></li>
            </ul>
        </nav>
    </header>
    
    <main id="main-content" tabindex="-1">
        <h1>Main Content</h1>
        <!-- Main content here -->
    </main>
</body>
```

---

## HTML Best Practices

### Document Structure
```html
<!-- ✅ Good: Proper document structure -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Descriptive Page Title</title>
</head>
<body>
    <header>
        <h1>Page Title</h1>
        <nav><!-- Navigation --></nav>
    </header>
    
    <main>
        <article>
            <h2>Article Title</h2>
            <p>Content...</p>
        </article>
    </main>
    
    <footer>
        <p>&copy; 2024 Company Name</p>
    </footer>
</body>
</html>

<!-- ❌ Bad: Missing structure -->
<html>
<head>
    <title>Page</title>
</head>
<body>
    <div>
        <div>Content</div>
    </div>
</body>
</html>
```

### Semantic Markup
```html
<!-- ✅ Good: Semantic elements -->
<article>
    <header>
        <h2>Article Title</h2>
        <time datetime="2024-01-15">January 15, 2024</time>
    </header>
    <p>Article content...</p>
    <footer>
        <p>Author: John Doe</p>
    </footer>
</article>

<!-- ❌ Bad: Non-semantic divs -->
<div class="article">
    <div class="title">Article Title</div>
    <div class="date">January 15, 2024</div>
    <div class="content">Article content...</div>
</div>
```

### Accessibility
```html
<!-- ✅ Good: Accessible form -->
<form>
    <label for="email">Email Address:</label>
    <input type="email" id="email" name="email" required>
    
    <button type="submit">Submit</button>
</form>

<!-- ❌ Bad: Inaccessible form -->
<form>
    <input type="email" placeholder="Email">
    <input type="submit" value="Submit">
</form>
```

### Performance
```html
<!-- ✅ Good: Optimized images -->
<img src="image.jpg" 
     alt="Descriptive alt text" 
     width="800" 
     height="400"
     loading="lazy">

<!-- ✅ Good: Preload critical resources -->
<link rel="preload" href="critical.css" as="style">
<link rel="preload" href="hero-image.jpg" as="image">

<!-- ✅ Good: Defer non-critical JavaScript -->
<script src="analytics.js" defer></script>
```

---

## Practice Projects

### Project 1: Personal Portfolio Page
Create a complete personal portfolio with:
- Semantic HTML structure
- Navigation menu
- About section with your photo
- Skills list
- Project showcase
- Contact form
- Proper accessibility attributes

### Project 2: Blog Article Page
Build a blog article page featuring:
- Article with proper heading hierarchy
- Author information and publication date
- Related articles sidebar
- Comment form
- Social sharing links
- Breadcrumb navigation

### Project 3: Restaurant Menu
Design a restaurant menu page with:
- Menu categories using description lists
- Food images with proper alt text
- Pricing table
- Reservation form with validation
- Contact information
- Opening hours

### Project 4: Event Registration Form
Create a comprehensive event registration form:
- Multi-step form with fieldsets
- Various input types (date, number, file upload)
- Form validation
- Accessibility features
- Progress indicator
- Terms and conditions

### Project 5: News Website Homepage
Build a news website homepage with:
- Header with logo and navigation
- Featured article section
- Article grid with images
- Sidebar with recent posts
- Newsletter signup form
- Footer with links and contact info

---

## Next Steps

After mastering HTML fundamentals:

1. **CSS Styling**: Learn to style your HTML elements
2. **JavaScript Interactivity**: Add dynamic behavior
3. **Responsive Design**: Make your sites work on all devices
4. **Web Accessibility**: Ensure your sites are usable by everyone
5. **Performance Optimization**: Make your sites load faster
6. **SEO Basics**: Help search engines understand your content

Remember: HTML is the foundation of all web development. Take time to master these fundamentals before moving on to more advanced topics. Practice building complete pages and always validate your HTML using tools like the W3C Markup Validator.