# BlogHub Setup & Deployment Guide

## Prerequisites

- **Node.js** v14 or higher
- **npm** (comes with Node.js)
- **Git** (optional, for version control)
- **A code editor** (VS Code recommended)
- **2 Terminal windows** (for running server & client)

Check your versions:
```bash
node --version  # Should be v14+
npm --version   # Should be v6+
```

---

## 📋 File-by-File Setup

### Step 1: Create Project Directory
```bash
mkdir blog-platform
cd blog-platform
```

### Step 2: Copy All 6 Files

Create these files in your project directory:

#### File 1: `package.json`
```json
{
  "name": "blog-platform",
  "version": "1.0.0",
  "description": "Full-stack blogging platform",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "client": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  },
  "keywords": ["blog", "react", "express"],
  "author": "",
  "license": "MIT",
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "express": "^4.18.2",
    "jsonwebtoken": "^9.0.0",
    "sqlite3": "^5.1.6",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1"
  }
}
```

#### File 2: `server.js`
[Copy the full server.js content - 200+ lines with Express, SQLite, auth]

#### File 3: `src/App.jsx`
[Copy the full React component - 380+ lines with all views and logic]

#### File 4: `src/App.css`
[Copy the full CSS stylesheet - 400+ lines of styling]

#### File 5: `src/index.jsx`
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

#### File 6: `public/index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BlogHub - Your Blogging Platform</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
```

### Step 3: Install Dependencies
```bash
npm install
```

This will:
- Download all packages from npm registry
- Create `node_modules/` folder
- Generate `package-lock.json`
- Takes ~2-3 minutes (internet dependent)

---

## 🚀 Running the Application

### Terminal 1: Start Backend Server

```bash
npm start
```

Expected output:
```
✓ Server running on http://localhost:5000
```

**What's happening:**
- Node.js starts Express server
- SQLite database initializes
- Tables (users, posts, comments) created
- Server listens on port 5000
- Ready to accept API requests

### Terminal 2: Start Frontend Client

```bash
npm run client
```

Expected output:
```
Compiled successfully!

You can now view blog-platform in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.x.x:3000

Note that the development build is not optimized.
To create a production build, use npm run build.
```

**What's happening:**
- React development server starts
- Webpack compiles JSX to JavaScript
- Hot reload enabled (changes refresh automatically)
- Opens http://localhost:3000 in your browser

---

## 🌐 First Time Using the App

### 1. Register New Account
- Click "Register" link
- Fill in:
  - Username (unique)
  - Email (unique)
  - Password (any string)
- Click "Register"
- You're logged in! ✅

### 2. Create Your First Post
- Click "+ New Post" button
- Enter:
  - Post Title
  - Post Content (multiline)
- Click "Publish Post"
- Redirects to posts feed

### 3. Read Posts & Comments
- Click "Read More" on any post
- Scroll down to see comments
- Add your comment (if logged in)
- Click "Post Comment"

### 4. Manage Your Content
- Only you can delete your posts/comments
- Use the delete buttons next to your content
- Confirm deletion when prompted

---

## 🔧 Common Development Tasks

### Hot Reloading
**Frontend:** Automatic (saves refresh browser)
**Backend:** Requires manual restart or install nodemon:
```bash
npm install --save-dev nodemon
npm run dev  # Uses nodemon for auto-restart
```

### View Database
Install SQLite browser (optional):
```bash
# Mac
brew install sqlite

# Linux
apt-get install sqlite3

# Then view database
sqlite3 blog.db
.tables  # Show all tables
SELECT * FROM users;
SELECT * FROM posts;
SELECT * FROM comments;
.exit
```

### Debug API Calls
Add logging in terminal 2 (client):
```javascript
// In src/App.jsx, add after fetch:
.then(res => {
  console.log('Response:', res);
  return res.json();
})
```

### Check Network Requests
- Open browser DevTools: F12
- Go to Network tab
- Create posts/comments
- See all API requests and responses

### Clear All Data
Option 1: Stop server, delete `blog.db`, restart
Option 2: Clear database from code (add route temporarily)

---

## 🚨 Troubleshooting

### "Port 5000 Already in Use"
```bash
# Kill process on port 5000
lsof -ti:5000 | xargs kill -9

# Or use different port (edit server.js):
const PORT = 5001;
```

### "Port 3000 Already in Use"
```bash
# Kill process on port 3000
lsof -ti:3000 | xargs kill -9

# Or set different port
PORT=3001 npm run client
```

### "Module not found" errors
```bash
# Reinstall all dependencies
rm -rf node_modules package-lock.json
npm install
```

### "Cannot GET /api/posts"
- Ensure backend server is running (terminal 1)
- Check API URL in src/App.jsx: `http://localhost:5000/api`
- Check browser console for errors (F12)

### Login/Registration not working
```bash
# Clear browser storage
localStorage.clear()
sessionStorage.clear()

# Try again
# Check server console for error messages
```

### "CORS error"
- Ensure backend runs on port 5000
- Check `app.use(cors())` is in server.js
- Restart backend server

### Build fails in terminal 2
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
npm run client
```

---

## 📝 Code Customization

### Change App Title
Edit `public/index.html`:
```html
<title>Your Blog Name</title>
```

### Change Color Scheme
Edit `src/App.css`:
```css
/* Change primary color from purple to blue */
.btn-primary {
  background: linear-gradient(135deg, #4169E1 0%, #1E90FF 100%);
}
```

### Persistent Database
Edit `server.js`:
```javascript
// Change from:
const db = new sqlite3.Database(':memory:');
// To:
const db = new sqlite3.Database('./blog.db');
```

### Change Server Port
Edit `server.js`:
```javascript
const PORT = process.env.PORT || 8000;
```

### API Base URL
Edit `src/App.jsx`:
```javascript
const API = 'http://localhost:8000/api';  // Change port
```

---

## 🏗️ Production Deployment

### Heroku Deployment

1. **Install Heroku CLI**
```bash
npm install -g heroku-cli
heroku login
```

2. **Create Heroku App**
```bash
heroku create your-app-name
```

3. **Set Environment Variable**
```bash
heroku config:set JWT_SECRET=your-secret-key
```

4. **Deploy**
```bash
git push heroku main
```

5. **View Live**
```bash
heroku open
```

### DigitalOcean/AWS Deployment

1. **Build React for production**
```bash
npm run build
```

2. **Create start script** that serves both:
```bash
npm install --save-dev concurrently
```

3. **Update package.json scripts**
```json
"scripts": {
  "start": "node server.js",
  "build": "react-scripts build",
  "dev": "concurrently \"npm start\" \"npm run client\"",
  "heroku-postbuild": "npm run build"
}
```

4. **Deploy to your server**
- Push code to Git
- SSH into server
- `npm install && npm start`

---

## 🧪 Testing the API

### Using cURL (command line)

**Register:**
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"john","email":"john@example.com","password":"123456"}'
```

**Login:**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"123456"}'
```

**Create Post (with token):**
```bash
curl -X POST http://localhost:5000/api/posts \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{"title":"My Post","content":"This is great!"}'
```

### Using Postman (GUI)

1. Download [Postman](https://www.postman.com/downloads/)
2. Create new request
3. Set method and URL
4. Add headers: `Content-Type: application/json`
5. Add body: Raw JSON
6. Send request
7. See response in lower panel

---

## 📚 Learning Resources

- [React Documentation](https://react.dev)
- [Express.js Guide](https://expressjs.com)
- [SQLite Tutorial](https://www.sqlite.org/docs.html)
- [JWT Authentication](https://jwt.io)
- [REST API Best Practices](https://restfulapi.net)

---

## 🎯 Next Steps

After getting this running:

1. **Add Features**
   - User profiles
   - Post categories
   - Search functionality
   - Like/upvote system

2. **Improve Database**
   - Switch to PostgreSQL
   - Add indexes for performance
   - Implement pagination

3. **Enhanced Security**
   - Add rate limiting
   - Input sanitization
   - Email verification
   - Password reset flow

4. **Better UX**
   - Markdown editor
   - Image uploads
   - Draft posts
   - Rich text formatting

5. **DevOps**
   - Docker containerization
   - CI/CD pipeline
   - Automated testing
   - Monitoring & logging

---

## 🆘 Need Help?

1. Check error messages in terminal
2. Look at browser console (F12)
3. Check server logs (terminal 1)
4. Review code comments
5. Try clearing cache: `Ctrl+Shift+Delete`
6. Restart both servers

**Enjoy building! 🚀**
