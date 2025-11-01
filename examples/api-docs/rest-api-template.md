# RESTful API Documentation - User Authentication

> Complete API reference for user authentication endpoints with examples in multiple languages

**Base URL:** `https://api.example.com/v1`  
**API Version:** 1.0  
**Last Updated:** October 31, 2025

---

## Table of Contents

- [Authentication](#authentication)
- [Endpoints](#endpoints)
  - [POST /auth/register](#post-authregister)
  - [POST /auth/login](#post-authlogin)
  - [POST /auth/refresh](#post-authrefresh)
  - [POST /auth/logout](#post-authlogout)
  - [GET /auth/verify](#get-authverify)
- [Error Codes](#error-codes)
- [Rate Limiting](#rate-limiting)
- [SDKs](#sdks)

---

## Authentication

All authenticated requests require a Bearer token in the Authorization header:

```
Authorization: Bearer YOUR_JWT_TOKEN
```

Tokens expire after 24 hours. Use the refresh token endpoint to obtain a new access token.

---

## Endpoints

### POST /auth/register

Creates a new user account.

**URL:** `POST https://api.example.com/v1/auth/register`

**Authentication:** None required

**Headers:**
```
Content-Type: application/json
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| email | string | Yes | Valid email address |
| password | string | Yes | Minimum 8 characters, must include uppercase, lowercase, and number |
| name | string | Yes | User's full name (2-100 characters) |
| terms_accepted | boolean | Yes | Must be true to indicate acceptance of terms |

**Example Request:**

```json
{
  "email": "john.doe@example.com",
  "password": "SecurePass123!",
  "name": "John Doe",
  "terms_accepted": true
}
```

**Success Response (201 Created):**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_1234567890",
      "email": "john.doe@example.com",
      "name": "John Doe",
      "created_at": "2025-10-31T10:00:00Z",
      "email_verified": false
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "ref_1234567890abcdef",
    "expires_in": 86400
  }
}
```

**Error Responses:**

**400 Bad Request** - Validation errors
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters"
      }
    ]
  }
}
```

**409 Conflict** - Email already exists
```json
{
  "success": false,
  "error": {
    "code": "EMAIL_EXISTS",
    "message": "An account with this email already exists"
  }
}
```

**Code Examples:**

**JavaScript (fetch):**
```javascript
const registerUser = async (email, password, name) => {
  try {
    const response = await fetch('https://api.example.com/v1/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        email,
        password,
        name,
        terms_accepted: true
      })
    });

    if (!response.ok) {
      const error = await response.json();
      throw new Error(error.error.message);
    }

    const data = await response.json();
    
    // Store token for future requests
    localStorage.setItem('token', data.data.token);
    localStorage.setItem('refresh_token', data.data.refresh_token);
    
    return data.data.user;
  } catch (error) {
    console.error('Registration failed:', error);
    throw error;
  }
};

// Usage
registerUser('john@example.com', 'SecurePass123!', 'John Doe')
  .then(user => console.log('User registered:', user))
  .catch(error => console.error('Error:', error));
```

**Python (requests):**
```python
import requests

def register_user(email, password, name):
    """
    Register a new user account
    
    Args:
        email (str): User's email address
        password (str): User's password
        name (str): User's full name
    
    Returns:
        dict: User data and authentication tokens
    
    Raises:
        requests.HTTPError: If registration fails
    """
    url = 'https://api.example.com/v1/auth/register'
    payload = {
        'email': email,
        'password': password,
        'name': name,
        'terms_accepted': True
    }
    
    response = requests.post(url, json=payload)
    
    if response.status_code == 201:
        data = response.json()
        # Store tokens securely
        token = data['data']['token']
        refresh_token = data['data']['refresh_token']
        return data['data']['user']
    else:
        error = response.json()
        raise requests.HTTPError(error['error']['message'])

# Usage
try:
    user = register_user('john@example.com', 'SecurePass123!', 'John Doe')
    print(f"User registered: {user}")
except requests.HTTPError as e:
    print(f"Registration failed: {e}")
```

**cURL:**
```bash
curl -X POST https://api.example.com/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john.doe@example.com",
    "password": "SecurePass123!",
    "name": "John Doe",
    "terms_accepted": true
  }'
```

---

### POST /auth/login

Authenticates a user and returns access tokens.

**URL:** `POST https://api.example.com/v1/auth/login`

**Authentication:** None required

**Headers:**
```
Content-Type: application/json
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| email | string | Yes | User's email address |
| password | string | Yes | User's password |
| remember_me | boolean | No | If true, refresh token expires in 30 days instead of 7 |

**Example Request:**

```json
{
  "email": "john.doe@example.com",
  "password": "SecurePass123!",
  "remember_me": true
}
```

**Success Response (200 OK):**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "usr_1234567890",
      "email": "john.doe@example.com",
      "name": "John Doe",
      "email_verified": true,
      "last_login": "2025-10-31T10:00:00Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "ref_1234567890abcdef",
    "expires_in": 86400
  }
}
```

**Error Responses:**

**401 Unauthorized** - Invalid credentials
```json
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Invalid email or password"
  }
}
```

**423 Locked** - Account locked
```json
{
  "success": false,
  "error": {
    "code": "ACCOUNT_LOCKED",
    "message": "Account has been locked due to multiple failed login attempts",
    "unlock_at": "2025-10-31T11:00:00Z"
  }
}
```

**Code Examples:**

**JavaScript (axios):**
```javascript
import axios from 'axios';

const login = async (email, password, rememberMe = false) => {
  try {
    const response = await axios.post(
      'https://api.example.com/v1/auth/login',
      {
        email,
        password,
        remember_me: rememberMe
      }
    );

    const { token, refresh_token, user } = response.data.data;

    // Store tokens
    localStorage.setItem('token', token);
    localStorage.setItem('refresh_token', refresh_token);

    // Configure axios to include token in future requests
    axios.defaults.headers.common['Authorization'] = `Bearer ${token}`;

    return user;
  } catch (error) {
    if (error.response) {
      // Server responded with error
      throw new Error(error.response.data.error.message);
    }
    throw error;
  }
};

// Usage
login('john@example.com', 'SecurePass123!', true)
  .then(user => console.log('Logged in:', user))
  .catch(error => console.error('Login failed:', error.message));
```

**Node.js (with error handling):**
```javascript
const https = require('https');

function login(email, password) {
  return new Promise((resolve, reject) => {
    const data = JSON.stringify({
      email,
      password,
      remember_me: false
    });

    const options = {
      hostname: 'api.example.com',
      path: '/v1/auth/login',
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Content-Length': data.length
      }
    };

    const req = https.request(options, (res) => {
      let body = '';

      res.on('data', (chunk) => {
        body += chunk;
      });

      res.on('end', () => {
        try {
          const response = JSON.parse(body);
          
          if (res.statusCode === 200) {
            resolve(response.data);
          } else {
            reject(new Error(response.error.message));
          }
        } catch (error) {
          reject(error);
        }
      });
    });

    req.on('error', reject);
    req.write(data);
    req.end();
  });
}

// Usage
login('john@example.com', 'SecurePass123!')
  .then(data => {
    console.log('Login successful');
    console.log('User:', data.user);
    console.log('Token:', data.token);
  })
  .catch(error => {
    console.error('Login failed:', error.message);
  });
```

---

### POST /auth/refresh

Refreshes an expired access token using a refresh token.

**URL:** `POST https://api.example.com/v1/auth/refresh`

**Authentication:** Refresh token required

**Headers:**
```
Content-Type: application/json
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| refresh_token | string | Yes | Valid refresh token |

**Example Request:**

```json
{
  "refresh_token": "ref_1234567890abcdef"
}
```

**Success Response (200 OK):**

```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "ref_0987654321fedcba",
    "expires_in": 86400
  }
}
```

**Error Response (401 Unauthorized):**

```json
{
  "success": false,
  "error": {
    "code": "INVALID_REFRESH_TOKEN",
    "message": "Refresh token is invalid or expired"
  }
}
```

**Code Example:**

```javascript
async function refreshToken(refreshToken) {
  const response = await fetch('https://api.example.com/v1/auth/refresh', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ refresh_token: refreshToken })
  });

  if (!response.ok) {
    // Refresh token expired, redirect to login
    window.location.href = '/login';
    return null;
  }

  const data = await response.json();
  
  // Update stored tokens
  localStorage.setItem('token', data.data.token);
  localStorage.setItem('refresh_token', data.data.refresh_token);
  
  return data.data.token;
}
```

---

### POST /auth/logout

Invalidates the current access token and refresh token.

**URL:** `POST https://api.example.com/v1/auth/logout`

**Authentication:** Bearer token required

**Headers:**
```
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json
```

**Request Body:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| refresh_token | string | No | Refresh token to invalidate (optional) |

**Success Response (200 OK):**

```json
{
  "success": true,
  "message": "Successfully logged out"
}
```

**Code Example:**

```javascript
async function logout() {
  const token = localStorage.getItem('token');
  const refreshToken = localStorage.getItem('refresh_token');

  try {
    await fetch('https://api.example.com/v1/auth/logout', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ refresh_token: refreshToken })
    });
  } finally {
    // Clear local storage regardless of response
    localStorage.removeItem('token');
    localStorage.removeItem('refresh_token');
    window.location.href = '/login';
  }
}
```

---

## Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| VALIDATION_ERROR | 400 | Request validation failed |
| EMAIL_EXISTS | 409 | Email already registered |
| INVALID_CREDENTIALS | 401 | Wrong email or password |
| ACCOUNT_LOCKED | 423 | Too many failed login attempts |
| INVALID_REFRESH_TOKEN | 401 | Refresh token expired or invalid |
| TOKEN_EXPIRED | 401 | Access token has expired |
| UNAUTHORIZED | 401 | Missing or invalid authentication |
| RATE_LIMIT_EXCEEDED | 429 | Too many requests |
| INTERNAL_ERROR | 500 | Server error |

---

## Rate Limiting

API requests are rate-limited to prevent abuse:

- **Anonymous requests:** 100 requests per hour per IP
- **Authenticated requests:** 1000 requests per hour per user

Rate limit information is included in response headers:

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 995
X-RateLimit-Reset: 1635696000
```

When rate limit is exceeded, the API returns:

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again later.",
    "retry_after": 3600
  }
}
```

---

## SDKs

Official SDKs are available for multiple languages:

**JavaScript/TypeScript:**
```bash
npm install @example/api-client
```

**Python:**
```bash
pip install example-api-client
```

**Go:**
```bash
go get github.com/example/api-client-go
```

**Usage Example (JavaScript SDK):**

```javascript
import { AuthClient } from '@example/api-client';

const auth = new AuthClient('https://api.example.com/v1');

// Register
const user = await auth.register({
  email: 'john@example.com',
  password: 'SecurePass123!',
  name: 'John Doe'
});

// Login
const session = await auth.login({
  email: 'john@example.com',
  password: 'SecurePass123!'
});

// SDK automatically handles token storage and refresh
```

---

## Need Help?

- **Documentation:** https://docs.example.com
- **Support:** support@example.com
- **GitHub:** https://github.com/example/api-docs
- **Status Page:** https://status.example.com

---

**Last updated:** October 31, 2025  
**API Version:** 1.0