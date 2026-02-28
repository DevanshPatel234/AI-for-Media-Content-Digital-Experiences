# AIC² - AI Content Intelligence Copilot

A full-stack application for generating AI-powered content that learns and adapts to your audience's preferences.

## 📋 Project Overview

AIC² consists of:
- **Backend** (Node.js + Express + MongoDB) - Intelligent content generation and learning
- **Frontend** (React + Vite) - Modern user interface for content creation and management

## 🚀 Quick Start

### Prerequisites
- Node.js 16+
- npm or yarn
- MongoDB Atlas account (for backend)

### Backend Setup

```bash
cd server

# Install dependencies
npm install

# Create .env file with:
# MONGODB_URI=your_mongodb_atlas_uri
# JWT_SECRET=your_jwt_secret
# OPENAI_API_KEY=your_openai_key
# PORT=5000
# NODE_ENV=development

# Start development server
npm run dev
```

Backend runs on **http://localhost:5000**

### Frontend Setup

```bash
cd client

# Install dependencies
npm install

# Start development server
npm run dev
```

Frontend runs on **http://localhost:5173**

## 📁 Project Structure

```
project/
├── server/                 # Backend (Express + MongoDB)
│   ├── config/            # Configuration files
│   ├── controllers/        # Route controllers
│   ├── middleware/         # Express middleware
│   ├── models/             # MongoDB schemas
│   ├── routes/             # API routes
│   ├── services/           # Business logic
│   ├── index.js            # Express app entry point
│   ├── package.json
│   ├── .env                # Environment variables
│   └── .env.example        # Example env file
│
└── client/                 # Frontend (React + Vite)
    ├── src/
    │   ├── pages/         # Page components
    │   ├── components/    # Reusable components
    │   ├── services/      # API integration
    │   ├── styles/        # CSS styling
    │   ├── App.jsx
    │   └── main.jsx
    ├── index.html
    ├── vite.config.js
    ├── package.json
    └── README.md
```

## 🔗 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/profile` - Get user profile (protected)

### Content
- `POST /api/content/generate` - Generate new content
- `GET /api/content/history` - Get content history
- `POST /api/content/:id/engage` - Simulate engagement & trigger learning
- `GET /api/content/analytics/summary` - Get analytics

### Health
- `GET /api/health` - Health check

## 🔐 Authentication Flow

1. **Register** - User creates account with email/password
2. **Login** - User gets JWT token
3. **Store Token** - Token saved in localStorage
4. **Protected Routes** - Token sent with each API request
5. **Token Validation** - Backend verifies token
6. **Logout** - Token cleared from storage

## 💾 Database Schema

### User Model
- `email` - User email (unique)
- `password` - Hashed password
- `preferences` - Learning preferences
  - `favoriteTone` - Best performing tone
  - `bestPlatform` - Best performing platform
  - `toneWeights` - AI learning weights
  - `bestLengthRange` - Optimal post length
  - `avgEngagement` - Average engagement rate

### Content Model
- `userId` - Reference to user
- `topic` - Content topic
- `platform` - Target platform
- `tone` - Content tone
- `text` - Generated content
- `engagement` - Engagement score
- `timestamp` - Creation date

## 🤖 How the Learning System Works

1. **Content Generation** - User requests content with topic, platform, tone
2. **AI Generation** - OpenAI generates optimized content based on user history
3. **Publishing** - Content stored in database
4. **Engagement Simulation** - User simulates engagement metrics
5. **Automatic Learning** - System analyzes performance:
   - Updates tone preferences
   - Tracks platform performance
   - Learns optimal content length
   - Generates recommendations

## 🎨 Frontend Features

- **Home Page** - Landing page with feature overview
- **Register Page** - New user registration
- **Login Page** - User authentication
- **Dashboard** - Protected content management area
  - Generate content with AI
  - View content history
  - Simulate engagement
  - Track analytics
  - View personalized recommendations

## 🛠️ Technology Stack

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose
- **Authentication**: JWT + bcryptjs
- **AI**: OpenAI API
- **Validation**: Custom middleware

### Frontend
- **Framework**: React 18
- **Build Tool**: Vite
- **Routing**: React Router v6
- **HTTP Client**: Axios
- **Styling**: Vanilla CSS with Grid/Flexbox

## 📊 Testing the Full Flow

### Register a New User

```
1. Go to http://localhost:5173
2. Click "Get Started"
3. Enter email and password
4. Click "Register"
```

### Login

```
1. Click "Login here"
2. Enter your email and password
3. Click "Login"
```

### Generate Content

```
1. On Dashboard, enter a topic
2. Select platform (Twitter, LinkedIn, etc.)
3. Select tone (Professional, Casual, etc.)
4. Click "Generate Content"
```

### Simulate Engagement

```
1. Find generated content in history
2. Click "Simulate Engagement"
3. System automatically learns from this
```

### View Analytics

```
1. View your Analytics section
2. See best tone, best platform
3. View engagement trends
4. Check AI recommendations
```

### Logout

```
1. Click "Logout" button
2. Redirected to login page
```

## 🐛 Troubleshooting

### Backend Issues

**MongoDB connection fails**
```
- Verify MONGODB_URI in .env
- Ensure IP is whitelisted in MongoDB Atlas
- Check internet connection
```

**Port 5000 already in use**
```
- Kill process: lsof -i :5000 (macOS/Linux)
- Change PORT in .env
```

**OpenAI API errors**
```
- Verify OPENAI_API_KEY is correct
- Check API quota and billing
- Ensure key has correct permissions
```

### Frontend Issues

**Can't connect to backend**
```
- Ensure backend is running on :5000
- Check CORS is enabled
- Verify network connectivity
```

**Login not working**
```
- Clear localStorage: localStorage.clear()
- Check token is being returned from login
- Verify JWT_SECRET matches between frontend/backend
```

**Page refreshes clear login**
```
- Verify token is saved in localStorage
- Check browser localStorage is enabled
- Look for clear triggers in code
```

## 📚 Documentation

### Backend
- See `server/API_DOCS.md` for detailed API documentation
- See `server/UPGRADE_GUIDE.md` for system architecture
- See `server/VALIDATION_CHECKLIST.md` for deployment steps

### Frontend
- See `client/README.md` for frontend-specific documentation

## 🌐 Environment Variables

### Backend (.env)
```
MONGODB_URI=mongodb+srv://...
JWT_SECRET=your_secret_key
OPENAI_API_KEY=sk-...
PORT=5000
NODE_ENV=development
```

### Frontend
Frontend configuration is in `src/services/api.js`:
```javascript
const API_BASE_URL = 'http://localhost:5000/api';
```

## 📈 Performance

- Lightweight React SPA with Vite
- Optimized MongoDB queries with indexes
- JWT stateless authentication
- CORS enabled for cross-origin requests
- Efficient learning algorithms using exponential moving averages

## 🔒 Security

- Passwords hashed with bcryptjs (10 rounds)
- JWT tokens for stateless auth
- CORS enabled with proper headers
- Sensitive keys in environment variables
- Protected routes on frontend and backend
- Input validation on all endpoints

## 📝 License

MIT License

## 🙋 Support

For issues and questions:
1. Check the troubleshooting section
2. Review API documentation
3. Check browser console for errors
4. Verify environment variables are set correctly

## 🎯 Next Steps

After running locally:

1. **Deploy Backend**
   - Use Heroku, Railway, or similar
   - Update MongoDB Atlas connection
   - Set production environment variables

2. **Deploy Frontend**
   - Run `npm run build` in client folder
   - Deploy to Vercel, Netlify, or similar
   - Update API_BASE_URL to production backend

3. **Scale Features**
   - Add more content generation models
   - Implement real engagement tracking
   - Add advanced analytics
   - Build mobile app

Enjoy using AIC²! 🚀
