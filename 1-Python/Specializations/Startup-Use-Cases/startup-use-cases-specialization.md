# Python for Startup Use-Cases - MVP Builder Path

Build products fast and validate ideas quickly. This isn't about perfect architecture - it's about getting from idea to working product in the shortest time possible while keeping options open for scaling later.

## Reality Check First

**What Startup Development Actually Is:**
- 70% building MVPs and iterating based on user feedback
- 20% integrating third-party services and APIs
- 10% the "scalable architecture" you'll need later

**What You'll Actually Build:**
- Rapid prototypes and MVPs
- Data collection and analytics systems
- Automation tools for business operations
- Integration scripts for various services
- Simple web applications and APIs
- Customer validation tools

**What You Won't Become:**
- A full-stack expert in every technology
- Someone who builds perfectly scalable systems from day one
- A replacement for focused domain expertise

## Prerequisites

**From Python Fundamentals:**
- Comfortable with APIs, web requests, and data handling
- Understanding of databases, file operations, and basic web concepts
- Experience with popular libraries and package management

**Startup Mindset:**
- **Speed Over Perfection**: Get something working quickly
- **User-Focused**: Build what users actually want, not what you think they want
- **Data-Driven**: Make decisions based on metrics and feedback
- **Resource Conscious**: Time and money are limited - choose tools wisely

## Phase 1: MVP Development and Rapid Prototyping

### Flask-Based MVP Framework

**Why Flask for MVPs**: Minimal setup, maximum flexibility, easy to understand

```python
from flask import Flask, render_template, request, jsonify, redirect, url_for, session
import sqlite3
import hashlib
import uuid
from datetime import datetime
import os
import requests
import json

class MVPFramework:
    def __init__(self, app_name):
        self.app = Flask(app_name)
        self.app.secret_key = os.urandom(24)
        self.db_name = f"{app_name}.db"
        self.init_database()
        self.setup_routes()
    
    def init_database(self):
        """Initialize SQLite database with common tables"""
        conn = sqlite3.connect(self.db_name)
        cursor = conn.cursor()
        
        # Users table
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS users (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                email TEXT UNIQUE NOT NULL,
                password_hash TEXT NOT NULL,
                name TEXT,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                is_active BOOLEAN DEFAULT TRUE
            )
        ''')
        
        # Analytics/Events table
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS events (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                user_id INTEGER,
                event_type TEXT NOT NULL,
                event_data TEXT,
                timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
                ip_address TEXT,
                user_agent TEXT
            )
        ''')
        
        # Feedback table
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS feedback (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                user_id INTEGER,
                feedback_type TEXT,
                message TEXT NOT NULL,
                rating INTEGER,
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        ''')
        
        conn.commit()
        conn.close()
    
    def hash_password(self, password):
        """Simple password hashing"""
        return hashlib.sha256(password.encode()).hexdigest()
    
    def create_user(self, email, password, name=None):
        """Create a new user"""
        try:
            conn = sqlite3.connect(self.db_name)
            cursor = conn.cursor()
            
            password_hash = self.hash_password(password)
            cursor.execute(
                'INSERT INTO users (email, password_hash, name) VALUES (?, ?, ?)',
                (email, password_hash, name)
            )
            
            user_id = cursor.lastrowid
            conn.commit()
            conn.close()
            
            # Track signup event
            self.track_event(user_id, 'user_signup', {'email': email})
            
            return user_id
            
        except sqlite3.IntegrityError:
            return None  # User already exists
    
    def authenticate_user(self, email, password):
        """Authenticate user login"""
        conn = sqlite3.connect(self.db_name)
        cursor = conn.cursor()
        
        password_hash = self.hash_password(password)
        cursor.execute(
            'SELECT id, name FROM users WHERE email = ? AND password_hash = ? AND is_active = TRUE',
            (email, password_hash)
        )
        
        user = cursor.fetchone()
        conn.close()
        
        if user:
            self.track_event(user[0], 'user_login', {'email': email})
            return {'id': user[0], 'name': user[1], 'email': email}
        
        return None
    
    def track_event(self, user_id, event_type, event_data=None, ip_address=None, user_agent=None):
        """Track user events for analytics"""
        conn = sqlite3.connect(self.db_name)
        cursor = conn.cursor()
        
        cursor.execute('''
            INSERT INTO events (user_id, event_type, event_data, ip_address, user_agent)
            VALUES (?, ?, ?, ?, ?)
        ''', (user_id, event_type, json.dumps(event_data) if event_data else None, ip_address, user_agent))
        
        conn.commit()
        conn.close()
    
    def get_analytics_data(self, days=30):
        """Get basic analytics data"""
        conn = sqlite3.connect(self.db_name)
        cursor = conn.cursor()
        
        # User signups over time
        cursor.execute('''
            SELECT DATE(created_at) as date, COUNT(*) as signups
            FROM users
            WHERE created_at >= datetime('now', '-{} days')
            GROUP BY DATE(created_at)
            ORDER BY date
        '''.format(days))
        
        signups = cursor.fetchall()
        
        # Event counts
        cursor.execute('''
            SELECT event_type, COUNT(*) as count
            FROM events
            WHERE timestamp >= datetime('now', '-{} days')
            GROUP BY event_type
            ORDER BY count DESC
        '''.format(days))
        
        events = cursor.fetchall()
        
        # Active users
        cursor.execute('''
            SELECT COUNT(DISTINCT user_id) as active_users
            FROM events
            WHERE timestamp >= datetime('now', '-{} days')
        '''.format(days))
        
        active_users = cursor.fetchone()[0]
        
        conn.close()
        
        return {
            'signups_by_date': signups,
            'event_counts': events,
            'active_users': active_users
        }
    
    def setup_routes(self):
        """Setup basic routes for MVP"""
        
        @self.app.route('/')
        def home():
            return render_template('home.html')
        
        @self.app.route('/signup', methods=['GET', 'POST'])
        def signup():
            if request.method == 'POST':
                email = request.form.get('email')
                password = request.form.get('password')
                name = request.form.get('name')
                
                if email and password:
                    user_id = self.create_user(email, password, name)
                    if user_id:
                        session['user_id'] = user_id
                        session['user_email'] = email
                        return redirect(url_for('dashboard'))
                    else:
                        return render_template('signup.html', error='User already exists')
                
                return render_template('signup.html', error='Please fill all fields')
            
            return render_template('signup.html')
        
        @self.app.route('/login', methods=['GET', 'POST'])
        def login():
            if request.method == 'POST':
                email = request.form.get('email')
                password = request.form.get('password')
                
                user = self.authenticate_user(email, password)
                if user:
                    session['user_id'] = user['id']
                    session['user_email'] = user['email']
                    return redirect(url_for('dashboard'))
                else:
                    return render_template('login.html', error='Invalid credentials')
            
            return render_template('login.html')
        
        @self.app.route('/dashboard')
        def dashboard():
            if 'user_id' not in session:
                return redirect(url_for('login'))
            
            # Track dashboard view
            self.track_event(
                session['user_id'], 
                'dashboard_view',
                ip_address=request.remote_addr,
                user_agent=request.headers.get('User-Agent')
            )
            
            return render_template('dashboard.html', user_email=session['user_email'])
        
        @self.app.route('/feedback', methods=['POST'])
        def submit_feedback():
            if 'user_id' not in session:
                return jsonify({'error': 'Not authenticated'}), 401
            
            feedback_type = request.json.get('type', 'general')
            message = request.json.get('message')
            rating = request.json.get('rating')
            
            if message:
                conn = sqlite3.connect(self.db_name)
                cursor = conn.cursor()
                
                cursor.execute('''
                    INSERT INTO feedback (user_id, feedback_type, message, rating)
                    VALUES (?, ?, ?, ?)
                ''', (session['user_id'], feedback_type, message, rating))
                
                conn.commit()
                conn.close()
                
                # Track feedback event
                self.track_event(session['user_id'], 'feedback_submitted', {
                    'type': feedback_type,
                    'rating': rating
                })
                
                return jsonify({'success': True})
            
            return jsonify({'error': 'Message required'}), 400
        
        @self.app.route('/api/analytics')
        def analytics_api():
            if 'user_id' not in session:
                return jsonify({'error': 'Not authenticated'}), 401
            
            # Simple admin check (in real app, use proper role system)
            if session['user_email'] != 'admin@example.com':
                return jsonify({'error': 'Not authorized'}), 403
            
            analytics = self.get_analytics_data()
            return jsonify(analytics)
        
        @self.app.route('/logout')
        def logout():
            if 'user_id' in session:
                self.track_event(session['user_id'], 'user_logout')
            
            session.clear()
            return redirect(url_for('home'))

# Third-party integrations for common startup needs
class StartupIntegrations:
    def __init__(self):
        self.integrations = {}
    
    def setup_stripe_payments(self, stripe_secret_key):
        """Setup Stripe for payments"""
        try:
            import stripe
            stripe.api_key = stripe_secret_key
            self.integrations['stripe'] = stripe
            return True
        except ImportError:
            print("Stripe library not installed. Run: pip install stripe")
            return False
    
    def setup_sendgrid_email(self, sendgrid_api_key):
        """Setup SendGrid for email"""
        try:
            import sendgrid
            from sendgrid.helpers.mail import Mail
            
            self.integrations['sendgrid'] = {
                'client': sendgrid.SendGridAPIClient(api_key=sendgrid_api_key),
                'Mail': Mail
            }
            return True
        except ImportError:
            print("SendGrid library not installed. Run: pip install sendgrid")
            return False
    
    def send_email(self, to_email, subject, content, from_email='noreply@yourstartup.com'):
        """Send email via SendGrid"""
        if 'sendgrid' not in self.integrations:
            print("SendGrid not configured")
            return False
        
        try:
            message = self.integrations['sendgrid']['Mail'](
                from_email=from_email,
                to_emails=to_email,
                subject=subject,
                html_content=content
            )
            
            response = self.integrations['sendgrid']['client'].send(message)
            return response.status_code == 202
            
        except Exception as e:
            print(f"Error sending email: {e}")
            return False
    
    def create_stripe_customer(self, email, name=None):
        """Create Stripe customer"""
        if 'stripe' not in self.integrations:
            return None
        
        try:
            customer = self.integrations['stripe'].Customer.create(
                email=email,
                name=name
            )
            return customer.id
        except Exception as e:
            print(f"Error creating Stripe customer: {e}")
            return None
    
    def create_payment_intent(self, amount, currency='usd', customer_id=None):
        """Create Stripe payment intent"""
        if 'stripe' not in self.integrations:
            return None
        
        try:
            intent = self.integrations['stripe'].PaymentIntent.create(
                amount=amount,  # Amount in cents
                currency=currency,
                customer=customer_id
            )
            return intent.client_secret
        except Exception as e:
            print(f"Error creating payment intent: {e}")
            return None
    
    def setup_google_analytics(self, tracking_id):
        """Setup Google Analytics tracking"""
        self.integrations['ga_tracking_id'] = tracking_id
        
        # Return JavaScript code to include in templates
        return f"""
        <!-- Global site tag (gtag.js) - Google Analytics -->
        <script async src="https://www.googletagmanager.com/gtag/js?id={tracking_id}"></script>
        <script>
          window.dataLayer = window.dataLayer || [];
          function gtag(){{dataLayer.push(arguments);}}
          gtag('js', new Date());
          gtag('config', '{tracking_id}');
        </script>
        """
    
    def track_conversion(self, event_name, value=None):
        """Track conversion event (for Google Analytics)"""
        # This would typically be handled client-side
        # Here we just log it for server-side tracking
        conversion_data = {
            'event': event_name,
            'value': value,
            'timestamp': datetime.now().isoformat()
        }
        
        # In a real app, you might send this to your analytics service
        print(f"Conversion tracked: {conversion_data}")
        return conversion_data

# Startup-specific utilities
class StartupUtils:
    @staticmethod
    def validate_email(email):
        """Simple email validation"""
        import re
        pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
        return re.match(pattern, email) is not None
    
    @staticmethod
    def generate_referral_code(user_id):
        """Generate unique referral code"""
        import hashlib
        import time
        
        data = f"{user_id}-{time.time()}"
        return hashlib.md5(data.encode()).hexdigest()[:8].upper()
    
    @staticmethod
    def calculate_mrr(revenue_data):
        """Calculate Monthly Recurring Revenue"""
        # Assuming revenue_data is a list of monthly revenue amounts
        if not revenue_data:
            return 0
        
        # Simple MRR calculation (average of last 3 months)
        recent_months = revenue_data[-3:] if len(revenue_data) >= 3 else revenue_data
        return sum(recent_months) / len(recent_months)
    
    @staticmethod
    def calculate_churn_rate(total_customers_start, customers_lost, period_days=30):
        """Calculate customer churn rate"""
        if total_customers_start == 0:
            return 0
        
        return (customers_lost / total_customers_start) * 100
    
    @staticmethod
    def a_b_test_assignment(user_id, test_name, variants=['A', 'B']):
        """Simple A/B test assignment"""
        import hashlib
        
        # Create deterministic assignment based on user_id and test_name
        hash_input = f"{user_id}-{test_name}"
        hash_value = int(hashlib.md5(hash_input.encode()).hexdigest(), 16)
        
        variant_index = hash_value % len(variants)
        return variants[variant_index]
    
    @staticmethod
    def create_landing_page_data():
        """Generate data structure for landing page A/B testing"""
        return {
            'headlines': [
                "Transform Your Business Today",
                "The Solution You've Been Looking For",
                "Grow Faster Than Ever Before"
            ],
            'cta_buttons': [
                "Get Started Free",
                "Try It Now",
                "Start Your Journey"
            ],
            'value_props': [
                "Save 10 hours per week",
                "Increase productivity by 50%",
                "Join 10,000+ happy customers"
            ]
        }

# Example startup application
def create_startup_app():
    """Create a complete startup MVP application"""
    
    # Initialize MVP framework
    mvp = MVPFramework('my_startup')
    
    # Setup integrations
    integrations = StartupIntegrations()
    
    # In production, use environment variables
    # integrations.setup_stripe_payments(os.getenv('STRIPE_SECRET_KEY'))
    # integrations.setup_sendgrid_email(os.getenv('SENDGRID_API_KEY'))
    
    # Add custom routes for your specific startup
    @mvp.app.route('/api/feature', methods=['POST'])
    def custom_feature():
        if 'user_id' not in session:
            return jsonify({'error': 'Not authenticated'}), 401
        
        # Your custom feature logic here
        feature_data = request.json
        
        # Track feature usage
        mvp.track_event(
            session['user_id'],
            'feature_used',
            feature_data,
            request.remote_addr,
            request.headers.get('User-Agent')
        )
        
        return jsonify({'success': True, 'message': 'Feature executed'})
    
    @mvp.app.route('/pricing')
    def pricing():
        # A/B test pricing page
        user_id = session.get('user_id', 0)
        variant = StartupUtils.a_b_test_assignment(user_id, 'pricing_page', ['monthly', 'annual'])
        
        # Track which variant user saw
        if 'user_id' in session:
            mvp.track_event(session['user_id'], 'pricing_page_view', {'variant': variant})
        
        return render_template('pricing.html', pricing_variant=variant)
    
    # Create basic HTML templates
    create_basic_templates()
    
    return mvp.app

def create_basic_templates():
    """Create basic HTML templates for the MVP"""
    import os
    
    # Create templates directory
    os.makedirs('templates', exist_ok=True)
    
    # Base template
    base_template = '''
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}My Startup{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar-brand" href="{{ url_for('home') }}">My Startup</a>
            <div class="navbar-nav ms-auto">
                {% if session.user_id %}
                    <a class="nav-link" href="{{ url_for('dashboard') }}">Dashboard</a>
                    <a class="nav-link" href="{{ url_for('logout') }}">Logout</a>
                {% else %}
                    <a class="nav-link" href="{{ url_for('login') }}">Login</a>
                    <a class="nav-link" href="{{ url_for('signup') }}">Sign Up</a>
                {% endif %}
            </div>
        </div>
    </nav>
    
    <div class="container mt-4">
        {% block content %}{% endblock %}
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"></script>
    {% block scripts %}{% endblock %}
</body>
</html>
    '''
    
    # Home page template
    home_template = '''
{% extends "base.html" %}

{% block content %}
<div class="row">
    <div class="col-lg-8 mx-auto text-center">
        <h1 class="display-4">Welcome to My Startup</h1>
        <p class="lead">The solution you've been looking for.</p>
        <a href="{{ url_for('signup') }}" class="btn btn-primary btn-lg">Get Started Free</a>
    </div>
</div>
{% endblock %}
    '''
    
    # Signup template
    signup_template = '''
{% extends "base.html" %}

{% block content %}
<div class="row">
    <div class="col-md-6 mx-auto">
        <h2>Sign Up</h2>
        {% if error %}
            <div class="alert alert-danger">{{ error }}</div>
        {% endif %}
        <form method="POST">
            <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input type="text" class="form-control" id="name" name="name">
            </div>
            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" class="form-control" id="email" name="email" required>
            </div>
            <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" class="form-control" id="password" name="password" required>
            </div>
            <button type="submit" class="btn btn-primary">Sign Up</button>
        </form>
    </div>
</div>
{% endblock %}
    '''
    
    # Login template
    login_template = '''
{% extends "base.html" %}

{% block content %}
<div class="row">
    <div class="col-md-6 mx-auto">
        <h2>Login</h2>
        {% if error %}
            <div class="alert alert-danger">{{ error }}</div>
        {% endif %}
        <form method="POST">
            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input type="email" class="form-control" id="email" name="email" required>
            </div>
            <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" class="form-control" id="password" name="password" required>
            </div>
            <button type="submit" class="btn btn-primary">Login</button>
        </form>
    </div>
</div>
{% endblock %}
    '''
    
    # Dashboard template
    dashboard_template = '''
{% extends "base.html" %}

{% block content %}
<div class="row">
    <div class="col-12">
        <h2>Dashboard</h2>
        <p>Welcome back, {{ user_email }}!</p>
        
        <div class="row">
            <div class="col-md-4">
                <div class="card">
                    <div class="card-body">
                        <h5 class="card-title">Feature 1</h5>
                        <p class="card-text">Your main feature goes here.</p>
                        <button class="btn btn-primary" onclick="useFeature()">Use Feature</button>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="mt-4">
            <h4>Feedback</h4>
            <form id="feedbackForm">
                <div class="mb-3">
                    <label for="message" class="form-label">Your feedback</label>
                    <textarea class="form-control" id="message" rows="3"></textarea>
                </div>
                <div class="mb-3">
                    <label for="rating" class="form-label">Rating (1-5)</label>
                    <select class="form-control" id="rating">
                        <option value="5">5 - Excellent</option>
                        <option value="4">4 - Good</option>
                        <option value="3">3 - Average</option>
                        <option value="2">2 - Poor</option>
                        <option value="1">1 - Terrible</option>
                    </select>
                </div>
                <button type="submit" class="btn btn-success">Submit Feedback</button>
            </form>
        </div>
    </div>
</div>
{% endblock %}

{% block scripts %}
<script>
function useFeature() {
    fetch('/api/feature', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({feature: 'main_feature'})
    })
    .then(response => response.json())
    .then(data => {
        alert('Feature used successfully!');
    });
}

document.getElementById('feedbackForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    const message = document.getElementById('message').value;
    const rating = document.getElementById('rating').value;
    
    fetch('/feedback', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            message: message,
            rating: parseInt(rating),
            type: 'general'
        })
    })
    .then(response => response.json())
    .then(data => {
        alert('Thank you for your feedback!');
        document.getElementById('feedbackForm').reset();
    });
});
</script>
{% endblock %}
    '''
    
    # Write templates to files
    templates = {
        'base.html': base_template,
        'home.html': home_template,
        'signup.html': signup_template,
        'login.html': login_template,
        'dashboard.html': dashboard_template
    }
    
    for filename, content in templates.items():
        with open(f'templates/{filename}', 'w') as f:
            f.write(content)
    
    print("Basic templates created in 'templates' directory")

# Usage example
if __name__ == "__main__":
    # Create and run the startup app
    app = create_startup_app()
    
    print("Startup MVP created!")
    print("Visit http://localhost:5000 to see your app")
    print("Features included:")
    print("- User registration and authentication")
    print("- Basic analytics tracking")
    print("- Feedback collection")
    print("- A/B testing framework")
    print("- Integration setup for Stripe and SendGrid")
    
    # Run the app
    app.run(debug=True, port=5000)
```

This covers the essential Startup Use-Cases specialization, focusing on rapid MVP development and practical business tools that startups actually need. The framework provides a solid foundation for building and iterating on product ideas quickly while maintaining the flexibility to scale later.

<function_calls>
<invoke name="taskStatus">
<parameter name="status">completed