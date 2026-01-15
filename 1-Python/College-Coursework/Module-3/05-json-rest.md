# JSON and the REST Architecture

## 📚 Theoretical Understanding

### What is JSON?

JSON (JavaScript Object Notation) is a lightweight data interchange format that's easy for humans to read and write, and easy for machines to parse and generate. Despite its name suggesting a connection to JavaScript, JSON is language-independent and has become the de facto standard for data exchange on the web. JSON represents data using two structures: collections of key-value pairs (objects/dictionaries) and ordered lists of values (arrays/lists). This simplicity makes JSON ideal for APIs, configuration files, and data storage.

JSON's popularity stems from its balance of human readability and machine efficiency. Unlike XML, which uses verbose tags, JSON uses minimal syntax: curly braces for objects, square brackets for arrays, and simple key-value pairs. A JSON object like `{"name": "John", "age": 30}` is immediately understandable and compact. This efficiency matters when transmitting data over networks - smaller payloads mean faster transmission and lower bandwidth costs. Modern web APIs overwhelmingly prefer JSON over XML for these reasons.

Python's native support for JSON through the `json` module makes working with JSON data seamless. Python dictionaries map directly to JSON objects, and Python lists map to JSON arrays. This natural correspondence means you can easily convert between Python data structures and JSON with simple function calls. Understanding JSON is essential for modern web development, as virtually every web API you interact with will use JSON for data exchange.

### Understanding REST Architecture

REST (Representational State Transfer) is an architectural style for designing networked applications. Unlike SOAP, which is a protocol with strict rules, REST is a set of principles and constraints that, when followed, create scalable, maintainable web services. REST leverages the existing infrastructure of the web - HTTP methods, URLs, and status codes - to provide a simple, standardized way for systems to communicate.

The core principle of REST is treating everything as a resource identified by a URL. A resource could be a user, a product, an order, or any entity in your system. You interact with resources using standard HTTP methods: GET to retrieve, POST to create, PUT to update, DELETE to remove. This uniform interface makes REST APIs intuitive - if you understand HTTP, you understand REST. For example, `GET /users/123` retrieves user 123, `POST /users` creates a new user, `PUT /users/123` updates user 123, and `DELETE /users/123` deletes user 123.

REST's stateless nature means each request contains all information needed to process it - the server doesn't maintain client state between requests. This constraint enables scalability: any server can handle any request without needing to know about previous requests. While this might seem limiting, it actually simplifies system design and enables features like load balancing and caching. Understanding REST principles is crucial for designing modern web APIs and consuming third-party services.

### REST Principles

REST is defined by six key constraints that guide API design:

**1. Client-Server Architecture**: Separation of concerns between client (user interface) and server (data storage). This separation allows each to evolve independently.

**2. Stateless**: Each request from client to server must contain all information needed to understand and process the request. The server doesn't store client context between requests.

**3. Cacheable**: Responses must define themselves as cacheable or non-cacheable. Caching improves performance by eliminating redundant requests.

**4. Uniform Interface**: A standardized way to interact with resources using HTTP methods, URLs, and status codes. This simplifies architecture and improves visibility.

**5. Layered System**: Architecture can be composed of hierarchical layers (load balancers, caches, proxies) invisible to the client.

**6. Code on Demand (Optional)**: Servers can extend client functionality by transferring executable code (like JavaScript).

These principles create APIs that are scalable, maintainable, and easy to understand. Well-designed REST APIs feel intuitive because they follow web conventions that developers already know.

### Working with REST APIs in Python

Python's `requests` library makes consuming REST APIs straightforward. The library provides methods that map directly to HTTP verbs: `requests.get()`, `requests.post()`, `requests.put()`, `requests.delete()`. Combined with Python's `json` module for parsing responses, you can interact with any REST API in just a few lines of code.

Most REST APIs return JSON responses, which Python handles elegantly. The `requests` library can automatically parse JSON responses with the `.json()` method, converting JSON directly to Python dictionaries and lists. This seamless integration means you can focus on your application logic rather than data parsing. Understanding how to authenticate, handle errors, and process responses is essential for building applications that consume REST APIs.

---

## 💻 Practical Examples

### Basic JSON Operations

```python
import json

# Python dictionary
person = {
    "name": "John Doe",
    "age": 30,
    "city": "New York",
    "hobbies": ["reading", "coding", "gaming"],
    "is_student": False
}

# Convert Python to JSON string
json_string = json.dumps(person)
print(json_string)
# {"name": "John Doe", "age": 30, "city": "New York", ...}

# Pretty print JSON
json_pretty = json.dumps(person, indent=2)
print(json_pretty)

# Convert JSON string to Python
json_data = '{"name": "Jane", "age": 25}'
python_dict = json.loads(json_data)
print(python_dict["name"])  # Jane

# Write JSON to file
with open('person.json', 'w') as f:
    json.dump(person, f, indent=2)

# Read JSON from file
with open('person.json', 'r') as f:
    data = json.load(f)
    print(data["name"])
```

### REST API - GET Request

```python
import requests

# Simple GET request
response = requests.get('https://api.github.com/users/octocat')

# Check status
if response.status_code == 200:
    # Parse JSON response
    user = response.json()
    
    print(f"Username: {user['login']}")
    print(f"Name: {user['name']}")
    print(f"Public Repos: {user['public_repos']}")
    print(f"Followers: {user['followers']}")
else:
    print(f"Error: {response.status_code}")

# GET with parameters
params = {
    'q': 'python',
    'sort': 'stars',
    'order': 'desc'
}

response = requests.get(
    'https://api.github.com/search/repositories',
    params=params
)

if response.status_code == 200:
    data = response.json()
    repos = data['items'][:5]  # Top 5
    
    for repo in repos:
        print(f"{repo['name']}: {repo['stargazers_count']} stars")
```


### REST API - POST Request

```python
import requests

# POST request to create resource
new_post = {
    "title": "My First Post",
    "body": "This is the content of my post",
    "userId": 1
}

response = requests.post(
    'https://jsonplaceholder.typicode.com/posts',
    json=new_post  # Automatically converts to JSON and sets Content-Type
)

if response.status_code == 201:  # 201 Created
    created_post = response.json()
    print(f"Created post with ID: {created_post['id']}")
    print(created_post)
else:
    print(f"Error: {response.status_code}")

# POST with headers
headers = {
    'Authorization': 'Bearer YOUR_TOKEN_HERE',
    'Content-Type': 'application/json'
}

response = requests.post(
    'https://api.example.com/data',
    json=new_post,
    headers=headers
)
```

### REST API - PUT and DELETE

```python
import requests

# PUT request to update resource
updated_post = {
    "id": 1,
    "title": "Updated Title",
    "body": "Updated content",
    "userId": 1
}

response = requests.put(
    'https://jsonplaceholder.typicode.com/posts/1',
    json=updated_post
)

if response.status_code == 200:
    print("Post updated successfully")
    print(response.json())

# DELETE request to remove resource
response = requests.delete(
    'https://jsonplaceholder.typicode.com/posts/1'
)

if response.status_code == 200:
    print("Post deleted successfully")
```

### Complete REST API Client

```python
import requests
import json

class APIClient:
    """REST API client with error handling"""
    
    def __init__(self, base_url, api_key=None):
        self.base_url = base_url
        self.headers = {
            'Content-Type': 'application/json'
        }
        if api_key:
            self.headers['Authorization'] = f'Bearer {api_key}'
    
    def get(self, endpoint, params=None):
        """GET request"""
        url = f"{self.base_url}/{endpoint}"
        try:
            response = requests.get(
                url,
                params=params,
                headers=self.headers,
                timeout=10
            )
            response.raise_for_status()
            return response.json()
        except requests.exceptions.HTTPError as e:
            print(f"HTTP Error: {e}")
            return None
        except requests.exceptions.RequestException as e:
            print(f"Request Error: {e}")
            return None
    
    def post(self, endpoint, data):
        """POST request"""
        url = f"{self.base_url}/{endpoint}"
        try:
            response = requests.post(
                url,
                json=data,
                headers=self.headers,
                timeout=10
            )
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Error: {e}")
            return None
    
    def put(self, endpoint, data):
        """PUT request"""
        url = f"{self.base_url}/{endpoint}"
        try:
            response = requests.put(
                url,
                json=data,
                headers=self.headers,
                timeout=10
            )
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Error: {e}")
            return None
    
    def delete(self, endpoint):
        """DELETE request"""
        url = f"{self.base_url}/{endpoint}"
        try:
            response = requests.delete(
                url,
                headers=self.headers,
                timeout=10
            )
            response.raise_for_status()
            return True
        except requests.exceptions.RequestException as e:
            print(f"Error: {e}")
            return False

# Usage
client = APIClient('https://jsonplaceholder.typicode.com')

# GET
posts = client.get('posts', params={'userId': 1})
if posts:
    print(f"Found {len(posts)} posts")

# POST
new_post = {
    'title': 'New Post',
    'body': 'Content here',
    'userId': 1
}
created = client.post('posts', new_post)
if created:
    print(f"Created post: {created['id']}")

# PUT
updated = client.put('posts/1', new_post)

# DELETE
deleted = client.delete('posts/1')
```

---

## 🎯 Two Most Important Questions

### Question 1: Explain JSON data format and how to work with it in Python. What are the differences between `json.dumps()`/`json.loads()` and `json.dump()`/`json.load()`? Provide examples of converting between Python data structures and JSON.

**Detailed Answer:**

JSON (JavaScript Object Notation) is a lightweight data format for data exchange. Understanding JSON and Python's `json` module is essential for working with web APIs and data storage.

**JSON Data Types:**

JSON supports six data types:

1. **Object**: Key-value pairs (Python dict)
   ```json
   {"name": "John", "age": 30}
   ```

2. **Array**: Ordered list (Python list)
   ```json
   ["apple", "banana", "cherry"]
   ```

3. **String**: Text in double quotes
   ```json
   "Hello, World!"
   ```

4. **Number**: Integer or float
   ```json
   42, 3.14
   ```

5. **Boolean**: true or false
   ```json
   true, false
   ```

6. **Null**: Represents absence of value
   ```json
   null
   ```

**Python to JSON Mapping:**

| Python | JSON |
|--------|------|
| dict | object |
| list, tuple | array |
| str | string |
| int, float | number |
| True | true |
| False | false |
| None | null |

**json.dumps() vs json.dump():**

**`json.dumps()`** - Dump String
- Converts Python object to JSON **string**
- "s" stands for "string"
- Returns a string

```python
import json

data = {"name": "John", "age": 30}

# dumps() returns JSON string
json_string = json.dumps(data)
print(type(json_string))  # <class 'str'>
print(json_string)  # {"name": "John", "age": 30}

# Pretty print with indent
json_pretty = json.dumps(data, indent=2)
print(json_pretty)
# {
#   "name": "John",
#   "age": 30
# }

# Sort keys
json_sorted = json.dumps(data, sort_keys=True)
```

**`json.dump()`** - Dump to File
- Writes Python object to JSON **file**
- Takes file object as second argument
- Returns None

```python
import json

data = {"name": "John", "age": 30}

# dump() writes to file
with open('data.json', 'w') as f:
    json.dump(data, f, indent=2)

# File now contains JSON
```

**json.loads() vs json.load():**

**`json.loads()`** - Load String
- Parses JSON **string** to Python object
- "s" stands for "string"
- Takes string as argument

```python
import json

# loads() parses JSON string
json_string = '{"name": "John", "age": 30}'
data = json.loads(json_string)

print(type(data))  # <class 'dict'>
print(data["name"])  # John
```

**`json.load()`** - Load from File
- Reads JSON from **file** to Python object
- Takes file object as argument

```python
import json

# load() reads from file
with open('data.json', 'r') as f:
    data = json.load(f)

print(data["name"])  # John
```

**Memory Aid:**
- **"s" functions** (dumps/loads) work with **strings**
- **No "s" functions** (dump/load) work with **files**

**Complete Example:**

```python
import json

# Complex Python data structure
data = {
    "users": [
        {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com",
            "active": True,
            "roles": ["admin", "user"],
            "metadata": None
        },
        {
            "id": 2,
            "name": "Jane Smith",
            "email": "jane@example.com",
            "active": False,
            "roles": ["user"],
            "metadata": {"department": "Sales"}
        }
    ],
    "total": 2
}

# 1. Python to JSON string
json_string = json.dumps(data, indent=2)
print("JSON String:")
print(json_string)

# 2. JSON string to Python
parsed_data = json.loads(json_string)
print(f"\nFirst user: {parsed_data['users'][0]['name']}")

# 3. Python to JSON file
with open('users.json', 'w') as f:
    json.dump(data, f, indent=2)
print("\nData written to users.json")

# 4. JSON file to Python
with open('users.json', 'r') as f:
    loaded_data = json.load(f)
print(f"Loaded {loaded_data['total']} users from file")

# Verify data integrity
print(f"Data matches: {data == loaded_data}")  # True
```

**Handling Special Cases:**

```python
import json
from datetime import datetime

# Custom encoder for non-JSON types
class DateTimeEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return super().default(obj)

data = {
    "event": "Meeting",
    "timestamp": datetime.now()
}

# Use custom encoder
json_string = json.dumps(data, cls=DateTimeEncoder)
print(json_string)

# Handling errors
try:
    invalid_json = '{"name": "John", "age": }'
    data = json.loads(invalid_json)
except json.JSONDecodeError as e:
    print(f"JSON Error: {e}")
```

**Conclusion:**

JSON is a lightweight data format mapping naturally to Python structures. Use `dumps()`/`loads()` for strings, `dump()`/`load()` for files. The "s" indicates string operations. JSON is essential for web APIs, configuration files, and data exchange.

---


### Question 2: Explain REST architecture and its principles. How do you consume REST APIs using Python's requests library? Demonstrate the four main HTTP methods (GET, POST, PUT, DELETE) with practical examples, including authentication and error handling.

**Detailed Answer:**

REST (Representational State Transfer) is an architectural style for designing networked applications. Understanding REST principles and how to consume REST APIs is essential for modern software development.

**REST Principles:**

**1. Resource-Based**
Everything is a resource identified by a URL:
- `/users` - Collection of users
- `/users/123` - Specific user
- `/users/123/posts` - User's posts

**2. HTTP Methods**
Standard operations on resources:
- **GET**: Retrieve resource
- **POST**: Create resource
- **PUT**: Update resource
- **DELETE**: Remove resource

**3. Stateless**
Each request contains all needed information. Server doesn't store client state.

**4. Representations**
Resources can have multiple representations (JSON, XML, HTML).

**5. HATEOAS** (Hypermedia as the Engine of Application State)
Responses include links to related resources.

**HTTP Methods in Detail:**

**GET - Retrieve Resources**

GET requests retrieve data without modifying it. They should be idempotent (multiple identical requests have the same effect as one).

```python
import requests

# GET single resource
response = requests.get('https://api.example.com/users/123')

if response.status_code == 200:
    user = response.json()
    print(f"User: {user['name']}")
elif response.status_code == 404:
    print("User not found")
else:
    print(f"Error: {response.status_code}")

# GET collection with query parameters
params = {
    'page': 1,
    'limit': 10,
    'sort': 'name',
    'filter': 'active'
}

response = requests.get(
    'https://api.example.com/users',
    params=params
)

if response.status_code == 200:
    users = response.json()
    print(f"Found {len(users)} users")
    for user in users:
        print(f"  - {user['name']}")

# GET with headers
headers = {
    'Accept': 'application/json',
    'User-Agent': 'MyApp/1.0'
}

response = requests.get(
    'https://api.example.com/data',
    headers=headers
)
```

**POST - Create Resources**

POST requests create new resources. The server typically assigns an ID and returns the created resource.

```python
import requests

# POST to create resource
new_user = {
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30
}

response = requests.post(
    'https://api.example.com/users',
    json=new_user  # Automatically sets Content-Type: application/json
)

if response.status_code == 201:  # 201 Created
    created_user = response.json()
    print(f"Created user with ID: {created_user['id']}")
    print(f"Location: {response.headers.get('Location')}")
elif response.status_code == 400:  # Bad Request
    errors = response.json()
    print(f"Validation errors: {errors}")
else:
    print(f"Error: {response.status_code}")

# POST with authentication
headers = {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json'
}

response = requests.post(
    'https://api.example.com/users',
    json=new_user,
    headers=headers
)
```

**PUT - Update Resources**

PUT requests update existing resources. They should be idempotent (multiple identical requests have the same effect).

```python
import requests

# PUT to update entire resource
updated_user = {
    "id": 123,
    "name": "John Doe Updated",
    "email": "john.new@example.com",
    "age": 31
}

response = requests.put(
    'https://api.example.com/users/123',
    json=updated_user
)

if response.status_code == 200:
    user = response.json()
    print(f"Updated user: {user['name']}")
elif response.status_code == 404:
    print("User not found")
else:
    print(f"Error: {response.status_code}")

# PATCH for partial updates (alternative to PUT)
partial_update = {
    "email": "john.new@example.com"
}

response = requests.patch(
    'https://api.example.com/users/123',
    json=partial_update
)
```

**DELETE - Remove Resources**

DELETE requests remove resources. They should be idempotent (deleting the same resource multiple times has the same effect).

```python
import requests

# DELETE resource
response = requests.delete('https://api.example.com/users/123')

if response.status_code == 204:  # 204 No Content
    print("User deleted successfully")
elif response.status_code == 404:
    print("User not found")
elif response.status_code == 403:
    print("Permission denied")
else:
    print(f"Error: {response.status_code}")

# DELETE with confirmation
response = requests.delete(
    'https://api.example.com/users/123',
    headers={'Authorization': 'Bearer TOKEN'}
)
```

**Authentication:**

**1. API Key Authentication**

```python
# API key in header
headers = {'X-API-Key': 'your_api_key_here'}
response = requests.get(url, headers=headers)

# API key in query parameter
params = {'api_key': 'your_api_key_here'}
response = requests.get(url, params=params)
```

**2. Bearer Token (OAuth)**

```python
headers = {'Authorization': 'Bearer YOUR_ACCESS_TOKEN'}
response = requests.get(url, headers=headers)
```

**3. Basic Authentication**

```python
from requests.auth import HTTPBasicAuth

response = requests.get(
    url,
    auth=HTTPBasicAuth('username', 'password')
)

# Or shorthand
response = requests.get(url, auth=('username', 'password'))
```

**Complete Example with Error Handling:**

```python
import requests
import time

class RESTClient:
    """REST API client with comprehensive error handling"""
    
    def __init__(self, base_url, api_key=None):
        self.base_url = base_url.rstrip('/')
        self.session = requests.Session()
        
        if api_key:
            self.session.headers.update({
                'Authorization': f'Bearer {api_key}'
            })
        
        self.session.headers.update({
            'Content-Type': 'application/json',
            'Accept': 'application/json'
        })
    
    def _request(self, method, endpoint, **kwargs):
        """Make HTTP request with error handling"""
        url = f"{self.base_url}/{endpoint.lstrip('/')}"
        
        try:
            response = self.session.request(
                method,
                url,
                timeout=kwargs.pop('timeout', 30),
                **kwargs
            )
            
            # Handle rate limiting
            if response.status_code == 429:
                retry_after = int(response.headers.get('Retry-After', 60))
                print(f"Rate limited. Waiting {retry_after} seconds...")
                time.sleep(retry_after)
                return self._request(method, endpoint, **kwargs)
            
            # Raise exception for HTTP errors
            response.raise_for_status()
            
            # Return JSON if available
            if response.content:
                return response.json()
            return None
        
        except requests.exceptions.HTTPError as e:
            print(f"HTTP Error: {e}")
            if e.response.content:
                print(f"Response: {e.response.json()}")
            raise
        
        except requests.exceptions.ConnectionError:
            print("Connection error. Check your internet connection.")
            raise
        
        except requests.exceptions.Timeout:
            print("Request timed out.")
            raise
        
        except requests.exceptions.RequestException as e:
            print(f"Request error: {e}")
            raise
    
    def get(self, endpoint, params=None):
        """GET request"""
        return self._request('GET', endpoint, params=params)
    
    def post(self, endpoint, data=None):
        """POST request"""
        return self._request('POST', endpoint, json=data)
    
    def put(self, endpoint, data=None):
        """PUT request"""
        return self._request('PUT', endpoint, json=data)
    
    def delete(self, endpoint):
        """DELETE request"""
        return self._request('DELETE', endpoint)

# Usage example
def main():
    # Initialize client
    client = RESTClient(
        'https://jsonplaceholder.typicode.com',
        api_key='your_api_key'  # Optional
    )
    
    try:
        # GET - Retrieve users
        print("Fetching users...")
        users = client.get('users', params={'_limit': 5})
        print(f"Found {len(users)} users")
        
        # POST - Create user
        print("\nCreating user...")
        new_user = {
            'name': 'John Doe',
            'email': 'john@example.com',
            'username': 'johndoe'
        }
        created = client.post('users', data=new_user)
        print(f"Created user with ID: {created['id']}")
        
        # PUT - Update user
        print("\nUpdating user...")
        updated_user = {
            'id': 1,
            'name': 'John Doe Updated',
            'email': 'john.updated@example.com',
            'username': 'johndoe'
        }
        updated = client.put('users/1', data=updated_user)
        print(f"Updated user: {updated['name']}")
        
        # DELETE - Remove user
        print("\nDeleting user...")
        client.delete('users/1')
        print("User deleted successfully")
    
    except requests.exceptions.RequestException as e:
        print(f"API request failed: {e}")

if __name__ == '__main__':
    main()
```

**Best Practices:**

1. **Use Sessions**: Reuse connections for better performance
2. **Set Timeouts**: Prevent hanging requests
3. **Handle Errors**: Check status codes and handle exceptions
4. **Respect Rate Limits**: Implement backoff strategies
5. **Use HTTPS**: Secure communication
6. **Version APIs**: Include version in URL (`/api/v1/users`)
7. **Authenticate Securely**: Use tokens, not passwords in URLs
8. **Log Requests**: For debugging and monitoring

**Conclusion:**

REST uses HTTP methods (GET, POST, PUT, DELETE) to operate on resources identified by URLs. Python's `requests` library makes consuming REST APIs simple. Always handle errors, implement authentication, and respect rate limits. REST's simplicity and use of web standards make it the dominant architecture for modern web APIs.

---

## ✅ Key Takeaways

1. JSON is a lightweight data format for data exchange
2. Use `dumps()`/`loads()` for strings, `dump()`/`load()` for files
3. REST is an architectural style using HTTP methods and URLs
4. GET retrieves, POST creates, PUT updates, DELETE removes
5. REST APIs are stateless and resource-based
6. Use `requests` library for consuming REST APIs
7. Always handle HTTP status codes and errors
8. Implement authentication (API keys, tokens, basic auth)
9. Respect rate limits and implement retry logic
10. JSON and REST are the standard for modern web APIs

---

**Previous Topic**: Web Services and XML
**Module**: 3 - Using Python to Access Web Data

**🎉 Congratulations! You've completed Module 3!**

Happy Learning! 🐍
