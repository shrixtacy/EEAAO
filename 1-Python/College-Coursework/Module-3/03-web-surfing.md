# Programs that Surf the Web

## 📚 Theoretical Understanding

### What is Web Scraping?

Web scraping is the automated process of extracting data from websites. Instead of manually copying information from web pages, you write programs that fetch web pages, parse their HTML content, and extract the specific data you need. Web scraping is essential for data collection, price monitoring, research, content aggregation, and many other applications where you need to gather information from the web programmatically.

The web is built on HTTP (Hypertext Transfer Protocol), which defines how browsers and servers communicate. When you visit a website, your browser sends an HTTP request to the server, which responds with HTML, CSS, JavaScript, and other resources. Web scraping programs simulate this process: they send HTTP requests, receive responses, and process the HTML to extract useful information. Python's `requests` library makes sending HTTP requests simple, while libraries like BeautifulSoup make parsing HTML straightforward.

However, web scraping comes with responsibilities. Websites have terms of service that may prohibit scraping, and excessive requests can overload servers. Always check a website's `robots.txt` file (which specifies scraping rules), respect rate limits, and consider using official APIs when available. Ethical web scraping means being a good internet citizen: identifying your bot with a user agent, limiting request frequency, and respecting website policies.

### The requests Library

Python's `requests` library is the standard tool for making HTTP requests. It provides a simple, elegant API for GET requests (retrieving data), POST requests (submitting data), and other HTTP methods. Unlike the lower-level `urllib` library, `requests` handles many complexities automatically: connection pooling, cookies, redirects, and encoding. Its intuitive design makes it the go-to choice for HTTP communication in Python.

A basic GET request with `requests` is as simple as `requests.get(url)`, which returns a Response object containing the server's response. This object provides access to the response content (`.text` for text, `.content` for bytes, `.json()` for JSON), status code (`.status_code`), headers (`.headers`), and more. The library also handles sessions, authentication, timeouts, and error handling, making it suitable for both simple scripts and complex applications.

### Parsing HTML with BeautifulSoup

HTML is structured markup, but it's designed for browsers, not programs. BeautifulSoup is a Python library that parses HTML and provides a convenient interface for navigating and searching the document tree. It handles malformed HTML gracefully, making it robust for real-world web pages that may not be perfectly formatted.

BeautifulSoup represents HTML as a tree of tags. You can search for tags by name, attributes, or CSS selectors. Methods like `find()`, `find_all()`, and `select()` let you locate specific elements. Once you've found an element, you can access its text content, attributes, or navigate to parent, sibling, or child elements. This makes extracting data from complex HTML structures straightforward and readable.

### Web Scraping Best Practices

Responsible web scraping requires following several best practices. First, always check `robots.txt` at the website's root (e.g., `example.com/robots.txt`) to see which pages you're allowed to scrape. Second, identify your bot with a descriptive User-Agent header so website administrators know who's accessing their site. Third, implement rate limiting to avoid overwhelming servers - add delays between requests using `time.sleep()`. Fourth, handle errors gracefully: websites may be temporarily unavailable, return unexpected content, or block your requests. Finally, prefer official APIs over scraping when available - APIs are designed for programmatic access and are more reliable and ethical.

---

## 💻 Practical Examples

### Basic HTTP Request

```python
import requests

# Simple GET request
response = requests.get('https://www.example.com')

# Check if request was successful
if response.status_code == 200:
    print("Success!")
    print(response.text[:500])  # First 500 characters
else:
    print(f"Error: {response.status_code}")

# Access response properties
print(f"Status Code: {response.status_code}")
print(f"Content Type: {response.headers['Content-Type']}")
print(f"Encoding: {response.encoding}")
```

### Fetching and Parsing HTML

```python
import requests
from bs4 import BeautifulSoup

# Fetch webpage
url = 'https://www.example.com'
response = requests.get(url)

# Parse HTML
soup = BeautifulSoup(response.text, 'html.parser')

# Extract title
title = soup.find('title')
print(f"Page Title: {title.text}")

# Find all links
links = soup.find_all('a')
print(f"\nFound {len(links)} links:")
for link in links[:5]:  # First 5 links
    href = link.get('href')
    text = link.text.strip()
    print(f"  {text}: {href}")

# Find all paragraphs
paragraphs = soup.find_all('p')
print(f"\nFound {len(paragraphs)} paragraphs")
```


### Web Scraping with Error Handling

```python
import requests
from bs4 import BeautifulSoup
import time

def scrape_safely(url, delay=1):
    """Scrape URL with proper error handling"""
    try:
        # Set user agent
        headers = {
            'User-Agent': 'Mozilla/5.0 (Educational Bot)'
        }
        
        # Make request with timeout
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()  # Raise exception for bad status
        
        # Parse HTML
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Add delay to be respectful
        time.sleep(delay)
        
        return soup
    
    except requests.exceptions.Timeout:
        print(f"Timeout error for {url}")
    except requests.exceptions.HTTPError as e:
        print(f"HTTP error: {e}")
    except requests.exceptions.RequestException as e:
        print(f"Request error: {e}")
    except Exception as e:
        print(f"Unexpected error: {e}")
    
    return None

# Usage
soup = scrape_safely('https://www.example.com')
if soup:
    print(soup.title.text)
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain how to use the `requests` library to fetch web pages and handle different HTTP status codes. What are the common HTTP status codes, and how should your program respond to each? Provide examples demonstrating proper error handling.

**Detailed Answer:**

The `requests` library is Python's standard tool for making HTTP requests. Understanding HTTP status codes and proper error handling is essential for robust web scraping and API interaction.

**Basic requests Usage:**

```python
import requests

# Simple GET request
response = requests.get('https://www.example.com')

# Access response properties
print(f"Status Code: {response.status_code}")
print(f"Content: {response.text[:100]}")
print(f"Headers: {response.headers}")
```

**HTTP Status Codes:**

Status codes indicate the result of an HTTP request. They're grouped into five categories:

**1xx - Informational (Rare)**
- 100 Continue: Server received request headers
- 101 Switching Protocols: Changing protocols

**2xx - Success**
- **200 OK**: Request succeeded, most common success code
- **201 Created**: Resource created (POST requests)
- **204 No Content**: Success but no content to return

**3xx - Redirection**
- **301 Moved Permanently**: Resource permanently moved
- **302 Found**: Temporary redirect
- **304 Not Modified**: Cached version is still valid

**4xx - Client Errors**
- **400 Bad Request**: Invalid request syntax
- **401 Unauthorized**: Authentication required
- **403 Forbidden**: Server refuses request
- **404 Not Found**: Resource doesn't exist
- **429 Too Many Requests**: Rate limit exceeded

**5xx - Server Errors**
- **500 Internal Server Error**: Server encountered error
- **502 Bad Gateway**: Invalid response from upstream server
- **503 Service Unavailable**: Server temporarily unavailable

**Handling Status Codes:**

```python
import requests
import time

def fetch_with_handling(url):
    """Fetch URL with comprehensive error handling"""
    try:
        response = requests.get(url, timeout=10)
        
        # Check status code
        if response.status_code == 200:
            print("Success!")
            return response.text
        
        elif response.status_code == 301 or response.status_code == 302:
            print(f"Redirected to: {response.url}")
            return response.text  # requests follows redirects automatically
        
        elif response.status_code == 404:
            print("Page not found")
            return None
        
        elif response.status_code == 429:
            print("Rate limited. Waiting...")
            time.sleep(60)  # Wait before retry
            return fetch_with_handling(url)  # Retry
        
        elif response.status_code >= 500:
            print(f"Server error: {response.status_code}")
            return None
        
        else:
            print(f"Unexpected status: {response.status_code}")
            return None
    
    except requests.exceptions.Timeout:
        print("Request timed out")
        return None
    except requests.exceptions.ConnectionError:
        print("Connection error")
        return None
    except Exception as e:
        print(f"Error: {e}")
        return None

# Usage
content = fetch_with_handling('https://www.example.com')
```

**Using raise_for_status():**

```python
# Automatic exception for bad status codes
try:
    response = requests.get(url)
    response.raise_for_status()  # Raises HTTPError for 4xx/5xx
    print("Success!")
except requests.exceptions.HTTPError as e:
    print(f"HTTP Error: {e}")
```

**Conclusion:**

Always check status codes and handle errors appropriately. Use `raise_for_status()` for simple error handling, or check specific codes for custom behavior. Implement retries for temporary errors (429, 503) and respect rate limits.

---

### Question 2: Explain how to use BeautifulSoup to parse HTML and extract specific data. What are the different methods for finding elements (find, find_all, select), and when should you use each? Provide practical examples of extracting data from real HTML structures.

**Detailed Answer:**

BeautifulSoup is Python's most popular HTML parsing library. Understanding its methods for finding and extracting data is essential for effective web scraping.

**Basic BeautifulSoup Usage:**

```python
from bs4 import BeautifulSoup
import requests

# Fetch and parse
response = requests.get('https://www.example.com')
soup = BeautifulSoup(response.text, 'html.parser')

# Access elements
title = soup.title.text
print(f"Title: {title}")
```

**Finding Methods:**

**1. find() - First Match**

```python
# Find first occurrence
first_link = soup.find('a')
print(first_link.text)
print(first_link.get('href'))

# Find with attributes
main_div = soup.find('div', class_='main')
header = soup.find('h1', id='title')
```

**2. find_all() - All Matches**

```python
# Find all occurrences
all_links = soup.find_all('a')
for link in all_links:
    print(link.get('href'))

# Limit results
first_five = soup.find_all('p', limit=5)

# Multiple tags
headers = soup.find_all(['h1', 'h2', 'h3'])
```

**3. select() - CSS Selectors**

```python
# CSS selectors (most powerful)
articles = soup.select('div.article')
nav_links = soup.select('nav a')
specific = soup.select('#main .content p')
```

**When to Use Each:**

- **find()**: When you need the first occurrence
- **find_all()**: When you need all occurrences
- **select()**: When you know CSS selectors (most flexible)

**Practical Example:**

```python
from bs4 import BeautifulSoup
import requests

def scrape_articles(url):
    """Extract article titles and links"""
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Find all article containers
    articles = soup.find_all('article', class_='post')
    
    results = []
    for article in articles:
        # Extract title
        title_elem = article.find('h2', class_='title')
        title = title_elem.text.strip() if title_elem else 'No title'
        
        # Extract link
        link_elem = article.find('a')
        link = link_elem.get('href') if link_elem else None
        
        # Extract date
        date_elem = article.find('time')
        date = date_elem.get('datetime') if date_elem else None
        
        results.append({
            'title': title,
            'link': link,
            'date': date
        })
    
    return results

# Usage
articles = scrape_articles('https://blog.example.com')
for article in articles:
    print(f"{article['title']} - {article['date']}")
```

**Conclusion:**

BeautifulSoup provides multiple ways to find elements. Use `find()` for single elements, `find_all()` for multiple elements, and `select()` for complex CSS selectors. Always handle missing elements gracefully with conditional checks.

---

## ✅ Key Takeaways

1. Web scraping extracts data from websites programmatically
2. `requests` library makes HTTP requests simple
3. BeautifulSoup parses HTML and extracts data
4. Always check robots.txt and respect website policies
5. Handle HTTP status codes appropriately
6. Implement rate limiting to avoid overloading servers
7. Use descriptive User-Agent headers
8. Handle errors gracefully (timeouts, connection errors)
9. Prefer official APIs over scraping when available
10. Be ethical: respect terms of service and rate limits

---

**Next Topic**: Web Services and XML
**Previous Topic**: Networks and Sockets
**Module**: 3 - Using Python to Access Web Data

Happy Learning! 🐍
