# BlogHub - Full-Stack Blogging Platform
## Complete 6-File Project Structure

```
📦 blog-platform/
│
├── 📄 server.js (220 lines)
│   └── Express backend server with:
│       • SQLite database (users, posts, comments)
│       • JWT authentication (register/login)
│       • RESTful API endpoints
│       • Password hashing with bcryptjs
│       • Authorization middleware
│       • CORS enabled
│
├── 📄 package.json
│   └── Project dependencies:
│       • express - web framework
│       • sqlite3 - database
│       • jsonwebtoken - JWT auth
│       • bcryptjs - password hashing
│       • cors - cross-origin requests
│       • react & react-dom
│       • react-scripts - development tools
│
├── 📁 src/
│   │
│   ├── 📄 App.jsx (380 lines)
│   │   └── Main React component with:
│   │       • User authentication (register/login)
│   │       • Posts CRUD operations
│   │       • Comments creation & deletion
│   │       • State management with hooks
│   │       • localStorage for session persistence
│   │       • API integration
│   │
│   ├── 📄 App.css (400 lines)
│   │   └── Complete styling:
│   │       • Gradient header (purple/blue theme)
│   │       • Responsive grid layout
│   │       • Card-based design
│   │       • Form styling
│   │       • Animations & transitions
│   │       • Mobile responsive (768px, 480px breakpoints)
│   │
│   └── 📄 index.jsx (15 lines)
│       └── React entry point
│           • ReactDOM.createRoot
│           • Renders App component
│
└── 📁 public/
    └── 📄 index.html (10 lines)
        └── HTML entry point
            • Root div for React
            • Meta tags

```

## 🎯 Key Features Implemented

### ✅ Authentication System
- User registration with email validation
- Secure login with JWT tokens
- Password hashing with bcryptjs (10 salt rounds)
- Persistent sessions using localStorage
- Logout functionality

### ✅ Blog Posts Management
- Create new posts (title + content)
- View all posts in grid layout
- Read full post with comments
- Edit own posts
- Delete own posts
- Author attribution with usernames
- Timestamps for all posts

### ✅ Comment System
- Add comments to any post
- Display comments with author and date
- Delete own comments
- Real-time comment updates
- Nested structure (post → comments)

### ✅ Authorization & Security
- JWT-based authentication
- Users can only modify their own content
- Server-side authorization checks
- Input validation on all endpoints
- Secure password storage

## 📊 Database Schema

```sql
-- Users Table
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  username TEXT UNIQUE,
  email TEXT UNIQUE,
  password TEXT  -- bcrypt hashed
)

-- Posts Table
CREATE TABLE posts (
  id INTEGER PRIMARY KEY,
  userId INTEGER (FK to users),
  title TEXT,
  content TEXT,
  createdAt DATETIME DEFAULT CURRENT_TIMESTAMP
)

-- Comments Table
CREATE TABLE comments (
  id INTEGER PRIMARY KEY,
  postId INTEGER (FK to posts),
  userId INTEGER (FK to users),
  content TEXT,
  createdAt DATETIME DEFAULT CURRENT_TIMESTAMP
)
```

## 🚀 API Endpoints (15 Total)

### Authentication (2)
- `POST /api/auth/register` - Create account
- `POST /api/auth/login` - Login to account

### Posts (5)
- `GET /api/posts` - List all posts
- `GET /api/posts/:id` - Get single post with comments
- `POST /api/posts` - Create new post [AUTH]
- `PUT /api/posts/:id` - Update post [AUTH]
- `DELETE /api/posts/:id` - Delete post [AUTH]

### Comments (2)
- `POST /api/posts/:postId/comments` - Add comment [AUTH]
- `DELETE /api/comments/:id` - Delete comment [AUTH]

[AUTH] = Requires JWT authentication token

## 🔧 Technologies & Libraries

### Backend
- **Express.js** - Web server framework
- **SQLite3** - Embedded relational database
- **jsonwebtoken** - JWT authentication
- **bcryptjs** - Password hashing
- **CORS** - Cross-origin resource sharing
- **Node.js** - JavaScript runtime

### Frontend
- **React 18** - UI component library
- **CSS3** - Modern styling with flexbox/grid
- **Fetch API** - HTTP client
- **localStorage** - Browser storage

## 📱 UI Components & Views

### Authentication View
- Email/password input fields
- Toggle between login/register
- Error handling

### Posts Feed View
- Grid layout of post cards
- Author name and date
- Post excerpt (first 100 chars)
- "Read More" button
- Delete button (own posts only)
- Create new post button

### Post Detail View
- Full post content
- Author and timestamp
- Comments section
- Comment form (authenticated users)
- Delete post button (own posts only)

### Comments Display
- Comment author and date
- Full comment text
- Delete button (own comments only)
- Newest comments first

## 🎨 Design Features

✨ **Gradient Header** - Purple/blue gradient background
✨ **Card Layout** - Hover effects with elevation
✨ **Color Scheme** - Accessible, modern purple/gray palette
✨ **Responsive Grid** - Auto-adjusting columns (300px min)
✨ **Mobile Breakpoints** - Optimized for 768px and 480px
✨ **Smooth Transitions** - 0.3s ease animations
✨ **Form Styling** - Clear focus states, validation feedback

## 🔒 Security Implementation

1. **Password Security**
   - bcryptjs hashing with 10 salt rounds
   - Never store plain text passwords

2. **Authentication**
   - JWT tokens with secret key
   - Bearer token in Authorization header
   - Token verification middleware

3. **Authorization**
   - Only authenticated users can create posts/comments
   - Users can only modify their own content
   - Server validates ownership on updates/deletes

4. **Input Validation**
   - Required field checks
   - Email format validation
   - Content length validation

## 🚀 Getting Started

```bash
# 1. Install dependencies
npm install

# 2. Terminal 1: Start backend (port 5000)
npm start

# 3. Terminal 2: Start frontend (port 3000)
npm run client

# 4. Open http://localhost:3000 in browser
```

## 💾 Data Storage

**Frontend:** localStorage for JWT token and user info
**Backend:** SQLite database file (blog.db) for all persistent data
**Session:** In-memory during development (can switch to persistent DB)

## ⚡ Performance Considerations

- Lightweight SQLite database
- Minimal dependencies (8 npm packages)
- Efficient query structure with JOINs
- Frontend caching of posts data
- Lazy loading not required (small dataset)

## 📈 Scalability Notes

For production deployment:
- Replace SQLite with PostgreSQL/MySQL
- Add connection pooling
- Implement pagination for posts
- Add caching layer (Redis)
- Use environment variables for secrets
- Add input sanitization/validation library
- Implement rate limiting
- Add logging and monitoring

## 🎓 Learning Outcomes

After building this project, you'll understand:

✅ Full-stack web application architecture
✅ RESTful API design patterns
✅ JWT authentication and authorization
✅ Database design with relationships
✅ React component lifecycle and hooks
✅ Form handling and validation
✅ Real-time state management
✅ Password security best practices
✅ CORS and security concerns
✅ Responsive web design
✅ Error handling patterns
✅ HTTP client libraries

---

**Total Code: ~1000 lines across 6 files**
**Development time: 4-6 hours**
**Difficulty: Intermediate Full-Stack**
