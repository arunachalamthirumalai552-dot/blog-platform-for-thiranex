# BlogHub API Documentation

Complete reference for all REST API endpoints with request/response examples.

---

## Base URL
```
http://localhost:5000/api
```

## Authentication
All protected endpoints require a JWT token in the Authorization header:
```
Authorization: Bearer YOUR_JWT_TOKEN
```

---

## Authentication Endpoints

### 1. Register New User
**Create a new user account**

```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response (201):**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Error Responses:**
```json
// 400 - User already exists
{
  "error": "User already exists"
}

// 400 - Missing fields
{
  "error": "All fields required"
}
```

**Notes:**
- Username must be unique
- Email must be unique and valid format
- Password is hashed with bcryptjs (10 salt rounds)
- Returns JWT token for immediate login

---

### 2. Login User
**Authenticate and get JWT token**

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response (200):**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Error Responses:**
```json
// 401 - User not found
{
  "error": "User not found"
}

// 401 - Wrong password
{
  "error": "Invalid password"
}

// 400 - Missing credentials
{
  "error": "Email and password required"
}
```

**Notes:**
- Token expires in session
- Save token to localStorage for persistent sessions
- Use token for all authenticated requests

---

## Posts Endpoints

### 3. Get All Posts
**Fetch all blog posts with author information**

```http
GET /api/posts
```

**Response (200):**
```json
[
  {
    "id": 1,
    "userId": 1,
    "title": "Getting Started with React",
    "content": "React is a JavaScript library for building user interfaces with components...",
    "username": "john_doe",
    "createdAt": "2024-01-15T10:30:00.000Z"
  },
  {
    "id": 2,
    "userId": 2,
    "title": "Understanding Node.js",
    "content": "Node.js is a JavaScript runtime built on Chrome's V8 engine...",
    "username": "jane_smith",
    "createdAt": "2024-01-14T14:45:00.000Z"
  }
]
```

**Notes:**
- No authentication required
- Returns posts ordered by newest first
- Returns empty array if no posts
- Includes author username for each post

---

### 4. Get Single Post with Comments
**Fetch full post details including all comments**

```http
GET /api/posts/1
```

**Response (200):**
```json
{
  "id": 1,
  "userId": 1,
  "title": "Getting Started with React",
  "content": "React is a JavaScript library for building user interfaces with components...",
  "username": "john_doe",
  "createdAt": "2024-01-15T10:30:00.000Z",
  "comments": [
    {
      "id": 5,
      "postId": 1,
      "userId": 2,
      "username": "jane_smith",
      "content": "Great article! Very helpful for beginners.",
      "createdAt": "2024-01-15T11:20:00.000Z"
    },
    {
      "id": 4,
      "postId": 1,
      "userId": 3,
      "username": "bob_wilson",
      "content": "Thanks for sharing this knowledge!",
      "createdAt": "2024-01-15T10:45:00.000Z"
    }
  ]
}
```

**Error Responses:**
```json
// 404 - Post not found
{
  "error": "Post not found"
}
```

**Notes:**
- No authentication required
- Includes all comments for the post
- Comments ordered by newest first
- Returns empty array if no comments

---

### 5. Create New Post
**Create a new blog post (requires authentication)**

```http
POST /api/posts
Content-Type: application/json
Authorization: Bearer YOUR_TOKEN_HERE

{
  "title": "My First Blog Post",
  "content": "This is the content of my first blog post. It can be quite long..."
}
```

**Response (201):**
```json
{
  "id": 3,
  "userId": 1,
  "title": "My First Blog Post",
  "content": "This is the content of my first blog post. It can be quite long...",
  "createdAt": "2024-01-16T09:00:00.000Z"
}
```

**Error Responses:**
```json
// 401 - No token provided
{
  "error": "No token provided"
}

// 403 - Invalid token
{
  "error": "Invalid token"
}

// 400 - Missing fields
{
  "error": "Title and content required"
}

// 500 - Server error
{
  "error": "Internal server error"
}
```

**Notes:**
- Requires valid JWT token
- userId is extracted from token
- Timestamp auto-generated on server
- Returns 201 (Created) on success

---

### 6. Update Post
**Edit an existing post (only author can update)**

```http
PUT /api/posts/1
Content-Type: application/json
Authorization: Bearer YOUR_TOKEN_HERE

{
  "title": "Updated Title",
  "content": "Updated content of the blog post..."
}
```

**Response (200):**
```json
{
  "success": true
}
```

**Error Responses:**
```json
// 403 - Not authorized
{
  "error": "Cannot update post"
}

// 400 - Missing fields
{
  "error": "Title and content required"
}

// 401 - No token
{
  "error": "No token provided"
}
```

**Notes:**
- Only post author can update
- Both title and content required
- createdAt timestamp not changed
- Returns success flag

---

### 7. Delete Post
**Remove a blog post (only author can delete)**

```http
DELETE /api/posts/1
Authorization: Bearer YOUR_TOKEN_HERE
```

**Response (200):**
```json
{
  "success": true
}
```

**Error Responses:**
```json
// 403 - Not authorized
{
  "error": "Cannot delete post"
}

// 401 - No token
{
  "error": "No token provided"
}
```

**Notes:**
- Only post author can delete
- All comments on post are also deleted
- Cannot be undone
- Returns success flag

---

## Comments Endpoints

### 8. Add Comment to Post
**Create a new comment on a blog post**

```http
POST /api/posts/1/comments
Content-Type: application/json
Authorization: Bearer YOUR_TOKEN_HERE

{
  "content": "This is a great article! Thanks for sharing."
}
```

**Response (201):**
```json
{
  "id": 6,
  "postId": 1,
  "userId": 2,
  "username": "jane_smith",
  "content": "This is a great article! Thanks for sharing.",
  "createdAt": "2024-01-16T10:15:00.000Z"
}
```

**Error Responses:**
```json
// 400 - Empty comment
{
  "error": "Comment required"
}

// 401 - No token
{
  "error": "No token provided"
}

// 403 - Invalid token
{
  "error": "Invalid token"
}

// 500 - Server error
{
  "error": "Internal server error"
}
```

**Notes:**
- Requires valid JWT token
- Comment content required (non-empty string)
- userId extracted from token
- Timestamp auto-generated on server
- Only logged-in users can comment

---

### 9. Delete Comment
**Remove a comment (only comment author can delete)**

```http
DELETE /api/comments/6
Authorization: Bearer YOUR_TOKEN_HERE
```

**Response (200):**
```json
{
  "success": true
}
```

**Error Responses:**
```json
// 403 - Not authorized
{
  "error": "Cannot delete comment"
}

// 401 - No token
{
  "error": "No token provided"
}
```

**Notes:**
- Only comment author can delete
- Cannot be undone
- Returns success flag

---

## Error Handling

### Common HTTP Status Codes

| Code | Meaning | Example |
|------|---------|---------|
| 200 | OK | Successful GET or PUT |
| 201 | Created | Successful POST |
| 400 | Bad Request | Missing required fields |
| 401 | Unauthorized | Invalid/missing token |
| 403 | Forbidden | User doesn't own resource |
| 404 | Not Found | Post/comment doesn't exist |
| 500 | Server Error | Database error |

### Error Response Format
```json
{
  "error": "Description of what went wrong"
}
```

---

## Authentication Flow

### 1. Register
```
POST /auth/register
  ↓
Returns { token, id, username }
  ↓
Save token to localStorage
```

### 2. Login
```
POST /auth/login
  ↓
Returns { token, id, username }
  ↓
Save token to localStorage
```

### 3. Use Token
```
All subsequent requests:
Authorization: Bearer {token}
  ↓
Server verifies token
  ↓
Grants access to protected resources
```

### 4. Logout
```
Clear localStorage
  ↓
token is removed
  ↓
Next request without token = 401 error
```

---

## Code Examples

### JavaScript/Fetch

**Register:**
```javascript
const response = await fetch('http://localhost:5000/api/auth/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    username: 'john_doe',
    email: 'john@example.com',
    password: 'password123'
  })
});

const data = await response.json();
localStorage.setItem('token', data.token);
```

**Create Post:**
```javascript
const token = localStorage.getItem('token');

const response = await fetch('http://localhost:5000/api/posts', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({
    title: 'My Post',
    content: 'Post content here'
  })
});

const post = await response.json();
```

**Add Comment:**
```javascript
const token = localStorage.getItem('token');

const response = await fetch('http://localhost:5000/api/posts/1/comments', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({
    content: 'Great post!'
  })
});

const comment = await response.json();
```

### cURL

**Register:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"john_doe","email":"john@example.com","password":"password123"}'
```

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123"}'
```

**Create Post (with token):**
```bash
curl -X POST http://localhost:5000/api/posts \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"title":"My Post","content":"Content here"}'
```

**Get All Posts:**
```bash
curl http://localhost:5000/api/posts
```

**Get Single Post:**
```bash
curl http://localhost:5000/api/posts/1
```

---

## Rate Limiting

Currently not implemented. For production:
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

---

## CORS Configuration

Currently enabled for all origins:
```javascript
app.use(cors());
```

For production, restrict origins:
```javascript
app.use(cors({
  origin: 'http://yourdomain.com',
  credentials: true
}));
```

---

## Database Relationships

```
users (id, username, email, password)
  ↓
posts (id, userId, title, content, createdAt)
  ↓
comments (id, postId, userId, content, createdAt)
```

Foreign key relationships:
- posts.userId → users.id
- comments.postId → posts.id
- comments.userId → users.id

---

## Response Time Expectations

- GET /posts: ~50-100ms
- GET /posts/:id: ~50-100ms
- POST /auth/register: ~100-150ms (password hashing)
- POST /auth/login: ~100-150ms (password comparison)
- POST /posts: ~50-100ms
- POST /comments: ~50-100ms
- DELETE operations: ~50-100ms

*Times vary based on system performance and network*

---

**Happy coding! 🚀**
