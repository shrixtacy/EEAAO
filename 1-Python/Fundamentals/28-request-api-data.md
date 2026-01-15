# Python Request API Data - Complete Guide

## 📚 Table of Contents
1. [requests Library](#requests-library)
2. [GET Requests](#get-requests)
3. [POST Requests](#post-requests)
4. [JSON Handling](#json-handling)
5. [API Authentication](#api-authentication)
6. [Error Handling](#error-handling)
7. [Practice Questions](#practice-questions)

---

## 1. requests Library

```python
# Install requests
# pip install requests

import requests

# Basic GET request
response = requests.get('https://api.github.com')
print(response.status_code)  # 200
print(response.text)  # Response body
```

---

## 2. GET Requests

```python
import requests

# Simple GET request
response = requests.get('https://jsonplaceholder.typicode.com/posts/1')

print(f"Status Code: {response.status_code}")
print(f"Content: {response.json()}")

# GET with parameters
params = {'userId': 1}
response = requests.get('https://jsonplaceholder.typicode.com/posts', params=params)
posts = response.json()

for post in posts:
    print(f"Title: {post['title']}")
```

---

## 3. POST Requests

```python
import requests

# POST request
data = {
    'title': 'My Post',
    'body': 'This is the content',
    'userId': 1
}

response = requests.post('https://jsonplaceholder.typicode.com/posts', json=data)

print(f"Status Code: {response.status_code}")
print(f"Response: {response.json()}")
```

---

## 4. JSON Handling

```python
import requests
import json

# GET JSON data
response = requests.get('https://api.github.com/users/github')
user_data = response.json()

print(f"Name: {user_data['name']}")
print(f"Followers: {user_data['followers']}")

# POST JSON data
headers = {'Content-Type': 'application/json'}
data = {'key': 'value'}

response = requests.post('https://httpbin.org/post', 
                        data=json.dumps(data), 
                        headers=headers)
```

---

## 5. API Authentication

```python
import requests

# Basic Authentication
response = requests.get('https://api.example.com/data',
                       auth=('username', 'password'))

# Token Authentication
headers = {'Authorization': 'Bearer YOUR_TOKEN_HERE'}
response = requests.get('https://api.example.com/data', headers=headers)

# API Key in URL
params = {'api_key': 'YOUR_API_KEY'}
response = requests.get('https://api.example.com/data', params=params)
```

---

## 6. Error Handling

```python
import requests

try:
    response = requests.get('https://api.example.com/data', timeout=5)
    response.raise_for_status()  # Raise exception for bad status codes
    
    data = response.json()
    print(data)
    
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.HTTPError as e:
    print(f"HTTP Error: {e}")
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

### Complete API Example

```python
import requests

class WeatherAPI:
    def __init__(self, api_key):
        self.api_key = api_key
        self.base_url = "https://api.openweathermap.org/data/2.5/weather"
    
    def get_weather(self, city):
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric'
        }
        
        try:
            response = requests.get(self.base_url, params=params, timeout=10)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            return {'error': str(e)}
    
    def display_weather(self, city):
        data = self.get_weather(city)
        
        if 'error' in data:
            print(f"Error: {data['error']}")
            return
        
        print(f"\nWeather in {city}:")
        print(f"Temperature: {data['main']['temp']}°C")
        print(f"Feels like: {data['main']['feels_like']}°C")
        print(f"Description: {data['weather'][0]['description']}")
        print(f"Humidity: {data['main']['humidity']}%")

# Usage
# api = WeatherAPI('YOUR_API_KEY')
# api.display_weather('London')
```

---

## 7. Practice Questions

### Beginner Level

**Question 1**: Make a GET request to a public API and print the response.
```python
# Your code here
```

**Question 2**: Fetch user data from JSONPlaceholder API.
```python
# Your code here
```

**Question 3**: Create a function to check if a website is online.
```python
# Your code here
```

**Question 4**: Download an image from a URL and save it.
```python
# Your code here
```

**Question 5**: Make a POST request to create a new resource.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Build a weather app that fetches data from OpenWeatherMap API.
```python
# Your code here
```

**Question 7**: Create a GitHub repository viewer using GitHub API.
```python
# Your code here
```

**Question 8**: Build a currency converter using an exchange rate API.
```python
# Your code here
```

**Question 9**: Create a news aggregator using a news API.
```python
# Your code here
```

**Question 10**: Build a movie search app using OMDB API.
```python
# Your code here
```

### Advanced Level

**Question 11**: Create a rate-limited API client with retry logic.
```python
# Your code here
```

**Question 12**: Build a caching layer for API responses.
```python
# Your code here
```

**Question 13**: Implement OAuth 2.0 authentication flow.
```python
# Your code here
```

**Question 14**: Create an API wrapper class with pagination support.
```python
# Your code here
```

**Question 15**: Build a webhook receiver and processor.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **requests.get()** - GET requests
2. **requests.post()** - POST requests
3. **response.json()** - Parse JSON response
4. **response.status_code** - HTTP status code
5. **timeout** parameter prevents hanging
6. Always handle exceptions
7. Use **headers** for authentication
8. **params** for URL parameters
9. **raise_for_status()** for error checking
10. Test with public APIs first

---

## 📚 Popular Public APIs

- **JSONPlaceholder**: https://jsonplaceholder.typicode.com/
- **GitHub API**: https://api.github.com
- **OpenWeatherMap**: https://openweathermap.org/api
- **REST Countries**: https://restcountries.com/
- **Dog API**: https://dog.ceo/dog-api/
- **Cat Facts**: https://catfact.ninja/
- **Advice Slip**: https://api.adviceslip.com/
- **Chuck Norris Jokes**: https://api.chucknorris.io/

---

## 📚 Additional Resources

- requests Documentation: https://requests.readthedocs.io/
- HTTP Status Codes: https://httpstatuses.com/
- REST API Tutorial: https://restfulapi.net/
- Public APIs List: https://github.com/public-apis/public-apis

---

**Next Topic**: PyQt5 GUI - Introduction
**Previous Topic**: Multithreading

Happy Coding! 🐍
