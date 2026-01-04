# Frontend Development Specialization - Complete Learning Path

Master modern frontend development with React, Next.js, and performance optimization techniques. This guide takes you from framework basics to production-ready applications.

## Table of Contents

1. [React Fundamentals](#react-fundamentals)
2. [Next.js Framework](#nextjs-framework)
3. [Modern CSS Frameworks](#modern-css-frameworks)
4. [State Management](#state-management)
5. [Performance Optimization](#performance-optimization)
6. [Testing Frontend Applications](#testing-frontend-applications)
7. [Production Deployment](#production-deployment)
8. [Career Path](#career-path)

---

## React Fundamentals

### Why React?

React is the most popular frontend framework with:
- **High Job Demand**: 70%+ of frontend jobs require React
- **Component-Based**: Reusable, maintainable code
- **Large Ecosystem**: Extensive libraries and tools
- **Industry Standard**: Used by Facebook, Netflix, Airbnb, Uber

### Setting Up React Development Environment

```bash
# Install Node.js (v18+ recommended)
# Download from nodejs.org

# Create a new React app
npx create-react-app my-app
cd my-app
npm start

# Or use Vite (faster alternative)
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

### React Components and JSX

```jsx
// Functional Component (modern approach)
import React from 'react';

function Welcome({ name, age }) {
    return (
        <div className="welcome-container">
            <h1>Hello, {name}!</h1>
            <p>You are {age} years old.</p>
        </div>
    );
}

// Arrow Function Component
const Greeting = ({ message }) => {
    return <h2>{message}</h2>;
};

// Component with conditional rendering
const UserStatus = ({ isLoggedIn, username }) => {
    return (
        <div>
            {isLoggedIn ? (
                <p>Welcome back, {username}!</p>
            ) : (
                <p>Please log in to continue.</p>
            )}
        </div>
    );
};

// Component with list rendering
const TodoList = ({ todos }) => {
    return (
        <ul>
            {todos.map(todo => (
                <li key={todo.id} className={todo.completed ? 'completed' : ''}>
                    {todo.text}
                </li>
            ))}
        </ul>
    );
};

export default Welcome;
```
### React Hooks

Hooks let you use state and other React features in functional components:

```jsx
import React, { useState, useEffect, useContext, useReducer } from 'react';

// useState Hook
const Counter = () => {
    const [count, setCount] = useState(0);
    const [name, setName] = useState('');

    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
            <button onClick={() => setCount(prev => prev - 1)}>Decrement</button>
            
            <input 
                value={name}
                onChange={(e) => setName(e.target.value)}
                placeholder="Enter your name"
            />
        </div>
    );
};

// useEffect Hook
const UserProfile = ({ userId }) => {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const fetchUser = async () => {
            try {
                setLoading(true);
                const response = await fetch(`/api/users/${userId}`);
                if (!response.ok) throw new Error('Failed to fetch user');
                const userData = await response.json();
                setUser(userData);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };

        if (userId) {
            fetchUser();
        }
    }, [userId]); // Dependency array

    // Cleanup effect
    useEffect(() => {
        const timer = setInterval(() => {
            console.log('Timer tick');
        }, 1000);

        return () => clearInterval(timer); // Cleanup
    }, []);

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;
    if (!user) return <div>No user found</div>;

    return (
        <div>
            <h2>{user.name}</h2>
            <p>{user.email}</p>
        </div>
    );
};

// Custom Hook
const useLocalStorage = (key, initialValue) => {
    const [storedValue, setStoredValue] = useState(() => {
        try {
            const item = window.localStorage.getItem(key);
            return item ? JSON.parse(item) : initialValue;
        } catch (error) {
            return initialValue;
        }
    });

    const setValue = (value) => {
        try {
            setStoredValue(value);
            window.localStorage.setItem(key, JSON.stringify(value));
        } catch (error) {
            console.error('Error saving to localStorage:', error);
        }
    };

    return [storedValue, setValue];
};

// Using custom hook
const Settings = () => {
    const [theme, setTheme] = useLocalStorage('theme', 'light');
    
    return (
        <div>
            <p>Current theme: {theme}</p>
            <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
                Toggle Theme
            </button>
        </div>
    );
};
```

### Event Handling and Forms

```jsx
import React, { useState } from 'react';

const ContactForm = () => {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        message: '',
        category: 'general'
    });
    const [errors, setErrors] = useState({});
    const [isSubmitting, setIsSubmitting] = useState(false);

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({
            ...prev,
            [name]: value
        }));
        
        // Clear error when user starts typing
        if (errors[name]) {
            setErrors(prev => ({
                ...prev,
                [name]: ''
            }));
        }
    };

    const validateForm = () => {
        const newErrors = {};
        
        if (!formData.name.trim()) {
            newErrors.name = 'Name is required';
        }
        
        if (!formData.email.trim()) {
            newErrors.email = 'Email is required';
        } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
            newErrors.email = 'Email is invalid';
        }
        
        if (!formData.message.trim()) {
            newErrors.message = 'Message is required';
        }
        
        return newErrors;
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        
        const newErrors = validateForm();
        if (Object.keys(newErrors).length > 0) {
            setErrors(newErrors);
            return;
        }

        setIsSubmitting(true);
        try {
            const response = await fetch('/api/contact', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(formData),
            });

            if (response.ok) {
                alert('Message sent successfully!');
                setFormData({ name: '', email: '', message: '', category: 'general' });
            } else {
                throw new Error('Failed to send message');
            }
        } catch (error) {
            alert('Error sending message: ' + error.message);
        } finally {
            setIsSubmitting(false);
        }
    };

    return (
        <form onSubmit={handleSubmit} className="contact-form">
            <div className="form-group">
                <label htmlFor="name">Name:</label>
                <input
                    type="text"
                    id="name"
                    name="name"
                    value={formData.name}
                    onChange={handleChange}
                    className={errors.name ? 'error' : ''}
                />
                {errors.name && <span className="error-message">{errors.name}</span>}
            </div>

            <div className="form-group">
                <label htmlFor="email">Email:</label>
                <input
                    type="email"
                    id="email"
                    name="email"
                    value={formData.email}
                    onChange={handleChange}
                    className={errors.email ? 'error' : ''}
                />
                {errors.email && <span className="error-message">{errors.email}</span>}
            </div>

            <div className="form-group">
                <label htmlFor="category">Category:</label>
                <select
                    id="category"
                    name="category"
                    value={formData.category}
                    onChange={handleChange}
                >
                    <option value="general">General</option>
                    <option value="support">Support</option>
                    <option value="business">Business</option>
                </select>
            </div>

            <div className="form-group">
                <label htmlFor="message">Message:</label>
                <textarea
                    id="message"
                    name="message"
                    value={formData.message}
                    onChange={handleChange}
                    rows="5"
                    className={errors.message ? 'error' : ''}
                />
                {errors.message && <span className="error-message">{errors.message}</span>}
            </div>

            <button type="submit" disabled={isSubmitting}>
                {isSubmitting ? 'Sending...' : 'Send Message'}
            </button>
        </form>
    );
};
```
---

## Next.js Framework

### Why Next.js?

Next.js is a React meta-framework that provides:
- **Server-Side Rendering (SSR)**: Better SEO and performance
- **Static Site Generation (SSG)**: Pre-built pages for speed
- **API Routes**: Backend functionality in the same project
- **File-based Routing**: Automatic routing based on file structure
- **Built-in Optimization**: Images, fonts, and bundle optimization

### Setting Up Next.js

```bash
# Create a new Next.js app
npx create-next-app@latest my-nextjs-app
cd my-nextjs-app
npm run dev

# With TypeScript
npx create-next-app@latest my-nextjs-app --typescript

# Project structure
my-nextjs-app/
├── pages/
│   ├── api/
│   ├── _app.js
│   ├── _document.js
│   └── index.js
├── public/
├── styles/
├── components/
└── package.json
```

### Next.js Routing and Pages

```jsx
// pages/index.js - Home page (/)
import Head from 'next/head';
import Link from 'next/link';

export default function Home() {
    return (
        <div>
            <Head>
                <title>My Next.js App</title>
                <meta name="description" content="Welcome to my Next.js application" />
            </Head>
            
            <h1>Welcome to Next.js!</h1>
            <Link href="/about">
                <a>About Us</a>
            </Link>
        </div>
    );
}

// pages/about.js - About page (/about)
export default function About() {
    return (
        <div>
            <h1>About Us</h1>
            <p>This is the about page.</p>
        </div>
    );
}

// pages/blog/[slug].js - Dynamic route (/blog/my-post)
import { useRouter } from 'next/router';

export default function BlogPost() {
    const router = useRouter();
    const { slug } = router.query;

    return (
        <div>
            <h1>Blog Post: {slug}</h1>
        </div>
    );
}

// pages/products/[...params].js - Catch-all route (/products/category/subcategory/item)
export default function Products() {
    const router = useRouter();
    const { params } = router.query;

    return (
        <div>
            <h1>Products</h1>
            <p>Params: {params?.join(' / ')}</p>
        </div>
    );
}
```

### Data Fetching in Next.js

```jsx
// Static Site Generation (SSG) - Build time
export async function getStaticProps() {
    const res = await fetch('https://api.example.com/posts');
    const posts = await res.json();

    return {
        props: {
            posts,
        },
        revalidate: 60, // Regenerate page every 60 seconds
    };
}

export default function Blog({ posts }) {
    return (
        <div>
            <h1>Blog Posts</h1>
            {posts.map(post => (
                <article key={post.id}>
                    <h2>{post.title}</h2>
                    <p>{post.excerpt}</p>
                </article>
            ))}
        </div>
    );
}

// Dynamic Static Generation
export async function getStaticPaths() {
    const res = await fetch('https://api.example.com/posts');
    const posts = await res.json();

    const paths = posts.map(post => ({
        params: { id: post.id.toString() }
    }));

    return {
        paths,
        fallback: 'blocking' // or false, true
    };
}

export async function getStaticProps({ params }) {
    const res = await fetch(`https://api.example.com/posts/${params.id}`);
    const post = await res.json();

    return {
        props: {
            post,
        },
    };
}

// Server-Side Rendering (SSR) - Request time
export async function getServerSideProps(context) {
    const { req, res, query } = context;
    
    const userAgent = req.headers['user-agent'];
    const res = await fetch(`https://api.example.com/user-data?ua=${userAgent}`);
    const userData = await res.json();

    return {
        props: {
            userData,
        },
    };
}
```

### API Routes

```jsx
// pages/api/hello.js - GET /api/hello
export default function handler(req, res) {
    if (req.method === 'GET') {
        res.status(200).json({ message: 'Hello World!' });
    } else {
        res.setHeader('Allow', ['GET']);
        res.status(405).end(`Method ${req.method} Not Allowed`);
    }
}

// pages/api/users/[id].js - Dynamic API route
export default async function handler(req, res) {
    const { id } = req.query;
    
    switch (req.method) {
        case 'GET':
            try {
                const user = await getUserById(id);
                res.status(200).json(user);
            } catch (error) {
                res.status(404).json({ error: 'User not found' });
            }
            break;
            
        case 'PUT':
            try {
                const updatedUser = await updateUser(id, req.body);
                res.status(200).json(updatedUser);
            } catch (error) {
                res.status(400).json({ error: 'Failed to update user' });
            }
            break;
            
        case 'DELETE':
            try {
                await deleteUser(id);
                res.status(204).end();
            } catch (error) {
                res.status(400).json({ error: 'Failed to delete user' });
            }
            break;
            
        default:
            res.setHeader('Allow', ['GET', 'PUT', 'DELETE']);
            res.status(405).end(`Method ${req.method} Not Allowed`);
    }
}

// pages/api/contact.js - Contact form API
export default async function handler(req, res) {
    if (req.method !== 'POST') {
        return res.status(405).json({ error: 'Method not allowed' });
    }

    const { name, email, message } = req.body;

    // Validation
    if (!name || !email || !message) {
        return res.status(400).json({ error: 'All fields are required' });
    }

    try {
        // Send email (using a service like SendGrid, Nodemailer, etc.)
        await sendEmail({
            to: 'contact@example.com',
            subject: `Contact form submission from ${name}`,
            text: `Name: ${name}\nEmail: ${email}\nMessage: ${message}`,
        });

        res.status(200).json({ message: 'Email sent successfully' });
    } catch (error) {
        console.error('Error sending email:', error);
        res.status(500).json({ error: 'Failed to send email' });
    }
}
```
---

## Modern CSS Frameworks

### Tailwind CSS (Recommended)

Tailwind is a utility-first CSS framework that's become the industry standard:

```bash
# Install Tailwind CSS
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# Configure tailwind.config.js
module.exports = {
  content: [
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```jsx
// Using Tailwind CSS classes
const Card = ({ title, description, image }) => {
    return (
        <div className="max-w-sm mx-auto bg-white rounded-xl shadow-md overflow-hidden md:max-w-2xl">
            <div className="md:flex">
                <div className="md:shrink-0">
                    <img 
                        className="h-48 w-full object-cover md:h-full md:w-48" 
                        src={image} 
                        alt={title}
                    />
                </div>
                <div className="p-8">
                    <div className="uppercase tracking-wide text-sm text-indigo-500 font-semibold">
                        {title}
                    </div>
                    <p className="mt-2 text-slate-500">
                        {description}
                    </p>
                    <button className="mt-4 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors">
                        Learn More
                    </button>
                </div>
            </div>
        </div>
    );
};

// Responsive design with Tailwind
const ResponsiveGrid = ({ items }) => {
    return (
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 p-6">
            {items.map(item => (
                <div key={item.id} className="bg-white rounded-lg shadow-lg p-6 hover:shadow-xl transition-shadow">
                    <h3 className="text-lg font-semibold mb-2">{item.title}</h3>
                    <p className="text-gray-600 text-sm">{item.description}</p>
                </div>
            ))}
        </div>
    );
};

// Custom Tailwind components
const Button = ({ children, variant = 'primary', size = 'md', ...props }) => {
    const baseClasses = 'font-medium rounded focus:outline-none focus:ring-2 focus:ring-offset-2 transition-colors';
    
    const variants = {
        primary: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
        secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300 focus:ring-gray-500',
        danger: 'bg-red-600 text-white hover:bg-red-700 focus:ring-red-500',
    };
    
    const sizes = {
        sm: 'px-3 py-1.5 text-sm',
        md: 'px-4 py-2 text-base',
        lg: 'px-6 py-3 text-lg',
    };
    
    const classes = `${baseClasses} ${variants[variant]} ${sizes[size]}`;
    
    return (
        <button className={classes} {...props}>
            {children}
        </button>
    );
};
```

### Styled Components (Alternative)

CSS-in-JS solution for component-scoped styling:

```jsx
import styled from 'styled-components';

// Basic styled component
const Button = styled.button`
    background: ${props => props.primary ? '#007bff' : '#6c757d'};
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 0.25rem;
    cursor: pointer;
    font-size: 1rem;
    
    &:hover {
        background: ${props => props.primary ? '#0056b3' : '#545b62'};
    }
    
    &:disabled {
        opacity: 0.6;
        cursor: not-allowed;
    }
`;

// Styled component with props
const Card = styled.div`
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    padding: ${props => props.padding || '1rem'};
    margin: ${props => props.margin || '0'};
    
    ${props => props.elevated && `
        box-shadow: 0 4px 8px rgba(0,0,0,0.15);
        transform: translateY(-2px);
    `}
`;

// Using styled components
const ProductCard = ({ product }) => {
    return (
        <Card padding="1.5rem" elevated>
            <h3>{product.name}</h3>
            <p>{product.description}</p>
            <Button primary onClick={() => addToCart(product.id)}>
                Add to Cart
            </Button>
        </Card>
    );
};
```

---

## State Management

### React Context (Built-in)

For simple to medium complexity state management:

```jsx
import React, { createContext, useContext, useReducer } from 'react';

// Create context
const AppContext = createContext();

// Reducer for complex state logic
const appReducer = (state, action) => {
    switch (action.type) {
        case 'SET_USER':
            return { ...state, user: action.payload };
        case 'SET_LOADING':
            return { ...state, loading: action.payload };
        case 'ADD_NOTIFICATION':
            return { 
                ...state, 
                notifications: [...state.notifications, action.payload] 
            };
        case 'REMOVE_NOTIFICATION':
            return {
                ...state,
                notifications: state.notifications.filter(n => n.id !== action.payload)
            };
        default:
            return state;
    }
};

// Context Provider
export const AppProvider = ({ children }) => {
    const [state, dispatch] = useReducer(appReducer, {
        user: null,
        loading: false,
        notifications: []
    });

    const login = async (credentials) => {
        dispatch({ type: 'SET_LOADING', payload: true });
        try {
            const user = await authService.login(credentials);
            dispatch({ type: 'SET_USER', payload: user });
            addNotification('Login successful!', 'success');
        } catch (error) {
            addNotification('Login failed: ' + error.message, 'error');
        } finally {
            dispatch({ type: 'SET_LOADING', payload: false });
        }
    };

    const logout = () => {
        dispatch({ type: 'SET_USER', payload: null });
        addNotification('Logged out successfully', 'info');
    };

    const addNotification = (message, type = 'info') => {
        const notification = {
            id: Date.now(),
            message,
            type,
            timestamp: new Date()
        };
        dispatch({ type: 'ADD_NOTIFICATION', payload: notification });
        
        // Auto-remove after 5 seconds
        setTimeout(() => {
            dispatch({ type: 'REMOVE_NOTIFICATION', payload: notification.id });
        }, 5000);
    };

    const value = {
        ...state,
        login,
        logout,
        addNotification
    };

    return (
        <AppContext.Provider value={value}>
            {children}
        </AppContext.Provider>
    );
};

// Custom hook to use context
export const useApp = () => {
    const context = useContext(AppContext);
    if (!context) {
        throw new Error('useApp must be used within an AppProvider');
    }
    return context;
};

// Using the context
const LoginForm = () => {
    const { login, loading } = useApp();
    const [credentials, setCredentials] = useState({ email: '', password: '' });

    const handleSubmit = (e) => {
        e.preventDefault();
        login(credentials);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                type="email"
                value={credentials.email}
                onChange={(e) => setCredentials(prev => ({ ...prev, email: e.target.value }))}
                placeholder="Email"
                required
            />
            <input
                type="password"
                value={credentials.password}
                onChange={(e) => setCredentials(prev => ({ ...prev, password: e.target.value }))}
                placeholder="Password"
                required
            />
            <button type="submit" disabled={loading}>
                {loading ? 'Logging in...' : 'Login'}
            </button>
        </form>
    );
};
```
### Zustand (Recommended for Complex Apps)

Lightweight state management library:

```bash
npm install zustand
```

```jsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

// Create store
const useStore = create(
    persist(
        (set, get) => ({
            // State
            user: null,
            cart: [],
            theme: 'light',
            
            // Actions
            setUser: (user) => set({ user }),
            
            addToCart: (product) => set((state) => ({
                cart: [...state.cart, { ...product, quantity: 1 }]
            })),
            
            removeFromCart: (productId) => set((state) => ({
                cart: state.cart.filter(item => item.id !== productId)
            })),
            
            updateQuantity: (productId, quantity) => set((state) => ({
                cart: state.cart.map(item => 
                    item.id === productId ? { ...item, quantity } : item
                )
            })),
            
            clearCart: () => set({ cart: [] }),
            
            toggleTheme: () => set((state) => ({
                theme: state.theme === 'light' ? 'dark' : 'light'
            })),
            
            // Computed values
            get cartTotal() {
                return get().cart.reduce((total, item) => total + (item.price * item.quantity), 0);
            },
            
            get cartItemCount() {
                return get().cart.reduce((total, item) => total + item.quantity, 0);
            }
        }),
        {
            name: 'app-storage', // localStorage key
            partialize: (state) => ({ 
                cart: state.cart, 
                theme: state.theme 
            }), // Only persist cart and theme
        }
    )
);

// Using Zustand store
const ShoppingCart = () => {
    const { cart, cartTotal, updateQuantity, removeFromCart, clearCart } = useStore();

    return (
        <div className="shopping-cart">
            <h2>Shopping Cart</h2>
            {cart.length === 0 ? (
                <p>Your cart is empty</p>
            ) : (
                <>
                    {cart.map(item => (
                        <div key={item.id} className="cart-item">
                            <h4>{item.name}</h4>
                            <p>${item.price}</p>
                            <input
                                type="number"
                                value={item.quantity}
                                onChange={(e) => updateQuantity(item.id, parseInt(e.target.value))}
                                min="1"
                            />
                            <button onClick={() => removeFromCart(item.id)}>Remove</button>
                        </div>
                    ))}
                    <div className="cart-total">
                        <strong>Total: ${cartTotal.toFixed(2)}</strong>
                    </div>
                    <button onClick={clearCart}>Clear Cart</button>
                </>
            )}
        </div>
    );
};

const ProductList = () => {
    const addToCart = useStore(state => state.addToCart);
    const [products, setProducts] = useState([]);

    useEffect(() => {
        fetchProducts().then(setProducts);
    }, []);

    return (
        <div className="product-list">
            {products.map(product => (
                <div key={product.id} className="product-card">
                    <h3>{product.name}</h3>
                    <p>${product.price}</p>
                    <button onClick={() => addToCart(product)}>
                        Add to Cart
                    </button>
                </div>
            ))}
        </div>
    );
};
```

---

## Performance Optimization

### Code Splitting and Lazy Loading

```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

// Lazy load components
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Products = lazy(() => import('./pages/Products'));
const Dashboard = lazy(() => import('./pages/Dashboard'));

// Loading component
const LoadingSpinner = () => (
    <div className="flex justify-center items-center h-64">
        <div className="animate-spin rounded-full h-32 w-32 border-b-2 border-blue-500"></div>
    </div>
);

const App = () => {
    return (
        <Router>
            <Suspense fallback={<LoadingSpinner />}>
                <Routes>
                    <Route path="/" element={<Home />} />
                    <Route path="/about" element={<About />} />
                    <Route path="/products" element={<Products />} />
                    <Route path="/dashboard" element={<Dashboard />} />
                </Routes>
            </Suspense>
        </Router>
    );
};

// Dynamic imports with conditions
const ConditionalComponent = ({ userRole }) => {
    const [AdminPanel, setAdminPanel] = useState(null);

    useEffect(() => {
        if (userRole === 'admin') {
            import('./components/AdminPanel').then(module => {
                setAdminPanel(() => module.default);
            });
        }
    }, [userRole]);

    if (userRole === 'admin' && AdminPanel) {
        return <AdminPanel />;
    }

    return <div>Regular user content</div>;
};
```

### React.memo and useMemo

```jsx
import React, { memo, useMemo, useCallback } from 'react';

// Memoized component - only re-renders if props change
const ExpensiveComponent = memo(({ data, onUpdate }) => {
    console.log('ExpensiveComponent rendered');
    
    const processedData = useMemo(() => {
        // Expensive calculation
        return data.map(item => ({
            ...item,
            processed: item.value * 2 + Math.random()
        }));
    }, [data]); // Only recalculate when data changes

    return (
        <div>
            {processedData.map(item => (
                <div key={item.id} onClick={() => onUpdate(item.id)}>
                    {item.name}: {item.processed}
                </div>
            ))}
        </div>
    );
});

const ParentComponent = () => {
    const [data, setData] = useState([]);
    const [count, setCount] = useState(0);

    // Memoized callback - prevents unnecessary re-renders
    const handleUpdate = useCallback((id) => {
        setData(prevData => 
            prevData.map(item => 
                item.id === id ? { ...item, updated: true } : item
            )
        );
    }, []); // Empty dependency array since we use functional update

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
            <ExpensiveComponent data={data} onUpdate={handleUpdate} />
        </div>
    );
};
```

### Image Optimization

```jsx
import Image from 'next/image';

// Next.js Image component (automatic optimization)
const OptimizedImage = () => {
    return (
        <div>
            <Image
                src="/hero-image.jpg"
                alt="Hero image"
                width={800}
                height={400}
                priority // Load immediately for above-the-fold images
                placeholder="blur"
                blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQ..."
            />
        </div>
    );
};

// Lazy loading images with Intersection Observer
const LazyImage = ({ src, alt, className }) => {
    const [isLoaded, setIsLoaded] = useState(false);
    const [isInView, setIsInView] = useState(false);
    const imgRef = useRef();

    useEffect(() => {
        const observer = new IntersectionObserver(
            ([entry]) => {
                if (entry.isIntersecting) {
                    setIsInView(true);
                    observer.disconnect();
                }
            },
            { threshold: 0.1 }
        );

        if (imgRef.current) {
            observer.observe(imgRef.current);
        }

        return () => observer.disconnect();
    }, []);

    return (
        <div ref={imgRef} className={className}>
            {isInView && (
                <img
                    src={src}
                    alt={alt}
                    onLoad={() => setIsLoaded(true)}
                    style={{
                        opacity: isLoaded ? 1 : 0,
                        transition: 'opacity 0.3s ease'
                    }}
                />
            )}
        </div>
    );
};
```

### Bundle Analysis and Optimization

```bash
# Analyze bundle size
npm install --save-dev webpack-bundle-analyzer

# Add to package.json scripts
"analyze": "npm run build && npx webpack-bundle-analyzer build/static/js/*.js"

# Run analysis
npm run analyze
```

```jsx
// Tree shaking - import only what you need
import { debounce } from 'lodash'; // ❌ Imports entire lodash
import debounce from 'lodash/debounce'; // ✅ Imports only debounce

// Dynamic imports for large libraries
const loadChartLibrary = async () => {
    const { Chart } = await import('chart.js');
    return Chart;
};

const ChartComponent = ({ data }) => {
    const [Chart, setChart] = useState(null);

    useEffect(() => {
        loadChartLibrary().then(setChart);
    }, []);

    if (!Chart) {
        return <div>Loading chart...</div>;
    }

    return <Chart data={data} />;
};
```

---

## Testing Frontend Applications

### Jest and React Testing Library

```bash
# Install testing dependencies
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

```jsx
// Button.test.js
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import '@testing-library/jest-dom';
import Button from './Button';

describe('Button Component', () => {
    test('renders button with text', () => {
        render(<Button>Click me</Button>);
        expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument();
    });

    test('calls onClick handler when clicked', async () => {
        const user = userEvent.setup();
        const handleClick = jest.fn();
        
        render(<Button onClick={handleClick}>Click me</Button>);
        
        await user.click(screen.getByRole('button'));
        expect(handleClick).toHaveBeenCalledTimes(1);
    });

    test('is disabled when disabled prop is true', () => {
        render(<Button disabled>Click me</Button>);
        expect(screen.getByRole('button')).toBeDisabled();
    });
});

// Form.test.js
import React from 'react';
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import ContactForm from './ContactForm';

describe('ContactForm', () => {
    test('submits form with valid data', async () => {
        const user = userEvent.setup();
        const mockSubmit = jest.fn();
        
        render(<ContactForm onSubmit={mockSubmit} />);
        
        await user.type(screen.getByLabelText(/name/i), 'John Doe');
        await user.type(screen.getByLabelText(/email/i), 'john@example.com');
        await user.type(screen.getByLabelText(/message/i), 'Hello world');
        
        await user.click(screen.getByRole('button', { name: /submit/i }));
        
        await waitFor(() => {
            expect(mockSubmit).toHaveBeenCalledWith({
                name: 'John Doe',
                email: 'john@example.com',
                message: 'Hello world'
            });
        });
    });

    test('shows validation errors for empty fields', async () => {
        const user = userEvent.setup();
        
        render(<ContactForm />);
        
        await user.click(screen.getByRole('button', { name: /submit/i }));
        
        expect(screen.getByText(/name is required/i)).toBeInTheDocument();
        expect(screen.getByText(/email is required/i)).toBeInTheDocument();
    });
});
```

### Component Integration Testing

```jsx
// App.test.js - Integration test
import React from 'react';
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

// Mock API calls
jest.mock('./services/api', () => ({
    fetchProducts: jest.fn(() => Promise.resolve([
        { id: 1, name: 'Product 1', price: 10 },
        { id: 2, name: 'Product 2', price: 20 }
    ]))
}));

const renderWithRouter = (component) => {
    return render(
        <BrowserRouter>
            {component}
        </BrowserRouter>
    );
};

describe('App Integration', () => {
    test('user can navigate and add products to cart', async () => {
        const user = userEvent.setup();
        
        renderWithRouter(<App />);
        
        // Navigate to products page
        await user.click(screen.getByText(/products/i));
        
        // Wait for products to load
        await waitFor(() => {
            expect(screen.getByText('Product 1')).toBeInTheDocument();
        });
        
        // Add product to cart
        await user.click(screen.getByText(/add to cart/i));
        
        // Check cart count
        expect(screen.getByText(/cart \(1\)/i)).toBeInTheDocument();
    });
});
```

---

## Production Deployment

### Next.js Deployment on Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy to Vercel
vercel

# Or connect GitHub repository for automatic deployments
```

```json
// vercel.json - Configuration
{
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/next"
    }
  ],
  "env": {
    "DATABASE_URL": "@database-url",
    "API_KEY": "@api-key"
  },
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        }
      ]
    }
  ]
}
```

### Performance Monitoring

```jsx
// pages/_app.js - Add performance monitoring
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

function sendToAnalytics(metric) {
    // Send to your analytics service
    console.log(metric);
}

export function reportWebVitals(metric) {
    sendToAnalytics(metric);
}

export default function MyApp({ Component, pageProps }) {
    return <Component {...pageProps} />;
}

// Custom performance hook
const usePerformanceMonitoring = () => {
    useEffect(() => {
        getCLS(sendToAnalytics);
        getFID(sendToAnalytics);
        getFCP(sendToAnalytics);
        getLCP(sendToAnalytics);
        getTTFB(sendToAnalytics);
    }, []);
};
```

---

## Career Path

### 🎯 Junior Frontend Developer (0-2 years)
**Skills Required:**
- HTML, CSS, JavaScript fundamentals
- React basics and hooks
- Basic state management
- Git version control
- Responsive design

**Typical Responsibilities:**
- Implement UI components from designs
- Fix bugs and make small feature additions
- Write basic tests
- Collaborate with senior developers

**Salary Range:** $50k-$80k

### 🚀 Mid-Level Frontend Developer (2-4 years)
**Skills Required:**
- Advanced React patterns
- State management (Context, Zustand)
- Testing (Jest, React Testing Library)
- Performance optimization
- Build tools and bundlers

**Typical Responsibilities:**
- Lead feature development
- Mentor junior developers
- Make architectural decisions
- Code reviews and quality assurance

**Salary Range:** $70k-$120k

### 🏆 Senior Frontend Developer (4+ years)
**Skills Required:**
- System design and architecture
- Advanced performance optimization
- Team leadership
- Cross-functional collaboration
- Emerging technologies

**Typical Responsibilities:**
- Design system architecture
- Lead technical initiatives
- Mentor team members
- Stakeholder communication

**Salary Range:** $100k-$180k+

### 🎨 Frontend Architect/Principal Engineer (7+ years)
**Skills Required:**
- Large-scale system design
- Technology strategy
- Team leadership
- Business acumen
- Innovation and R&D

**Typical Responsibilities:**
- Define technical vision
- Lead multiple teams
- Drive technology adoption
- Strategic planning

**Salary Range:** $150k-$250k+

### Next Steps in Your Learning Journey

1. **Master the Fundamentals** (Weeks 1-4)
   - Complete React official tutorial
   - Build 3-5 small React projects
   - Learn modern CSS (Flexbox, Grid, Tailwind)

2. **Learn Next.js** (Weeks 5-8)
   - Build a full-stack application
   - Implement SSR/SSG
   - Create API routes

3. **Add State Management** (Weeks 9-10)
   - Learn Context API
   - Try Zustand for complex state

4. **Focus on Performance** (Weeks 11-12)
   - Optimize bundle size
   - Implement lazy loading
   - Monitor Core Web Vitals

5. **Testing and Quality** (Weeks 13-14)
   - Write comprehensive tests
   - Set up CI/CD pipeline
   - Code quality tools (ESLint, Prettier)

6. **Deploy and Monitor** (Weeks 15-16)
   - Deploy to production
   - Set up monitoring
   - Performance optimization

Remember: Build projects throughout your learning journey. The best way to learn frontend development is by creating real applications that solve actual problems!