# System Design Fundamentals

## Overview

System design is the art of building software systems that can handle real-world scale, traffic, and complexity. It's about making architectural decisions that balance performance, reliability, scalability, and maintainability.

**Key Principle**: There are no perfect systems, only systems with acceptable trade-offs for your specific requirements.

---

## Prerequisites: What You Need First

### ❌ Don't Start System Design If You Haven't:
- Built a full-stack web application with a database
- Worked with APIs and understood HTTP
- Deployed an application to production
- Experienced performance issues in a real application

### ✅ You're Ready When You:
- Understand databases, caching, and load balancing conceptually
- Have built applications that multiple users actually use
- Want to understand how large-scale systems work
- Are preparing for senior engineering roles

---

## Core System Design Concepts

### 1. Scalability: Handling Growth

#### Vertical Scaling (Scale Up)
Add more power to existing machines.

```
Before: 1 server with 4GB RAM, 2 CPU cores
After:  1 server with 16GB RAM, 8 CPU cores

Pros:
✅ Simple - no code changes needed
✅ No complexity of distributed systems
✅ Strong consistency (single machine)

Cons:
❌ Hardware limits (can't scale infinitely)
❌ Single point of failure
❌ Expensive at high levels
❌ Downtime during upgrades
```

#### Horizontal Scaling (Scale Out)
Add more machines to handle the load.

```
Before: 1 server handling all requests
After:  5 servers sharing the load

Pros:
✅ Can scale almost infinitely
✅ Better fault tolerance
✅ Cost-effective with commodity hardware
✅ No downtime for individual machine upgrades

Cons:
❌ Complex application architecture
❌ Data consistency challenges
❌ Network latency between machines
❌ More operational complexity
```

### Real-World Example: E-commerce Site
```
Traffic Growth:
- Month 1: 100 users/day → Single server (vertical scaling)
- Month 6: 10,000 users/day → Add database server + load balancer
- Month 12: 100,000 users/day → Multiple app servers + caching
- Month 24: 1M users/day → Microservices + CDN + database sharding
```

---

### 2. Load Balancing: Distributing Traffic

#### What Is Load Balancing?
Distribute incoming requests across multiple servers to prevent any single server from being overwhelmed.

```
Client Requests → Load Balancer → Server 1
                                → Server 2  
                                → Server 3
```

#### Load Balancing Algorithms

**Round Robin**: Requests go to servers in order
```python
class RoundRobinBalancer:
    def __init__(self, servers):
        self.servers = servers
        self.current = 0
    
    def get_server(self):
        server = self.servers[self.current]
        self.current = (self.current + 1) % len(self.servers)
        return server

# Usage
balancer = RoundRobinBalancer(['server1', 'server2', 'server3'])
print(balancer.get_server())  # server1
print(balancer.get_server())  # server2
print(balancer.get_server())  # server3
print(balancer.get_server())  # server1 (cycles back)
```

**Weighted Round Robin**: Servers with different capacities
```python
class WeightedRoundRobinBalancer:
    def __init__(self, servers_weights):
        # servers_weights = [('server1', 3), ('server2', 2), ('server3', 1)]
        self.servers = []
        for server, weight in servers_weights:
            self.servers.extend([server] * weight)
        self.current = 0
    
    def get_server(self):
        server = self.servers[self.current]
        self.current = (self.current + 1) % len(self.servers)
        return server
```

**Least Connections**: Route to server with fewest active connections
```python
class LeastConnectionsBalancer:
    def __init__(self, servers):
        self.servers = {server: 0 for server in servers}
    
    def get_server(self):
        return min(self.servers, key=self.servers.get)
    
    def add_connection(self, server):
        self.servers[server] += 1
    
    def remove_connection(self, server):
        self.servers[server] -= 1
```

#### Types of Load Balancers

**Layer 4 (Transport Layer)**
- Routes based on IP and port
- Faster, less CPU intensive
- Can't inspect application data

**Layer 7 (Application Layer)**
- Routes based on HTTP headers, URLs, cookies
- More intelligent routing decisions
- Higher CPU overhead

### Real-World Applications
- **Netflix**: Routes video requests to nearest CDN servers
- **Amazon**: Distributes shopping requests across data centers
- **Google**: Load balances search queries across thousands of servers

---

### 3. Caching: Speeding Up Access

#### What Is Caching?
Store frequently accessed data in fast storage to avoid expensive operations.

#### Cache Levels
```
Browser Cache → CDN → Load Balancer → Application Cache → Database Cache → Disk
(Fastest)                                                                (Slowest)
```

#### Caching Strategies

**Cache-Aside (Lazy Loading)**
```python
import redis

cache = redis.Redis()

def get_user(user_id):
    # Try cache first
    cached_user = cache.get(f"user:{user_id}")
    if cached_user:
        return json.loads(cached_user)
    
    # Cache miss - get from database
    user = database.get_user(user_id)
    
    # Store in cache for next time
    cache.setex(f"user:{user_id}", 3600, json.dumps(user))  # 1 hour TTL
    return user

def update_user(user_id, user_data):
    # Update database
    database.update_user(user_id, user_data)
    
    # Invalidate cache
    cache.delete(f"user:{user_id}")
```

**Write-Through Cache**
```python
def update_user_write_through(user_id, user_data):
    # Update database first
    database.update_user(user_id, user_data)
    
    # Update cache immediately
    cache.setex(f"user:{user_id}", 3600, json.dumps(user_data))
```

**Write-Behind (Write-Back) Cache**
```python
def update_user_write_behind(user_id, user_data):
    # Update cache immediately
    cache.setex(f"user:{user_id}", 3600, json.dumps(user_data))
    
    # Mark for database update (async)
    background_queue.add_task('update_user_db', user_id, user_data)
```

#### Cache Eviction Policies

**LRU (Least Recently Used)**
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()
    
    def get(self, key):
        if key in self.cache:
            # Move to end (most recently used)
            self.cache.move_to_end(key)
            return self.cache[key]
        return None
    
    def put(self, key, value):
        if key in self.cache:
            # Update existing key
            self.cache.move_to_end(key)
        elif len(self.cache) >= self.capacity:
            # Remove least recently used
            self.cache.popitem(last=False)
        
        self.cache[key] = value

# Usage
cache = LRUCache(3)
cache.put('a', 1)
cache.put('b', 2)
cache.put('c', 3)
cache.put('d', 4)  # 'a' gets evicted
print(cache.get('a'))  # None
print(cache.get('b'))  # 2
```

### Real-World Caching Examples
- **Reddit**: Caches popular posts and comments
- **Instagram**: Caches user feeds and photo metadata
- **Spotify**: Caches song metadata and playlists

---

### 4. Database Design and Scaling

#### Database Normalization
Organize data to reduce redundancy and improve integrity.

**First Normal Form (1NF)**: Atomic values, no repeating groups
```sql
-- Bad: Multiple values in one column
CREATE TABLE users (
    id INT,
    name VARCHAR(100),
    hobbies VARCHAR(500)  -- "reading,gaming,cooking"
);

-- Good: Separate table for hobbies
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE user_hobbies (
    user_id INT,
    hobby VARCHAR(100),
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

**Second Normal Form (2NF)**: 1NF + no partial dependencies
```sql
-- Bad: Order details depend on both order_id and product_id
CREATE TABLE order_details (
    order_id INT,
    product_id INT,
    product_name VARCHAR(100),  -- Depends only on product_id
    quantity INT,
    price DECIMAL(10,2)
);

-- Good: Separate product information
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10,2)
);

CREATE TABLE order_details (
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

#### Database Scaling Strategies

**Read Replicas**
```
Master Database (Writes) → Replica 1 (Reads)
                        → Replica 2 (Reads)
                        → Replica 3 (Reads)

Pros:
✅ Scales read operations
✅ Improves read performance
✅ Provides backup in case of master failure

Cons:
❌ Eventual consistency (replica lag)
❌ Doesn't help with write scaling
❌ Increased complexity
```

**Database Sharding**
Split data across multiple databases based on a key.

```python
class DatabaseSharding:
    def __init__(self, shards):
        self.shards = shards  # List of database connections
    
    def get_shard(self, user_id):
        # Simple hash-based sharding
        shard_index = hash(user_id) % len(self.shards)
        return self.shards[shard_index]
    
    def get_user(self, user_id):
        shard = self.get_shard(user_id)
        return shard.execute("SELECT * FROM users WHERE id = %s", [user_id])
    
    def create_user(self, user_id, user_data):
        shard = self.get_shard(user_id)
        return shard.execute("INSERT INTO users ...", user_data)

# Usage
sharding = DatabaseSharding([db1, db2, db3, db4])
user = sharding.get_user(12345)  # Routes to appropriate shard
```

**Sharding Strategies**:
- **Range-based**: Users 1-1000 → Shard 1, Users 1001-2000 → Shard 2
- **Hash-based**: hash(user_id) % num_shards
- **Directory-based**: Lookup table maps keys to shards

---

### 5. Microservices vs Monoliths

#### Monolithic Architecture
All functionality in a single deployable unit.

```
┌─────────────────────────────────┐
│         Monolithic App          │
│  ┌─────────┬─────────┬────────┐ │
│  │  User   │ Product │Payment │ │
│  │ Service │ Service │Service │ │
│  └─────────┴─────────┴────────┘ │
│         Single Database         │
└─────────────────────────────────┘

Pros:
✅ Simple to develop and test
✅ Easy to deploy (single unit)
✅ Good performance (no network calls)
✅ Strong consistency

Cons:
❌ Hard to scale individual components
❌ Technology lock-in
❌ Large codebase becomes unwieldy
❌ Single point of failure
```

#### Microservices Architecture
Functionality split into independent services.

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│ User Service│    │Product      │    │Payment      │
│             │    │Service      │    │Service      │
│   User DB   │    │ Product DB  │    │ Payment DB  │
└─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                    ┌─────────────┐
                    │API Gateway  │
                    └─────────────┘

Pros:
✅ Independent scaling
✅ Technology diversity
✅ Team autonomy
✅ Fault isolation

Cons:
❌ Network complexity
❌ Data consistency challenges
❌ Operational overhead
❌ Testing complexity
```

#### When to Choose Each

**Start with Monolith When:**
- Small team (< 10 developers)
- Unclear requirements
- Rapid prototyping needed
- Simple application

**Move to Microservices When:**
- Team size > 20 developers
- Clear service boundaries
- Different scaling requirements
- Need for technology diversity

---

## System Design Case Studies

### Case Study 1: URL Shortener (like bit.ly)

#### Requirements
- Shorten long URLs to short URLs
- Redirect short URLs to original URLs
- Handle 100M URLs per day
- 100:1 read/write ratio

#### High-Level Design
```
Client → Load Balancer → Web Servers → Cache → Database
                                    → URL Encoding Service
```

#### Database Schema
```sql
CREATE TABLE urls (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    original_url TEXT NOT NULL,
    short_code VARCHAR(7) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    expires_at TIMESTAMP,
    INDEX idx_short_code (short_code)
);
```

#### URL Encoding Algorithm
```python
import string
import random

class URLShortener:
    def __init__(self):
        self.chars = string.ascii_letters + string.digits
        self.base = len(self.chars)  # 62
    
    def encode(self, num):
        """Convert number to base62 string"""
        if num == 0:
            return self.chars[0]
        
        result = []
        while num > 0:
            result.append(self.chars[num % self.base])
            num //= self.base
        
        return ''.join(reversed(result))
    
    def decode(self, short_code):
        """Convert base62 string back to number"""
        num = 0
        for char in short_code:
            num = num * self.base + self.chars.index(char)
        return num
    
    def shorten_url(self, original_url):
        # Generate unique ID (could be database auto-increment)
        url_id = self.generate_unique_id()
        short_code = self.encode(url_id)
        
        # Store in database
        self.store_url(url_id, original_url, short_code)
        
        return f"https://short.ly/{short_code}"
```

#### Scaling Considerations
- **Caching**: Cache popular URLs in Redis
- **Database**: Shard by short_code hash
- **CDN**: Cache redirects globally
- **Rate Limiting**: Prevent abuse

---

### Case Study 2: Chat System (like WhatsApp)

#### Requirements
- Send/receive messages in real-time
- Support 1M concurrent users
- Message history storage
- Online/offline status

#### High-Level Design
```
Mobile App ↔ WebSocket Gateway ↔ Chat Service ↔ Message Queue ↔ Database
                                              ↔ Notification Service
```

#### Real-time Communication
```python
import asyncio
import websockets
import json

class ChatServer:
    def __init__(self):
        self.connections = {}  # user_id -> websocket
        self.user_rooms = {}   # user_id -> set of room_ids
    
    async def handle_connection(self, websocket, path):
        try:
            async for message in websocket:
                data = json.loads(message)
                await self.handle_message(websocket, data)
        except websockets.exceptions.ConnectionClosed:
            await self.handle_disconnect(websocket)
    
    async def handle_message(self, websocket, data):
        message_type = data.get('type')
        
        if message_type == 'join':
            await self.join_room(websocket, data)
        elif message_type == 'chat':
            await self.send_chat_message(websocket, data)
        elif message_type == 'typing':
            await self.handle_typing(websocket, data)
    
    async def join_room(self, websocket, data):
        user_id = data['user_id']
        room_id = data['room_id']
        
        self.connections[user_id] = websocket
        
        if user_id not in self.user_rooms:
            self.user_rooms[user_id] = set()
        self.user_rooms[user_id].add(room_id)
    
    async def send_chat_message(self, websocket, data):
        room_id = data['room_id']
        message = data['message']
        sender_id = data['sender_id']
        
        # Store message in database
        await self.store_message(room_id, sender_id, message)
        
        # Send to all users in room
        await self.broadcast_to_room(room_id, {
            'type': 'message',
            'room_id': room_id,
            'sender_id': sender_id,
            'message': message,
            'timestamp': time.time()
        })
    
    async def broadcast_to_room(self, room_id, message):
        for user_id, rooms in self.user_rooms.items():
            if room_id in rooms and user_id in self.connections:
                websocket = self.connections[user_id]
                try:
                    await websocket.send(json.dumps(message))
                except websockets.exceptions.ConnectionClosed:
                    del self.connections[user_id]

# Start server
start_server = websockets.serve(ChatServer().handle_connection, "localhost", 8765)
asyncio.get_event_loop().run_until_complete(start_server)
asyncio.get_event_loop().run_forever()
```

#### Message Storage
```sql
CREATE TABLE messages (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    room_id VARCHAR(50) NOT NULL,
    sender_id VARCHAR(50) NOT NULL,
    message TEXT NOT NULL,
    message_type ENUM('text', 'image', 'file') DEFAULT 'text',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_room_time (room_id, created_at)
);

CREATE TABLE chat_rooms (
    id VARCHAR(50) PRIMARY KEY,
    name VARCHAR(100),
    type ENUM('direct', 'group') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE room_members (
    room_id VARCHAR(50),
    user_id VARCHAR(50),
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (room_id, user_id)
);
```

---

## System Design Principles

### 1. Reliability
System continues to work correctly even when failures occur.

**Strategies:**
- **Redundancy**: Multiple instances of critical components
- **Failover**: Automatic switching to backup systems
- **Circuit Breakers**: Prevent cascading failures

```python
import time
import random

class CircuitBreaker:
    def __init__(self, failure_threshold=5, timeout=60):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = 'CLOSED'  # CLOSED, OPEN, HALF_OPEN
    
    def call(self, func, *args, **kwargs):
        if self.state == 'OPEN':
            if time.time() - self.last_failure_time > self.timeout:
                self.state = 'HALF_OPEN'
            else:
                raise Exception("Circuit breaker is OPEN")
        
        try:
            result = func(*args, **kwargs)
            self.on_success()
            return result
        except Exception as e:
            self.on_failure()
            raise e
    
    def on_success(self):
        self.failure_count = 0
        self.state = 'CLOSED'
    
    def on_failure(self):
        self.failure_count += 1
        self.last_failure_time = time.time()
        
        if self.failure_count >= self.failure_threshold:
            self.state = 'OPEN'

# Usage
circuit_breaker = CircuitBreaker()

def unreliable_service():
    if random.random() < 0.7:  # 70% failure rate
        raise Exception("Service failed")
    return "Success"

try:
    result = circuit_breaker.call(unreliable_service)
    print(result)
except Exception as e:
    print(f"Error: {e}")
```

### 2. Scalability
System's ability to handle increased load gracefully.

**Horizontal Scaling Patterns:**
- **Stateless Services**: No server-side state
- **Database Sharding**: Split data across multiple databases
- **Caching**: Reduce database load

### 3. Availability
System remains operational over time.

**Measuring Availability:**
- 99% = 3.65 days downtime/year
- 99.9% = 8.76 hours downtime/year
- 99.99% = 52.56 minutes downtime/year
- 99.999% = 5.26 minutes downtime/year

### 4. Consistency
All nodes see the same data simultaneously.

**CAP Theorem**: You can only guarantee 2 out of 3:
- **Consistency**: All nodes have the same data
- **Availability**: System remains operational
- **Partition Tolerance**: System continues despite network failures

---

## Common System Design Patterns

### 1. Event-Driven Architecture
```python
import asyncio
from typing import Dict, List, Callable

class EventBus:
    def __init__(self):
        self.subscribers: Dict[str, List[Callable]] = {}
    
    def subscribe(self, event_type: str, handler: Callable):
        if event_type not in self.subscribers:
            self.subscribers[event_type] = []
        self.subscribers[event_type].append(handler)
    
    async def publish(self, event_type: str, data: dict):
        if event_type in self.subscribers:
            tasks = []
            for handler in self.subscribers[event_type]:
                tasks.append(handler(data))
            await asyncio.gather(*tasks)

# Usage
event_bus = EventBus()

async def send_email(data):
    print(f"Sending email to {data['email']}")

async def update_analytics(data):
    print(f"Updating analytics for user {data['user_id']}")

event_bus.subscribe('user_registered', send_email)
event_bus.subscribe('user_registered', update_analytics)

# When user registers
await event_bus.publish('user_registered', {
    'user_id': 123,
    'email': 'user@example.com'
})
```

### 2. CQRS (Command Query Responsibility Segregation)
Separate read and write operations for better performance.

```python
class UserCommand:
    def __init__(self, write_db):
        self.write_db = write_db
    
    def create_user(self, user_data):
        # Optimized for writes
        user_id = self.write_db.insert('users', user_data)
        
        # Publish event for read model update
        event_bus.publish('user_created', {
            'user_id': user_id,
            'user_data': user_data
        })
        
        return user_id

class UserQuery:
    def __init__(self, read_db):
        self.read_db = read_db  # Could be different from write DB
    
    def get_user(self, user_id):
        # Optimized for reads (denormalized, cached)
        return self.read_db.get('user_profiles', user_id)
    
    def search_users(self, query):
        # Optimized search indexes
        return self.read_db.search('user_search_index', query)
```

---

## Next Steps in System Design

### 1. Study Real Systems
- **Netflix**: Content delivery and recommendation systems
- **Uber**: Real-time matching and routing
- **Instagram**: Photo storage and social features
- **WhatsApp**: Real-time messaging at scale

### 2. Practice System Design Interviews
- Start with simple systems (URL shortener, chat app)
- Progress to complex systems (social media, ride-sharing)
- Focus on trade-offs and scalability

### 3. Build and Scale Your Own Systems
- Start with a simple web app
- Add caching when you hit performance issues
- Implement load balancing when one server isn't enough
- Experience the problems before learning the solutions

### 4. Learn from Failures
- Read post-mortems from major outages
- Understand how systems fail in practice
- Learn about monitoring and observability

Remember: System design is about making informed trade-offs. There's no single "correct" design - only designs that are appropriate for your specific requirements, constraints, and scale.