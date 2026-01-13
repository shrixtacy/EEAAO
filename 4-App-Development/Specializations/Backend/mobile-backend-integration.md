# Mobile Backend Integration Guide

Your comprehensive guide to integrating mobile applications with backend services. This guide covers API integration, authentication, real-time features, and app store deployment with practical examples and best practices.

## 🎯 **WHAT YOU'LL LEARN**

- **API integration** patterns and best practices
- **Authentication and authorization** flows
- **Real-time features** with WebSockets and push notifications
- **Data synchronization** and offline capabilities
- **App store deployment** processes and requirements

## 📋 **PREREQUISITES**

- Basic mobile development knowledge (Android, Flutter, or React Native)
- Understanding of HTTP protocols and REST APIs
- Basic knowledge of JSON and data formats
- Familiarity with authentication concepts

---

## 🌐 **API INTEGRATION FUNDAMENTALS**

### **RESTful API Integration**

**HTTP Client Setup (React Native Example)**:
```javascript
// api/client.js
class ApiClient {
  constructor(baseURL) {
    this.baseURL = baseURL;
    this.defaultHeaders = {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    };
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    const config = {
      headers: {
        ...this.defaultHeaders,
        ...options.headers,
      },
      ...options,
    };

    try {
      const response = await fetch(url, config);
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      const contentType = response.headers.get('content-type');
      if (contentType && contentType.includes('application/json')) {
        return await response.json();
      }
      
      return await response.text();
    } catch (error) {
      console.error('API Request failed:', error);
      throw error;
    }
  }

  // GET request
  async get(endpoint, params = {}) {
    const queryString = new URLSearchParams(params).toString();
    const url = queryString ? `${endpoint}?${queryString}` : endpoint;
    
    return this.request(url, {
      method: 'GET',
    });
  }

  // POST request
  async post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      body: JSON.stringify(data),
    });
  }

  // PUT request
  async put(endpoint, data) {
    return this.request(endpoint, {
      method: 'PUT',
      body: JSON.stringify(data),
    });
  }

  // DELETE request
  async delete(endpoint) {
    return this.request(endpoint, {
      method: 'DELETE',
    });
  }

  // Set authorization header
  setAuthToken(token) {
    this.defaultHeaders['Authorization'] = `Bearer ${token}`;
  }

  // Remove authorization header
  clearAuthToken() {
    delete this.defaultHeaders['Authorization'];
  }
}

// Create API client instance
const apiClient = new ApiClient('https://api.example.com/v1');

export default apiClient;
```
**Service Layer Implementation**:
```javascript
// services/userService.js
import apiClient from '../api/client';

export class UserService {
  // Get user profile
  static async getProfile(userId) {
    try {
      const response = await apiClient.get(`/users/${userId}`);
      return {
        success: true,
        data: response,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Update user profile
  static async updateProfile(userId, profileData) {
    try {
      const response = await apiClient.put(`/users/${userId}`, profileData);
      return {
        success: true,
        data: response,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Get user posts
  static async getUserPosts(userId, page = 1, limit = 10) {
    try {
      const response = await apiClient.get('/posts', {
        userId,
        page,
        limit,
      });
      return {
        success: true,
        data: response.posts,
        pagination: {
          page: response.page,
          totalPages: response.totalPages,
          hasMore: response.hasMore,
        },
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Upload user avatar
  static async uploadAvatar(userId, imageFile) {
    try {
      const formData = new FormData();
      formData.append('avatar', {
        uri: imageFile.uri,
        type: imageFile.type,
        name: imageFile.fileName || 'avatar.jpg',
      });

      const response = await apiClient.request(`/users/${userId}/avatar`, {
        method: 'POST',
        body: formData,
        headers: {
          'Content-Type': 'multipart/form-data',
        },
      });

      return {
        success: true,
        data: response,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }
}

// services/postService.js
export class PostService {
  // Get posts feed
  static async getFeed(page = 1, limit = 10) {
    try {
      const response = await apiClient.get('/posts/feed', {
        page,
        limit,
      });
      return {
        success: true,
        data: response.posts,
        pagination: {
          page: response.page,
          totalPages: response.totalPages,
          hasMore: response.hasMore,
        },
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Create new post
  static async createPost(postData) {
    try {
      const response = await apiClient.post('/posts', postData);
      return {
        success: true,
        data: response,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Like/unlike post
  static async toggleLike(postId) {
    try {
      const response = await apiClient.post(`/posts/${postId}/like`);
      return {
        success: true,
        data: response,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Add comment to post
  static async addComment(postId, commentText) {
    try {
      const response = await apiClient.post(`/posts/${postId}/comments`, {
        text: commentText,
      });
      return {
        success: true,
        data: response,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }
}
```

**Error Handling and Retry Logic**:
```javascript
// utils/apiUtils.js
export class ApiUtils {
  // Retry mechanism for failed requests
  static async retryRequest(requestFn, maxRetries = 3, delay = 1000) {
    let lastError;
    
    for (let attempt = 1; attempt <= maxRetries; attempt++) {
      try {
        return await requestFn();
      } catch (error) {
        lastError = error;
        
        // Don't retry on client errors (4xx)
        if (error.status >= 400 && error.status < 500) {
          throw error;
        }
        
        if (attempt < maxRetries) {
          await this.delay(delay * attempt); // Exponential backoff
        }
      }
    }
    
    throw lastError;
  }

  // Delay utility
  static delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  // Network status checker
  static async checkNetworkStatus() {
    try {
      const response = await fetch('https://www.google.com/favicon.ico', {
        method: 'HEAD',
        mode: 'no-cors',
      });
      return true;
    } catch (error) {
      return false;
    }
  }

  // Handle API errors
  static handleApiError(error) {
    if (error.status) {
      switch (error.status) {
        case 400:
          return 'Bad request. Please check your input.';
        case 401:
          return 'Authentication required. Please log in.';
        case 403:
          return 'Access denied. You don\'t have permission.';
        case 404:
          return 'Resource not found.';
        case 429:
          return 'Too many requests. Please try again later.';
        case 500:
          return 'Server error. Please try again later.';
        default:
          return `Request failed with status ${error.status}`;
      }
    }
    
    if (error.message.includes('Network')) {
      return 'Network error. Please check your connection.';
    }
    
    return 'An unexpected error occurred.';
  }
}

// Usage in components
import { ApiUtils } from '../utils/apiUtils';
import { UserService } from '../services/userService';

const ProfileScreen = () => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const loadUserProfile = async (userId) => {
    setLoading(true);
    setError(null);

    try {
      const result = await ApiUtils.retryRequest(
        () => UserService.getProfile(userId),
        3, // max retries
        1000 // initial delay
      );

      if (result.success) {
        setUser(result.data);
      } else {
        setError(ApiUtils.handleApiError(result.error));
      }
    } catch (error) {
      setError(ApiUtils.handleApiError(error));
    } finally {
      setLoading(false);
    }
  };

  // Component render logic...
};
```

### **GraphQL Integration**

**Apollo Client Setup (React Native)**:
```javascript
// apollo/client.js
import { ApolloClient, InMemoryCache, createHttpLink } from '@apollo/client';
import { setContext } from '@apollo/client/link/context';
import AsyncStorage from '@react-native-async-storage/async-storage';

// HTTP link
const httpLink = createHttpLink({
  uri: 'https://api.example.com/graphql',
});

// Auth link
const authLink = setContext(async (_, { headers }) => {
  const token = await AsyncStorage.getItem('authToken');
  
  return {
    headers: {
      ...headers,
      authorization: token ? `Bearer ${token}` : '',
    },
  };
});

// Apollo Client instance
const client = new ApolloClient({
  link: authLink.concat(httpLink),
  cache: new InMemoryCache(),
  defaultOptions: {
    watchQuery: {
      errorPolicy: 'all',
    },
    query: {
      errorPolicy: 'all',
    },
  },
});

export default client;
```

**GraphQL Queries and Mutations**:
```javascript
// graphql/queries.js
import { gql } from '@apollo/client';

export const GET_USER_PROFILE = gql`
  query GetUserProfile($userId: ID!) {
    user(id: $userId) {
      id
      name
      email
      avatar
      bio
      createdAt
      posts {
        id
        title
        content
        createdAt
        likesCount
        commentsCount
      }
    }
  }
`;

export const GET_POSTS_FEED = gql`
  query GetPostsFeed($first: Int!, $after: String) {
    posts(first: $first, after: $after) {
      edges {
        node {
          id
          title
          content
          createdAt
          author {
            id
            name
            avatar
          }
          likesCount
          commentsCount
          isLiked
        }
        cursor
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
  }
`;

// graphql/mutations.js
export const CREATE_POST = gql`
  mutation CreatePost($input: CreatePostInput!) {
    createPost(input: $input) {
      id
      title
      content
      createdAt
      author {
        id
        name
        avatar
      }
    }
  }
`;

export const TOGGLE_LIKE = gql`
  mutation ToggleLike($postId: ID!) {
    toggleLike(postId: $postId) {
      id
      likesCount
      isLiked
    }
  }
`;

export const UPDATE_PROFILE = gql`
  mutation UpdateProfile($input: UpdateProfileInput!) {
    updateProfile(input: $input) {
      id
      name
      bio
      avatar
    }
  }
`;
```

**Using GraphQL in Components**:
```javascript
// components/ProfileScreen.js
import React from 'react';
import { View, Text, ActivityIndicator, Alert } from 'react-native';
import { useQuery, useMutation } from '@apollo/client';
import { GET_USER_PROFILE, UPDATE_PROFILE } from '../graphql/queries';

const ProfileScreen = ({ userId }) => {
  const { data, loading, error, refetch } = useQuery(GET_USER_PROFILE, {
    variables: { userId },
    errorPolicy: 'all',
  });

  const [updateProfile, { loading: updating }] = useMutation(UPDATE_PROFILE, {
    onCompleted: (data) => {
      Alert.alert('Success', 'Profile updated successfully');
    },
    onError: (error) => {
      Alert.alert('Error', error.message);
    },
    // Update cache after mutation
    update: (cache, { data: { updateProfile } }) => {
      cache.modify({
        id: cache.identify({ __typename: 'User', id: userId }),
        fields: {
          name: () => updateProfile.name,
          bio: () => updateProfile.bio,
          avatar: () => updateProfile.avatar,
        },
      });
    },
  });

  const handleUpdateProfile = (profileData) => {
    updateProfile({
      variables: {
        input: profileData,
      },
    });
  };

  if (loading) {
    return (
      <View style={styles.loadingContainer}>
        <ActivityIndicator size="large" />
        <Text>Loading profile...</Text>
      </View>
    );
  }

  if (error) {
    return (
      <View style={styles.errorContainer}>
        <Text>Error: {error.message}</Text>
        <TouchableOpacity onPress={() => refetch()}>
          <Text>Retry</Text>
        </TouchableOpacity>
      </View>
    );
  }

  const user = data?.user;

  return (
    <View style={styles.container}>
      <Text style={styles.name}>{user.name}</Text>
      <Text style={styles.email}>{user.email}</Text>
      <Text style={styles.bio}>{user.bio}</Text>
      
      {/* Profile editing UI */}
      <TouchableOpacity
        onPress={() => handleUpdateProfile({
          name: 'Updated Name',
          bio: 'Updated bio',
        })}
        disabled={updating}
      >
        <Text>{updating ? 'Updating...' : 'Update Profile'}</Text>
      </TouchableOpacity>
    </View>
  );
};
```

---

## 🔐 **AUTHENTICATION AND AUTHORIZATION**

### **JWT Token Authentication**

**Authentication Service**:
```javascript
// services/authService.js
import AsyncStorage from '@react-native-async-storage/async-storage';
import apiClient from '../api/client';

export class AuthService {
  static TOKEN_KEY = 'authToken';
  static REFRESH_TOKEN_KEY = 'refreshToken';
  static USER_KEY = 'userData';

  // Login with email and password
  static async login(email, password) {
    try {
      const response = await apiClient.post('/auth/login', {
        email,
        password,
      });

      if (response.success) {
        await this.storeTokens(response.accessToken, response.refreshToken);
        await this.storeUserData(response.user);
        apiClient.setAuthToken(response.accessToken);
        
        return {
          success: true,
          user: response.user,
        };
      }

      return {
        success: false,
        error: response.message || 'Login failed',
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Register new user
  static async register(userData) {
    try {
      const response = await apiClient.post('/auth/register', userData);

      if (response.success) {
        await this.storeTokens(response.accessToken, response.refreshToken);
        await this.storeUserData(response.user);
        apiClient.setAuthToken(response.accessToken);
        
        return {
          success: true,
          user: response.user,
        };
      }

      return {
        success: false,
        error: response.message || 'Registration failed',
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Logout user
  static async logout() {
    try {
      // Call logout endpoint to invalidate token on server
      await apiClient.post('/auth/logout');
    } catch (error) {
      console.warn('Logout API call failed:', error);
    } finally {
      // Clear local storage regardless of API call result
      await this.clearTokens();
      await this.clearUserData();
      apiClient.clearAuthToken();
    }
  }

  // Refresh access token
  static async refreshToken() {
    try {
      const refreshToken = await AsyncStorage.getItem(this.REFRESH_TOKEN_KEY);
      
      if (!refreshToken) {
        throw new Error('No refresh token available');
      }

      const response = await apiClient.post('/auth/refresh', {
        refreshToken,
      });

      if (response.success) {
        await this.storeTokens(response.accessToken, response.refreshToken);
        apiClient.setAuthToken(response.accessToken);
        
        return {
          success: true,
          accessToken: response.accessToken,
        };
      }

      throw new Error(response.message || 'Token refresh failed');
    } catch (error) {
      // If refresh fails, logout user
      await this.logout();
      return {
        success: false,
        error: error.message,
      };
    }
  }

  // Check if user is authenticated
  static async isAuthenticated() {
    try {
      const token = await AsyncStorage.getItem(this.TOKEN_KEY);
      const userData = await AsyncStorage.getItem(this.USER_KEY);
      
      if (token && userData) {
        apiClient.setAuthToken(token);
        return {
          isAuthenticated: true,
          user: JSON.parse(userData),
        };
      }

      return {
        isAuthenticated: false,
        user: null,
      };
    } catch (error) {
      return {
        isAuthenticated: false,
        user: null,
      };
    }
  }

  // Store tokens securely
  static async storeTokens(accessToken, refreshToken) {
    await AsyncStorage.multiSet([
      [this.TOKEN_KEY, accessToken],
      [this.REFRESH_TOKEN_KEY, refreshToken],
    ]);
  }

  // Store user data
  static async storeUserData(userData) {
    await AsyncStorage.setItem(this.USER_KEY, JSON.stringify(userData));
  }

  // Clear tokens
  static async clearTokens() {
    await AsyncStorage.multiRemove([this.TOKEN_KEY, this.REFRESH_TOKEN_KEY]);
  }

  // Clear user data
  static async clearUserData() {
    await AsyncStorage.removeItem(this.USER_KEY);
  }

  // Get stored user data
  static async getUserData() {
    try {
      const userData = await AsyncStorage.getItem(this.USER_KEY);
      return userData ? JSON.parse(userData) : null;
    } catch (error) {
      return null;
    }
  }
}
```
**Authentication Context Provider**:
```javascript
// context/AuthContext.js
import React, { createContext, useContext, useReducer, useEffect } from 'react';
import { AuthService } from '../services/authService';

const AuthContext = createContext();

const authReducer = (state, action) => {
  switch (action.type) {
    case 'LOGIN_START':
      return {
        ...state,
        loading: true,
        error: null,
      };
    case 'LOGIN_SUCCESS':
      return {
        ...state,
        loading: false,
        isAuthenticated: true,
        user: action.payload.user,
        error: null,
      };
    case 'LOGIN_FAILURE':
      return {
        ...state,
        loading: false,
        isAuthenticated: false,
        user: null,
        error: action.payload.error,
      };
    case 'LOGOUT':
      return {
        ...state,
        loading: false,
        isAuthenticated: false,
        user: null,
        error: null,
      };
    case 'UPDATE_USER':
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

export const AuthProvider = ({ children }) => {
  const [state, dispatch] = useReducer(authReducer, {
    isAuthenticated: false,
    user: null,
    loading: true,
    error: null,
  });

  // Check authentication status on app start
  useEffect(() => {
    checkAuthStatus();
  }, []);

  const checkAuthStatus = async () => {
    try {
      const result = await AuthService.isAuthenticated();
      
      if (result.isAuthenticated) {
        dispatch({
          type: 'LOGIN_SUCCESS',
          payload: { user: result.user },
        });
      } else {
        dispatch({ type: 'LOGOUT' });
      }
    } catch (error) {
      dispatch({ type: 'LOGOUT' });
    }
  };

  const login = async (email, password) => {
    dispatch({ type: 'LOGIN_START' });
    
    try {
      const result = await AuthService.login(email, password);
      
      if (result.success) {
        dispatch({
          type: 'LOGIN_SUCCESS',
          payload: { user: result.user },
        });
        return { success: true };
      } else {
        dispatch({
          type: 'LOGIN_FAILURE',
          payload: { error: result.error },
        });
        return { success: false, error: result.error };
      }
    } catch (error) {
      dispatch({
        type: 'LOGIN_FAILURE',
        payload: { error: error.message },
      });
      return { success: false, error: error.message };
    }
  };

  const register = async (userData) => {
    dispatch({ type: 'LOGIN_START' });
    
    try {
      const result = await AuthService.register(userData);
      
      if (result.success) {
        dispatch({
          type: 'LOGIN_SUCCESS',
          payload: { user: result.user },
        });
        return { success: true };
      } else {
        dispatch({
          type: 'LOGIN_FAILURE',
          payload: { error: result.error },
        });
        return { success: false, error: result.error };
      }
    } catch (error) {
      dispatch({
        type: 'LOGIN_FAILURE',
        payload: { error: error.message },
      });
      return { success: false, error: error.message };
    }
  };

  const logout = async () => {
    await AuthService.logout();
    dispatch({ type: 'LOGOUT' });
  };

  const updateUser = (userData) => {
    dispatch({
      type: 'UPDATE_USER',
      payload: userData,
    });
  };

  return (
    <AuthContext.Provider
      value={{
        ...state,
        login,
        register,
        logout,
        updateUser,
      }}
    >
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```

### **OAuth Integration (Google Sign-In)**

**Google Sign-In Setup**:
```javascript
// services/googleAuthService.js
import { GoogleSignin } from '@react-native-google-signin/google-signin';
import { AuthService } from './authService';

export class GoogleAuthService {
  static configure() {
    GoogleSignin.configure({
      webClientId: 'YOUR_WEB_CLIENT_ID', // From Google Console
      offlineAccess: true,
      hostedDomain: '',
      forceCodeForRefreshToken: true,
    });
  }

  static async signIn() {
    try {
      await GoogleSignin.hasPlayServices();
      const userInfo = await GoogleSignin.signIn();
      
      // Send Google token to your backend for verification
      const result = await this.authenticateWithBackend(userInfo.idToken);
      
      if (result.success) {
        await AuthService.storeTokens(result.accessToken, result.refreshToken);
        await AuthService.storeUserData(result.user);
        
        return {
          success: true,
          user: result.user,
        };
      }

      return {
        success: false,
        error: result.error,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  static async signOut() {
    try {
      await GoogleSignin.signOut();
      await AuthService.logout();
    } catch (error) {
      console.error('Google sign out error:', error);
    }
  }

  static async authenticateWithBackend(idToken) {
    try {
      const response = await fetch('https://api.example.com/auth/google', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          idToken,
        }),
      });

      return await response.json();
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  static async getCurrentUser() {
    try {
      const userInfo = await GoogleSignin.signInSilently();
      return userInfo;
    } catch (error) {
      return null;
    }
  }
}
```

### **Biometric Authentication**

**Biometric Authentication Setup**:
```javascript
// services/biometricService.js
import TouchID from 'react-native-touch-id';
import AsyncStorage from '@react-native-async-storage/async-storage';

export class BiometricService {
  static BIOMETRIC_ENABLED_KEY = 'biometricEnabled';

  static async isSupported() {
    try {
      const biometryType = await TouchID.isSupported();
      return {
        isSupported: true,
        biometryType,
      };
    } catch (error) {
      return {
        isSupported: false,
        error: error.message,
      };
    }
  }

  static async authenticate(reason = 'Authenticate to access your account') {
    try {
      const optionalConfigObject = {
        title: 'Authentication Required',
        imageColor: '#e00606',
        imageErrorColor: '#ff0000',
        sensorDescription: 'Touch sensor',
        sensorErrorDescription: 'Failed',
        cancelText: 'Cancel',
        fallbackLabel: 'Show Passcode',
        unifiedErrors: false,
        passcodeFallback: false,
      };

      const success = await TouchID.authenticate(reason, optionalConfigObject);
      return {
        success: true,
      };
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  static async enableBiometric() {
    try {
      const result = await this.authenticate('Enable biometric authentication');
      
      if (result.success) {
        await AsyncStorage.setItem(this.BIOMETRIC_ENABLED_KEY, 'true');
        return { success: true };
      }

      return result;
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }

  static async disableBiometric() {
    await AsyncStorage.removeItem(this.BIOMETRIC_ENABLED_KEY);
  }

  static async isBiometricEnabled() {
    try {
      const enabled = await AsyncStorage.getItem(this.BIOMETRIC_ENABLED_KEY);
      return enabled === 'true';
    } catch (error) {
      return false;
    }
  }

  static async authenticateWithBiometric() {
    try {
      const isEnabled = await this.isBiometricEnabled();
      
      if (!isEnabled) {
        return {
          success: false,
          error: 'Biometric authentication is not enabled',
        };
      }

      return await this.authenticate('Use biometric to sign in');
    } catch (error) {
      return {
        success: false,
        error: error.message,
      };
    }
  }
}
```

---

## 🔄 **REAL-TIME FEATURES**

### **WebSocket Integration**

**WebSocket Service**:
```javascript
// services/websocketService.js
export class WebSocketService {
  constructor(url) {
    this.url = url;
    this.ws = null;
    this.reconnectAttempts = 0;
    this.maxReconnectAttempts = 5;
    this.reconnectInterval = 1000;
    this.listeners = new Map();
    this.isConnecting = false;
  }

  connect(token) {
    if (this.isConnecting || (this.ws && this.ws.readyState === WebSocket.OPEN)) {
      return;
    }

    this.isConnecting = true;
    
    try {
      this.ws = new WebSocket(`${this.url}?token=${token}`);
      
      this.ws.onopen = () => {
        console.log('WebSocket connected');
        this.isConnecting = false;
        this.reconnectAttempts = 0;
        this.emit('connected');
      };

      this.ws.onmessage = (event) => {
        try {
          const data = JSON.parse(event.data);
          this.handleMessage(data);
        } catch (error) {
          console.error('Failed to parse WebSocket message:', error);
        }
      };

      this.ws.onclose = (event) => {
        console.log('WebSocket disconnected:', event.code, event.reason);
        this.isConnecting = false;
        this.emit('disconnected', { code: event.code, reason: event.reason });
        
        if (event.code !== 1000) { // Not a normal closure
          this.attemptReconnect(token);
        }
      };

      this.ws.onerror = (error) => {
        console.error('WebSocket error:', error);
        this.isConnecting = false;
        this.emit('error', error);
      };
    } catch (error) {
      console.error('Failed to create WebSocket connection:', error);
      this.isConnecting = false;
      this.emit('error', error);
    }
  }

  disconnect() {
    if (this.ws) {
      this.ws.close(1000, 'Client disconnect');
      this.ws = null;
    }
    this.reconnectAttempts = this.maxReconnectAttempts; // Prevent reconnection
  }

  send(type, data) {
    if (this.ws && this.ws.readyState === WebSocket.OPEN) {
      const message = JSON.stringify({ type, data });
      this.ws.send(message);
    } else {
      console.warn('WebSocket is not connected');
    }
  }

  handleMessage(message) {
    const { type, data } = message;
    this.emit(type, data);
  }

  attemptReconnect(token) {
    if (this.reconnectAttempts >= this.maxReconnectAttempts) {
      console.log('Max reconnection attempts reached');
      return;
    }

    this.reconnectAttempts++;
    const delay = this.reconnectInterval * Math.pow(2, this.reconnectAttempts - 1);
    
    console.log(`Attempting to reconnect in ${delay}ms (attempt ${this.reconnectAttempts})`);
    
    setTimeout(() => {
      this.connect(token);
    }, delay);
  }

  on(event, callback) {
    if (!this.listeners.has(event)) {
      this.listeners.set(event, []);
    }
    this.listeners.get(event).push(callback);
  }

  off(event, callback) {
    if (this.listeners.has(event)) {
      const callbacks = this.listeners.get(event);
      const index = callbacks.indexOf(callback);
      if (index > -1) {
        callbacks.splice(index, 1);
      }
    }
  }

  emit(event, data) {
    if (this.listeners.has(event)) {
      this.listeners.get(event).forEach(callback => {
        try {
          callback(data);
        } catch (error) {
          console.error('Error in WebSocket event callback:', error);
        }
      });
    }
  }

  isConnected() {
    return this.ws && this.ws.readyState === WebSocket.OPEN;
  }
}

// Create singleton instance
const websocketService = new WebSocketService('wss://api.example.com/ws');
export default websocketService;
```

**Real-time Chat Implementation**:
```javascript
// services/chatService.js
import websocketService from './websocketService';

export class ChatService {
  static initialize(authToken) {
    websocketService.connect(authToken);
    
    // Set up event listeners
    websocketService.on('message', this.handleNewMessage);
    websocketService.on('typing', this.handleTypingIndicator);
    websocketService.on('user_joined', this.handleUserJoined);
    websocketService.on('user_left', this.handleUserLeft);
  }

  static cleanup() {
    websocketService.off('message', this.handleNewMessage);
    websocketService.off('typing', this.handleTypingIndicator);
    websocketService.off('user_joined', this.handleUserJoined);
    websocketService.off('user_left', this.handleUserLeft);
    websocketService.disconnect();
  }

  static sendMessage(roomId, message) {
    websocketService.send('send_message', {
      roomId,
      message,
      timestamp: new Date().toISOString(),
    });
  }

  static joinRoom(roomId) {
    websocketService.send('join_room', { roomId });
  }

  static leaveRoom(roomId) {
    websocketService.send('leave_room', { roomId });
  }

  static sendTypingIndicator(roomId, isTyping) {
    websocketService.send('typing', {
      roomId,
      isTyping,
    });
  }

  static handleNewMessage = (data) => {
    // Emit custom event for components to listen to
    this.emit('newMessage', data);
  };

  static handleTypingIndicator = (data) => {
    this.emit('typingIndicator', data);
  };

  static handleUserJoined = (data) => {
    this.emit('userJoined', data);
  };

  static handleUserLeft = (data) => {
    this.emit('userLeft', data);
  };

  // Event emitter for components
  static listeners = new Map();

  static on(event, callback) {
    if (!this.listeners.has(event)) {
      this.listeners.set(event, []);
    }
    this.listeners.get(event).push(callback);
  }

  static off(event, callback) {
    if (this.listeners.has(event)) {
      const callbacks = this.listeners.get(event);
      const index = callbacks.indexOf(callback);
      if (index > -1) {
        callbacks.splice(index, 1);
      }
    }
  }

  static emit(event, data) {
    if (this.listeners.has(event)) {
      this.listeners.get(event).forEach(callback => {
        callback(data);
      });
    }
  }
}
```
### **Push Notifications**

**Firebase Cloud Messaging Setup**:
```javascript
// services/notificationService.js
import messaging from '@react-native-firebase/messaging';
import AsyncStorage from '@react-native-async-storage/async-storage';
import { Platform } from 'react-native';

export class NotificationService {
  static FCM_TOKEN_KEY = 'fcmToken';

  static async initialize() {
    // Request permission for iOS
    if (Platform.OS === 'ios') {
      const authStatus = await messaging().requestPermission();
      const enabled =
        authStatus === messaging.AuthorizationStatus.AUTHORIZED ||
        authStatus === messaging.AuthorizationStatus.PROVISIONAL;

      if (!enabled) {
        console.log('Push notification permission denied');
        return false;
      }
    }

    // Get FCM token
    const token = await this.getFCMToken();
    if (token) {
      await this.sendTokenToServer(token);
    }

    // Listen for token refresh
    messaging().onTokenRefresh(async (newToken) => {
      await AsyncStorage.setItem(this.FCM_TOKEN_KEY, newToken);
      await this.sendTokenToServer(newToken);
    });

    // Handle foreground messages
    messaging().onMessage(async (remoteMessage) => {
      console.log('Foreground message:', remoteMessage);
      this.handleForegroundMessage(remoteMessage);
    });

    // Handle background messages
    messaging().setBackgroundMessageHandler(async (remoteMessage) => {
      console.log('Background message:', remoteMessage);
      this.handleBackgroundMessage(remoteMessage);
    });

    // Handle notification opened app
    messaging().onNotificationOpenedApp((remoteMessage) => {
      console.log('Notification opened app:', remoteMessage);
      this.handleNotificationOpened(remoteMessage);
    });

    // Check if app was opened from a notification
    const initialNotification = await messaging().getInitialNotification();
    if (initialNotification) {
      console.log('App opened from notification:', initialNotification);
      this.handleNotificationOpened(initialNotification);
    }

    return true;
  }

  static async getFCMToken() {
    try {
      let token = await AsyncStorage.getItem(this.FCM_TOKEN_KEY);
      
      if (!token) {
        token = await messaging().getToken();
        if (token) {
          await AsyncStorage.setItem(this.FCM_TOKEN_KEY, token);
        }
      }
      
      return token;
    } catch (error) {
      console.error('Error getting FCM token:', error);
      return null;
    }
  }

  static async sendTokenToServer(token) {
    try {
      await fetch('https://api.example.com/notifications/register', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${await AsyncStorage.getItem('authToken')}`,
        },
        body: JSON.stringify({
          fcmToken: token,
          platform: Platform.OS,
        }),
      });
    } catch (error) {
      console.error('Error sending FCM token to server:', error);
    }
  }

  static handleForegroundMessage(remoteMessage) {
    // Show in-app notification or update UI
    const { notification, data } = remoteMessage;
    
    // You can show a custom in-app notification here
    // or update the app state based on the message
    
    // Example: Show alert for foreground messages
    if (notification) {
      Alert.alert(
        notification.title || 'Notification',
        notification.body || 'You have a new message',
        [
          {
            text: 'OK',
            onPress: () => {
              if (data) {
                this.handleNotificationData(data);
              }
            },
          },
        ]
      );
    }
  }

  static handleBackgroundMessage(remoteMessage) {
    // Handle background message
    // This runs in a separate JS context
    console.log('Background message handled:', remoteMessage);
  }

  static handleNotificationOpened(remoteMessage) {
    // Navigate to specific screen based on notification data
    const { data } = remoteMessage;
    
    if (data) {
      this.handleNotificationData(data);
    }
  }

  static handleNotificationData(data) {
    // Handle notification data and navigate accordingly
    switch (data.type) {
      case 'chat_message':
        // Navigate to chat screen
        // NavigationService.navigate('Chat', { roomId: data.roomId });
        break;
      case 'friend_request':
        // Navigate to friends screen
        // NavigationService.navigate('Friends');
        break;
      case 'post_like':
        // Navigate to post
        // NavigationService.navigate('Post', { postId: data.postId });
        break;
      default:
        // Navigate to home or default screen
        // NavigationService.navigate('Home');
        break;
    }
  }

  static async subscribeToTopic(topic) {
    try {
      await messaging().subscribeToTopic(topic);
      console.log(`Subscribed to topic: ${topic}`);
    } catch (error) {
      console.error(`Error subscribing to topic ${topic}:`, error);
    }
  }

  static async unsubscribeFromTopic(topic) {
    try {
      await messaging().unsubscribeFromTopic(topic);
      console.log(`Unsubscribed from topic: ${topic}`);
    } catch (error) {
      console.error(`Error unsubscribing from topic ${topic}:`, error);
    }
  }

  static async clearToken() {
    try {
      await messaging().deleteToken();
      await AsyncStorage.removeItem(this.FCM_TOKEN_KEY);
    } catch (error) {
      console.error('Error clearing FCM token:', error);
    }
  }
}
```

**Local Notifications**:
```javascript
// services/localNotificationService.js
import PushNotification from 'react-native-push-notification';
import PushNotificationIOS from '@react-native-community/push-notification-ios';
import { Platform } from 'react-native';

export class LocalNotificationService {
  static configure() {
    PushNotification.configure({
      onRegister: function (token) {
        console.log('Local notification token:', token);
      },

      onNotification: function (notification) {
        console.log('Local notification:', notification);
        
        if (notification.userInteraction) {
          // User tapped on notification
          this.handleNotificationTap(notification);
        }

        // Required on iOS only
        if (Platform.OS === 'ios') {
          notification.finish(PushNotificationIOS.FetchResult.NoData);
        }
      },

      permissions: {
        alert: true,
        badge: true,
        sound: true,
      },

      popInitialNotification: true,
      requestPermissions: Platform.OS === 'ios',
    });

    // Create default channel for Android
    if (Platform.OS === 'android') {
      PushNotification.createChannel(
        {
          channelId: 'default-channel',
          channelName: 'Default Channel',
          channelDescription: 'Default notification channel',
          soundName: 'default',
          importance: 4,
          vibrate: true,
        },
        (created) => console.log(`Default channel created: ${created}`)
      );
    }
  }

  static scheduleNotification(options) {
    PushNotification.localNotificationSchedule({
      channelId: 'default-channel',
      title: options.title,
      message: options.message,
      date: options.date,
      userInfo: options.data || {},
      soundName: 'default',
      playSound: true,
      vibrate: true,
      ...options,
    });
  }

  static showNotification(options) {
    PushNotification.localNotification({
      channelId: 'default-channel',
      title: options.title,
      message: options.message,
      userInfo: options.data || {},
      soundName: 'default',
      playSound: true,
      vibrate: true,
      ...options,
    });
  }

  static cancelNotification(notificationId) {
    PushNotification.cancelLocalNotifications({ id: notificationId });
  }

  static cancelAllNotifications() {
    PushNotification.cancelAllLocalNotifications();
  }

  static setBadgeNumber(number) {
    if (Platform.OS === 'ios') {
      PushNotificationIOS.setApplicationIconBadgeNumber(number);
    } else {
      // Android badge handling varies by launcher
      PushNotification.setApplicationIconBadgeNumber(number);
    }
  }

  static handleNotificationTap(notification) {
    // Handle notification tap
    const { userInfo } = notification;
    
    if (userInfo && userInfo.type) {
      switch (userInfo.type) {
        case 'reminder':
          // Navigate to reminder screen
          break;
        case 'message':
          // Navigate to message screen
          break;
        default:
          // Navigate to home screen
          break;
      }
    }
  }

  // Schedule reminder notification
  static scheduleReminder(title, message, date, data = {}) {
    this.scheduleNotification({
      title,
      message,
      date,
      data: {
        type: 'reminder',
        ...data,
      },
    });
  }

  // Schedule daily notification
  static scheduleDailyNotification(title, message, hour, minute) {
    const now = new Date();
    const scheduledDate = new Date();
    scheduledDate.setHours(hour, minute, 0, 0);
    
    // If the time has passed today, schedule for tomorrow
    if (scheduledDate <= now) {
      scheduledDate.setDate(scheduledDate.getDate() + 1);
    }

    this.scheduleNotification({
      title,
      message,
      date: scheduledDate,
      repeatType: 'day',
    });
  }
}
```

---

## 📱 **APP STORE DEPLOYMENT**

### **Pre-Deployment Checklist**

**iOS App Store Preparation**:
```javascript
// Build configuration for iOS release
// ios/YourApp/Info.plist additions

// App Transport Security (if needed)
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSExceptionDomains</key>
  <dict>
    <key>your-api-domain.com</key>
    <dict>
      <key>NSExceptionAllowsInsecureHTTPLoads</key>
      <true/>
      <key>NSExceptionMinimumTLSVersion</key>
      <string>TLSv1.0</string>
    </dict>
  </dict>
</dict>

// Camera usage description
<key>NSCameraUsageDescription</key>
<string>This app needs access to camera to take photos</string>

// Photo library usage description
<key>NSPhotoLibraryUsageDescription</key>
<string>This app needs access to photo library to select images</string>

// Location usage description
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs location access to provide location-based features</string>
```

**Android Play Store Preparation**:
```xml
<!-- android/app/src/main/AndroidManifest.xml -->
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
  
  <!-- Permissions -->
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.CAMERA" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  
  <!-- Network security config for API calls -->
  <application
    android:networkSecurityConfig="@xml/network_security_config"
    android:usesCleartextTraffic="true">
    
    <!-- App components -->
    
  </application>
</manifest>
```

**Build Scripts and Configuration**:
```json
// package.json scripts for deployment
{
  "scripts": {
    "android:build": "cd android && ./gradlew assembleRelease",
    "android:bundle": "cd android && ./gradlew bundleRelease",
    "ios:build": "xcodebuild -workspace ios/YourApp.xcworkspace -scheme YourApp -configuration Release -destination generic/platform=iOS -archivePath ios/build/YourApp.xcarchive archive",
    "ios:export": "xcodebuild -exportArchive -archivePath ios/build/YourApp.xcarchive -exportPath ios/build -exportOptionsPlist ios/ExportOptions.plist",
    "version:patch": "npm version patch && react-native-version",
    "version:minor": "npm version minor && react-native-version",
    "version:major": "npm version major && react-native-version"
  }
}
```

### **Deployment Automation**

**Fastlane Configuration**:
```ruby
# fastlane/Fastfile
default_platform(:ios)

platform :ios do
  desc "Build and upload to TestFlight"
  lane :beta do
    # Increment build number
    increment_build_number(xcodeproj: "YourApp.xcodeproj")
    
    # Build the app
    build_app(
      workspace: "YourApp.xcworkspace",
      scheme: "YourApp",
      configuration: "Release",
      export_method: "app-store"
    )
    
    # Upload to TestFlight
    upload_to_testflight(
      skip_waiting_for_build_processing: true
    )
    
    # Send notification
    slack(
      message: "New iOS build uploaded to TestFlight! 🚀",
      channel: "#mobile-releases"
    )
  end

  desc "Deploy to App Store"
  lane :release do
    # Increment version
    increment_version_number(
      bump_type: "patch"
    )
    
    # Build and upload
    build_app(
      workspace: "YourApp.xcworkspace",
      scheme: "YourApp",
      configuration: "Release",
      export_method: "app-store"
    )
    
    upload_to_app_store(
      force: true,
      submit_for_review: true,
      automatic_release: false,
      submission_information: {
        add_id_info_uses_idfa: false,
        add_id_info_serves_ads: false,
        add_id_info_tracks_install: false,
        add_id_info_tracks_action: false,
        export_compliance_encryption_updated: false,
        export_compliance_uses_encryption: false
      }
    )
  end
end

platform :android do
  desc "Build and upload to Play Console"
  lane :beta do
    # Increment version code
    increment_version_code(
      gradle_file_path: "app/build.gradle"
    )
    
    # Build AAB
    gradle(
      task: "bundle",
      build_type: "Release"
    )
    
    # Upload to Play Console
    upload_to_play_store(
      track: "internal",
      aab: "app/build/outputs/bundle/release/app-release.aab"
    )
    
    # Send notification
    slack(
      message: "New Android build uploaded to Play Console! 🚀",
      channel: "#mobile-releases"
    )
  end

  desc "Deploy to Play Store"
  lane :release do
    # Build and upload
    gradle(
      task: "bundle",
      build_type: "Release"
    )
    
    upload_to_play_store(
      track: "production",
      aab: "app/build/outputs/bundle/release/app-release.aab"
    )
  end
end
```

### **App Store Optimization**

**Metadata and Screenshots**:
```javascript
// App Store metadata structure
const appStoreMetadata = {
  name: "Your App Name",
  subtitle: "Short descriptive subtitle", // iOS only
  description: `
    Detailed app description that highlights key features and benefits.
    
    Key Features:
    • Feature 1 with clear benefit
    • Feature 2 with user value
    • Feature 3 with unique selling point
    
    Why choose our app:
    - Reason 1
    - Reason 2
    - Reason 3
    
    Download now and experience the difference!
  `,
  keywords: "keyword1,keyword2,keyword3", // iOS only, max 100 characters
  supportURL: "https://yourapp.com/support",
  privacyURL: "https://yourapp.com/privacy",
  categories: {
    primary: "Productivity",
    secondary: "Business"
  },
  contentRating: "4+", // iOS / "Everyone" for Android
  screenshots: {
    iPhone: [
      "screenshot1_iphone.png",
      "screenshot2_iphone.png",
      "screenshot3_iphone.png"
    ],
    iPad: [
      "screenshot1_ipad.png",
      "screenshot2_ipad.png"
    ],
    android: [
      "screenshot1_android.png",
      "screenshot2_android.png",
      "screenshot3_android.png"
    ]
  }
};
```

---

## 🎯 **BEST PRACTICES AND OPTIMIZATION**

### **API Performance Optimization**

**Caching Strategy**:
```javascript
// utils/cacheManager.js
import AsyncStorage from '@react-native-async-storage/async-storage';

export class CacheManager {
  static CACHE_PREFIX = 'api_cache_';
  static DEFAULT_TTL = 5 * 60 * 1000; // 5 minutes

  static async get(key) {
    try {
      const cacheKey = this.CACHE_PREFIX + key;
      const cached = await AsyncStorage.getItem(cacheKey);
      
      if (cached) {
        const { data, timestamp, ttl } = JSON.parse(cached);
        
        if (Date.now() - timestamp < ttl) {
          return data;
        } else {
          // Cache expired, remove it
          await AsyncStorage.removeItem(cacheKey);
        }
      }
      
      return null;
    } catch (error) {
      console.error('Cache get error:', error);
      return null;
    }
  }

  static async set(key, data, ttl = this.DEFAULT_TTL) {
    try {
      const cacheKey = this.CACHE_PREFIX + key;
      const cacheData = {
        data,
        timestamp: Date.now(),
        ttl,
      };
      
      await AsyncStorage.setItem(cacheKey, JSON.stringify(cacheData));
    } catch (error) {
      console.error('Cache set error:', error);
    }
  }

  static async remove(key) {
    try {
      const cacheKey = this.CACHE_PREFIX + key;
      await AsyncStorage.removeItem(cacheKey);
    } catch (error) {
      console.error('Cache remove error:', error);
    }
  }

  static async clear() {
    try {
      const keys = await AsyncStorage.getAllKeys();
      const cacheKeys = keys.filter(key => key.startsWith(this.CACHE_PREFIX));
      await AsyncStorage.multiRemove(cacheKeys);
    } catch (error) {
      console.error('Cache clear error:', error);
    }
  }
}

// Enhanced API client with caching
class CachedApiClient extends ApiClient {
  async get(endpoint, params = {}, options = {}) {
    const cacheKey = `${endpoint}_${JSON.stringify(params)}`;
    
    // Try to get from cache first
    if (!options.skipCache) {
      const cached = await CacheManager.get(cacheKey);
      if (cached) {
        return cached;
      }
    }
    
    // Make API request
    const response = await super.get(endpoint, params);
    
    // Cache the response
    if (options.cacheTTL !== 0) {
      await CacheManager.set(cacheKey, response, options.cacheTTL);
    }
    
    return response;
  }
}
```

### **Offline Data Synchronization**

**Offline Queue Manager**:
```javascript
// services/offlineQueueService.js
import AsyncStorage from '@react-native-async-storage/async-storage';
import NetInfo from '@react-native-netinfo/netinfo';

export class OfflineQueueService {
  static QUEUE_KEY = 'offline_queue';
  static isProcessing = false;

  static async addToQueue(request) {
    try {
      const queue = await this.getQueue();
      const queueItem = {
        id: Date.now().toString(),
        timestamp: Date.now(),
        ...request,
      };
      
      queue.push(queueItem);
      await AsyncStorage.setItem(this.QUEUE_KEY, JSON.stringify(queue));
      
      // Try to process queue if online
      this.processQueue();
      
      return queueItem.id;
    } catch (error) {
      console.error('Error adding to offline queue:', error);
      return null;
    }
  }

  static async getQueue() {
    try {
      const queue = await AsyncStorage.getItem(this.QUEUE_KEY);
      return queue ? JSON.parse(queue) : [];
    } catch (error) {
      console.error('Error getting offline queue:', error);
      return [];
    }
  }

  static async removeFromQueue(itemId) {
    try {
      const queue = await this.getQueue();
      const filteredQueue = queue.filter(item => item.id !== itemId);
      await AsyncStorage.setItem(this.QUEUE_KEY, JSON.stringify(filteredQueue));
    } catch (error) {
      console.error('Error removing from offline queue:', error);
    }
  }

  static async processQueue() {
    if (this.isProcessing) {
      return;
    }

    const netInfo = await NetInfo.fetch();
    if (!netInfo.isConnected) {
      return;
    }

    this.isProcessing = true;

    try {
      const queue = await this.getQueue();
      
      for (const item of queue) {
        try {
          await this.processQueueItem(item);
          await this.removeFromQueue(item.id);
        } catch (error) {
          console.error('Error processing queue item:', error);
          // Keep item in queue for retry
        }
      }
    } catch (error) {
      console.error('Error processing offline queue:', error);
    } finally {
      this.isProcessing = false;
    }
  }

  static async processQueueItem(item) {
    const { method, endpoint, data, headers } = item;
    
    const response = await fetch(endpoint, {
      method,
      headers: {
        'Content-Type': 'application/json',
        ...headers,
      },
      body: data ? JSON.stringify(data) : undefined,
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    return response.json();
  }

  static initialize() {
    // Listen for network changes
    NetInfo.addEventListener(state => {
      if (state.isConnected) {
        this.processQueue();
      }
    });
  }
}
```

---

## 🎯 **NEXT STEPS**

1. **Master API integration patterns** with proper error handling and caching
2. **Implement authentication flows** including OAuth and biometric authentication
3. **Add real-time features** with WebSockets and push notifications
4. **Set up offline capabilities** with data synchronization
5. **Prepare for app store deployment** with proper build configurations

Remember: Backend integration is crucial for modern mobile apps. Focus on creating robust, scalable solutions that handle network failures gracefully and provide excellent user experiences both online and offline. Always consider security, performance, and user privacy when integrating with backend services.

Test thoroughly on different network conditions and devices before deploying to production. Use analytics and crash reporting to monitor your app's performance in the wild and continuously improve the user experience.