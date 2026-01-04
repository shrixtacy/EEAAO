# Python Tools - Problem-Solution Mapping

Stop tool shopping. Start solving problems. This guide maps specific problems to the right Python tools, with honest assessments of when NOT to use each tool.

## Development Environment Tools

### Code Quality and Formatting

**Black - Code Formatter**
```bash
pip install black
black your_file.py
```
- **Problem it solves**: Eliminates code formatting debates and ensures consistent style
- **When NOT to use**: When you have strong style preferences that conflict with Black's opinionated formatting
- **Free tier**: Completely free and open source
- **Real-world usage**: Run in CI/CD pipelines, integrate with pre-commit hooks
- **Alternative**: autopep8 (less opinionated), yapf (more configurable)

**isort - Import Sorter**
```bash
pip install isort
isort your_file.py --profile black  # Compatible with Black
```
- **Problem it solves**: Automatically sorts and organizes import statements
- **When NOT to use**: For projects with complex import requirements that need manual organization
- **Free tier**: Completely free
- **Real-world usage**: Combine with Black for complete code formatting automation

**flake8 - Linting**
```bash
pip install flake8
flake8 your_project/
```
- **Problem it solves**: Catches style violations, unused imports, and potential bugs
- **When NOT to use**: When you want more comprehensive static analysis (use pylint instead)
- **Free tier**: Completely free
- **Configuration example**:
```ini
# setup.cfg
[flake8]
max-line-length = 88
extend-ignore = E203, W503
exclude = .git,__pycache__,venv
```

**mypy - Static Type Checking**
```bash
pip install mypy
mypy your_project/
```
- **Problem it solves**: Catches type-related bugs before runtime
- **When NOT to use**: For small scripts or when team isn't ready for type hints
- **Free tier**: Completely free
- **Example usage**:
```python
def calculate_total(price: float, tax_rate: float) -> float:
    return price * (1 + tax_rate)

# mypy will catch: calculate_total("10", 0.08)  # Error: str not compatible with float
```

### Testing Tools

**pytest - Testing Framework**
```bash
pip install pytest
pytest tests/
```
- **Problem it solves**: Comprehensive testing with better error messages and powerful fixtures
- **When NOT to use**: For very simple projects where unittest is sufficient
- **Free tier**: Completely free
- **Example test**:
```python
import pytest

def test_user_creation():
    user = create_user("test@example.com", "password123")
    assert user.email == "test@example.com"
    assert user.is_active is True

@pytest.fixture
def sample_user():
    return create_user("fixture@example.com", "password123")

def test_user_login(sample_user):
    assert authenticate_user(sample_user.email, "password123") is not None
```

**pytest-cov - Coverage Testing**
```bash
pip install pytest-cov
pytest --cov=your_package tests/
```
- **Problem it solves**: Shows which parts of your code are tested
- **When NOT to use**: When you're focused on functionality over coverage metrics
- **Free tier**: Completely free
- **Usage**: Aim for 80%+ coverage on critical business logic

**Hypothesis - Property-Based Testing**
```bash
pip install hypothesis
```
- **Problem it solves**: Automatically generates test cases to find edge cases you didn't think of
- **When NOT to use**: For simple functions or when team isn't familiar with property-based testing
- **Free tier**: Completely free
- **Example**:
```python
from hypothesis import given, strategies as st

@given(st.lists(st.integers()))
def test_sort_is_idempotent(lst):
    """Sorting twice should give the same result as sorting once"""
    assert sorted(sorted(lst)) == sorted(lst)
```

### Environment Management

**venv - Virtual Environments**
```bash
python -m venv myproject_env
source myproject_env/bin/activate  # Linux/Mac
myproject_env\Scripts\activate     # Windows
```
- **Problem it solves**: Isolates project dependencies from system Python
- **When NOT to use**: For complex dependency management (use Poetry) or data science (use Conda)
- **Free tier**: Built into Python 3.3+
- **Best practice**: Always use virtual environments, never install packages globally

**Poetry - Modern Dependency Management**
```bash
pip install poetry
poetry new myproject
poetry add requests
poetry install
```
- **Problem it solves**: Reproducible builds with lock files and better dependency resolution
- **When NOT to use**: For simple projects or when team isn't ready for new tooling
- **Free tier**: Completely free
- **Example pyproject.toml**:
```toml
[tool.poetry]
name = "myproject"
version = "0.1.0"

[tool.poetry.dependencies]
python = "^3.8"
requests = "^2.28.0"
fastapi = "^0.85.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.0.0"
black = "^22.0.0"
```

## Web Development Tools

### Frameworks and Libraries

**Flask - Lightweight Web Framework**
```bash
pip install flask
```
- **Problem it solves**: Quick web applications and APIs with minimal boilerplate
- **When NOT to use**: For large applications with complex requirements (use Django)
- **Free tier**: Completely free
- **Quick start**:
```python
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/api/health')
def health_check():
    return jsonify({'status': 'healthy'})

if __name__ == '__main__':
    app.run(debug=True)
```

**Django - Full-Featured Web Framework**
```bash
pip install django
django-admin startproject myproject
```
- **Problem it solves**: Complete web applications with admin interface, ORM, and security features
- **When NOT to use**: For simple APIs or microservices (use Flask/FastAPI)
- **Free tier**: Completely free
- **Best for**: Content management systems, e-commerce sites, complex web applications

**FastAPI - Modern API Framework**
```bash
pip install fastapi uvicorn
```
- **Problem it solves**: High-performance APIs with automatic documentation and type validation
- **When NOT to use**: For traditional web applications with server-side rendering
- **Free tier**: Completely free
- **Example**:
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    email: str

@app.post("/users/")
async def create_user(user: User):
    return {"message": f"User {user.name} created"}
```

**Requests - HTTP Client**
```bash
pip install requests
```
- **Problem it solves**: Making HTTP requests with session management and authentication
- **When NOT to use**: For async applications (use aiohttp) or when you need HTTP/2 (use httpx)
- **Free tier**: Completely free
- **Example**:
```python
import requests

# GET request
response = requests.get('https://api.github.com/user', 
                       headers={'Authorization': 'token YOUR_TOKEN'})
data = response.json()

# POST request with JSON
payload = {'name': 'John', 'email': 'john@example.com'}
response = requests.post('https://api.example.com/users', json=payload)
```

### Database Tools

**SQLAlchemy - ORM**
```bash
pip install sqlalchemy
```
- **Problem it solves**: Database abstraction with object-relational mapping
- **When NOT to use**: For simple database operations (use raw SQL) or NoSQL databases
- **Free tier**: Completely free
- **Example**:
```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    email = Column(String(100), unique=True)

engine = create_engine('sqlite:///example.db')
Base.metadata.create_all(engine)
```

**Alembic - Database Migrations**
```bash
pip install alembic
alembic init migrations
alembic revision --autogenerate -m "Create users table"
alembic upgrade head
```
- **Problem it solves**: Version control for database schema changes
- **When NOT to use**: For applications that don't evolve their schema
- **Free tier**: Completely free
- **Best practice**: Always use migrations for schema changes in production

## Data Science Tools

### Core Libraries

**Pandas - Data Manipulation**
```bash
pip install pandas
```
- **Problem it solves**: Excel-like data manipulation in Python
- **When NOT to use**: For large datasets that don't fit in memory (use Dask) or real-time processing
- **Free tier**: Completely free
- **Common operations**:
```python
import pandas as pd

# Read data
df = pd.read_csv('data.csv')

# Basic operations
df.head()                    # First 5 rows
df.describe()               # Statistical summary
df.groupby('category').sum() # Group and aggregate
df[df['price'] > 100]       # Filter rows

# Data cleaning
df.dropna()                 # Remove missing values
df.fillna(0)               # Fill missing values
```

**NumPy - Numerical Computing**
```bash
pip install numpy
```
- **Problem it solves**: Efficient numerical operations on arrays
- **When NOT to use**: For non-numerical data processing (use pandas)
- **Free tier**: Completely free
- **Example**:
```python
import numpy as np

# Create arrays
arr = np.array([1, 2, 3, 4, 5])
matrix = np.array([[1, 2], [3, 4]])

# Mathematical operations
np.mean(arr)        # Average
np.std(arr)         # Standard deviation
np.dot(matrix, matrix)  # Matrix multiplication
```

**Matplotlib - Plotting**
```bash
pip install matplotlib
```
- **Problem it solves**: Creating static plots and visualizations
- **When NOT to use**: For interactive dashboards (use Plotly) or quick statistical plots (use Seaborn)
- **Free tier**: Completely free
- **Example**:
```python
import matplotlib.pyplot as plt

# Simple line plot
plt.plot([1, 2, 3, 4], [1, 4, 2, 3])
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('My Plot')
plt.show()
```

**Seaborn - Statistical Visualization**
```bash
pip install seaborn
```
- **Problem it solves**: Beautiful statistical plots with minimal code
- **When NOT to use**: For custom plot types or interactive visualizations
- **Free tier**: Completely free
- **Example**:
```python
import seaborn as sns
import matplotlib.pyplot as plt

# Load sample data
tips = sns.load_dataset("tips")

# Create plots
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="day")
sns.boxplot(data=tips, x="day", y="total_bill")
plt.show()
```

### Machine Learning

**Scikit-learn - Machine Learning**
```bash
pip install scikit-learn
```
- **Problem it solves**: Traditional machine learning algorithms and preprocessing
- **When NOT to use**: For deep learning (use PyTorch/TensorFlow) or large-scale ML
- **Free tier**: Completely free
- **Example**:
```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Train model
model = RandomForestClassifier()
model.fit(X_train, y_train)

# Make predictions
predictions = model.predict(X_test)
accuracy = accuracy_score(y_test, predictions)
```

**Jupyter - Interactive Development**
```bash
pip install jupyter
jupyter notebook
```
- **Problem it solves**: Interactive data exploration and sharing analysis
- **When NOT to use**: For production code or version control of analysis
- **Free tier**: Completely free
- **Best practices**: Use for exploration, convert to scripts for production

## Automation Tools

### Task Automation

**Schedule - Job Scheduling**
```bash
pip install schedule
```
- **Problem it solves**: Simple job scheduling in Python
- **When NOT to use**: For complex scheduling (use Celery) or system-level cron jobs
- **Free tier**: Completely free
- **Example**:
```python
import schedule
import time

def job():
    print("Running scheduled job...")

schedule.every(10).minutes.do(job)
schedule.every().hour.do(job)
schedule.every().day.at("10:30").do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

**Click - Command Line Interfaces**
```bash
pip install click
```
- **Problem it solves**: Creating professional command-line tools
- **When NOT to use**: For simple scripts with minimal arguments (use argparse)
- **Free tier**: Completely free
- **Example**:
```python
import click

@click.command()
@click.option('--count', default=1, help='Number of greetings.')
@click.option('--name', prompt='Your name', help='The person to greet.')
def hello(count, name):
    """Simple program that greets NAME for a total of COUNT times."""
    for _ in range(count):
        click.echo(f'Hello, {name}!')

if __name__ == '__main__':
    hello()
```

### Web Scraping

**Beautiful Soup - HTML Parsing**
```bash
pip install beautifulsoup4
```
- **Problem it solves**: Parsing and navigating HTML/XML documents
- **When NOT to use**: For large documents (use lxml) or when you need CSS selectors
- **Free tier**: Completely free
- **Example**:
```python
from bs4 import BeautifulSoup
import requests

response = requests.get('https://example.com')
soup = BeautifulSoup(response.content, 'html.parser')

# Find elements
title = soup.find('title').text
links = soup.find_all('a')
specific_div = soup.find('div', class_='content')
```

**Selenium - Browser Automation**
```bash
pip install selenium
```
- **Problem it solves**: Automating web browsers for JavaScript-heavy sites
- **When NOT to use**: For simple HTML scraping (use Requests + BeautifulSoup)
- **Free tier**: Free library, requires browser drivers
- **Example**:
```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()  # Requires chromedriver
driver.get('https://example.com')

element = driver.find_element(By.ID, 'my-id')
element.click()

driver.quit()
```

## DevOps and Infrastructure Tools

### Cloud Integration

**Boto3 - AWS SDK**
```bash
pip install boto3
```
- **Problem it solves**: AWS service integration and infrastructure automation
- **When NOT to use**: For multi-cloud deployments or other cloud providers
- **Free tier**: Library is free, AWS services have their own pricing
- **Example**:
```python
import boto3

# S3 operations
s3 = boto3.client('s3')
s3.upload_file('local_file.txt', 'my-bucket', 'remote_file.txt')

# EC2 operations
ec2 = boto3.resource('ec2')
instances = ec2.instances.filter(Filters=[{'Name': 'instance-state-name', 'Values': ['running']}])
```

**Docker SDK - Container Management**
```bash
pip install docker
```
- **Problem it solves**: Managing Docker containers from Python
- **When NOT to use**: For simple container operations (use Docker CLI)
- **Free tier**: Completely free
- **Example**:
```python
import docker

client = docker.from_env()

# Run container
container = client.containers.run('nginx', detach=True, ports={'80/tcp': 8080})

# List containers
containers = client.containers.list()

# Stop container
container.stop()
```

### Configuration and Secrets

**python-dotenv - Environment Variables**
```bash
pip install python-dotenv
```
- **Problem it solves**: Loading environment variables from .env files
- **When NOT to use**: For complex configuration management
- **Free tier**: Completely free
- **Example**:
```python
from dotenv import load_dotenv
import os

load_dotenv()  # Load .env file

database_url = os.getenv('DATABASE_URL')
api_key = os.getenv('API_KEY')
```

**Pydantic - Data Validation**
```bash
pip install pydantic
```
- **Problem it solves**: Data validation and settings management with type hints
- **When NOT to use**: For simple applications without complex validation needs
- **Free tier**: Completely free
- **Example**:
```python
from pydantic import BaseModel, validator

class User(BaseModel):
    name: str
    email: str
    age: int

    @validator('email')
    def email_must_contain_at(cls, v):
        if '@' not in v:
            raise ValueError('Invalid email')
        return v

user = User(name='John', email='john@example.com', age=30)
```

## Monitoring and Debugging Tools

### Logging and Monitoring

**Loguru - Simplified Logging**
```bash
pip install loguru
```
- **Problem it solves**: Better logging with less configuration than standard library
- **When NOT to use**: When you need compatibility with existing logging infrastructure
- **Free tier**: Completely free
- **Example**:
```python
from loguru import logger

logger.add("app.log", rotation="500 MB")

logger.info("Application started")
logger.error("Something went wrong")
logger.debug("Debug information")
```

**Rich - Rich Text and Beautiful Formatting**
```bash
pip install rich
```
- **Problem it solves**: Beautiful console output with colors, tables, and progress bars
- **When NOT to use**: For production logging or when console output needs to be parsed
- **Free tier**: Completely free
- **Example**:
```python
from rich.console import Console
from rich.table import Table

console = Console()
console.print("Hello", style="bold red")

table = Table()
table.add_column("Name")
table.add_column("Age")
table.add_row("Alice", "30")
console.print(table)
```

### Performance Profiling

**cProfile - Built-in Profiler**
```bash
python -m cProfile -o profile.stats your_script.py
```
- **Problem it solves**: Finding performance bottlenecks in your code
- **When NOT to use**: For production monitoring (use APM tools)
- **Free tier**: Built into Python
- **Analysis**:
```python
import pstats

stats = pstats.Stats('profile.stats')
stats.sort_stats('cumulative')
stats.print_stats(10)  # Top 10 functions
```

**memory_profiler - Memory Usage**
```bash
pip install memory_profiler
python -m memory_profiler your_script.py
```
- **Problem it solves**: Monitoring memory usage line by line
- **When NOT to use**: For production monitoring
- **Free tier**: Completely free
- **Usage**: Add `@profile` decorator to functions you want to profile

## Tool Selection Decision Tree

### Web Development
```
Need a web application?
├── Simple API → FastAPI
├── Complex web app → Django
├── Microservice → Flask
└── Real-time features → FastAPI + WebSockets
```

### Data Work
```
Working with data?
├── Data analysis → Pandas + Jupyter
├── Machine learning → Scikit-learn
├── Deep learning → PyTorch
├── Data visualization → Matplotlib/Seaborn
└── Big data → Dask or PySpark
```

### Automation
```
Need automation?
├── Web scraping → Requests + BeautifulSoup
├── Browser automation → Selenium
├── System tasks → Standard library + Click
├── Scheduled jobs → Schedule or Celery
└── Server management → Fabric or Ansible
```

### Testing
```
Need testing?
├── Unit tests → pytest
├── Integration tests → pytest + fixtures
├── Property-based tests → Hypothesis
├── Load testing → Locust
└── Browser testing → Selenium + pytest
```

## Common Mistakes to Avoid

**Over-engineering**: Don't use complex tools for simple problems
- ❌ Using Django for a simple API
- ✅ Using Flask or FastAPI for simple APIs

**Under-engineering**: Don't use simple tools for complex problems
- ❌ Using Flask for a large content management system
- ✅ Using Django for complex web applications

**Tool proliferation**: Don't add tools without clear benefits
- ❌ Adding 5 different testing frameworks
- ✅ Standardizing on pytest for all testing needs

**Ignoring maintenance**: Don't choose tools that aren't maintained
- ❌ Using packages with no recent updates
- ✅ Checking GitHub activity and PyPI download stats

Remember: The best tool is the one that solves your problem with the least complexity and maintenance overhead.