# Campus Connect Project Structure

```
📁 campus-connect-project/
├── 📄 package.json             # Project configuration and dependencies
├── 📄 server.js               # Main application entry point
├── 📄 .gitignore             # Git ignore file
│
├── 📁 config/                 # Configuration files
│   └── 📄 database.js        # Database configuration
│
├── 📁 controllers/           # Route controllers
│   └── 📄 authController.js  # Authentication controller
│
├── 📁 middleware/            # Express middleware
│   ├── 📄 auth.js           # Authentication middleware
│   ├── 📄 errorHandler.js   # Error handling middleware
│   ├── 📄 rateLimiter.js    # Rate limiting middleware
│   └── 📄 validateInput.js  # Input validation middleware
│
├── 📁 models/               # Database models
│   └── 📄 User.js          # User model schema
│
├── 📁 routes/              # API routes
│   └── 📄 authRoutes.js   # Authentication routes
│
└── 📁 utils/              # Utility functions
    ├── 📄 generateOTP.js  # OTP generation utilities
    ├── 📄 sendEmail.js    # Email sending utilities
    └── 📄 verifyToken.js  # JWT token verification

```

## Project Overview

Campus Connect is a secure student networking API built with Node.js and Express. It features:

### Core Features
- User Authentication System
- Email Verification with OTP
- Rate Limiting
- JWT Token-based Authentication
- Input Validation
- Error Handling

### Technical Stack
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB
- **Authentication**: JWT (JSON Web Tokens)
- **Email Service**: Integrated email functionality
- **Security Features**: Rate limiting, input validation, secure password handling

### Key Components

1. **Authentication System** (`controllers/authController.js`, `routes/authRoutes.js`)
   - User registration with email verification
   - Login with rate limiting
   - Password reset functionality
   - Session management

2. **Security Middleware** (`middleware/`)
   - Request validation
   - Rate limiting
   - Authentication checks
   - Error handling

3. **Data Models** (`models/`)
   - User schema with secure password handling
   - Account verification system
   - Session management

4. **Utilities** (`utils/`)
   - Email sending functionality
   - OTP generation and verification
   - Token management

### Security Features
- Password hashing
- Rate limiting on sensitive routes
- Input validation and sanitization
- JWT-based authentication
- Secure session handling



# 🧪 Postman Testing Guide - Authentication APIs

## Base URL
```
http://localhost:5000/api/auth
```

---

## 1️⃣ Health Check

**GET** `http://localhost:5000/`

**Expected Response:**
```json
{
  "success": true,
  "message": "🚀 Student Network API is running!",
  "version": "1.0.0",
  "timestamp": "2025-11-03T10:30:00.000Z"
}
```

---

## 2️⃣ Register User

**POST** `/register`

**Headers:**
```
Content-Type: application/json
```

**Body (JSON):**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "rollNumber": "21CS001",
  "password": "Password@123",
  "year": "3",
  "branch": "Computer Science"
}
```

**Expected Response (201):**
```json
{
  "success": true,
  "message": "OTP sent to your email. Please verify to complete registration.",
  "data": {
    "email": "john@example.com",
    "otpExpiresIn": "10 minutes"
  }
}
```

**Check your email for the 6-digit OTP!**

---

## 3️⃣ Verify OTP

**POST** `/verify-otp`

**Body (JSON):**
```json
{
  "email": "john@example.com",
  "otp": "123456"
}
```

**Expected Response (200):**
```json
{
  "success": true,
  "message": "Email verified successfully! Welcome to Student Network.",
  "data": {
    "user": {
      "_id": "...",
      "name": "John Doe",
      "email": "john@example.com",
      "rollNumber": "21CS001",
      "year": "3",
      "branch": "Computer Science",
      "verified": true,
      "isAdmin": false,
      "accountStatus": "active"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**Save this token for authenticated requests!**

---

## 4️⃣ Resend OTP

**POST** `/resend-otp`

**Body (JSON):**
```json
{
  "email": "john@example.com"
}
```

**Expected Response (200):**
```json
{
  "success": true,
  "message": "New OTP sent to your email",
  "data": {
    "email": "john@example.com",
    "otpExpiresIn": "10 minutes"
  }
}
```

---

## 5️⃣ Login

**POST** `/login`

**Body (JSON):**
```json
{
  "rollNumber": "21CS001",
  "password": "Password@123"
}
```

**Expected Response (200):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "_id": "...",
      "name": "John Doe",
      "email": "john@example.com",
      "rollNumber": "21CS001",
      "year": "3",
      "branch": "Computer Science"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

---

## 6️⃣ Get Profile (Protected Route)

**GET** `/profile`

**Headers:**
```
Authorization: Bearer YOUR_JWT_TOKEN_HERE
```

**Expected Response (200):**
```json
{
  "success": true,
  "data": {
    "user": {
      "_id": "...",
      "name": "John Doe",
      "email": "john@example.com",
      "rollNumber": "21CS001",
      "year": "3",
      "branch": "Computer Science",
      "verified": true
    }
  }
}
```

---

## 7️⃣ Forgot Password

**POST** `/forgot-password`

**Body (JSON):**
```json
{
  "email": "john@example.com"
}
```

**Expected Response (200):**
```json
{
  "success": true,
  "message": "Password reset OTP sent to your email",
  "data": {
    "email": "john@example.com",
    "otpExpiresIn": "10 minutes"
  }
}
```

---

## 8️⃣ Reset Password

**POST** `/reset-password`

**Body (JSON):**
```json
{
  "email": "john@example.com",
  "otp": "654321",
  "newPassword": "NewPassword@123"
}
```

**Expected Response (200):**
```json
{
  "success": true,
  "message": "Password reset successful. Please login with your new password."
}
```

---

## 9️⃣ Logout (Protected Route)

**POST** `/logout`

**Headers:**
```
Authorization: Bearer YOUR_JWT_TOKEN_HERE
```

**Expected Response (200):**
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

## 🎯 Testing Workflow

### Complete User Journey:
1. **Register** → Get OTP in email
2. **Verify OTP** → Get JWT token
3. **Login** → Get new token
4. **Get Profile** → View user data
5. **Forgot Password** → Get reset OTP
6. **Reset Password** → Change password
7. **Login** with new password
8. **Logout**

---

## 📋 Error Responses

### Validation Error (400):
```json
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    {
      "field": "email",
      "message": "Please provide a valid email"
    }
  ]
}
```

### Unauthorized (401):
```json
{
  "success": false,
  "message": "Not authorized to access this route. Please login."
}
```

### Rate Limit (429):
```json
{
  "success": false,
  "message": "Too many attempts from this IP. Please try again after 15 minutes."
}
```

---

## 🔧 Postman Environment Variables

Create these variables in Postman:

| Variable | Value |
|----------|-------|
| `base_url` | `http://localhost:5000` |
| `auth_token` | `<empty initially>` |

Then use:
- URL: `{{base_url}}/api/auth/login`
- Header: `Authorization: Bearer {{auth_token}}`

**Auto-save token:** In the "Tests" tab of login/verify-otp requests:
```javascript
if (pm.response.code === 200) {
    const response = pm.response.json();
    pm.environment.set("auth_token", response.data.token);
}
```

---

## ✅ Success Criteria

All 9 APIs should work:
- ✅ Register & send OTP
- ✅ Verify OTP
- ✅ Resend OTP
- ✅ Login
- ✅ Get Profile
- ✅ Logout
- ✅ Forgot Password
- ✅ Reset Password
- ✅ Rate limiting works

---

## 🐛 Common Issues

### 1. Email not sending?
- Check Gmail App Password (not regular password)
- Enable "Less secure app access" OR use App Password
- Verify `.env` email credentials

### 2. MongoDB connection failed?
- Check if MongoDB is running: `mongod` or MongoDB Compass
- Verify `MONGODB_URI` in `.env`

### 3. Token not working?
- Check if token is in cookies OR Authorization header
- Verify `JWT_SECRET` in `.env`

### 4. Rate limit hit?
- Wait for cooldown period
- Or restart server (clears in-memory limits)

---

**🎉 Congratulations! You've successfully built and tested the Authentication System!**