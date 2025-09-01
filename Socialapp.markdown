# Social App -Development Documentation

## 1. Overview

### Project Summary

A full-stack social media application enabling users to connect, share content, and communicate in real-time. The platform provides core social networking features including user profiles, content sharing, engagement mechanisms, and real-time messaging.

### System Objectives

* Provide a seamless social networking experience
* Enable real-time communication between users
* Ensure high performance and scalability
* Maintain robust security and data privacy
* Deliver responsive user experience across devices

### Key Features & Modules

* **User Management**: Registration, authentication, profile management
* **Content System**: Post creation, media uploads, feed management
* **Social Graph**: Following/follower system, relationship management
* **Engagement**: Likes, comments, and interactions
* **Real-time Chat**: Instant messaging with read receipts and typing indicators
* **Notifications**: Real-time updates and activity alerts
* **Administration**: Content moderation and user management
* **Search & Discovery**: User and content discovery features

### Tech Stack Overview

* **Frontend**: React 19, Vite, Tailwind CSS
* **State Management**: Redux Toolkit + React-Redux
* **Backend**: Node.js, Express 5
* **Database**: MongoDB with Mongoose ODM
* **Real-time Communication**: Socket.io
* **Authentication**: JWT (access/refresh tokens)
* **File Processing**: Multer for uploads
---

## 2. Architecture & Directory Structure

### System Architecture Diagram

```
┌─────────────────┐    HTTP/REST     ┌─────────────────┐    Database     ┌─────────────────┐
│   React Client  │◄────────────────►│  Express API    │◄───────────────►│    MongoDB      │
│   (Port 3000)   │                  │  (Port 8080)    │                 │   (Atlas/Local) │
└─────────────────┘                  └─────────────────┘                 └─────────────────┘
         │                                    │
         │           WebSocket                │
         └───────────────────────────────────┘
                    Socket.io
                    (Real-time)
```

### Directory Structure

```
social-app/
├── client/                         
│   ├── src/
│   │   ├── components/
│   │   │   ├── common/
│   │   │   │   ├── Navbar.jsx
│   │   │   │   ├── Sidebar.jsx
│   │   │   │   └── Loader.jsx
│   │   │   ├── forms/
│   │   │   │   ├── LoginForm.jsx
│   │   │   │   ├── SignupForm.jsx
│   │   │   │   └── PostForm.jsx
│   │   │   ├── layout/
│   │   │   │   └── MainLayout.jsx
│   │   │   ├── post/
│   │   │   │   ├── PostCard.jsx
│   │   │   │   ├── PostList.jsx
│   │   │   │   └── CommentCard.jsx
│   │   │   ├── chat/
│   │   │   │   ├── ChatBox.jsx
│   │   │   │   └── MessageBubble.jsx
│   │   │   └── user/
│   │   │       ├── UserCard.jsx
│   │   │       └── ProfileHeader.jsx
│   │   ├── pages/
│   │   │   ├── Auth/
│   │   │   │   ├── Login.jsx
│   │   │   │   └── Signup.jsx
│   │   │   ├── Feed.jsx
│   │   │   ├── Profile.jsx
│   │   │   ├── Chat.jsx
│   │   │   ├── Search.jsx
│   │   │   └── Settings.jsx
│   │   ├── hooks/
│   │   │   ├── useAuth.js
│   │   │   ├── useSocket.js
│   │   │   └── useForm.js
│   │   ├── store/
│   │   │   ├── index.js
│   │   │   ├── authSlice.js
│   │   │   ├── postSlice.js
│   │   │   ├── chatSlice.js
│   │   │   └── notificationSlice.js
│   │   ├── services/
│   │   │   ├── api.js
│   │   │   ├── authService.js
│   │   │   ├── postService.js
│   │   │   ├── userService.js
│   │   │   └── chatService.js
│   │   ├── utils/
│   │   │   ├── validators.js
│   │   │   ├── formatDate.js
│   │   │   ├── constants.js
│   │   │   └── storage.js
│   │   ├── assets/
│   │   │   ├── images/
│   │   │   └── styles/
│   │   │       └── global.css
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── routes.js
│   ├── public/
│   │   ├── index.html
│   │   ├── manifest.json
│   │   └── robots.txt
│   ├── package.json
│   └── vite.config.js
├── server/                         
│   ├── src/
│   │   ├── config/
│   │   │   ├── db.js
│   │   │   └── env.js
│   │   ├── models/
│   │   │   ├── User.js
│   │   │   ├── Post.js
│   │   │   ├── Comment.js
│   │   │   ├── Message.js
│   │   │   └── Notification.js
│   │   ├── controllers/
│   │   │   ├── authController.js
│   │   │   ├── userController.js
│   │   │   ├── postController.js
│   │   │   ├── commentController.js
│   │   │   ├── chatController.js
│   │   │   └── notificationController.js
│   │   ├── routes/
│   │   │   ├── authRoutes.js
│   │   │   ├── userRoutes.js
│   │   │   ├── postRoutes.js
│   │   │   ├── commentRoutes.js
│   │   │   ├── chatRoutes.js
│   │   │   └── notificationRoutes.js
│   │   ├── middleware/
│   │   │   ├── authMiddleware.js
│   │   │   ├── errorMiddleware.js
│   │   │   └── validateMiddleware.js
│   │   ├── utils/
│   │   │   ├── token.js
│   │   │   ├── email.js
│   │   │   └── socket.js
│   │   ├── app.js
│   │   └── server.js
│   ├── uploads/
│   ├── package.json
│   └── .env
├── shared/
│   ├── constants/
│   └── package.json
├── docs/
│   ├── architecture.md
│   ├── api-reference.md
│   └── database-schema.md
├── README.md
└── .gitignore
```

---

## 3. Database Design

### ER Diagram (Entities & Relationships)

```
    User
     │
     │ 1     *
     │╱─────Post
     │       │
     │       │ 1     *
     │       │╱─────Comment
     │       │
     │       │ 1     *
     │       │╱─────Like
     │
     │ *     *
     │╱─────╱│
     Follow  │
     │       │
     │       │ *     *
     │       │╱─────╱│
     │    Conversation
     │       │
     │       │ 1     *
     │       │╱─────Message
     │
     │ 1     *
     │╱─────Notification
```

### Database Schema

#### User Collection

```javascript
{
  _id: ObjectId,
  handle: String,           // Unique, indexed
  name: String,
  email: String,            // Unique, indexed
  avatarUrl: String,
  bio: String,              // Max 280 characters
  role: String,             // 'user' or 'admin'
  passwordHash: String,
  stats: {
    posts: Number,
    followers: Number,
    following: Number,
    likes: Number
  },
  createdAt: Date,          // Indexed
  updatedAt: Date           // Indexed
}
```

#### Post Collection

```javascript
{
  _id: ObjectId,
  userId: ObjectId,        // Ref User, indexed
  text: String,            // Max 2000 characters
  media: [{
    url: String,
    type: String,          // 'image' or 'video'
    width: Number,
    height: Number
  }],
  likesCount: Number,      // Default: 0, indexed
  commentsCount: Number,   // Default: 0
  visibility: String,      // 'public' or 'followers'
  createdAt: Date,         // Indexed
  updatedAt: Date
}
```

#### Comment Collection

```javascript
{
  _id: ObjectId,
  postId: ObjectId,        // Ref Post, indexed
  userId: ObjectId,        // Ref User, indexed
  parentId: ObjectId,      // Ref Comment (for replies)
  text: String,            // Max 1000 characters
  createdAt: Date,         // Indexed
  updatedAt: Date
}
```

#### Message Collection

```javascript
{
  _id: ObjectId,
  conversationId: ObjectId, // Ref Conversation, indexed
  senderId: ObjectId,       // Ref User, indexed
  text: String,             // Max 4000 characters
  media: [{
    url: String,
    type: String,           // 'image' or 'video'
    width: Number,
    height: Number
  }],
  readBy: [ObjectId],       // Array of User refs
  createdAt: Date           // Indexed
}
```

---

## 4. API Design

### Authentication Endpoints

* `POST /api/v1/auth/signup` - User registration
* `POST /api/v1/auth/login` - User login
* `POST /api/v1/auth/refresh` - Refresh access token
* `POST /api/v1/auth/logout` - User logout
* `GET /api/v1/auth/me` - Get current user

### User Endpoints

* `GET /api/v1/users/:id` - Get user profile
* `GET /api/v1/users` - Search users
* `PATCH /api/v1/users/:id` - Update user profile
* `POST /api/v1/users/:id/follow` - Follow user
* `DELETE /api/v1/users/:id/follow` - Unfollow user

### Post Endpoints

* `GET /api/v1/posts/feed` - Get user feed
* `GET /api/v1/posts` - Get posts with filters
* `POST /api/v1/posts` - Create new post
* `POST /api/v1/posts/:id/like` - Like/unlike post

### Chat Endpoints

* `GET /api/v1/chat/conversations` - Get user conversations
* `POST /api/v1/chat/conversations` - Create conversation
* `POST /api/v1/chat/conversations/:id/messages` - Send message

---

## Redux State Management Structure

```javascript
{
  auth: {
    user: null,
    token: null,
    isAuthenticated: false,
    loading: false,
    error: null
  },
  posts: {
    feed: [],
    userPosts: [],
    loading: false,
    error: null,
    pagination: {
      page: 1,
      hasMore: true
    }
  },
  users: {
    currentProfile: null,
    searchResults: [],
    followers: [],
    following: [],
    loading: false,
    error: null
  },
  chat: {
    conversations: [],
    activeConversation: null,
    messages: [],
    typing: {},
    loading: false,
    error: null
  },
  notifications: {
    items: [],
    unreadCount: 0,
    loading: false,
    error: null
  }
}
```
