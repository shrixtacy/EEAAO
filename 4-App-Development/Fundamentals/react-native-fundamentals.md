# React Native Development Fundamentals

Your comprehensive guide to building cross-platform mobile applications with React Native and JavaScript. This guide covers everything from React Native setup to advanced patterns with practical examples and real-world projects.

## 🎯 **WHAT YOU'LL LEARN**

- **React Native CLI vs Expo** setup and project structure
- **JSX for mobile** components and styling
- **Navigation** with React Navigation
- **State management** (useState, Context, Redux)
- **Native modules** and platform-specific code
- **Performance optimization** and debugging techniques

## 📋 **PREREQUISITES**

- **Strong JavaScript knowledge** (ES6+, async/await, destructuring)
- **React experience** (hooks, state management, component lifecycle)
- **Basic understanding** of mobile app concepts
- **Node.js and npm/yarn** installed on your system

---

## 🚀 **REACT NATIVE SETUP AND PROJECT STRUCTURE**

### **Development Environment Setup**

**React Native CLI Setup**:
```bash
# Install Node.js (14 or newer)
# Install React Native CLI globally
npm install -g react-native-cli

# For iOS development (macOS only)
# Install Xcode from App Store
# Install CocoaPods
sudo gem install cocoapods

# For Android development
# Install Android Studio
# Set up Android SDK and emulator
```
**Expo CLI Setup (Recommended for Beginners)**:
```bash
# Install Expo CLI globally
npm install -g @expo/cli

# Create new Expo project
npx create-expo-app MyFirstApp
cd MyFirstApp

# Start development server
npx expo start
```

**Project Structure Understanding**:
```
MyReactNativeApp/
├── App.js                    # Main app component
├── package.json              # Dependencies and scripts
├── metro.config.js           # Metro bundler configuration
├── babel.config.js           # Babel configuration
├── android/                  # Android-specific files
├── ios/                      # iOS-specific files (macOS only)
├── src/
│   ├── components/           # Reusable components
│   ├── screens/              # Screen components
│   ├── navigation/           # Navigation configuration
│   ├── services/             # API and utility services
│   └── styles/               # Style definitions
└── assets/                   # Images, fonts, etc.
```

### **First React Native App**

**Basic App.js Structure**:
```jsx
import React from 'react';
import {
  SafeAreaView,
  ScrollView,
  StatusBar,
  StyleSheet,
  Text,
  View,
} from 'react-native';

const App = () => {
  return (
    <SafeAreaView style={styles.container}>
      <StatusBar barStyle="dark-content" />
      <ScrollView contentInsetAdjustmentBehavior="automatic">
        <View style={styles.body}>
          <Text style={styles.title}>Welcome to React Native!</Text>
          <Text style={styles.subtitle}>
            Edit App.js to change this screen and then come back to see your edits.
          </Text>
        </View>
      </ScrollView>
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  body: {
    padding: 20,
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 20,
  },
  subtitle: {
    fontSize: 16,
    textAlign: 'center',
    color: '#666',
  },
});

export default App;
```

---

## 📱 **REACT NATIVE COMPONENTS AND JSX**

### **Core Components**

**View, Text, and Image Components**:
```jsx
import React from 'react';
import {
  View,
  Text,
  Image,
  StyleSheet,
  ScrollView,
} from 'react-native';

const ComponentsExample = () => {
  return (
    <ScrollView style={styles.container}>
      {/* Basic View container */}
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Text Examples</Text>
        
        {/* Different text styles */}
        <Text style={styles.normalText}>Normal text</Text>
        <Text style={styles.boldText}>Bold text</Text>
        <Text style={styles.italicText}>Italic text</Text>
        <Text style={styles.coloredText}>Colored text</Text>
        
        {/* Text with nested styling */}
        <Text style={styles.normalText}>
          This is <Text style={styles.boldText}>bold</Text> and{' '}
          <Text style={styles.coloredText}>colored</Text> text.
        </Text>
      </View>

      {/* Image examples */}
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Image Examples</Text>
        
        {/* Network image */}
        <Image
          source={{uri: 'https://picsum.photos/200/200'}}
          style={styles.networkImage}
          resizeMode="cover"
        />
        
        {/* Local image (add to assets folder) */}
        <Image
          source={require('../assets/logo.png')}
          style={styles.localImage}
          resizeMode="contain"
        />
      </View>

      {/* Layout examples */}
      <View style={styles.section}>
        <Text style={styles.sectionTitle}>Layout Examples</Text>
        
        {/* Horizontal layout */}
        <View style={styles.horizontalContainer}>
          <View style={[styles.box, {backgroundColor: 'red'}]} />
          <View style={[styles.box, {backgroundColor: 'green'}]} />
          <View style={[styles.box, {backgroundColor: 'blue'}]} />
        </View>
        
        {/* Vertical layout with flex */}
        <View style={styles.verticalContainer}>
          <View style={[styles.flexBox, {flex: 1, backgroundColor: 'orange'}]}>
            <Text>Flex: 1</Text>
          </View>
          <View style={[styles.flexBox, {flex: 2, backgroundColor: 'purple'}]}>
            <Text>Flex: 2</Text>
          </View>
          <View style={[styles.flexBox, {flex: 1, backgroundColor: 'cyan'}]}>
            <Text>Flex: 1</Text>
          </View>
        </View>
      </View>
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  section: {
    margin: 20,
    padding: 15,
    backgroundColor: 'white',
    borderRadius: 10,
    shadowColor: '#000',
    shadowOffset: {width: 0, height: 2},
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  sectionTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 15,
    color: '#333',
  },
  normalText: {
    fontSize: 16,
    marginBottom: 5,
  },
  boldText: {
    fontWeight: 'bold',
  },
  italicText: {
    fontStyle: 'italic',
  },
  coloredText: {
    color: '#007AFF',
  },
  networkImage: {
    width: 200,
    height: 200,
    marginBottom: 10,
    borderRadius: 10,
  },
  localImage: {
    width: 100,
    height: 100,
  },
  horizontalContainer: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 20,
  },
  box: {
    width: 50,
    height: 50,
    borderRadius: 5,
  },
  verticalContainer: {
    height: 150,
    flexDirection: 'column',
  },
  flexBox: {
    justifyContent: 'center',
    alignItems: 'center',
    margin: 2,
    borderRadius: 5,
  },
});

export default ComponentsExample;
```
**TouchableOpacity and Button Components**:
```jsx
import React, {useState} from 'react';
import {
  View,
  Text,
  TouchableOpacity,
  Button,
  Alert,
  StyleSheet,
} from 'react-native';

const InteractionExample = () => {
  const [counter, setCounter] = useState(0);
  const [buttonPressed, setButtonPressed] = useState(false);

  const handleIncrement = () => {
    setCounter(counter + 1);
  };

  const handleDecrement = () => {
    setCounter(counter - 1);
  };

  const handleReset = () => {
    setCounter(0);
    Alert.alert('Reset', 'Counter has been reset to 0');
  };

  const handleLongPress = () => {
    Alert.alert('Long Press', 'You held the button!');
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Interactive Components</Text>
      
      {/* Counter display */}
      <View style={styles.counterContainer}>
        <Text style={styles.counterText}>Counter: {counter}</Text>
      </View>

      {/* Custom TouchableOpacity buttons */}
      <View style={styles.buttonRow}>
        <TouchableOpacity
          style={[styles.customButton, styles.decrementButton]}
          onPress={handleDecrement}
          activeOpacity={0.7}
        >
          <Text style={styles.buttonText}>-</Text>
        </TouchableOpacity>

        <TouchableOpacity
          style={[styles.customButton, styles.resetButton]}
          onPress={handleReset}
          onLongPress={handleLongPress}
          activeOpacity={0.7}
        >
          <Text style={styles.buttonText}>Reset</Text>
        </TouchableOpacity>

        <TouchableOpacity
          style={[styles.customButton, styles.incrementButton]}
          onPress={handleIncrement}
          activeOpacity={0.7}
        >
          <Text style={styles.buttonText}>+</Text>
        </TouchableOpacity>
      </View>

      {/* Standard Button components */}
      <View style={styles.standardButtons}>
        <Button
          title="Standard Button"
          onPress={() => Alert.alert('Button Pressed')}
          color="#007AFF"
        />
        
        <View style={styles.buttonSpacer} />
        
        <Button
          title="Disabled Button"
          onPress={() => {}}
          disabled={true}
        />
      </View>

      {/* Pressable with state feedback */}
      <TouchableOpacity
        style={[
          styles.pressableButton,
          buttonPressed && styles.pressableButtonPressed
        ]}
        onPressIn={() => setButtonPressed(true)}
        onPressOut={() => setButtonPressed(false)}
        onPress={() => Alert.alert('Pressable', 'Button with visual feedback')}
      >
        <Text style={styles.pressableButtonText}>
          {buttonPressed ? 'Pressed!' : 'Press Me'}
        </Text>
      </TouchableOpacity>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 30,
  },
  counterContainer: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 10,
    marginBottom: 30,
    alignItems: 'center',
  },
  counterText: {
    fontSize: 32,
    fontWeight: 'bold',
    color: '#333',
  },
  buttonRow: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 30,
  },
  customButton: {
    flex: 1,
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
    marginHorizontal: 5,
  },
  decrementButton: {
    backgroundColor: '#FF3B30',
  },
  resetButton: {
    backgroundColor: '#FF9500',
  },
  incrementButton: {
    backgroundColor: '#34C759',
  },
  buttonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  standardButtons: {
    marginBottom: 30,
  },
  buttonSpacer: {
    height: 10,
  },
  pressableButton: {
    backgroundColor: '#007AFF',
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
  },
  pressableButtonPressed: {
    backgroundColor: '#0051D5',
    transform: [{scale: 0.95}],
  },
  pressableButtonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default InteractionExample;
```

### **TextInput and Form Handling**

**Form Components with Validation**:
```jsx
import React, {useState} from 'react';
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  Alert,
  StyleSheet,
  KeyboardAvoidingView,
  Platform,
  ScrollView,
} from 'react-native';

const FormExample = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    password: '',
    confirmPassword: '',
    bio: '',
  });
  
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleInputChange = (field, value) => {
    setFormData(prev => ({
      ...prev,
      [field]: value,
    }));
    
    // Clear error when user starts typing
    if (errors[field]) {
      setErrors(prev => ({
        ...prev,
        [field]: null,
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

    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }

    if (formData.password !== formData.confirmPassword) {
      newErrors.confirmPassword = 'Passwords do not match';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async () => {
    if (!validateForm()) {
      return;
    }

    setIsSubmitting(true);
    
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      
      Alert.alert('Success', 'Form submitted successfully!');
      
      // Reset form
      setFormData({
        name: '',
        email: '',
        password: '',
        confirmPassword: '',
        bio: '',
      });
    } catch (error) {
      Alert.alert('Error', 'Failed to submit form');
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <KeyboardAvoidingView
      style={styles.container}
      behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
    >
      <ScrollView contentContainerStyle={styles.scrollContainer}>
        <Text style={styles.title}>User Registration</Text>

        {/* Name Input */}
        <View style={styles.inputContainer}>
          <Text style={styles.label}>Full Name</Text>
          <TextInput
            style={[styles.input, errors.name && styles.inputError]}
            value={formData.name}
            onChangeText={(value) => handleInputChange('name', value)}
            placeholder="Enter your full name"
            autoCapitalize="words"
            autoCorrect={false}
          />
          {errors.name && <Text style={styles.errorText}>{errors.name}</Text>}
        </View>

        {/* Email Input */}
        <View style={styles.inputContainer}>
          <Text style={styles.label}>Email Address</Text>
          <TextInput
            style={[styles.input, errors.email && styles.inputError]}
            value={formData.email}
            onChangeText={(value) => handleInputChange('email', value)}
            placeholder="Enter your email"
            keyboardType="email-address"
            autoCapitalize="none"
            autoCorrect={false}
          />
          {errors.email && <Text style={styles.errorText}>{errors.email}</Text>}
        </View>

        {/* Password Input */}
        <View style={styles.inputContainer}>
          <Text style={styles.label}>Password</Text>
          <TextInput
            style={[styles.input, errors.password && styles.inputError]}
            value={formData.password}
            onChangeText={(value) => handleInputChange('password', value)}
            placeholder="Enter your password"
            secureTextEntry={true}
            autoCapitalize="none"
          />
          {errors.password && <Text style={styles.errorText}>{errors.password}</Text>}
        </View>

        {/* Confirm Password Input */}
        <View style={styles.inputContainer}>
          <Text style={styles.label}>Confirm Password</Text>
          <TextInput
            style={[styles.input, errors.confirmPassword && styles.inputError]}
            value={formData.confirmPassword}
            onChangeText={(value) => handleInputChange('confirmPassword', value)}
            placeholder="Confirm your password"
            secureTextEntry={true}
            autoCapitalize="none"
          />
          {errors.confirmPassword && (
            <Text style={styles.errorText}>{errors.confirmPassword}</Text>
          )}
        </View>

        {/* Bio Input (Multiline) */}
        <View style={styles.inputContainer}>
          <Text style={styles.label}>Bio (Optional)</Text>
          <TextInput
            style={[styles.input, styles.multilineInput]}
            value={formData.bio}
            onChangeText={(value) => handleInputChange('bio', value)}
            placeholder="Tell us about yourself..."
            multiline={true}
            numberOfLines={4}
            textAlignVertical="top"
          />
        </View>

        {/* Submit Button */}
        <TouchableOpacity
          style={[styles.submitButton, isSubmitting && styles.submitButtonDisabled]}
          onPress={handleSubmit}
          disabled={isSubmitting}
        >
          <Text style={styles.submitButtonText}>
            {isSubmitting ? 'Submitting...' : 'Register'}
          </Text>
        </TouchableOpacity>
      </ScrollView>
    </KeyboardAvoidingView>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
  },
  scrollContainer: {
    padding: 20,
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 30,
    color: '#333',
  },
  inputContainer: {
    marginBottom: 20,
  },
  label: {
    fontSize: 16,
    fontWeight: '600',
    marginBottom: 8,
    color: '#333',
  },
  input: {
    borderWidth: 1,
    borderColor: '#ddd',
    borderRadius: 8,
    padding: 12,
    fontSize: 16,
    backgroundColor: 'white',
  },
  inputError: {
    borderColor: '#FF3B30',
  },
  multilineInput: {
    height: 100,
  },
  errorText: {
    color: '#FF3B30',
    fontSize: 14,
    marginTop: 5,
  },
  submitButton: {
    backgroundColor: '#007AFF',
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
    marginTop: 20,
  },
  submitButtonDisabled: {
    backgroundColor: '#ccc',
  },
  submitButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
});

export default FormExample;
```
---

## 🎨 **STYLING WITH STYLESHEET**

### **StyleSheet Fundamentals**

**Basic Styling Concepts**:
```jsx
import React from 'react';
import {View, Text, StyleSheet, Dimensions} from 'react-native';

// Get device dimensions
const {width, height} = Dimensions.get('window');

const StylingExample = () => {
  return (
    <View style={styles.container}>
      {/* Flexbox Layout */}
      <View style={styles.header}>
        <Text style={styles.headerText}>Styling Examples</Text>
      </View>

      {/* Card with shadow */}
      <View style={styles.card}>
        <Text style={styles.cardTitle}>Card Component</Text>
        <Text style={styles.cardContent}>
          This card demonstrates shadows, borders, and spacing.
        </Text>
      </View>

      {/* Responsive layout */}
      <View style={styles.responsiveContainer}>
        <View style={styles.responsiveBox}>
          <Text style={styles.boxText}>Box 1</Text>
        </View>
        <View style={styles.responsiveBox}>
          <Text style={styles.boxText}>Box 2</Text>
        </View>
      </View>

      {/* Absolute positioning */}
      <View style={styles.relativeContainer}>
        <Text style={styles.relativeText}>Relative Container</Text>
        <View style={styles.absoluteBox}>
          <Text style={styles.absoluteText}>Absolute</Text>
        </View>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f0f0f0',
    padding: 20,
  },
  header: {
    backgroundColor: '#007AFF',
    padding: 20,
    borderRadius: 10,
    marginBottom: 20,
    alignItems: 'center',
  },
  headerText: {
    color: 'white',
    fontSize: 24,
    fontWeight: 'bold',
  },
  card: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 15,
    marginBottom: 20,
    // iOS shadow
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 2,
    },
    shadowOpacity: 0.25,
    shadowRadius: 3.84,
    // Android shadow
    elevation: 5,
  },
  cardTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 10,
    color: '#333',
  },
  cardContent: {
    fontSize: 16,
    color: '#666',
    lineHeight: 24,
  },
  responsiveContainer: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 20,
  },
  responsiveBox: {
    width: (width - 60) / 2, // Responsive width
    height: 100,
    backgroundColor: '#34C759',
    borderRadius: 10,
    justifyContent: 'center',
    alignItems: 'center',
  },
  boxText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
  relativeContainer: {
    position: 'relative',
    backgroundColor: '#FF9500',
    height: 120,
    borderRadius: 10,
    justifyContent: 'center',
    alignItems: 'center',
  },
  relativeText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  absoluteBox: {
    position: 'absolute',
    top: 10,
    right: 10,
    backgroundColor: '#FF3B30',
    padding: 8,
    borderRadius: 5,
  },
  absoluteText: {
    color: 'white',
    fontSize: 12,
    fontWeight: 'bold',
  },
});

export default StylingExample;
```

**Dynamic Styling and Themes**:
```jsx
import React, {useState} from 'react';
import {
  View,
  Text,
  TouchableOpacity,
  StyleSheet,
  Switch,
} from 'react-native';

const ThemeExample = () => {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const theme = {
    light: {
      backgroundColor: '#ffffff',
      textColor: '#000000',
      cardBackground: '#f8f8f8',
      primaryColor: '#007AFF',
    },
    dark: {
      backgroundColor: '#1c1c1e',
      textColor: '#ffffff',
      cardBackground: '#2c2c2e',
      primaryColor: '#0A84FF',
    },
  };

  const currentTheme = isDarkMode ? theme.dark : theme.light;

  const dynamicStyles = StyleSheet.create({
    container: {
      flex: 1,
      backgroundColor: currentTheme.backgroundColor,
      padding: 20,
    },
    title: {
      fontSize: 24,
      fontWeight: 'bold',
      color: currentTheme.textColor,
      textAlign: 'center',
      marginBottom: 30,
    },
    card: {
      backgroundColor: currentTheme.cardBackground,
      padding: 20,
      borderRadius: 10,
      marginBottom: 20,
    },
    cardText: {
      color: currentTheme.textColor,
      fontSize: 16,
    },
    button: {
      backgroundColor: currentTheme.primaryColor,
      padding: 15,
      borderRadius: 8,
      alignItems: 'center',
      marginBottom: 20,
    },
    buttonText: {
      color: 'white',
      fontSize: 16,
      fontWeight: 'bold',
    },
    switchContainer: {
      flexDirection: 'row',
      justifyContent: 'space-between',
      alignItems: 'center',
      padding: 20,
      backgroundColor: currentTheme.cardBackground,
      borderRadius: 10,
    },
    switchLabel: {
      fontSize: 18,
      color: currentTheme.textColor,
    },
  });

  return (
    <View style={dynamicStyles.container}>
      <Text style={dynamicStyles.title}>Dynamic Theming</Text>

      <View style={dynamicStyles.card}>
        <Text style={dynamicStyles.cardText}>
          This card adapts to the current theme. The colors change based on
          whether dark mode is enabled or not.
        </Text>
      </View>

      <TouchableOpacity style={dynamicStyles.button}>
        <Text style={dynamicStyles.buttonText}>Themed Button</Text>
      </TouchableOpacity>

      <View style={dynamicStyles.switchContainer}>
        <Text style={dynamicStyles.switchLabel}>Dark Mode</Text>
        <Switch
          value={isDarkMode}
          onValueChange={setIsDarkMode}
          trackColor={{false: '#767577', true: currentTheme.primaryColor}}
          thumbColor={isDarkMode ? '#f5dd4b' : '#f4f3f4'}
        />
      </View>
    </View>
  );
};

export default ThemeExample;
```

---

## 🧭 **NAVIGATION WITH REACT NAVIGATION**

### **Setting up React Navigation**

**Installation (package.json)**:
```json
{
  "dependencies": {
    "@react-navigation/native": "^6.1.9",
    "@react-navigation/stack": "^6.3.20",
    "@react-navigation/bottom-tabs": "^6.5.11",
    "react-native-screens": "^3.27.0",
    "react-native-safe-area-context": "^4.7.4",
    "react-native-gesture-handler": "^2.13.4"
  }
}
```

**Basic Stack Navigation**:
```jsx
import React from 'react';
import {NavigationContainer} from '@react-navigation/native';
import {createStackNavigator} from '@react-navigation/stack';
import {
  View,
  Text,
  TouchableOpacity,
  StyleSheet,
} from 'react-native';

const Stack = createStackNavigator();

// Home Screen
const HomeScreen = ({navigation}) => {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Home Screen</Text>
      
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate('Details', {
          itemId: 86,
          itemTitle: 'Sample Item',
        })}
      >
        <Text style={styles.buttonText}>Go to Details</Text>
      </TouchableOpacity>

      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate('Profile', {
          userId: '123',
          userName: 'John Doe',
        })}
      >
        <Text style={styles.buttonText}>Go to Profile</Text>
      </TouchableOpacity>
    </View>
  );
};

// Details Screen
const DetailsScreen = ({route, navigation}) => {
  const {itemId, itemTitle} = route.params;

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Details Screen</Text>
      <Text style={styles.subtitle}>Item ID: {itemId}</Text>
      <Text style={styles.subtitle}>Title: {itemTitle}</Text>
      
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.goBack()}
      >
        <Text style={styles.buttonText}>Go Back</Text>
      </TouchableOpacity>

      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.navigate('Home')}
      >
        <Text style={styles.buttonText}>Go to Home</Text>
      </TouchableOpacity>
    </View>
  );
};

// Profile Screen
const ProfileScreen = ({route, navigation}) => {
  const {userId, userName} = route.params;

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Profile Screen</Text>
      <Text style={styles.subtitle}>User ID: {userId}</Text>
      <Text style={styles.subtitle}>Name: {userName}</Text>
      
      <TouchableOpacity
        style={styles.button}
        onPress={() => navigation.popToTop()}
      >
        <Text style={styles.buttonText}>Go to Top</Text>
      </TouchableOpacity>
    </View>
  );
};

// Main App with Navigation
const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator
        initialRouteName="Home"
        screenOptions={{
          headerStyle: {
            backgroundColor: '#007AFF',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        }}
      >
        <Stack.Screen 
          name="Home" 
          component={HomeScreen}
          options={{title: 'My Home'}}
        />
        <Stack.Screen 
          name="Details" 
          component={DetailsScreen}
          options={({route}) => ({title: route.params.itemTitle})}
        />
        <Stack.Screen 
          name="Profile" 
          component={ProfileScreen}
          options={({route}) => ({title: `${route.params.userName}'s Profile`})}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
    color: '#333',
  },
  subtitle: {
    fontSize: 18,
    marginBottom: 10,
    color: '#666',
  },
  button: {
    backgroundColor: '#007AFF',
    padding: 15,
    borderRadius: 8,
    marginVertical: 10,
    minWidth: 200,
    alignItems: 'center',
  },
  buttonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default App;
```
**Bottom Tab Navigation**:
```jsx
import React from 'react';
import {NavigationContainer} from '@react-navigation/native';
import {createBottomTabNavigator} from '@react-navigation/bottom-tabs';
import {
  View,
  Text,
  StyleSheet,
} from 'react-native';
import Icon from 'react-native-vector-icons/Ionicons';

const Tab = createBottomTabNavigator();

// Tab Screens
const HomeTab = () => (
  <View style={styles.tabContainer}>
    <Text style={styles.tabTitle}>Home Tab</Text>
    <Text style={styles.tabContent}>Welcome to the home screen!</Text>
  </View>
);

const SearchTab = () => (
  <View style={styles.tabContainer}>
    <Text style={styles.tabTitle}>Search Tab</Text>
    <Text style={styles.tabContent}>Search for anything here.</Text>
  </View>
);

const NotificationsTab = () => (
  <View style={styles.tabContainer}>
    <Text style={styles.tabTitle}>Notifications Tab</Text>
    <Text style={styles.tabContent}>Your notifications appear here.</Text>
  </View>
);

const ProfileTab = () => (
  <View style={styles.tabContainer}>
    <Text style={styles.tabTitle}>Profile Tab</Text>
    <Text style={styles.tabContent}>Manage your profile settings.</Text>
  </View>
);

// Main App with Tab Navigation
const TabApp = () => {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({route}) => ({
          tabBarIcon: ({focused, color, size}) => {
            let iconName;

            if (route.name === 'Home') {
              iconName = focused ? 'home' : 'home-outline';
            } else if (route.name === 'Search') {
              iconName = focused ? 'search' : 'search-outline';
            } else if (route.name === 'Notifications') {
              iconName = focused ? 'notifications' : 'notifications-outline';
            } else if (route.name === 'Profile') {
              iconName = focused ? 'person' : 'person-outline';
            }

            return <Icon name={iconName} size={size} color={color} />;
          },
          tabBarActiveTintColor: '#007AFF',
          tabBarInactiveTintColor: 'gray',
          tabBarStyle: {
            paddingBottom: 5,
            paddingTop: 5,
            height: 60,
          },
          headerStyle: {
            backgroundColor: '#007AFF',
          },
          headerTintColor: '#fff',
          headerTitleStyle: {
            fontWeight: 'bold',
          },
        })}
      >
        <Tab.Screen name="Home" component={HomeTab} />
        <Tab.Screen name="Search" component={SearchTab} />
        <Tab.Screen name="Notifications" component={NotificationsTab} />
        <Tab.Screen name="Profile" component={ProfileTab} />
      </Tab.Navigator>
    </NavigationContainer>
  );
};

const styles = StyleSheet.create({
  tabContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  tabTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 20,
    color: '#333',
  },
  tabContent: {
    fontSize: 16,
    textAlign: 'center',
    color: '#666',
  },
});

export default TabApp;
```

---

## 🔄 **STATE MANAGEMENT**

### **useState and useEffect Hooks**

**Basic State Management with Hooks**:
```jsx
import React, {useState, useEffect} from 'react';
import {
  View,
  Text,
  TouchableOpacity,
  FlatList,
  StyleSheet,
  ActivityIndicator,
} from 'react-native';

const TodoApp = () => {
  const [todos, setTodos] = useState([]);
  const [loading, setLoading] = useState(true);
  const [filter, setFilter] = useState('all'); // all, active, completed

  // Load initial data
  useEffect(() => {
    loadTodos();
  }, []);

  const loadTodos = async () => {
    setLoading(true);
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      const initialTodos = [
        {id: '1', text: 'Learn React Native', completed: false},
        {id: '2', text: 'Build a mobile app', completed: false},
        {id: '3', text: 'Deploy to app store', completed: false},
      ];
      
      setTodos(initialTodos);
    } catch (error) {
      console.error('Failed to load todos:', error);
    } finally {
      setLoading(false);
    }
  };

  const addTodo = (text) => {
    const newTodo = {
      id: Date.now().toString(),
      text,
      completed: false,
    };
    setTodos(prev => [...prev, newTodo]);
  };

  const toggleTodo = (id) => {
    setTodos(prev =>
      prev.map(todo =>
        todo.id === id ? {...todo, completed: !todo.completed} : todo
      )
    );
  };

  const deleteTodo = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  };

  const getFilteredTodos = () => {
    switch (filter) {
      case 'active':
        return todos.filter(todo => !todo.completed);
      case 'completed':
        return todos.filter(todo => todo.completed);
      default:
        return todos;
    }
  };

  const renderTodo = ({item}) => (
    <View style={styles.todoItem}>
      <TouchableOpacity
        style={styles.todoContent}
        onPress={() => toggleTodo(item.id)}
      >
        <Text style={[
          styles.todoText,
          item.completed && styles.todoTextCompleted
        ]}>
          {item.text}
        </Text>
      </TouchableOpacity>
      
      <TouchableOpacity
        style={styles.deleteButton}
        onPress={() => deleteTodo(item.id)}
      >
        <Text style={styles.deleteButtonText}>Delete</Text>
      </TouchableOpacity>
    </View>
  );

  if (loading) {
    return (
      <View style={styles.loadingContainer}>
        <ActivityIndicator size="large" color="#007AFF" />
        <Text style={styles.loadingText}>Loading todos...</Text>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Todo App</Text>
      
      {/* Filter buttons */}
      <View style={styles.filterContainer}>
        {['all', 'active', 'completed'].map(filterType => (
          <TouchableOpacity
            key={filterType}
            style={[
              styles.filterButton,
              filter === filterType && styles.filterButtonActive
            ]}
            onPress={() => setFilter(filterType)}
          >
            <Text style={[
              styles.filterButtonText,
              filter === filterType && styles.filterButtonTextActive
            ]}>
              {filterType.charAt(0).toUpperCase() + filterType.slice(1)}
            </Text>
          </TouchableOpacity>
        ))}
      </View>

      {/* Add todo button */}
      <TouchableOpacity
        style={styles.addButton}
        onPress={() => addTodo(`New todo ${todos.length + 1}`)}
      >
        <Text style={styles.addButtonText}>Add Todo</Text>
      </TouchableOpacity>

      {/* Todo list */}
      <FlatList
        data={getFilteredTodos()}
        renderItem={renderTodo}
        keyExtractor={item => item.id}
        style={styles.todoList}
        showsVerticalScrollIndicator={false}
      />

      {/* Stats */}
      <View style={styles.stats}>
        <Text style={styles.statsText}>
          Total: {todos.length} | 
          Active: {todos.filter(t => !t.completed).length} | 
          Completed: {todos.filter(t => t.completed).length}
        </Text>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  loadingContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  loadingText: {
    marginTop: 10,
    fontSize: 16,
    color: '#666',
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 20,
    color: '#333',
  },
  filterContainer: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginBottom: 20,
  },
  filterButton: {
    padding: 10,
    borderRadius: 20,
    backgroundColor: '#e0e0e0',
    minWidth: 80,
    alignItems: 'center',
  },
  filterButtonActive: {
    backgroundColor: '#007AFF',
  },
  filterButtonText: {
    color: '#666',
    fontWeight: '600',
  },
  filterButtonTextActive: {
    color: 'white',
  },
  addButton: {
    backgroundColor: '#34C759',
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
    marginBottom: 20,
  },
  addButtonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
  todoList: {
    flex: 1,
  },
  todoItem: {
    flexDirection: 'row',
    backgroundColor: 'white',
    padding: 15,
    borderRadius: 8,
    marginBottom: 10,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: {width: 0, height: 1},
    shadowOpacity: 0.2,
    shadowRadius: 2,
    elevation: 2,
  },
  todoContent: {
    flex: 1,
  },
  todoText: {
    fontSize: 16,
    color: '#333',
  },
  todoTextCompleted: {
    textDecorationLine: 'line-through',
    color: '#999',
  },
  deleteButton: {
    backgroundColor: '#FF3B30',
    padding: 8,
    borderRadius: 5,
  },
  deleteButtonText: {
    color: 'white',
    fontSize: 12,
    fontWeight: 'bold',
  },
  stats: {
    marginTop: 20,
    padding: 15,
    backgroundColor: 'white',
    borderRadius: 8,
    alignItems: 'center',
  },
  statsText: {
    fontSize: 14,
    color: '#666',
  },
});

export default TodoApp;
```
### **Context API for Global State**

**Creating and Using Context**:
```jsx
import React, {createContext, useContext, useReducer} from 'react';
import {
  View,
  Text,
  TouchableOpacity,
  StyleSheet,
} from 'react-native';

// User Context
const UserContext = createContext();

// User reducer
const userReducer = (state, action) => {
  switch (action.type) {
    case 'LOGIN':
      return {
        ...state,
        isLoggedIn: true,
        user: action.payload,
      };
    case 'LOGOUT':
      return {
        ...state,
        isLoggedIn: false,
        user: null,
      };
    case 'UPDATE_PROFILE':
      return {
        ...state,
        user: {
          ...state.user,
          ...action.payload,
        },
      };
    default:
      return state;
  }
};

// User Provider Component
export const UserProvider = ({children}) => {
  const [state, dispatch] = useReducer(userReducer, {
    isLoggedIn: false,
    user: null,
  });

  const login = (userData) => {
    dispatch({type: 'LOGIN', payload: userData});
  };

  const logout = () => {
    dispatch({type: 'LOGOUT'});
  };

  const updateProfile = (updates) => {
    dispatch({type: 'UPDATE_PROFILE', payload: updates});
  };

  return (
    <UserContext.Provider
      value={{
        ...state,
        login,
        logout,
        updateProfile,
      }}
    >
      {children}
    </UserContext.Provider>
  );
};

// Custom hook to use user context
export const useUser = () => {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within a UserProvider');
  }
  return context;
};

// Login Screen Component
const LoginScreen = () => {
  const {login} = useUser();

  const handleLogin = () => {
    const userData = {
      id: '123',
      name: 'John Doe',
      email: 'john@example.com',
      avatar: 'https://picsum.photos/100/100',
    };
    login(userData);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Welcome!</Text>
      <Text style={styles.subtitle}>Please log in to continue</Text>
      
      <TouchableOpacity style={styles.loginButton} onPress={handleLogin}>
        <Text style={styles.loginButtonText}>Log In</Text>
      </TouchableOpacity>
    </View>
  );
};

// Profile Screen Component
const ProfileScreen = () => {
  const {user, logout, updateProfile} = useUser();

  const handleUpdateProfile = () => {
    updateProfile({
      name: 'John Updated',
      email: 'john.updated@example.com',
    });
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Profile</Text>
      
      <View style={styles.profileCard}>
        <Text style={styles.profileName}>{user.name}</Text>
        <Text style={styles.profileEmail}>{user.email}</Text>
        <Text style={styles.profileId}>ID: {user.id}</Text>
      </View>

      <TouchableOpacity
        style={styles.updateButton}
        onPress={handleUpdateProfile}
      >
        <Text style={styles.updateButtonText}>Update Profile</Text>
      </TouchableOpacity>

      <TouchableOpacity style={styles.logoutButton} onPress={logout}>
        <Text style={styles.logoutButtonText}>Log Out</Text>
      </TouchableOpacity>
    </View>
  );
};

// Main App Component
const ContextApp = () => {
  const {isLoggedIn} = useUser();

  return (
    <View style={styles.appContainer}>
      {isLoggedIn ? <ProfileScreen /> : <LoginScreen />}
    </View>
  );
};

// App with Provider
const AppWithProvider = () => {
  return (
    <UserProvider>
      <ContextApp />
    </UserProvider>
  );
};

const styles = StyleSheet.create({
  appContainer: {
    flex: 1,
  },
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 28,
    fontWeight: 'bold',
    marginBottom: 10,
    color: '#333',
  },
  subtitle: {
    fontSize: 16,
    marginBottom: 30,
    color: '#666',
    textAlign: 'center',
  },
  loginButton: {
    backgroundColor: '#007AFF',
    padding: 15,
    borderRadius: 8,
    minWidth: 200,
    alignItems: 'center',
  },
  loginButtonText: {
    color: 'white',
    fontSize: 18,
    fontWeight: 'bold',
  },
  profileCard: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 10,
    marginBottom: 30,
    minWidth: 250,
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: {width: 0, height: 2},
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 3,
  },
  profileName: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 5,
    color: '#333',
  },
  profileEmail: {
    fontSize: 16,
    marginBottom: 5,
    color: '#666',
  },
  profileId: {
    fontSize: 14,
    color: '#999',
  },
  updateButton: {
    backgroundColor: '#34C759',
    padding: 15,
    borderRadius: 8,
    minWidth: 200,
    alignItems: 'center',
    marginBottom: 10,
  },
  updateButtonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
  logoutButton: {
    backgroundColor: '#FF3B30',
    padding: 15,
    borderRadius: 8,
    minWidth: 200,
    alignItems: 'center',
  },
  logoutButtonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default AppWithProvider;
```

---

## 🔌 **NATIVE MODULES AND PLATFORM INTEGRATION**

### **Platform-Specific Code**

**Using Platform API**:
```jsx
import React from 'react';
import {
  View,
  Text,
  StyleSheet,
  Platform,
  Alert,
  TouchableOpacity,
} from 'react-native';

const PlatformExample = () => {
  const showPlatformAlert = () => {
    if (Platform.OS === 'ios') {
      Alert.alert(
        'iOS Alert',
        'This is running on iOS',
        [
          {text: 'Cancel', style: 'cancel'},
          {text: 'OK', style: 'default'},
        ]
      );
    } else {
      Alert.alert('Android Alert', 'This is running on Android');
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Platform Detection</Text>
      
      <View style={styles.infoCard}>
        <Text style={styles.infoLabel}>Platform:</Text>
        <Text style={styles.infoValue}>{Platform.OS}</Text>
        
        <Text style={styles.infoLabel}>Version:</Text>
        <Text style={styles.infoValue}>{Platform.Version}</Text>
        
        {Platform.OS === 'ios' && (
          <>
            <Text style={styles.infoLabel}>Is iPad:</Text>
            <Text style={styles.infoValue}>
              {Platform.isPad ? 'Yes' : 'No'}
            </Text>
          </>
        )}
      </View>

      <TouchableOpacity
        style={[styles.button, styles.platformButton]}
        onPress={showPlatformAlert}
      >
        <Text style={styles.buttonText}>Show Platform Alert</Text>
      </TouchableOpacity>

      {/* Platform-specific styling */}
      <View style={styles.platformSpecificContainer}>
        <Text style={styles.platformText}>
          This container has platform-specific styling
        </Text>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 30,
    color: '#333',
  },
  infoCard: {
    backgroundColor: 'white',
    padding: 20,
    borderRadius: 10,
    marginBottom: 20,
    ...Platform.select({
      ios: {
        shadowColor: '#000',
        shadowOffset: {width: 0, height: 2},
        shadowOpacity: 0.1,
        shadowRadius: 4,
      },
      android: {
        elevation: 4,
      },
    }),
  },
  infoLabel: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#333',
    marginTop: 10,
  },
  infoValue: {
    fontSize: 16,
    color: '#666',
    marginBottom: 5,
  },
  button: {
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
    marginBottom: 20,
  },
  platformButton: {
    backgroundColor: Platform.OS === 'ios' ? '#007AFF' : '#4CAF50',
  },
  buttonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
  platformSpecificContainer: {
    padding: 20,
    borderRadius: 10,
    ...Platform.select({
      ios: {
        backgroundColor: '#E3F2FD',
        borderWidth: 1,
        borderColor: '#2196F3',
      },
      android: {
        backgroundColor: '#E8F5E8',
        borderWidth: 2,
        borderColor: '#4CAF50',
      },
    }),
  },
  platformText: {
    textAlign: 'center',
    fontSize: 16,
    color: Platform.OS === 'ios' ? '#1976D2' : '#2E7D32',
  },
});

export default PlatformExample;
```

### **Accessing Device Features**

**Camera and Image Picker Integration**:
```jsx
import React, {useState} from 'react';
import {
  View,
  Text,
  Image,
  TouchableOpacity,
  Alert,
  StyleSheet,
  PermissionsAndroid,
  Platform,
} from 'react-native';
import {launchCamera, launchImageLibrary} from 'react-native-image-picker';

const CameraExample = () => {
  const [imageUri, setImageUri] = useState(null);

  const requestCameraPermission = async () => {
    if (Platform.OS === 'android') {
      try {
        const granted = await PermissionsAndroid.request(
          PermissionsAndroid.PERMISSIONS.CAMERA,
          {
            title: 'Camera Permission',
            message: 'This app needs access to camera to take photos.',
            buttonNeutral: 'Ask Me Later',
            buttonNegative: 'Cancel',
            buttonPositive: 'OK',
          }
        );
        return granted === PermissionsAndroid.RESULTS.GRANTED;
      } catch (err) {
        console.warn(err);
        return false;
      }
    }
    return true;
  };

  const showImagePicker = () => {
    Alert.alert(
      'Select Image',
      'Choose an option',
      [
        {text: 'Camera', onPress: openCamera},
        {text: 'Gallery', onPress: openGallery},
        {text: 'Cancel', style: 'cancel'},
      ]
    );
  };

  const openCamera = async () => {
    const hasPermission = await requestCameraPermission();
    if (!hasPermission) {
      Alert.alert('Permission Denied', 'Camera permission is required');
      return;
    }

    const options = {
      mediaType: 'photo',
      quality: 0.8,
      maxWidth: 800,
      maxHeight: 600,
    };

    launchCamera(options, (response) => {
      if (response.didCancel || response.error) {
        return;
      }
      
      if (response.assets && response.assets[0]) {
        setImageUri(response.assets[0].uri);
      }
    });
  };

  const openGallery = () => {
    const options = {
      mediaType: 'photo',
      quality: 0.8,
      maxWidth: 800,
      maxHeight: 600,
    };

    launchImageLibrary(options, (response) => {
      if (response.didCancel || response.error) {
        return;
      }
      
      if (response.assets && response.assets[0]) {
        setImageUri(response.assets[0].uri);
      }
    });
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Camera & Gallery</Text>
      
      <View style={styles.imageContainer}>
        {imageUri ? (
          <Image source={{uri: imageUri}} style={styles.image} />
        ) : (
          <View style={styles.placeholder}>
            <Text style={styles.placeholderText}>No image selected</Text>
          </View>
        )}
      </View>

      <TouchableOpacity style={styles.button} onPress={showImagePicker}>
        <Text style={styles.buttonText}>Select Image</Text>
      </TouchableOpacity>

      {imageUri && (
        <TouchableOpacity
          style={[styles.button, styles.clearButton]}
          onPress={() => setImageUri(null)}
        >
          <Text style={styles.buttonText}>Clear Image</Text>
        </TouchableOpacity>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#f5f5f5',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 30,
    color: '#333',
  },
  imageContainer: {
    alignItems: 'center',
    marginBottom: 30,
  },
  image: {
    width: 300,
    height: 300,
    borderRadius: 10,
  },
  placeholder: {
    width: 300,
    height: 300,
    backgroundColor: '#e0e0e0',
    borderRadius: 10,
    justifyContent: 'center',
    alignItems: 'center',
    borderWidth: 2,
    borderColor: '#ccc',
    borderStyle: 'dashed',
  },
  placeholderText: {
    fontSize: 16,
    color: '#999',
  },
  button: {
    backgroundColor: '#007AFF',
    padding: 15,
    borderRadius: 8,
    alignItems: 'center',
    marginBottom: 10,
  },
  clearButton: {
    backgroundColor: '#FF3B30',
  },
  buttonText: {
    color: 'white',
    fontSize: 16,
    fontWeight: 'bold',
  },
});

export default CameraExample;
```

---

## 🏗️ **PRACTICAL PROJECTS**

### **Project 1: Weather App with API Integration**

**Features to Implement**:
- Current weather display
- Search by city name
- 5-day forecast
- Location-based weather
- Favorite cities

**Key Learning Objectives**:
- HTTP requests with fetch API
- JSON parsing and error handling
- Navigation between screens
- AsyncStorage for favorites
- Loading states and user feedback

### **Project 2: Social Media Feed**

**Features to Implement**:
- Post list with images
- Like and comment functionality
- User profiles
- Pull-to-refresh
- Infinite scrolling

**Key Learning Objectives**:
- FlatList optimization
- Image loading and caching
- Complex state management
- Navigation with parameters
- Performance optimization

### **Project 3: E-commerce App**

**Features to Implement**:
- Product catalog
- Shopping cart
- User authentication
- Order history
- Payment integration

**Key Learning Objectives**:
- Complex navigation flows
- Global state management
- Form validation
- Secure storage
- Third-party integrations

---

## 🎯 **NEXT STEPS**

1. **Set up React Native development environment** and create your first app
2. **Master React fundamentals** if you haven't already (hooks, state, props)
3. **Build the Weather App** to understand API integration and navigation
4. **Create the Social Media Feed** to learn complex UI patterns
5. **Explore advanced topics** like animations, performance optimization, and native modules

Remember: React Native leverages your existing React knowledge but adds mobile-specific concepts. Focus on understanding the differences between web and mobile development, platform-specific considerations, and performance implications. The React Native community is very active, so utilize resources like the official documentation, Expo docs, and community packages.

Start with Expo for easier setup and development, then eject to bare React Native when you need more control or native modules that aren't supported by Expo.