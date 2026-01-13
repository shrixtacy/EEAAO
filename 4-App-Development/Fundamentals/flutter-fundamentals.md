# Flutter Development Fundamentals

Your comprehensive guide to building cross-platform mobile applications with Flutter and Dart. This guide covers everything from Dart language basics to advanced Flutter concepts with practical examples and real-world projects.

## 🎯 **WHAT YOU'LL LEARN**

- **Dart language** fundamentals and Flutter-specific patterns
- **Widget-based UI** development and composition
- **State management** approaches (setState, Provider, Bloc, Riverpod)
- **Navigation and routing** for multi-screen apps
- **Platform integration** and native features
- **Performance optimization** and best practices

## 📋 **PREREQUISITES**

- Basic programming knowledge (any language)
- Understanding of object-oriented programming concepts
- No prior mobile development experience needed
- Familiarity with JSON and REST APIs helpful but not required

---

## 🎯 **DART LANGUAGE FUNDAMENTALS**

### **Dart Basics for Flutter Development**

**Variables and Data Types**:
```dart
void main() {
  // Variable declarations
  String appName = 'My Flutter App';
  int userCount = 1000;
  double rating = 4.5;
  bool isPublished = true;
  
  // Type inference
  var userName = 'John Doe'; // String inferred
  var age = 25; // int inferred
  
  // Nullable types (important for Flutter)
  String? optionalText; // Can be null
  String nonNullableText = 'Always has a value';
  
  // Late initialization
  late String configValue; // Will be initialized later
  
  // Constants
  const String apiUrl = 'https://api.example.com';
  final DateTime currentTime = DateTime.now();
  
  print('App: $appName, Users: $userCount, Rating: $rating');
}
```

**Collections and Iteration**:
```dart
void main() {
  // Lists (Arrays)
  List<String> fruits = ['apple', 'banana', 'orange'];
  var numbers = [1, 2, 3, 4, 5]; // Type inferred as List<int>
  
  // Adding elements
  fruits.add('grape');
  fruits.addAll(['mango', 'kiwi']);
  
  // Maps (Key-Value pairs)
  Map<String, int> scores = {
    'Alice': 95,
    'Bob': 87,
    'Charlie': 92,
  };
  
  // Sets (Unique elements)
  Set<String> uniqueColors = {'red', 'green', 'blue'};
  
  // Iteration
  for (String fruit in fruits) {
    print('Fruit: $fruit');
  }
  
  // Functional programming style
  var evenNumbers = numbers.where((n) => n % 2 == 0).toList();
  var doubledNumbers = numbers.map((n) => n * 2).toList();
  
  print('Even numbers: $evenNumbers');
  print('Doubled: $doubledNumbers');
}
```

**Functions and Arrow Functions**:
```dart
// Regular function
String greetUser(String name, {String greeting = 'Hello'}) {
  return '$greeting, $name!';
}

// Arrow function
String greetUserShort(String name) => 'Hello, $name!';

// Function with optional parameters
void logMessage(String message, {bool isError = false, String? category}) {
  String prefix = isError ? 'ERROR' : 'INFO';
  String categoryText = category != null ? '[$category]' : '';
  print('$prefix $categoryText: $message');
}

// Higher-order functions
void processItems<T>(List<T> items, void Function(T) processor) {
  for (T item in items) {
    processor(item);
  }
}

void main() {
  print(greetUser('Alice'));
  print(greetUser('Bob', greeting: 'Hi'));
  
  logMessage('App started');
  logMessage('Connection failed', isError: true, category: 'Network');
  
  processItems(['a', 'b', 'c'], (item) => print('Processing: $item'));
}
```

### **Object-Oriented Programming in Dart**

**Classes and Constructors**:
```dart
class User {
  // Properties
  final String id;
  String name;
  String email;
  DateTime? lastLoginAt;
  
  // Constructor
  User({
    required this.id,
    required this.name,
    required this.email,
    this.lastLoginAt,
  });
  
  // Named constructor
  User.guest() : 
    id = 'guest',
    name = 'Guest User',
    email = 'guest@example.com';
  
  // Factory constructor
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
      lastLoginAt: json['lastLoginAt'] != null 
        ? DateTime.parse(json['lastLoginAt'])
        : null,
    );
  }
  
  // Methods
  void updateLastLogin() {
    lastLoginAt = DateTime.now();
  }
  
  bool get isGuest => id == 'guest';
  
  String get displayName => name.isEmpty ? email : name;
  
  // Convert to JSON
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'name': name,
      'email': email,
      'lastLoginAt': lastLoginAt?.toIso8601String(),
    };
  }
  
  @override
  String toString() {
    return 'User(id: $id, name: $name, email: $email)';
  }
}

// Usage
void main() {
  var user1 = User(
    id: '123',
    name: 'Alice Johnson',
    email: 'alice@example.com',
  );
  
  var guestUser = User.guest();
  
  var userFromApi = User.fromJson({
    'id': '456',
    'name': 'Bob Smith',
    'email': 'bob@example.com',
  });
  
  user1.updateLastLogin();
  print(user1);
  print('Is guest: ${user1.isGuest}');
}
```

**Inheritance and Mixins**:
```dart
// Base class
abstract class Animal {
  String name;
  
  Animal(this.name);
  
  void makeSound(); // Abstract method
  
  void sleep() {
    print('$name is sleeping');
  }
}

// Mixin for flying behavior
mixin Flying {
  void fly() {
    print('Flying high!');
  }
}

// Mixin for swimming behavior
mixin Swimming {
  void swim() {
    print('Swimming gracefully!');
  }
}

// Concrete classes
class Dog extends Animal {
  Dog(String name) : super(name);
  
  @override
  void makeSound() {
    print('$name says Woof!');
  }
}

class Duck extends Animal with Flying, Swimming {
  Duck(String name) : super(name);
  
  @override
  void makeSound() {
    print('$name says Quack!');
  }
}

void main() {
  var dog = Dog('Buddy');
  var duck = Duck('Donald');
  
  dog.makeSound();
  dog.sleep();
  
  duck.makeSound();
  duck.fly();
  duck.swim();
}
```

### **Asynchronous Programming**

**Futures and Async/Await**:
```dart
import 'dart:convert';
import 'dart:io';

// Simulated API call
Future<String> fetchUserData(String userId) async {
  // Simulate network delay
  await Future.delayed(Duration(seconds: 2));
  
  // Simulate API response
  return jsonEncode({
    'id': userId,
    'name': 'John Doe',
    'email': 'john@example.com',
  });
}

// Error handling with try-catch
Future<User?> loadUser(String userId) async {
  try {
    print('Loading user $userId...');
    String jsonData = await fetchUserData(userId);
    Map<String, dynamic> userData = jsonDecode(jsonData);
    return User.fromJson(userData);
  } catch (e) {
    print('Error loading user: $e');
    return null;
  }
}

// Multiple async operations
Future<List<User>> loadMultipleUsers(List<String> userIds) async {
  List<Future<User?>> futures = userIds.map((id) => loadUser(id)).toList();
  List<User?> results = await Future.wait(futures);
  
  // Filter out null results
  return results.whereType<User>().toList();
}

void main() async {
  // Single user
  User? user = await loadUser('123');
  if (user != null) {
    print('Loaded user: ${user.name}');
  }
  
  // Multiple users
  List<User> users = await loadMultipleUsers(['123', '456', '789']);
  print('Loaded ${users.length} users');
}
```

**Streams for Real-time Data**:
```dart
import 'dart:async';

// Stream controller for real-time updates
class ChatService {
  final StreamController<String> _messageController = StreamController<String>();
  
  Stream<String> get messageStream => _messageController.stream;
  
  void sendMessage(String message) {
    _messageController.add(message);
  }
  
  void dispose() {
    _messageController.close();
  }
}

// Stream transformation
Stream<int> countdownStream(int start) async* {
  for (int i = start; i >= 0; i--) {
    await Future.delayed(Duration(seconds: 1));
    yield i;
  }
}

void main() async {
  var chatService = ChatService();
  
  // Listen to stream
  chatService.messageStream.listen((message) {
    print('Received: $message');
  });
  
  // Send messages
  chatService.sendMessage('Hello!');
  chatService.sendMessage('How are you?');
  
  // Countdown stream
  print('Starting countdown...');
  await for (int count in countdownStream(5)) {
    print('Count: $count');
  }
  print('Done!');
  
  chatService.dispose();
}
```

---

## 🎨 **FLUTTER WIDGET FUNDAMENTALS**

### **Understanding Widgets**

**Everything is a Widget Philosophy**:
```dart
import 'package:flutter/material.dart';

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Flutter Fundamentals'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Welcome to Flutter!',
              style: TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                print('Button pressed!');
              },
              child: Text('Click Me'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### **Basic Widgets**

**Text and Styling**:
```dart
class TextExamplesWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        // Basic text
        Text('Simple text'),
        
        // Styled text
        Text(
          'Styled text',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Colors.blue,
            decoration: TextDecoration.underline,
          ),
        ),
        
        // Rich text with multiple styles
        RichText(
          text: TextSpan(
            style: DefaultTextStyle.of(context).style,
            children: [
              TextSpan(text: 'Hello '),
              TextSpan(
                text: 'Flutter',
                style: TextStyle(
                  fontWeight: FontWeight.bold,
                  color: Colors.blue,
                ),
              ),
              TextSpan(text: ' World!'),
            ],
          ),
        ),
        
        // Text with overflow handling
        Container(
          width: 100,
          child: Text(
            'This is a very long text that will overflow',
            overflow: TextOverflow.ellipsis,
            maxLines: 2,
          ),
        ),
      ],
    );
  }
}
```

**Buttons and Interactions**:
```dart
class ButtonExamplesWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Elevated button (primary)
        ElevatedButton(
          onPressed: () => _handleButtonPress('Elevated'),
          child: Text('Elevated Button'),
        ),
        
        // Outlined button
        OutlinedButton(
          onPressed: () => _handleButtonPress('Outlined'),
          child: Text('Outlined Button'),
        ),
        
        // Text button
        TextButton(
          onPressed: () => _handleButtonPress('Text'),
          child: Text('Text Button'),
        ),
        
        // Icon button
        IconButton(
          onPressed: () => _handleButtonPress('Icon'),
          icon: Icon(Icons.favorite),
        ),
        
        // Floating action button
        FloatingActionButton(
          onPressed: () => _handleButtonPress('FAB'),
          child: Icon(Icons.add),
        ),
        
        // Custom styled button
        ElevatedButton(
          onPressed: () => _handleButtonPress('Custom'),
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.green,
            foregroundColor: Colors.white,
            padding: EdgeInsets.symmetric(horizontal: 30, vertical: 15),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
          ),
          child: Text('Custom Button'),
        ),
      ],
    );
  }
  
  void _handleButtonPress(String buttonType) {
    print('$buttonType button pressed');
  }
}
```

**Images and Assets**:
```dart
class ImageExamplesWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Network image
        Image.network(
          'https://picsum.photos/200/200',
          width: 200,
          height: 200,
          fit: BoxFit.cover,
          loadingBuilder: (context, child, loadingProgress) {
            if (loadingProgress == null) return child;
            return CircularProgressIndicator();
          },
          errorBuilder: (context, error, stackTrace) {
            return Icon(Icons.error);
          },
        ),
        
        SizedBox(height: 20),
        
        // Asset image (add to pubspec.yaml first)
        Image.asset(
          'assets/images/flutter_logo.png',
          width: 100,
          height: 100,
        ),
        
        SizedBox(height: 20),
        
        // Circular avatar with image
        CircleAvatar(
          radius: 50,
          backgroundImage: NetworkImage('https://picsum.photos/100/100'),
        ),
        
        SizedBox(height: 20),
        
        // Container with background image
        Container(
          width: 200,
          height: 100,
          decoration: BoxDecoration(
            image: DecorationImage(
              image: NetworkImage('https://picsum.photos/200/100'),
              fit: BoxFit.cover,
            ),
            borderRadius: BorderRadius.circular(10),
          ),
        ),
      ],
    );
  }
}
```

### **Layout Widgets**

**Container and Padding**:
```dart
class LayoutExamplesWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Container with decoration
        Container(
          width: 200,
          height: 100,
          padding: EdgeInsets.all(16),
          margin: EdgeInsets.all(8),
          decoration: BoxDecoration(
            color: Colors.blue,
            borderRadius: BorderRadius.circular(10),
            boxShadow: [
              BoxShadow(
                color: Colors.grey.withOpacity(0.5),
                spreadRadius: 2,
                blurRadius: 5,
                offset: Offset(0, 3),
              ),
            ],
          ),
          child: Center(
            child: Text(
              'Styled Container',
              style: TextStyle(color: Colors.white),
            ),
          ),
        ),
        
        // Padding widget
        Padding(
          padding: EdgeInsets.symmetric(horizontal: 20, vertical: 10),
          child: Text('Padded text'),
        ),
        
        // Sized box for spacing
        SizedBox(height: 20),
        
        // Flexible spacing
        Spacer(),
      ],
    );
  }
}
```

**Row and Column Layouts**:
```dart
class RowColumnExamplesWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Row with different alignments
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            Icon(Icons.star, color: Colors.yellow),
            Icon(Icons.star, color: Colors.yellow),
            Icon(Icons.star, color: Colors.yellow),
            Icon(Icons.star_border),
            Icon(Icons.star_border),
          ],
        ),
        
        SizedBox(height: 20),
        
        // Column with cross axis alignment
        Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Title', style: TextStyle(fontWeight: FontWeight.bold)),
            Text('Subtitle'),
            Text('Description text that is longer'),
          ],
        ),
        
        SizedBox(height: 20),
        
        // Nested row and column
        Container(
          padding: EdgeInsets.all(16),
          decoration: BoxDecoration(
            border: Border.all(color: Colors.grey),
            borderRadius: BorderRadius.circular(8),
          ),
          child: Column(
            children: [
              Row(
                children: [
                  Icon(Icons.person),
                  SizedBox(width: 8),
                  Expanded(
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text('John Doe', style: TextStyle(fontWeight: FontWeight.bold)),
                        Text('Software Developer'),
                      ],
                    ),
                  ),
                  IconButton(
                    onPressed: () {},
                    icon: Icon(Icons.more_vert),
                  ),
                ],
              ),
            ],
          ),
        ),
      ],
    );
  }
}
```

**Stack and Positioned**:
```dart
class StackExamplesWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // Basic stack
        Container(
          width: 200,
          height: 200,
          child: Stack(
            children: [
              // Background
              Container(
                width: 200,
                height: 200,
                color: Colors.blue,
              ),
              
              // Positioned elements
              Positioned(
                top: 10,
                right: 10,
                child: Icon(Icons.close, color: Colors.white),
              ),
              
              Positioned(
                bottom: 10,
                left: 10,
                child: Text(
                  'Bottom Left',
                  style: TextStyle(color: Colors.white),
                ),
              ),
              
              // Centered element
              Center(
                child: Text(
                  'Centered',
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 20,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
            ],
          ),
        ),
        
        SizedBox(height: 20),
        
        // Profile card with stack
        Container(
          width: 300,
          height: 200,
          child: Stack(
            children: [
              // Background image
              Container(
                width: 300,
                height: 120,
                decoration: BoxDecoration(
                  gradient: LinearGradient(
                    colors: [Colors.blue, Colors.purple],
                  ),
                ),
              ),
              
              // Profile picture
              Positioned(
                top: 60,
                left: 20,
                child: CircleAvatar(
                  radius: 40,
                  backgroundColor: Colors.white,
                  child: CircleAvatar(
                    radius: 35,
                    backgroundImage: NetworkImage('https://picsum.photos/70/70'),
                  ),
                ),
              ),
              
              // User info
              Positioned(
                top: 130,
                left: 120,
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'John Doe',
                      style: TextStyle(
                        fontSize: 18,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    Text('Flutter Developer'),
                  ],
                ),
              ),
            ],
          ),
        ),
      ],
    );
  }
}
```

---

## 🔄 **STATE MANAGEMENT**

### **StatefulWidget and setState**

**Basic State Management**:
```dart
class CounterWidget extends StatefulWidget {
  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  int _counter = 0;
  
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
  
  void _decrementCounter() {
    setState(() {
      _counter--;
    });
  }
  
  void _resetCounter() {
    setState(() {
      _counter = 0;
    });
  }
  
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Text(
          'Counter Value:',
          style: TextStyle(fontSize: 18),
        ),
        Text(
          '$_counter',
          style: TextStyle(
            fontSize: 48,
            fontWeight: FontWeight.bold,
            color: _counter > 0 ? Colors.green : 
                   _counter < 0 ? Colors.red : Colors.black,
          ),
        ),
        SizedBox(height: 20),
        Row(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            ElevatedButton(
              onPressed: _decrementCounter,
              child: Icon(Icons.remove),
            ),
            ElevatedButton(
              onPressed: _resetCounter,
              child: Text('Reset'),
            ),
            ElevatedButton(
              onPressed: _incrementCounter,
              child: Icon(Icons.add),
            ),
          ],
        ),
      ],
    );
  }
}
```

**Form State Management**:
```dart
class UserFormWidget extends StatefulWidget {
  @override
  _UserFormWidgetState createState() => _UserFormWidgetState();
}

class _UserFormWidgetState extends State<UserFormWidget> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  
  bool _isLoading = false;
  String? _errorMessage;
  
  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    super.dispose();
  }
  
  Future<void> _submitForm() async {
    if (_formKey.currentState!.validate()) {
      setState(() {
        _isLoading = true;
        _errorMessage = null;
      });
      
      try {
        // Simulate API call
        await Future.delayed(Duration(seconds: 2));
        
        // Success
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('User created successfully!')),
        );
        
        // Clear form
        _nameController.clear();
        _emailController.clear();
        
      } catch (e) {
        setState(() {
          _errorMessage = 'Failed to create user: $e';
        });
      } finally {
        setState(() {
          _isLoading = false;
        });
      }
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: EdgeInsets.all(16),
      child: Form(
        key: _formKey,
        child: Column(
          children: [
            TextFormField(
              controller: _nameController,
              decoration: InputDecoration(
                labelText: 'Name',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Please enter a name';
                }
                return null;
              },
            ),
            
            SizedBox(height: 16),
            
            TextFormField(
              controller: _emailController,
              decoration: InputDecoration(
                labelText: 'Email',
                border: OutlineInputBorder(),
              ),
              validator: (value) {
                if (value == null || value.isEmpty) {
                  return 'Please enter an email';
                }
                if (!value.contains('@')) {
                  return 'Please enter a valid email';
                }
                return null;
              },
            ),
            
            SizedBox(height: 20),
            
            if (_errorMessage != null)
              Container(
                padding: EdgeInsets.all(8),
                decoration: BoxDecoration(
                  color: Colors.red.withOpacity(0.1),
                  borderRadius: BorderRadius.circular(4),
                ),
                child: Text(
                  _errorMessage!,
                  style: TextStyle(color: Colors.red),
                ),
              ),
            
            SizedBox(height: 20),
            
            SizedBox(
              width: double.infinity,
              child: ElevatedButton(
                onPressed: _isLoading ? null : _submitForm,
                child: _isLoading
                  ? CircularProgressIndicator()
                  : Text('Submit'),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

### **Provider State Management**

**Setting up Provider (pubspec.yaml)**:
```yaml
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.0.5
```

**Counter Model with ChangeNotifier**:
```dart
import 'package:flutter/foundation.dart';

class CounterModel extends ChangeNotifier {
  int _count = 0;
  
  int get count => _count;
  
  void increment() {
    _count++;
    notifyListeners();
  }
  
  void decrement() {
    _count--;
    notifyListeners();
  }
  
  void reset() {
    _count = 0;
    notifyListeners();
  }
}
```

**Using Provider in App**:
```dart
import 'package:provider/provider.dart';

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => CounterModel(),
      child: MaterialApp(
        title: 'Provider Demo',
        home: CounterScreen(),
      ),
    );
  }
}

class CounterScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Provider Counter')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Counter Value:'),
            Consumer<CounterModel>(
              builder: (context, counter, child) {
                return Text(
                  '${counter.count}',
                  style: TextStyle(fontSize: 48),
                );
              },
            ),
            SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                ElevatedButton(
                  onPressed: () {
                    context.read<CounterModel>().decrement();
                  },
                  child: Icon(Icons.remove),
                ),
                ElevatedButton(
                  onPressed: () {
                    context.read<CounterModel>().reset();
                  },
                  child: Text('Reset'),
                ),
                ElevatedButton(
                  onPressed: () {
                    context.read<CounterModel>().increment();
                  },
                  child: Icon(Icons.add),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

**Shopping Cart Example with Provider**:
```dart
class Product {
  final String id;
  final String name;
  final double price;
  
  Product({required this.id, required this.name, required this.price});
}

class CartItem {
  final Product product;
  int quantity;
  
  CartItem({required this.product, this.quantity = 1});
  
  double get totalPrice => product.price * quantity;
}

class CartModel extends ChangeNotifier {
  final List<CartItem> _items = [];
  
  List<CartItem> get items => List.unmodifiable(_items);
  
  int get itemCount => _items.fold(0, (sum, item) => sum + item.quantity);
  
  double get totalPrice => _items.fold(0, (sum, item) => sum + item.totalPrice);
  
  void addProduct(Product product) {
    final existingIndex = _items.indexWhere((item) => item.product.id == product.id);
    
    if (existingIndex >= 0) {
      _items[existingIndex].quantity++;
    } else {
      _items.add(CartItem(product: product));
    }
    
    notifyListeners();
  }
  
  void removeProduct(String productId) {
    _items.removeWhere((item) => item.product.id == productId);
    notifyListeners();
  }
  
  void updateQuantity(String productId, int quantity) {
    final index = _items.indexWhere((item) => item.product.id == productId);
    if (index >= 0) {
      if (quantity <= 0) {
        _items.removeAt(index);
      } else {
        _items[index].quantity = quantity;
      }
      notifyListeners();
    }
  }
  
  void clear() {
    _items.clear();
    notifyListeners();
  }
}

class ShoppingScreen extends StatelessWidget {
  final List<Product> products = [
    Product(id: '1', name: 'Laptop', price: 999.99),
    Product(id: '2', name: 'Phone', price: 599.99),
    Product(id: '3', name: 'Headphones', price: 199.99),
  ];
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Shopping'),
        actions: [
          Consumer<CartModel>(
            builder: (context, cart, child) {
              return Stack(
                children: [
                  IconButton(
                    onPressed: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => CartScreen()),
                      );
                    },
                    icon: Icon(Icons.shopping_cart),
                  ),
                  if (cart.itemCount > 0)
                    Positioned(
                      right: 8,
                      top: 8,
                      child: Container(
                        padding: EdgeInsets.all(2),
                        decoration: BoxDecoration(
                          color: Colors.red,
                          borderRadius: BorderRadius.circular(10),
                        ),
                        constraints: BoxConstraints(
                          minWidth: 16,
                          minHeight: 16,
                        ),
                        child: Text(
                          '${cart.itemCount}',
                          style: TextStyle(
                            color: Colors.white,
                            fontSize: 12,
                          ),
                          textAlign: TextAlign.center,
                        ),
                      ),
                    ),
                ],
              );
            },
          ),
        ],
      ),
      body: ListView.builder(
        itemCount: products.length,
        itemBuilder: (context, index) {
          final product = products[index];
          return ListTile(
            title: Text(product.name),
            subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
            trailing: ElevatedButton(
              onPressed: () {
                context.read<CartModel>().addProduct(product);
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(content: Text('${product.name} added to cart')),
                );
              },
              child: Text('Add to Cart'),
            ),
          );
        },
      ),
    );
  }
}
```

---

## 🧭 **NAVIGATION AND ROUTING**

### **Basic Navigation**

**Navigator.push and pop**:
```dart
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => ProfileScreen(userId: '123'),
                  ),
                );
              },
              child: Text('Go to Profile'),
            ),
            
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => SettingsScreen(),
                  ),
                );
              },
              child: Text('Go to Settings'),
            ),
          ],
        ),
      ),
    );
  }
}

class ProfileScreen extends StatelessWidget {
  final String userId;
  
  ProfileScreen({required this.userId});
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Profile'),
        leading: IconButton(
          icon: Icon(Icons.arrow_back),
          onPressed: () {
            Navigator.pop(context);
          },
        ),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('User ID: $userId'),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: Text('Go Back'),
            ),
          ],
        ),
      ),
    );
  }
}
```

**Passing Data Between Screens**:
```dart
class User {
  final String id;
  final String name;
  final String email;
  
  User({required this.id, required this.name, required this.email});
}

class UserListScreen extends StatelessWidget {
  final List<User> users = [
    User(id: '1', name: 'Alice', email: 'alice@example.com'),
    User(id: '2', name: 'Bob', email: 'bob@example.com'),
    User(id: '3', name: 'Charlie', email: 'charlie@example.com'),
  ];
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Users')),
      body: ListView.builder(
        itemCount: users.length,
        itemBuilder: (context, index) {
          final user = users[index];
          return ListTile(
            title: Text(user.name),
            subtitle: Text(user.email),
            onTap: () async {
              // Navigate and wait for result
              final result = await Navigator.push<String>(
                context,
                MaterialPageRoute(
                  builder: (context) => UserDetailScreen(user: user),
                ),
              );
              
              if (result != null) {
                ScaffoldMessenger.of(context).showSnackBar(
                  SnackBar(content: Text('Result: $result')),
                );
              }
            },
          );
        },
      ),
    );
  }
}

class UserDetailScreen extends StatelessWidget {
  final User user;
  
  UserDetailScreen({required this.user});
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(user.name)),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('ID: ${user.id}'),
            Text('Name: ${user.name}'),
            Text('Email: ${user.email}'),
            SizedBox(height: 20),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {
                    Navigator.pop(context, 'User updated');
                  },
                  child: Text('Update'),
                ),
                SizedBox(width: 10),
                ElevatedButton(
                  onPressed: () {
                    Navigator.pop(context, 'User deleted');
                  },
                  style: ElevatedButton.styleFrom(backgroundColor: Colors.red),
                  child: Text('Delete'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

### **Named Routes**

**Route Configuration**:
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Navigation Demo',
      initialRoute: '/',
      routes: {
        '/': (context) => HomeScreen(),
        '/profile': (context) => ProfileScreen(),
        '/settings': (context) => SettingsScreen(),
        '/about': (context) => AboutScreen(),
      },
      onGenerateRoute: (settings) {
        // Handle dynamic routes
        if (settings.name!.startsWith('/user/')) {
          final userId = settings.name!.split('/')[2];
          return MaterialPageRoute(
            builder: (context) => UserDetailScreen(userId: userId),
          );
        }
        return null;
      },
    );
  }
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/profile');
              },
              child: Text('Go to Profile'),
            ),
            
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/settings');
              },
              child: Text('Go to Settings'),
            ),
            
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/user/123');
              },
              child: Text('Go to User 123'),
            ),
          ],
        ),
      ),
    );
  }
}
```

### **Bottom Navigation**

**Bottom Navigation Bar Implementation**:
```dart
class MainScreen extends StatefulWidget {
  @override
  _MainScreenState createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int _currentIndex = 0;
  
  final List<Widget> _screens = [
    HomeTab(),
    SearchTab(),
    FavoritesTab(),
    ProfileTab(),
  ];
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: _screens[_currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        type: BottomNavigationBarType.fixed,
        currentIndex: _currentIndex,
        onTap: (index) {
          setState(() {
            _currentIndex = index;
          });
        },
        items: [
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.search),
            label: 'Search',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.favorite),
            label: 'Favorites',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.person),
            label: 'Profile',
          ),
        ],
      ),
    );
  }
}

class HomeTab extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Center(
        child: Text('Home Tab Content'),
      ),
    );
  }
}

class SearchTab extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Search')),
      body: Center(
        child: Text('Search Tab Content'),
      ),
    );
  }
}
```

---

## 🔌 **PLATFORM INTEGRATION AND NATIVE FEATURES**

### **HTTP Requests and API Integration**

**Setting up HTTP (pubspec.yaml)**:
```yaml
dependencies:
  flutter:
    sdk: flutter
  http: ^0.13.5
```

**API Service Class**:
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class Post {
  final int id;
  final int userId;
  final String title;
  final String body;
  
  Post({
    required this.id,
    required this.userId,
    required this.title,
    required this.body,
  });
  
  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'],
      userId: json['userId'],
      title: json['title'],
      body: json['body'],
    );
  }
  
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'userId': userId,
      'title': title,
      'body': body,
    };
  }
}

class ApiService {
  static const String baseUrl = 'https://jsonplaceholder.typicode.com';
  
  static Future<List<Post>> fetchPosts() async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/posts'),
        headers: {'Content-Type': 'application/json'},
      );
      
      if (response.statusCode == 200) {
        List<dynamic> jsonList = jsonDecode(response.body);
        return jsonList.map((json) => Post.fromJson(json)).toList();
      } else {
        throw Exception('Failed to load posts: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
  
  static Future<Post> fetchPost(int id) async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/posts/$id'),
        headers: {'Content-Type': 'application/json'},
      );
      
      if (response.statusCode == 200) {
        return Post.fromJson(jsonDecode(response.body));
      } else {
        throw Exception('Failed to load post: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
  
  static Future<Post> createPost(Post post) async {
    try {
      final response = await http.post(
        Uri.parse('$baseUrl/posts'),
        headers: {'Content-Type': 'application/json'},
        body: jsonEncode(post.toJson()),
      );
      
      if (response.statusCode == 201) {
        return Post.fromJson(jsonDecode(response.body));
      } else {
        throw Exception('Failed to create post: ${response.statusCode}');
      }
    } catch (e) {
      throw Exception('Network error: $e');
    }
  }
}
```

**Using API Service in Widget**:
```dart
class PostsScreen extends StatefulWidget {
  @override
  _PostsScreenState createState() => _PostsScreenState();
}

class _PostsScreenState extends State<PostsScreen> {
  List<Post> posts = [];
  bool isLoading = true;
  String? errorMessage;
  
  @override
  void initState() {
    super.initState();
    loadPosts();
  }
  
  Future<void> loadPosts() async {
    try {
      setState(() {
        isLoading = true;
        errorMessage = null;
      });
      
      final fetchedPosts = await ApiService.fetchPosts();
      
      setState(() {
        posts = fetchedPosts;
        isLoading = false;
      });
    } catch (e) {
      setState(() {
        errorMessage = e.toString();
        isLoading = false;
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Posts'),
        actions: [
          IconButton(
            onPressed: loadPosts,
            icon: Icon(Icons.refresh),
          ),
        ],
      ),
      body: _buildBody(),
    );
  }
  
  Widget _buildBody() {
    if (isLoading) {
      return Center(child: CircularProgressIndicator());
    }
    
    if (errorMessage != null) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.error, size: 64, color: Colors.red),
            SizedBox(height: 16),
            Text(
              'Error: $errorMessage',
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 16),
            ElevatedButton(
              onPressed: loadPosts,
              child: Text('Retry'),
            ),
          ],
        ),
      );
    }
    
    return RefreshIndicator(
      onRefresh: loadPosts,
      child: ListView.builder(
        itemCount: posts.length,
        itemBuilder: (context, index) {
          final post = posts[index];
          return Card(
            margin: EdgeInsets.all(8),
            child: ListTile(
              title: Text(
                post.title,
                style: TextStyle(fontWeight: FontWeight.bold),
              ),
              subtitle: Text(
                post.body,
                maxLines: 2,
                overflow: TextOverflow.ellipsis,
              ),
              onTap: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => PostDetailScreen(postId: post.id),
                  ),
                );
              },
            ),
          );
        },
      ),
    );
  }
}
```

### **Local Storage with SharedPreferences**

**SharedPreferences Helper**:
```dart
import 'package:shared_preferences/shared_preferences.dart';

class PreferencesService {
  static SharedPreferences? _preferences;
  
  static Future<void> init() async {
    _preferences = await SharedPreferences.getInstance();
  }
  
  // String methods
  static Future<bool> setString(String key, String value) async {
    return await _preferences?.setString(key, value) ?? false;
  }
  
  static String? getString(String key) {
    return _preferences?.getString(key);
  }
  
  // Int methods
  static Future<bool> setInt(String key, int value) async {
    return await _preferences?.setInt(key, value) ?? false;
  }
  
  static int? getInt(String key) {
    return _preferences?.getInt(key);
  }
  
  // Bool methods
  static Future<bool> setBool(String key, bool value) async {
    return await _preferences?.setBool(key, value) ?? false;
  }
  
  static bool? getBool(String key) {
    return _preferences?.getBool(key);
  }
  
  // List methods
  static Future<bool> setStringList(String key, List<String> value) async {
    return await _preferences?.setStringList(key, value) ?? false;
  }
  
  static List<String>? getStringList(String key) {
    return _preferences?.getStringList(key);
  }
  
  // Remove methods
  static Future<bool> remove(String key) async {
    return await _preferences?.remove(key) ?? false;
  }
  
  static Future<bool> clear() async {
    return await _preferences?.clear() ?? false;
  }
}

// Usage in main.dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await PreferencesService.init();
  runApp(MyApp());
}
```

**Settings Screen with Preferences**:
```dart
class SettingsScreen extends StatefulWidget {
  @override
  _SettingsScreenState createState() => _SettingsScreenState();
}

class _SettingsScreenState extends State<SettingsScreen> {
  bool _notificationsEnabled = true;
  bool _darkModeEnabled = false;
  String _username = '';
  
  @override
  void initState() {
    super.initState();
    _loadSettings();
  }
  
  void _loadSettings() {
    setState(() {
      _notificationsEnabled = PreferencesService.getBool('notifications') ?? true;
      _darkModeEnabled = PreferencesService.getBool('dark_mode') ?? false;
      _username = PreferencesService.getString('username') ?? '';
    });
  }
  
  Future<void> _saveSettings() async {
    await PreferencesService.setBool('notifications', _notificationsEnabled);
    await PreferencesService.setBool('dark_mode', _darkModeEnabled);
    await PreferencesService.setString('username', _username);
    
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Settings saved')),
    );
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Settings'),
        actions: [
          IconButton(
            onPressed: _saveSettings,
            icon: Icon(Icons.save),
          ),
        ],
      ),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          children: [
            TextField(
              decoration: InputDecoration(
                labelText: 'Username',
                border: OutlineInputBorder(),
              ),
              onChanged: (value) {
                setState(() {
                  _username = value;
                });
              },
              controller: TextEditingController(text: _username),
            ),
            
            SizedBox(height: 20),
            
            SwitchListTile(
              title: Text('Enable Notifications'),
              subtitle: Text('Receive push notifications'),
              value: _notificationsEnabled,
              onChanged: (value) {
                setState(() {
                  _notificationsEnabled = value;
                });
              },
            ),
            
            SwitchListTile(
              title: Text('Dark Mode'),
              subtitle: Text('Use dark theme'),
              value: _darkModeEnabled,
              onChanged: (value) {
                setState(() {
                  _darkModeEnabled = value;
                });
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## 🏗️ **PRACTICAL PROJECTS**

### **Project 1: Todo App with Local Storage**

**Features to Implement**:
- Add, edit, delete todos
- Mark todos as complete
- Filter todos (all, active, completed)
- Persist data locally

**Key Learning Objectives**:
- StatefulWidget and state management
- ListView and custom widgets
- Local data persistence
- Form handling and validation

### **Project 2: Weather App with API Integration**

**Features to Implement**:
- Current weather display
- 5-day forecast
- Search by city name
- Favorite cities list

**Key Learning Objectives**:
- HTTP requests and JSON parsing
- Error handling and loading states
- Navigation between screens
- SharedPreferences for favorites

### **Project 3: Social Media Feed**

**Features to Implement**:
- Post list with images
- Like and comment functionality
- User profiles
- Pull-to-refresh

**Key Learning Objectives**:
- Complex UI layouts
- Image loading and caching
- State management with Provider
- Navigation and data passing

---

## 🎯 **NEXT STEPS**

1. **Set up Flutter development environment** and create your first app
2. **Master Dart fundamentals** with focus on async programming
3. **Build the Todo App** to understand state management
4. **Create the Weather App** to learn API integration
5. **Explore advanced state management** (Bloc, Riverpod) for complex apps

Remember: Flutter's "everything is a widget" philosophy takes time to internalize. Start with simple projects and gradually build complexity. The Flutter community is very active, so don't hesitate to explore packages on pub.dev and join Flutter communities for support and learning.

Focus on understanding the widget lifecycle, state management patterns, and platform integration before moving to advanced topics like custom animations or platform channels.