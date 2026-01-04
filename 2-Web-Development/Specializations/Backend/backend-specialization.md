# Backend Development Specialization - Complete Learning Path

Master server-side development with Node.js, Express, databases, and deployment. This guide takes you from API basics to production-ready backend systems.

## Table of Contents

1. [Node.js and Express Fundamentals](#nodejs-and-express-fundamentals)
2. [Database Integration](#database-integration)
3. [Authentication and Authorization](#authentication-and-authorization)
4. [API Design and Development](#api-design-and-development)
5. [Security Best Practices](#security-best-practices)
6. [Testing Backend Applications](#testing-backend-applications)
7. [Deployment and DevOps](#deployment-and-devops)
8. [Career Path](#career-path)

---

## Node.js and Express Fundamentals

### Why Node.js?

Node.js is the most popular backend runtime for JavaScript developers:
- **Same Language**: Use JavaScript for both frontend and backend
- **High Performance**: Non-blocking I/O and event-driven architecture
- **Large Ecosystem**: npm has the largest package repository
- **Industry Adoption**: Used by Netflix, Uber, LinkedIn, PayPal

### Setting Up Node.js Development Environment

```bash
# Install Node.js (v18+ recommended)
# Download from nodejs.org or use nvm

# Initialize a new project
mkdir my-backend-app
cd my-backend-app
npm init -y

# Install Express and essential packages
npm install express
npm install -D nodemon

# Install additional utilities
npm install cors helmet morgan dotenv
npm install -D eslint prettier
```

### Basic Express Server

```javascript
// server.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(helmet()); // Security headers
app.use(cors()); // Enable CORS
app.use(morgan('combined')); // Logging
app.use(express.json()); // Parse JSON bodies
app.use(express.urlencoded({ extended: true })); // Parse URL-encoded bodies

// Basic route
app.get('/', (req, res) => {
    res.json({ 
        message: 'Welcome to my API',
        version: '1.0.0',
        timestamp: new Date().toISOString()
    });
});

// Health check endpoint
app.get('/health', (req, res) => {
    res.status(200).json({ 
        status: 'OK',
        uptime: process.uptime(),
        timestamp: new Date().toISOString()
    });
});

// Error handling middleware
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ 
        error: 'Something went wrong!',
        message: process.env.NODE_ENV === 'development' ? err.message : 'Internal server error'
    });
});

// 404 handler
app.use('*', (req, res) => {
    res.status(404).json({ error: 'Route not found' });
});

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

module.exports = app;
```
### Express Routing and Middleware

```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

// Sample data (replace with database)
let users = [
    { id: 1, name: 'John Doe', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
];

// Custom middleware for this router
router.use((req, res, next) => {
    console.log(`Users route accessed: ${req.method} ${req.path}`);
    next();
});

// Validation middleware
const validateUser = (req, res, next) => {
    const { name, email } = req.body;
    
    if (!name || !email) {
        return res.status(400).json({ 
            error: 'Name and email are required' 
        });
    }
    
    if (!email.includes('@')) {
        return res.status(400).json({ 
            error: 'Invalid email format' 
        });
    }
    
    next();
};

// GET /api/users - Get all users
router.get('/', (req, res) => {
    const { page = 1, limit = 10, search } = req.query;
    
    let filteredUsers = users;
    
    // Search functionality
    if (search) {
        filteredUsers = users.filter(user => 
            user.name.toLowerCase().includes(search.toLowerCase()) ||
            user.email.toLowerCase().includes(search.toLowerCase())
        );
    }
    
    // Pagination
    const startIndex = (page - 1) * limit;
    const endIndex = page * limit;
    const paginatedUsers = filteredUsers.slice(startIndex, endIndex);
    
    res.json({
        users: paginatedUsers,
        pagination: {
            page: parseInt(page),
            limit: parseInt(limit),
            total: filteredUsers.length,
            pages: Math.ceil(filteredUsers.length / limit)
        }
    });
});

// GET /api/users/:id - Get user by ID
router.get('/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const user = users.find(u => u.id === id);
    
    if (!user) {
        return res.status(404).json({ error: 'User not found' });
    }
    
    res.json(user);
});

// POST /api/users - Create new user
router.post('/', validateUser, (req, res) => {
    const { name, email } = req.body;
    
    // Check if email already exists
    const existingUser = users.find(u => u.email === email);
    if (existingUser) {
        return res.status(409).json({ error: 'Email already exists' });
    }
    
    const newUser = {
        id: users.length + 1,
        name,
        email,
        createdAt: new Date().toISOString()
    };
    
    users.push(newUser);
    res.status(201).json(newUser);
});

// PUT /api/users/:id - Update user
router.put('/:id', validateUser, (req, res) => {
    const id = parseInt(req.params.id);
    const userIndex = users.findIndex(u => u.id === id);
    
    if (userIndex === -1) {
        return res.status(404).json({ error: 'User not found' });
    }
    
    const { name, email } = req.body;
    
    // Check if email already exists (excluding current user)
    const existingUser = users.find(u => u.email === email && u.id !== id);
    if (existingUser) {
        return res.status(409).json({ error: 'Email already exists' });
    }
    
    users[userIndex] = {
        ...users[userIndex],
        name,
        email,
        updatedAt: new Date().toISOString()
    };
    
    res.json(users[userIndex]);
});

// DELETE /api/users/:id - Delete user
router.delete('/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const userIndex = users.findIndex(u => u.id === id);
    
    if (userIndex === -1) {
        return res.status(404).json({ error: 'User not found' });
    }
    
    users.splice(userIndex, 1);
    res.status(204).send();
});

module.exports = router;

// server.js - Use the router
const userRoutes = require('./routes/users');
app.use('/api/users', userRoutes);
```

### Async/Await and Error Handling

```javascript
// utils/asyncHandler.js - Wrapper for async route handlers
const asyncHandler = (fn) => (req, res, next) => {
    Promise.resolve(fn(req, res, next)).catch(next);
};

// Custom error class
class AppError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = true;
        
        Error.captureStackTrace(this, this.constructor);
    }
}

// routes/posts.js - Using async/await
const asyncHandler = require('../utils/asyncHandler');
const AppError = require('../utils/AppError');

// Simulated async database operations
const fetchPostsFromDB = async (filters) => {
    // Simulate database delay
    await new Promise(resolve => setTimeout(resolve, 100));
    
    // Simulate potential error
    if (Math.random() < 0.1) {
        throw new Error('Database connection failed');
    }
    
    return [
        { id: 1, title: 'First Post', content: 'Content 1' },
        { id: 2, title: 'Second Post', content: 'Content 2' }
    ];
};

const createPostInDB = async (postData) => {
    await new Promise(resolve => setTimeout(resolve, 200));
    
    return {
        id: Date.now(),
        ...postData,
        createdAt: new Date().toISOString()
    };
};

// GET /api/posts
router.get('/', asyncHandler(async (req, res) => {
    const { category, author } = req.query;
    
    const posts = await fetchPostsFromDB({ category, author });
    
    res.json({
        success: true,
        count: posts.length,
        data: posts
    });
}));

// POST /api/posts
router.post('/', asyncHandler(async (req, res) => {
    const { title, content, author } = req.body;
    
    if (!title || !content) {
        throw new AppError('Title and content are required', 400);
    }
    
    const post = await createPostInDB({ title, content, author });
    
    res.status(201).json({
        success: true,
        data: post
    });
}));

// Global error handler
app.use((err, req, res, next) => {
    let error = { ...err };
    error.message = err.message;
    
    // Log error
    console.error(err);
    
    // Mongoose bad ObjectId
    if (err.name === 'CastError') {
        const message = 'Resource not found';
        error = new AppError(message, 404);
    }
    
    // Mongoose duplicate key
    if (err.code === 11000) {
        const message = 'Duplicate field value entered';
        error = new AppError(message, 400);
    }
    
    // Mongoose validation error
    if (err.name === 'ValidationError') {
        const message = Object.values(err.errors).map(val => val.message);
        error = new AppError(message, 400);
    }
    
    res.status(error.statusCode || 500).json({
        success: false,
        error: error.message || 'Server Error'
    });
});

module.exports = { asyncHandler, AppError };
```
---

## Database Integration

### PostgreSQL with Node.js

PostgreSQL is the most popular production database for Node.js applications:

```bash
# Install PostgreSQL client
npm install pg
npm install -D @types/pg

# Install query builder (optional but recommended)
npm install knex
```

```javascript
// config/database.js
const { Pool } = require('pg');

const pool = new Pool({
    user: process.env.DB_USER || 'postgres',
    host: process.env.DB_HOST || 'localhost',
    database: process.env.DB_NAME || 'myapp',
    password: process.env.DB_PASSWORD || 'password',
    port: process.env.DB_PORT || 5432,
    max: 20, // Maximum number of clients in the pool
    idleTimeoutMillis: 30000,
    connectionTimeoutMillis: 2000,
});

// Test connection
pool.on('connect', () => {
    console.log('Connected to PostgreSQL database');
});

pool.on('error', (err) => {
    console.error('Unexpected error on idle client', err);
    process.exit(-1);
});

module.exports = pool;

// Database initialization
const initDB = async () => {
    try {
        // Create users table
        await pool.query(`
            CREATE TABLE IF NOT EXISTS users (
                id SERIAL PRIMARY KEY,
                name VARCHAR(100) NOT NULL,
                email VARCHAR(100) UNIQUE NOT NULL,
                password_hash VARCHAR(255) NOT NULL,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        `);

        // Create posts table
        await pool.query(`
            CREATE TABLE IF NOT EXISTS posts (
                id SERIAL PRIMARY KEY,
                title VARCHAR(200) NOT NULL,
                content TEXT NOT NULL,
                author_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
                published BOOLEAN DEFAULT false,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        `);

        console.log('Database tables created successfully');
    } catch (error) {
        console.error('Error creating database tables:', error);
    }
};

module.exports = { pool, initDB };
```

### Database Models and Queries

```javascript
// models/User.js
const pool = require('../config/database');
const bcrypt = require('bcrypt');

class User {
    static async findAll(limit = 10, offset = 0) {
        const query = `
            SELECT id, name, email, created_at, updated_at 
            FROM users 
            ORDER BY created_at DESC 
            LIMIT $1 OFFSET $2
        `;
        
        const result = await pool.query(query, [limit, offset]);
        return result.rows;
    }

    static async findById(id) {
        const query = 'SELECT id, name, email, created_at, updated_at FROM users WHERE id = $1';
        const result = await pool.query(query, [id]);
        return result.rows[0];
    }

    static async findByEmail(email) {
        const query = 'SELECT * FROM users WHERE email = $1';
        const result = await pool.query(query, [email]);
        return result.rows[0];
    }

    static async create({ name, email, password }) {
        const client = await pool.connect();
        
        try {
            await client.query('BEGIN');
            
            // Check if email already exists
            const existingUser = await client.query(
                'SELECT id FROM users WHERE email = $1', 
                [email]
            );
            
            if (existingUser.rows.length > 0) {
                throw new Error('Email already exists');
            }
            
            // Hash password
            const saltRounds = 12;
            const passwordHash = await bcrypt.hash(password, saltRounds);
            
            // Insert user
            const query = `
                INSERT INTO users (name, email, password_hash) 
                VALUES ($1, $2, $3) 
                RETURNING id, name, email, created_at
            `;
            
            const result = await client.query(query, [name, email, passwordHash]);
            
            await client.query('COMMIT');
            return result.rows[0];
            
        } catch (error) {
            await client.query('ROLLBACK');
            throw error;
        } finally {
            client.release();
        }
    }

    static async update(id, { name, email }) {
        const query = `
            UPDATE users 
            SET name = $1, email = $2, updated_at = CURRENT_TIMESTAMP 
            WHERE id = $3 
            RETURNING id, name, email, updated_at
        `;
        
        const result = await pool.query(query, [name, email, id]);
        return result.rows[0];
    }

    static async delete(id) {
        const query = 'DELETE FROM users WHERE id = $1 RETURNING id';
        const result = await pool.query(query, [id]);
        return result.rows[0];
    }

    static async validatePassword(email, password) {
        const user = await this.findByEmail(email);
        if (!user) return null;
        
        const isValid = await bcrypt.compare(password, user.password_hash);
        if (!isValid) return null;
        
        // Return user without password hash
        const { password_hash, ...userWithoutPassword } = user;
        return userWithoutPassword;
    }
}

module.exports = User;

// models/Post.js
class Post {
    static async findAll(filters = {}) {
        let query = `
            SELECT p.*, u.name as author_name 
            FROM posts p 
            JOIN users u ON p.author_id = u.id
        `;
        
        const conditions = [];
        const values = [];
        
        if (filters.published !== undefined) {
            conditions.push(`p.published = $${values.length + 1}`);
            values.push(filters.published);
        }
        
        if (filters.author_id) {
            conditions.push(`p.author_id = $${values.length + 1}`);
            values.push(filters.author_id);
        }
        
        if (conditions.length > 0) {
            query += ' WHERE ' + conditions.join(' AND ');
        }
        
        query += ' ORDER BY p.created_at DESC';
        
        if (filters.limit) {
            query += ` LIMIT $${values.length + 1}`;
            values.push(filters.limit);
        }
        
        const result = await pool.query(query, values);
        return result.rows;
    }

    static async findById(id) {
        const query = `
            SELECT p.*, u.name as author_name 
            FROM posts p 
            JOIN users u ON p.author_id = u.id 
            WHERE p.id = $1
        `;
        
        const result = await pool.query(query, [id]);
        return result.rows[0];
    }

    static async create({ title, content, author_id, published = false }) {
        const query = `
            INSERT INTO posts (title, content, author_id, published) 
            VALUES ($1, $2, $3, $4) 
            RETURNING *
        `;
        
        const result = await pool.query(query, [title, content, author_id, published]);
        return result.rows[0];
    }

    static async update(id, { title, content, published }) {
        const query = `
            UPDATE posts 
            SET title = $1, content = $2, published = $3, updated_at = CURRENT_TIMESTAMP 
            WHERE id = $4 
            RETURNING *
        `;
        
        const result = await pool.query(query, [title, content, published, id]);
        return result.rows[0];
    }

    static async delete(id) {
        const query = 'DELETE FROM posts WHERE id = $1 RETURNING id';
        const result = await pool.query(query, [id]);
        return result.rows[0];
    }
}

module.exports = Post;
```

### MongoDB with Mongoose (Alternative)

```bash
# Install MongoDB driver and ODM
npm install mongoose
```

```javascript
// config/mongodb.js
const mongoose = require('mongoose');

const connectDB = async () => {
    try {
        const conn = await mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/myapp', {
            useNewUrlParser: true,
            useUnifiedTopology: true,
        });
        
        console.log(`MongoDB Connected: ${conn.connection.host}`);
    } catch (error) {
        console.error('Error connecting to MongoDB:', error);
        process.exit(1);
    }
};

module.exports = connectDB;

// models/User.js (Mongoose)
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: [true, 'Name is required'],
        trim: true,
        maxlength: [100, 'Name cannot be more than 100 characters']
    },
    email: {
        type: String,
        required: [true, 'Email is required'],
        unique: true,
        lowercase: true,
        match: [/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/, 'Please enter a valid email']
    },
    password: {
        type: String,
        required: [true, 'Password is required'],
        minlength: [6, 'Password must be at least 6 characters'],
        select: false // Don't include password in queries by default
    },
    role: {
        type: String,
        enum: ['user', 'admin'],
        default: 'user'
    },
    isActive: {
        type: Boolean,
        default: true
    }
}, {
    timestamps: true
});

// Hash password before saving
userSchema.pre('save', async function(next) {
    if (!this.isModified('password')) return next();
    
    this.password = await bcrypt.hash(this.password, 12);
    next();
});

// Instance method to check password
userSchema.methods.comparePassword = async function(candidatePassword) {
    return await bcrypt.compare(candidatePassword, this.password);
};

// Static method to find active users
userSchema.statics.findActive = function() {
    return this.find({ isActive: true });
};

module.exports = mongoose.model('User', userSchema);

// models/Post.js (Mongoose)
const postSchema = new mongoose.Schema({
    title: {
        type: String,
        required: [true, 'Title is required'],
        trim: true,
        maxlength: [200, 'Title cannot be more than 200 characters']
    },
    content: {
        type: String,
        required: [true, 'Content is required']
    },
    author: {
        type: mongoose.Schema.Types.ObjectId,
        ref: 'User',
        required: true
    },
    published: {
        type: Boolean,
        default: false
    },
    tags: [{
        type: String,
        trim: true
    }],
    views: {
        type: Number,
        default: 0
    }
}, {
    timestamps: true
});

// Index for better query performance
postSchema.index({ published: 1, createdAt: -1 });
postSchema.index({ author: 1 });

// Virtual for URL
postSchema.virtual('url').get(function() {
    return `/posts/${this._id}`;
});

// Populate author information by default
postSchema.pre(/^find/, function(next) {
    this.populate({
        path: 'author',
        select: 'name email'
    });
    next();
});

module.exports = mongoose.model('Post', postSchema);
```
---

## Authentication and Authorization

### JWT Authentication

```bash
# Install authentication packages
npm install jsonwebtoken bcrypt
npm install express-rate-limit
```

```javascript
// middleware/auth.js
const jwt = require('jsonwebtoken');
const User = require('../models/User');

const generateToken = (userId) => {
    return jwt.sign({ userId }, process.env.JWT_SECRET, {
        expiresIn: process.env.JWT_EXPIRE || '7d'
    });
};

const generateRefreshToken = (userId) => {
    return jwt.sign({ userId }, process.env.JWT_REFRESH_SECRET, {
        expiresIn: '30d'
    });
};

// Middleware to protect routes
const protect = async (req, res, next) => {
    try {
        let token;
        
        // Get token from header
        if (req.headers.authorization && req.headers.authorization.startsWith('Bearer')) {
            token = req.headers.authorization.split(' ')[1];
        }
        
        if (!token) {
            return res.status(401).json({ error: 'Not authorized, no token' });
        }
        
        // Verify token
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        
        // Get user from token
        const user = await User.findById(decoded.userId);
        
        if (!user) {
            return res.status(401).json({ error: 'Not authorized, user not found' });
        }
        
        req.user = user;
        next();
    } catch (error) {
        console.error('Auth middleware error:', error);
        return res.status(401).json({ error: 'Not authorized, token failed' });
    }
};

// Middleware to check user roles
const authorize = (...roles) => {
    return (req, res, next) => {
        if (!roles.includes(req.user.role)) {
            return res.status(403).json({ 
                error: `User role ${req.user.role} is not authorized to access this route` 
            });
        }
        next();
    };
};

module.exports = { generateToken, generateRefreshToken, protect, authorize };

// routes/auth.js
const express = require('express');
const rateLimit = require('express-rate-limit');
const User = require('../models/User');
const { generateToken, generateRefreshToken, protect } = require('../middleware/auth');
const { asyncHandler } = require('../utils/asyncHandler');

const router = express.Router();

// Rate limiting for auth routes
const authLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 5, // Limit each IP to 5 requests per windowMs
    message: 'Too many authentication attempts, please try again later'
});

// Register user
router.post('/register', authLimiter, asyncHandler(async (req, res) => {
    const { name, email, password } = req.body;
    
    // Validation
    if (!name || !email || !password) {
        return res.status(400).json({ error: 'Please provide name, email and password' });
    }
    
    if (password.length < 6) {
        return res.status(400).json({ error: 'Password must be at least 6 characters' });
    }
    
    // Check if user exists
    const existingUser = await User.findByEmail(email);
    if (existingUser) {
        return res.status(400).json({ error: 'User already exists' });
    }
    
    // Create user
    const user = await User.create({ name, email, password });
    
    // Generate tokens
    const token = generateToken(user.id);
    const refreshToken = generateRefreshToken(user.id);
    
    res.status(201).json({
        success: true,
        token,
        refreshToken,
        user: {
            id: user.id,
            name: user.name,
            email: user.email
        }
    });
}));

// Login user
router.post('/login', authLimiter, asyncHandler(async (req, res) => {
    const { email, password } = req.body;
    
    // Validation
    if (!email || !password) {
        return res.status(400).json({ error: 'Please provide email and password' });
    }
    
    // Check for user and validate password
    const user = await User.validatePassword(email, password);
    
    if (!user) {
        return res.status(401).json({ error: 'Invalid credentials' });
    }
    
    // Generate tokens
    const token = generateToken(user.id);
    const refreshToken = generateRefreshToken(user.id);
    
    res.json({
        success: true,
        token,
        refreshToken,
        user: {
            id: user.id,
            name: user.name,
            email: user.email
        }
    });
}));

// Get current user
router.get('/me', protect, asyncHandler(async (req, res) => {
    const user = await User.findById(req.user.id);
    
    res.json({
        success: true,
        user: {
            id: user.id,
            name: user.name,
            email: user.email
        }
    });
}));

// Update user profile
router.put('/me', protect, asyncHandler(async (req, res) => {
    const { name, email } = req.body;
    
    const user = await User.update(req.user.id, { name, email });
    
    res.json({
        success: true,
        user
    });
}));

// Change password
router.put('/change-password', protect, asyncHandler(async (req, res) => {
    const { currentPassword, newPassword } = req.body;
    
    if (!currentPassword || !newPassword) {
        return res.status(400).json({ error: 'Please provide current and new password' });
    }
    
    // Validate current password
    const user = await User.validatePassword(req.user.email, currentPassword);
    if (!user) {
        return res.status(401).json({ error: 'Current password is incorrect' });
    }
    
    if (newPassword.length < 6) {
        return res.status(400).json({ error: 'New password must be at least 6 characters' });
    }
    
    // Update password
    await User.updatePassword(req.user.id, newPassword);
    
    res.json({
        success: true,
        message: 'Password updated successfully'
    });
}));

// Refresh token
router.post('/refresh', asyncHandler(async (req, res) => {
    const { refreshToken } = req.body;
    
    if (!refreshToken) {
        return res.status(401).json({ error: 'Refresh token required' });
    }
    
    try {
        const decoded = jwt.verify(refreshToken, process.env.JWT_REFRESH_SECRET);
        const user = await User.findById(decoded.userId);
        
        if (!user) {
            return res.status(401).json({ error: 'Invalid refresh token' });
        }
        
        const newToken = generateToken(user.id);
        
        res.json({
            success: true,
            token: newToken
        });
    } catch (error) {
        return res.status(401).json({ error: 'Invalid refresh token' });
    }
}));

module.exports = router;
```

### Session-Based Authentication (Alternative)

```bash
# Install session packages
npm install express-session connect-redis redis
```

```javascript
// config/session.js
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redis = require('redis');

const redisClient = redis.createClient({
    host: process.env.REDIS_HOST || 'localhost',
    port: process.env.REDIS_PORT || 6379,
    password: process.env.REDIS_PASSWORD
});

const sessionConfig = {
    store: new RedisStore({ client: redisClient }),
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false,
    cookie: {
        secure: process.env.NODE_ENV === 'production', // HTTPS only in production
        httpOnly: true, // Prevent XSS
        maxAge: 24 * 60 * 60 * 1000 // 24 hours
    }
};

module.exports = sessionConfig;

// middleware/sessionAuth.js
const requireAuth = (req, res, next) => {
    if (req.session && req.session.userId) {
        return next();
    } else {
        return res.status(401).json({ error: 'Authentication required' });
    }
};

const requireRole = (role) => {
    return (req, res, next) => {
        if (req.session && req.session.userRole === role) {
            return next();
        } else {
            return res.status(403).json({ error: 'Insufficient permissions' });
        }
    };
};

module.exports = { requireAuth, requireRole };
```
---

## API Design and Development

### RESTful API Design Principles

```javascript
// routes/api/v1/index.js - API versioning
const express = require('express');
const router = express.Router();

// Import route modules
const authRoutes = require('./auth');
const userRoutes = require('./users');
const postRoutes = require('./posts');
const commentRoutes = require('./comments');

// API documentation endpoint
router.get('/', (req, res) => {
    res.json({
        message: 'API v1',
        version: '1.0.0',
        endpoints: {
            auth: '/api/v1/auth',
            users: '/api/v1/users',
            posts: '/api/v1/posts',
            comments: '/api/v1/comments'
        },
        documentation: '/api/v1/docs'
    });
});

// Mount routes
router.use('/auth', authRoutes);
router.use('/users', userRoutes);
router.use('/posts', postRoutes);
router.use('/comments', commentRoutes);

module.exports = router;

// routes/api/v1/posts.js - Complete CRUD API
const express = require('express');
const router = express.Router();
const Post = require('../../../models/Post');
const { protect, authorize } = require('../../../middleware/auth');
const { asyncHandler } = require('../../../utils/asyncHandler');

// GET /api/v1/posts - Get all posts with filtering, sorting, pagination
router.get('/', asyncHandler(async (req, res) => {
    const {
        page = 1,
        limit = 10,
        sort = '-createdAt',
        published,
        author,
        search,
        tags
    } = req.query;

    // Build filter object
    const filter = {};
    
    if (published !== undefined) {
        filter.published = published === 'true';
    }
    
    if (author) {
        filter.author = author;
    }
    
    if (search) {
        filter.$or = [
            { title: { $regex: search, $options: 'i' } },
            { content: { $regex: search, $options: 'i' } }
        ];
    }
    
    if (tags) {
        filter.tags = { $in: tags.split(',') };
    }

    // Execute query with pagination
    const posts = await Post.find(filter)
        .sort(sort)
        .limit(limit * 1)
        .skip((page - 1) * limit)
        .populate('author', 'name email');

    // Get total count for pagination
    const total = await Post.countDocuments(filter);

    res.json({
        success: true,
        count: posts.length,
        pagination: {
            page: parseInt(page),
            limit: parseInt(limit),
            total,
            pages: Math.ceil(total / limit)
        },
        data: posts
    });
}));

// GET /api/v1/posts/:id - Get single post
router.get('/:id', asyncHandler(async (req, res) => {
    const post = await Post.findById(req.params.id).populate('author', 'name email');
    
    if (!post) {
        return res.status(404).json({ error: 'Post not found' });
    }
    
    // Increment view count
    post.views += 1;
    await post.save();
    
    res.json({
        success: true,
        data: post
    });
}));

// POST /api/v1/posts - Create new post
router.post('/', protect, asyncHandler(async (req, res) => {
    const { title, content, published = false, tags = [] } = req.body;
    
    const post = await Post.create({
        title,
        content,
        published,
        tags,
        author: req.user.id
    });
    
    await post.populate('author', 'name email');
    
    res.status(201).json({
        success: true,
        data: post
    });
}));

// PUT /api/v1/posts/:id - Update post
router.put('/:id', protect, asyncHandler(async (req, res) => {
    let post = await Post.findById(req.params.id);
    
    if (!post) {
        return res.status(404).json({ error: 'Post not found' });
    }
    
    // Check ownership or admin role
    if (post.author.toString() !== req.user.id && req.user.role !== 'admin') {
        return res.status(403).json({ error: 'Not authorized to update this post' });
    }
    
    const { title, content, published, tags } = req.body;
    
    post = await Post.findByIdAndUpdate(
        req.params.id,
        { title, content, published, tags },
        { new: true, runValidators: true }
    ).populate('author', 'name email');
    
    res.json({
        success: true,
        data: post
    });
}));

// DELETE /api/v1/posts/:id - Delete post
router.delete('/:id', protect, asyncHandler(async (req, res) => {
    const post = await Post.findById(req.params.id);
    
    if (!post) {
        return res.status(404).json({ error: 'Post not found' });
    }
    
    // Check ownership or admin role
    if (post.author.toString() !== req.user.id && req.user.role !== 'admin') {
        return res.status(403).json({ error: 'Not authorized to delete this post' });
    }
    
    await post.remove();
    
    res.json({
        success: true,
        message: 'Post deleted successfully'
    });
}));

// POST /api/v1/posts/:id/publish - Publish/unpublish post
router.post('/:id/publish', protect, asyncHandler(async (req, res) => {
    const post = await Post.findById(req.params.id);
    
    if (!post) {
        return res.status(404).json({ error: 'Post not found' });
    }
    
    // Check ownership or admin role
    if (post.author.toString() !== req.user.id && req.user.role !== 'admin') {
        return res.status(403).json({ error: 'Not authorized to publish this post' });
    }
    
    post.published = !post.published;
    await post.save();
    
    res.json({
        success: true,
        message: `Post ${post.published ? 'published' : 'unpublished'} successfully`,
        data: post
    });
}));

module.exports = router;
```

### Input Validation and Sanitization

```bash
# Install validation packages
npm install joi express-validator
npm install express-mongo-sanitize xss-clean
```

```javascript
// middleware/validation.js
const Joi = require('joi');

const validate = (schema) => {
    return (req, res, next) => {
        const { error } = schema.validate(req.body);
        
        if (error) {
            const errorMessage = error.details.map(detail => detail.message).join(', ');
            return res.status(400).json({ error: errorMessage });
        }
        
        next();
    };
};

// Validation schemas
const schemas = {
    createUser: Joi.object({
        name: Joi.string().min(2).max(100).required(),
        email: Joi.string().email().required(),
        password: Joi.string().min(6).max(128).required()
    }),
    
    updateUser: Joi.object({
        name: Joi.string().min(2).max(100),
        email: Joi.string().email()
    }),
    
    createPost: Joi.object({
        title: Joi.string().min(5).max(200).required(),
        content: Joi.string().min(10).required(),
        published: Joi.boolean().default(false),
        tags: Joi.array().items(Joi.string().max(50)).max(10)
    }),
    
    login: Joi.object({
        email: Joi.string().email().required(),
        password: Joi.string().required()
    })
};

module.exports = { validate, schemas };

// Using validation middleware
const { validate, schemas } = require('../middleware/validation');

router.post('/register', validate(schemas.createUser), asyncHandler(async (req, res) => {
    // Registration logic here
}));

router.post('/login', validate(schemas.login), asyncHandler(async (req, res) => {
    // Login logic here
}));

// Alternative: express-validator
const { body, validationResult } = require('express-validator');

const validateCreatePost = [
    body('title')
        .isLength({ min: 5, max: 200 })
        .withMessage('Title must be between 5 and 200 characters')
        .trim()
        .escape(),
    body('content')
        .isLength({ min: 10 })
        .withMessage('Content must be at least 10 characters')
        .trim(),
    body('published')
        .optional()
        .isBoolean()
        .withMessage('Published must be a boolean'),
    body('tags')
        .optional()
        .isArray({ max: 10 })
        .withMessage('Tags must be an array with maximum 10 items')
];

const handleValidationErrors = (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
        return res.status(400).json({
            error: 'Validation failed',
            details: errors.array()
        });
    }
    next();
};

router.post('/posts', 
    validateCreatePost, 
    handleValidationErrors, 
    protect, 
    asyncHandler(async (req, res) => {
        // Create post logic here
    })
);
```

### File Upload Handling

```bash
# Install file upload packages
npm install multer sharp
```

```javascript
// middleware/upload.js
const multer = require('multer');
const sharp = require('sharp');
const path = require('path');
const fs = require('fs').promises;

// Configure multer for memory storage
const storage = multer.memoryStorage();

const fileFilter = (req, file, cb) => {
    // Check file type
    if (file.mimetype.startsWith('image/')) {
        cb(null, true);
    } else {
        cb(new Error('Not an image! Please upload only images.'), false);
    }
};

const upload = multer({
    storage,
    fileFilter,
    limits: {
        fileSize: 5 * 1024 * 1024 // 5MB limit
    }
});

// Image processing middleware
const processImage = async (req, res, next) => {
    if (!req.file) return next();
    
    try {
        const filename = `user-${req.user.id}-${Date.now()}.jpeg`;
        const filepath = path.join(__dirname, '../uploads/images', filename);
        
        // Ensure upload directory exists
        await fs.mkdir(path.dirname(filepath), { recursive: true });
        
        // Process image with sharp
        await sharp(req.file.buffer)
            .resize(800, 600, {
                fit: 'inside',
                withoutEnlargement: true
            })
            .jpeg({ quality: 90 })
            .toFile(filepath);
        
        req.file.filename = filename;
        req.file.path = filepath;
        
        next();
    } catch (error) {
        next(error);
    }
};

// Multiple file upload processing
const processMultipleImages = async (req, res, next) => {
    if (!req.files || req.files.length === 0) return next();
    
    try {
        const processedFiles = [];
        
        for (let i = 0; i < req.files.length; i++) {
            const file = req.files[i];
            const filename = `gallery-${req.user.id}-${Date.now()}-${i}.jpeg`;
            const filepath = path.join(__dirname, '../uploads/gallery', filename);
            
            await fs.mkdir(path.dirname(filepath), { recursive: true });
            
            await sharp(file.buffer)
                .resize(1200, 800, {
                    fit: 'inside',
                    withoutEnlargement: true
                })
                .jpeg({ quality: 85 })
                .toFile(filepath);
            
            processedFiles.push({
                filename,
                path: filepath,
                originalName: file.originalname
            });
        }
        
        req.processedFiles = processedFiles;
        next();
    } catch (error) {
        next(error);
    }
};

module.exports = { upload, processImage, processMultipleImages };

// routes/upload.js
const express = require('express');
const router = express.Router();
const { upload, processImage, processMultipleImages } = require('../middleware/upload');
const { protect } = require('../middleware/auth');
const { asyncHandler } = require('../utils/asyncHandler');

// Single file upload
router.post('/avatar', 
    protect,
    upload.single('avatar'),
    processImage,
    asyncHandler(async (req, res) => {
        if (!req.file) {
            return res.status(400).json({ error: 'No file uploaded' });
        }
        
        // Update user avatar in database
        await User.findByIdAndUpdate(req.user.id, {
            avatar: `/uploads/images/${req.file.filename}`
        });
        
        res.json({
            success: true,
            message: 'Avatar uploaded successfully',
            filename: req.file.filename,
            url: `/uploads/images/${req.file.filename}`
        });
    })
);

// Multiple file upload
router.post('/gallery',
    protect,
    upload.array('images', 10), // Max 10 files
    processMultipleImages,
    asyncHandler(async (req, res) => {
        if (!req.processedFiles || req.processedFiles.length === 0) {
            return res.status(400).json({ error: 'No files uploaded' });
        }
        
        const imageUrls = req.processedFiles.map(file => ({
            filename: file.filename,
            url: `/uploads/gallery/${file.filename}`,
            originalName: file.originalName
        }));
        
        res.json({
            success: true,
            message: `${imageUrls.length} images uploaded successfully`,
            images: imageUrls
        });
    })
);

module.exports = router;
```
---

## Security Best Practices

### Essential Security Middleware

```bash
# Install security packages
npm install helmet cors express-rate-limit express-mongo-sanitize xss-clean hpp
```

```javascript
// middleware/security.js
const helmet = require('helmet');
const cors = require('cors');
const rateLimit = require('express-rate-limit');
const mongoSanitize = require('express-mongo-sanitize');
const xss = require('xss-clean');
const hpp = require('hpp');

// Rate limiting
const createRateLimiter = (windowMs, max, message) => {
    return rateLimit({
        windowMs,
        max,
        message: { error: message },
        standardHeaders: true,
        legacyHeaders: false,
    });
};

// Different rate limits for different endpoints
const rateLimiters = {
    general: createRateLimiter(15 * 60 * 1000, 100, 'Too many requests from this IP'),
    auth: createRateLimiter(15 * 60 * 1000, 5, 'Too many authentication attempts'),
    api: createRateLimiter(15 * 60 * 1000, 1000, 'API rate limit exceeded')
};

// CORS configuration
const corsOptions = {
    origin: function (origin, callback) {
        const allowedOrigins = process.env.ALLOWED_ORIGINS?.split(',') || ['http://localhost:3000'];
        
        // Allow requests with no origin (mobile apps, etc.)
        if (!origin) return callback(null, true);
        
        if (allowedOrigins.includes(origin)) {
            callback(null, true);
        } else {
            callback(new Error('Not allowed by CORS'));
        }
    },
    credentials: true,
    optionsSuccessStatus: 200
};

// Security middleware setup
const setupSecurity = (app) => {
    // Set security headers
    app.use(helmet({
        contentSecurityPolicy: {
            directives: {
                defaultSrc: ["'self'"],
                styleSrc: ["'self'", "'unsafe-inline'"],
                scriptSrc: ["'self'"],
                imgSrc: ["'self'", "data:", "https:"],
            },
        },
        crossOriginEmbedderPolicy: false
    }));
    
    // Enable CORS
    app.use(cors(corsOptions));
    
    // Data sanitization against NoSQL query injection
    app.use(mongoSanitize());
    
    // Data sanitization against XSS
    app.use(xss());
    
    // Prevent parameter pollution
    app.use(hpp({
        whitelist: ['sort', 'fields', 'page', 'limit']
    }));
    
    // Apply rate limiting
    app.use('/api/', rateLimiters.api);
    app.use('/auth/', rateLimiters.auth);
    app.use(rateLimiters.general);
};

module.exports = { setupSecurity, rateLimiters };

// utils/encryption.js
const crypto = require('crypto');

class Encryption {
    constructor() {
        this.algorithm = 'aes-256-gcm';
        this.secretKey = process.env.ENCRYPTION_KEY || crypto.randomBytes(32);
    }
    
    encrypt(text) {
        const iv = crypto.randomBytes(16);
        const cipher = crypto.createCipher(this.algorithm, this.secretKey);
        cipher.setAAD(Buffer.from('additional-data'));
        
        let encrypted = cipher.update(text, 'utf8', 'hex');
        encrypted += cipher.final('hex');
        
        const authTag = cipher.getAuthTag();
        
        return {
            encrypted,
            iv: iv.toString('hex'),
            authTag: authTag.toString('hex')
        };
    }
    
    decrypt(encryptedData) {
        const { encrypted, iv, authTag } = encryptedData;
        
        const decipher = crypto.createDecipher(this.algorithm, this.secretKey);
        decipher.setAAD(Buffer.from('additional-data'));
        decipher.setAuthTag(Buffer.from(authTag, 'hex'));
        
        let decrypted = decipher.update(encrypted, 'hex', 'utf8');
        decrypted += decipher.final('utf8');
        
        return decrypted;
    }
    
    hash(data) {
        return crypto.createHash('sha256').update(data).digest('hex');
    }
    
    generateSecureToken(length = 32) {
        return crypto.randomBytes(length).toString('hex');
    }
}

module.exports = new Encryption();
```

### Input Validation and SQL Injection Prevention

```javascript
// middleware/sqlInjectionPrevention.js
const validator = require('validator');

const sanitizeInput = (req, res, next) => {
    // Sanitize all string inputs
    const sanitizeObject = (obj) => {
        for (let key in obj) {
            if (typeof obj[key] === 'string') {
                // Remove potential SQL injection patterns
                obj[key] = validator.escape(obj[key]);
                
                // Remove common SQL injection keywords
                const sqlPatterns = [
                    /(\b(SELECT|INSERT|UPDATE|DELETE|DROP|CREATE|ALTER|EXEC|UNION|SCRIPT)\b)/gi,
                    /(--|\/\*|\*\/|;)/g,
                    /(\b(OR|AND)\s+\d+\s*=\s*\d+)/gi
                ];
                
                sqlPatterns.forEach(pattern => {
                    obj[key] = obj[key].replace(pattern, '');
                });
            } else if (typeof obj[key] === 'object' && obj[key] !== null) {
                sanitizeObject(obj[key]);
            }
        }
    };
    
    if (req.body) sanitizeObject(req.body);
    if (req.query) sanitizeObject(req.query);
    if (req.params) sanitizeObject(req.params);
    
    next();
};

// Parameterized query helper for PostgreSQL
class DatabaseQuery {
    constructor(pool) {
        this.pool = pool;
    }
    
    async query(text, params = []) {
        const start = Date.now();
        
        try {
            const result = await this.pool.query(text, params);
            const duration = Date.now() - start;
            
            console.log('Executed query', { text, duration, rows: result.rowCount });
            return result;
        } catch (error) {
            console.error('Database query error:', error);
            throw error;
        }
    }
    
    // Safe dynamic query builder
    buildSelectQuery(table, conditions = {}, options = {}) {
        let query = `SELECT * FROM ${table}`;
        const params = [];
        const whereClauses = [];
        
        // Build WHERE clauses safely
        Object.keys(conditions).forEach((key, index) => {
            // Validate column name (whitelist approach)
            const allowedColumns = options.allowedColumns || [];
            if (allowedColumns.length > 0 && !allowedColumns.includes(key)) {
                throw new Error(`Column ${key} is not allowed`);
            }
            
            whereClauses.push(`${key} = $${params.length + 1}`);
            params.push(conditions[key]);
        });
        
        if (whereClauses.length > 0) {
            query += ` WHERE ${whereClauses.join(' AND ')}`;
        }
        
        // Add ORDER BY safely
        if (options.orderBy) {
            const allowedSortColumns = options.allowedSortColumns || options.allowedColumns || [];
            if (allowedSortColumns.includes(options.orderBy)) {
                query += ` ORDER BY ${options.orderBy}`;
                if (options.orderDirection === 'DESC') {
                    query += ' DESC';
                }
            }
        }
        
        // Add LIMIT and OFFSET
        if (options.limit) {
            query += ` LIMIT $${params.length + 1}`;
            params.push(options.limit);
        }
        
        if (options.offset) {
            query += ` OFFSET $${params.length + 1}`;
            params.push(options.offset);
        }
        
        return { query, params };
    }
}

module.exports = { sanitizeInput, DatabaseQuery };
```

### Environment Variables and Secrets Management

```javascript
// config/environment.js
const dotenv = require('dotenv');
const path = require('path');

// Load environment variables
dotenv.config({ path: path.join(__dirname, '../.env') });

const requiredEnvVars = [
    'NODE_ENV',
    'PORT',
    'DATABASE_URL',
    'JWT_SECRET',
    'JWT_REFRESH_SECRET'
];

// Validate required environment variables
const validateEnvironment = () => {
    const missing = requiredEnvVars.filter(envVar => !process.env[envVar]);
    
    if (missing.length > 0) {
        console.error('Missing required environment variables:', missing);
        process.exit(1);
    }
};

const config = {
    env: process.env.NODE_ENV || 'development',
    port: parseInt(process.env.PORT) || 3000,
    
    database: {
        url: process.env.DATABASE_URL,
        ssl: process.env.NODE_ENV === 'production'
    },
    
    jwt: {
        secret: process.env.JWT_SECRET,
        refreshSecret: process.env.JWT_REFRESH_SECRET,
        expiresIn: process.env.JWT_EXPIRES_IN || '7d'
    },
    
    redis: {
        host: process.env.REDIS_HOST || 'localhost',
        port: parseInt(process.env.REDIS_PORT) || 6379,
        password: process.env.REDIS_PASSWORD
    },
    
    email: {
        host: process.env.EMAIL_HOST,
        port: parseInt(process.env.EMAIL_PORT) || 587,
        user: process.env.EMAIL_USER,
        password: process.env.EMAIL_PASSWORD
    },
    
    aws: {
        accessKeyId: process.env.AWS_ACCESS_KEY_ID,
        secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
        region: process.env.AWS_REGION || 'us-east-1',
        s3Bucket: process.env.AWS_S3_BUCKET
    }
};

// Validate environment on startup
validateEnvironment();

module.exports = config;

// .env.example file
/*
NODE_ENV=development
PORT=3000

# Database
DATABASE_URL=postgresql://username:password@localhost:5432/myapp
MONGODB_URI=mongodb://localhost:27017/myapp

# JWT
JWT_SECRET=your-super-secret-jwt-key-here
JWT_REFRESH_SECRET=your-super-secret-refresh-key-here
JWT_EXPIRES_IN=7d

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Email
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-password

# AWS
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_REGION=us-east-1
AWS_S3_BUCKET=your-bucket-name

# Security
ENCRYPTION_KEY=your-32-byte-encryption-key-here
ALLOWED_ORIGINS=http://localhost:3000,https://yourdomain.com

# External APIs
STRIPE_SECRET_KEY=sk_test_...
SENDGRID_API_KEY=SG...
*/
```
---

## Testing Backend Applications

### Unit Testing with Jest

```bash
# Install testing dependencies
npm install -D jest supertest
npm install -D @types/jest @types/supertest
```

```javascript
// tests/unit/user.test.js
const User = require('../../models/User');
const bcrypt = require('bcrypt');

// Mock the database
jest.mock('../../config/database');

describe('User Model', () => {
    beforeEach(() => {
        jest.clearAllMocks();
    });

    describe('User.create', () => {
        test('should create a user with hashed password', async () => {
            const userData = {
                name: 'John Doe',
                email: 'john@example.com',
                password: 'password123'
            };

            const mockUser = {
                id: 1,
                name: 'John Doe',
                email: 'john@example.com',
                created_at: new Date()
            };

            // Mock database query
            const mockQuery = jest.fn().mockResolvedValue({ rows: [mockUser] });
            require('../../config/database').query = mockQuery;

            const result = await User.create(userData);

            expect(result).toEqual(mockUser);
            expect(mockQuery).toHaveBeenCalledWith(
                expect.stringContaining('INSERT INTO users'),
                expect.arrayContaining([userData.name, userData.email, expect.any(String)])
            );
        });

        test('should throw error if email already exists', async () => {
            const userData = {
                name: 'John Doe',
                email: 'existing@example.com',
                password: 'password123'
            };

            const mockQuery = jest.fn().mockRejectedValue(new Error('Email already exists'));
            require('../../config/database').query = mockQuery;

            await expect(User.create(userData)).rejects.toThrow('Email already exists');
        });
    });

    describe('User.validatePassword', () => {
        test('should return user if password is correct', async () => {
            const email = 'john@example.com';
            const password = 'password123';
            const hashedPassword = await bcrypt.hash(password, 12);

            const mockUser = {
                id: 1,
                name: 'John Doe',
                email,
                password_hash: hashedPassword
            };

            const mockQuery = jest.fn().mockResolvedValue({ rows: [mockUser] });
            require('../../config/database').query = mockQuery;

            const result = await User.validatePassword(email, password);

            expect(result).toEqual({
                id: 1,
                name: 'John Doe',
                email
            });
        });

        test('should return null if password is incorrect', async () => {
            const email = 'john@example.com';
            const password = 'wrongpassword';
            const hashedPassword = await bcrypt.hash('correctpassword', 12);

            const mockUser = {
                id: 1,
                email,
                password_hash: hashedPassword
            };

            const mockQuery = jest.fn().mockResolvedValue({ rows: [mockUser] });
            require('../../config/database').query = mockQuery;

            const result = await User.validatePassword(email, password);

            expect(result).toBeNull();
        });
    });
});

// tests/unit/auth.test.js
const jwt = require('jsonwebtoken');
const { generateToken, protect } = require('../../middleware/auth');

describe('Auth Middleware', () => {
    describe('generateToken', () => {
        test('should generate a valid JWT token', () => {
            const userId = 123;
            const token = generateToken(userId);

            const decoded = jwt.verify(token, process.env.JWT_SECRET);
            expect(decoded.userId).toBe(userId);
        });
    });

    describe('protect middleware', () => {
        let req, res, next;

        beforeEach(() => {
            req = {
                headers: {}
            };
            res = {
                status: jest.fn().mockReturnThis(),
                json: jest.fn()
            };
            next = jest.fn();
        });

        test('should authenticate user with valid token', async () => {
            const userId = 123;
            const token = generateToken(userId);
            
            req.headers.authorization = `Bearer ${token}`;

            // Mock User.findById
            const mockUser = { id: userId, name: 'John Doe' };
            const User = require('../../models/User');
            User.findById = jest.fn().mockResolvedValue(mockUser);

            await protect(req, res, next);

            expect(req.user).toEqual(mockUser);
            expect(next).toHaveBeenCalled();
        });

        test('should return 401 if no token provided', async () => {
            await protect(req, res, next);

            expect(res.status).toHaveBeenCalledWith(401);
            expect(res.json).toHaveBeenCalledWith({ error: 'Not authorized, no token' });
            expect(next).not.toHaveBeenCalled();
        });

        test('should return 401 if token is invalid', async () => {
            req.headers.authorization = 'Bearer invalid-token';

            await protect(req, res, next);

            expect(res.status).toHaveBeenCalledWith(401);
            expect(res.json).toHaveBeenCalledWith({ error: 'Not authorized, token failed' });
            expect(next).not.toHaveBeenCalled();
        });
    });
});
```

### Integration Testing

```javascript
// tests/integration/auth.test.js
const request = require('supertest');
const app = require('../../server');
const User = require('../../models/User');

describe('Auth Endpoints', () => {
    beforeEach(async () => {
        // Clean up database before each test
        await User.deleteAll();
    });

    describe('POST /api/auth/register', () => {
        test('should register a new user', async () => {
            const userData = {
                name: 'John Doe',
                email: 'john@example.com',
                password: 'password123'
            };

            const response = await request(app)
                .post('/api/auth/register')
                .send(userData)
                .expect(201);

            expect(response.body.success).toBe(true);
            expect(response.body.token).toBeDefined();
            expect(response.body.user.email).toBe(userData.email);
            expect(response.body.user.password).toBeUndefined();
        });

        test('should return 400 if email already exists', async () => {
            const userData = {
                name: 'John Doe',
                email: 'john@example.com',
                password: 'password123'
            };

            // Create user first
            await User.create(userData);

            const response = await request(app)
                .post('/api/auth/register')
                .send(userData)
                .expect(400);

            expect(response.body.error).toBe('User already exists');
        });

        test('should return 400 if required fields are missing', async () => {
            const response = await request(app)
                .post('/api/auth/register')
                .send({ name: 'John Doe' })
                .expect(400);

            expect(response.body.error).toContain('email');
        });
    });

    describe('POST /api/auth/login', () => {
        beforeEach(async () => {
            // Create a test user
            await User.create({
                name: 'John Doe',
                email: 'john@example.com',
                password: 'password123'
            });
        });

        test('should login with valid credentials', async () => {
            const response = await request(app)
                .post('/api/auth/login')
                .send({
                    email: 'john@example.com',
                    password: 'password123'
                })
                .expect(200);

            expect(response.body.success).toBe(true);
            expect(response.body.token).toBeDefined();
            expect(response.body.user.email).toBe('john@example.com');
        });

        test('should return 401 with invalid credentials', async () => {
            const response = await request(app)
                .post('/api/auth/login')
                .send({
                    email: 'john@example.com',
                    password: 'wrongpassword'
                })
                .expect(401);

            expect(response.body.error).toBe('Invalid credentials');
        });
    });

    describe('GET /api/auth/me', () => {
        let authToken;

        beforeEach(async () => {
            // Create and login user
            const user = await User.create({
                name: 'John Doe',
                email: 'john@example.com',
                password: 'password123'
            });

            const loginResponse = await request(app)
                .post('/api/auth/login')
                .send({
                    email: 'john@example.com',
                    password: 'password123'
                });

            authToken = loginResponse.body.token;
        });

        test('should return user profile with valid token', async () => {
            const response = await request(app)
                .get('/api/auth/me')
                .set('Authorization', `Bearer ${authToken}`)
                .expect(200);

            expect(response.body.success).toBe(true);
            expect(response.body.user.email).toBe('john@example.com');
        });

        test('should return 401 without token', async () => {
            const response = await request(app)
                .get('/api/auth/me')
                .expect(401);

            expect(response.body.error).toBe('Not authorized, no token');
        });
    });
});

// tests/integration/posts.test.js
describe('Posts API', () => {
    let authToken;
    let userId;

    beforeEach(async () => {
        // Clean database
        await User.deleteAll();
        await Post.deleteAll();

        // Create and authenticate user
        const user = await User.create({
            name: 'John Doe',
            email: 'john@example.com',
            password: 'password123'
        });
        userId = user.id;

        const loginResponse = await request(app)
            .post('/api/auth/login')
            .send({
                email: 'john@example.com',
                password: 'password123'
            });

        authToken = loginResponse.body.token;
    });

    describe('GET /api/posts', () => {
        test('should return paginated posts', async () => {
            // Create test posts
            await Post.create({
                title: 'Test Post 1',
                content: 'Content 1',
                author_id: userId,
                published: true
            });

            await Post.create({
                title: 'Test Post 2',
                content: 'Content 2',
                author_id: userId,
                published: false
            });

            const response = await request(app)
                .get('/api/posts?published=true&limit=10&page=1')
                .expect(200);

            expect(response.body.success).toBe(true);
            expect(response.body.data).toHaveLength(1);
            expect(response.body.pagination.total).toBe(1);
        });
    });

    describe('POST /api/posts', () => {
        test('should create a new post', async () => {
            const postData = {
                title: 'New Test Post',
                content: 'This is test content',
                published: true
            };

            const response = await request(app)
                .post('/api/posts')
                .set('Authorization', `Bearer ${authToken}`)
                .send(postData)
                .expect(201);

            expect(response.body.success).toBe(true);
            expect(response.body.data.title).toBe(postData.title);
            expect(response.body.data.author_id).toBe(userId);
        });

        test('should return 401 without authentication', async () => {
            const postData = {
                title: 'New Test Post',
                content: 'This is test content'
            };

            const response = await request(app)
                .post('/api/posts')
                .send(postData)
                .expect(401);

            expect(response.body.error).toBe('Not authorized, no token');
        });
    });
});
```

### Load Testing

```bash
# Install load testing tools
npm install -D artillery
```

```yaml
# artillery-config.yml
config:
  target: 'http://localhost:3000'
  phases:
    - duration: 60
      arrivalRate: 10
    - duration: 120
      arrivalRate: 50
    - duration: 60
      arrivalRate: 100
  defaults:
    headers:
      Content-Type: 'application/json'

scenarios:
  - name: "API Load Test"
    weight: 100
    flow:
      - get:
          url: "/api/health"
      - post:
          url: "/api/auth/login"
          json:
            email: "test@example.com"
            password: "password123"
          capture:
            - json: "$.token"
              as: "authToken"
      - get:
          url: "/api/posts"
          headers:
            Authorization: "Bearer {{ authToken }}"
      - post:
          url: "/api/posts"
          headers:
            Authorization: "Bearer {{ authToken }}"
          json:
            title: "Load Test Post {{ $randomString() }}"
            content: "This is a load test post content"
```

```bash
# Run load tests
npx artillery run artillery-config.yml
```

---

## Deployment and DevOps

### Docker Configuration

```dockerfile
# Dockerfile
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001

# Change ownership of the app directory
RUN chown -R nodejs:nodejs /app
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node healthcheck.js

# Start the application
CMD ["npm", "start"]

# .dockerignore
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
.nyc_output
coverage
.nyc_output
.coverage
.coverage/
tests/
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://postgres:password@db:5432/myapp
      - REDIS_HOST=redis
    depends_on:
      - db
      - redis
    restart: unless-stopped
    volumes:
      - ./uploads:/app/uploads

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    restart: unless-stopped
    volumes:
      - redis_data:/data

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - app
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

### CI/CD Pipeline (GitHub Actions)

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run linting
      run: npm run lint
    
    - name: Run tests
      run: npm test
      env:
        NODE_ENV: test
        DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db
        REDIS_HOST: localhost
        JWT_SECRET: test-secret
        JWT_REFRESH_SECRET: test-refresh-secret
    
    - name: Run integration tests
      run: npm run test:integration
      env:
        NODE_ENV: test
        DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db
        REDIS_HOST: localhost
        JWT_SECRET: test-secret
        JWT_REFRESH_SECRET: test-refresh-secret
    
    - name: Upload coverage reports
      uses: codecov/codecov-action@v3

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2
    
    - name: Login to DockerHub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v4
      with:
        context: .
        push: true
        tags: |
          myapp/backend:latest
          myapp/backend:${{ github.sha }}

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Deploy to production
      uses: appleboy/ssh-action@v0.1.5
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        key: ${{ secrets.SSH_KEY }}
        script: |
          cd /app
          docker-compose pull
          docker-compose up -d
          docker system prune -f
```

---

## Career Path

### 🎯 Junior Backend Developer (0-2 years)
**Skills Required:**
- Node.js and Express fundamentals
- Basic database operations (SQL or NoSQL)
- RESTful API development
- Git version control
- Basic authentication

**Typical Responsibilities:**
- Implement API endpoints from specifications
- Fix bugs and add small features
- Write basic tests
- Database queries and data manipulation

**Salary Range:** $50k-$75k

### 🚀 Mid-Level Backend Developer (2-4 years)
**Skills Required:**
- Advanced Node.js patterns
- Database design and optimization
- Authentication and authorization
- Testing strategies
- Basic DevOps (Docker, deployment)

**Typical Responsibilities:**
- Design and implement complex features
- Database schema design
- Performance optimization
- Code reviews and mentoring juniors

**Salary Range:** $70k-$110k

### 🏆 Senior Backend Developer (4+ years)
**Skills Required:**
- System architecture and design
- Advanced security practices
- Performance optimization
- Team leadership
- Multiple databases and technologies

**Typical Responsibilities:**
- System architecture decisions
- Lead technical initiatives
- Mentor team members
- Cross-functional collaboration

**Salary Range:** $100k-$160k+

### 🎨 Backend Architect/Principal Engineer (7+ years)
**Skills Required:**
- Large-scale system design
- Technology strategy
- Team leadership
- Business acumen
- Innovation and emerging technologies

**Typical Responsibilities:**
- Define technical vision
- Lead multiple teams
- Drive technology adoption
- Strategic planning

**Salary Range:** $140k-$220k+

### Next Steps in Your Learning Journey

1. **Master Node.js Fundamentals** (Weeks 1-4)
   - Complete Express.js tutorial
   - Build 3-5 REST APIs
   - Learn async/await patterns

2. **Database Integration** (Weeks 5-8)
   - Choose PostgreSQL or MongoDB
   - Learn database design principles
   - Implement CRUD operations

3. **Authentication & Security** (Weeks 9-10)
   - Implement JWT authentication
   - Learn security best practices
   - Add input validation

4. **Testing & Quality** (Weeks 11-12)
   - Write unit and integration tests
   - Set up CI/CD pipeline
   - Code quality tools

5. **Deployment & DevOps** (Weeks 13-14)
   - Learn Docker basics
   - Deploy to cloud platforms
   - Set up monitoring

6. **Advanced Topics** (Weeks 15-16)
   - Microservices architecture
   - Performance optimization
   - Advanced security

Remember: Build real projects throughout your learning journey. The best way to learn backend development is by creating APIs that solve actual problems and can handle real-world traffic!