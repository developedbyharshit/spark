# Dating App API - Backend

A comprehensive dating application backend with real-time chat, gamification features, and interactive games between matched couples.

## Features

### Core Functionality
- **Authentication**: JWT-based auth with refresh tokens, password reset with OTP
- **User Profiles**: Extensive profile management with photo uploads to Cloudinary
- **Discovery Feed**: Geospatial search with preferences filtering
- **Matching System**: Swipe-based matching with instant notifications
- **Real-time Chat**: WebSocket-based messaging with compatibility gating
- **Playroom**: Interactive games between matched couples

### Gamified Features
- **Dare Roulette**: Spin wheel, generate dares, submit proof, rate completions
- **Story Builder**: Co-write romantic stories with AI hints and exports
- **Would You Rather**: Sync score calculations based on matching answers
- **Rewards System**: Daily streak, points, and badges
- **Notifications**: Real-time notifications for matches, messages, dares

## Project Structure

```
dating/
├── models/                 # MongoDB schemas
│   ├── User.js
│   ├── Profile.js
│   ├── Swipe.js
│   ├── Match.js
│   ├── Message.js
│   ├── Playroom.js
│   ├── PlayroomSession.js
│   ├── PlayroomScore.js
│   ├── DareRouletteSession.js
│   ├── DareSpin.js
│   ├── DareConsent.js
│   ├── DareCard.js
│   ├── DareCompletion.js
│   ├── StorySession.js
│   ├── StoryEntry.js
│   ├── StoryExport.js
│   ├── WYRSession.js
│   ├── WYRQuestion.js
│   ├── WYRAnswer.js
│   ├── CompatibilityTest.js
│   ├── Notification.js
│   ├── Reward.js
│   ├── NewsFeed.js
│   ├── College.js
│   └── index.js            # Model exports
├── controllers/            # Route handlers (43 endpoints)
│   ├── authController.js   # 6 endpoints
│   ├── profileController.js # 7 endpoints
│   ├── discoveryController.js # 3 endpoints
│   ├── matchController.js   # 6 endpoints
│   ├── playroomController.js # 5 endpoints
│   ├── dareController.js    # 4 endpoints
│   ├── storyController.js   # 4 endpoints
│   ├── wyrController.js     # 3 endpoints
│   ├── gamificationController.js # 5 endpoints
│   └── index.js             # Controller exports
├── routes/
│   └── index.js             # All 43 API routes
├── middleware/
│   ├── authMiddleware.js    # JWT verification
│   └── errorHandler.js      # Error handling
├── app.js                   # Express & Socket.io setup
├── server.js                # Server entry point
├── package.json             # Dependencies
├── .env.example             # Environment variables template
└── README.md                # This file
```

## Installation

1. **Clone and install dependencies**
```bash
npm install
```

2. **Set up environment variables**
```bash
cp .env.example .env
# Update .env with your credentials
```

3. **Configure MongoDB**
```bash
# Local MongoDB (default)
# Or update MONGODB_URI in .env
```

4. **Set up Cloudinary** (optional, for photo uploads)
- Create free account at https://cloudinary.com
- Add credentials to .env

5. **Start the server**
```bash
# Development (with auto-reload)
npm run dev

# Production
npm start
```

## API Endpoints (43 Total)

### Auth (6)
- `POST /api/auth/register` - Register user
- `POST /api/auth/login` - Login and get tokens
- `POST /api/auth/refresh` - Refresh access token
- `POST /api/auth/logout` - Logout
- `POST /api/auth/forgot-password` - Send reset OTP
- `POST /api/auth/reset-password` - Reset password with OTP

### Profile (7)
- `POST /api/profile` - Create profile
- `GET /api/profile/me` - Get own profile
- `PUT /api/profile/me` - Update profile
- `POST /api/profile/ai-assessment` - Submit AI quiz
- `GET /api/profile/:userId` - View user profile
- `POST /api/profile/photos` - Upload photos
- `DELETE /api/profile/photos/:photoId` - Delete photo

### Discovery (3)
- `GET /api/feed` - Get discovery feed
- `POST /api/swipe` - Swipe on user
- `GET /api/colleges/search` - Search colleges

### Matches & Chat (6)
- `GET /api/matches` - List matches
- `GET /api/matches/:matchId` - Get match detail
- `POST /api/matches/:matchId/compatibility-test` - Submit compatibility test
- `GET /api/matches/:matchId/messages` - Get messages (paginated)
- `WS ws://api/chat/:matchId` - Real-time chat
- `DELETE /api/matches/:matchId` - Unmatch

### Playroom (5)
- `GET /api/matches/:matchId/playroom` - Get playroom state
- `POST /api/matches/:matchId/playroom/activate` - Activate playroom
- `POST /api/playroom/:playroomId/session` - Start game session
- `PATCH /api/playroom/:playroomId/session/:sessionId/end` - End session
- `GET /api/playroom/:playroomId/scores` - Get scores

### Dare Roulette (4)
- `POST /api/dare/:drSessionId/spin` - Spin wheel
- `POST /api/dare/spin/:spinId/consent` - Submit consent
- `POST /api/dare/card/:dareCardId/complete` - Complete dare
- `PATCH /api/dare/completion/:completionId/rate` - Rate completion

### Story Builder (4)
- `GET /api/story/:storySessionId` - Get story
- `POST /api/story/:storySessionId/entry` - Add entry
- `POST /api/story/:storySessionId/like/:entryId` - Like entry
- `POST /api/story/:storySessionId/export` - Export as card

### Would You Rather (3)
- `GET /api/wyr/:wyrSessionId` - Get questions
- `POST /api/wyr/question/:questionId/answer` - Submit answer
- `PATCH /api/wyr/answer/:answerId/react` - Add reaction

### Gamification (5)
- `POST /api/rewards/claim` - Claim daily reward
- `GET /api/rewards/me` - Get reward status
- `GET /api/feed/news` - Get news feed
- `GET /api/notifications` - Get notifications
- `PATCH /api/notifications/:notifId/read` - Mark as read
- `PATCH /api/notifications/read-all` - Mark all as read

## Authentication

All protected endpoints require JWT in Authorization header:
```
Authorization: Bearer <access_token>
```

Get tokens from signup/login, refresh expired tokens using refresh_token.

## WebSocket Events

### Chat
- `join:chat` - Join match chat room
- `message:send` - Send message
- `message:received` - Receive message
- `message:read` - Mark message as read

### Story
- `join:story` - Join story room
- `story:entry-added` - New entry added
- `story:entry-liked` - Entry liked

### WYR
- `join:wyr` - Join session
- `wyr:reveal` - Answers revealed

## Error Handling

All errors follow standard HTTP status codes:
- `400` - Bad request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not found
- `409` - Conflict (duplicate)
- `500` - Server error

## Database Models

24 MongoDB models with relationships:
- Users → Profiles, Swipes, Matches
- Matches → Messages, Compatibility Tests, Playrooms
- Playrooms → Sessions (Dare, Story, WYR)
- Game sessions → Completions, Entries, Answers

## Technologies

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB + Mongoose
- **Real-time**: Socket.io
- **Auth**: JWT
- **Cloud**: Cloudinary
- **Rendering**: Puppeteer (story cards)
- **Validation**: bcrypt, express-validator

## Development

```bash
# Run tests
npm test

# Run linter
npm run lint

# Debug mode
DEBUG=* npm run dev
```

## Environment Variables

See `.env.example` - Key variables:
- `MONGODB_URI` - Database connection
- `JWT_SECRET` - Token signing
- `CLOUDINARY_*` - Cloudinary configuration
- `SMTP_*` - Email configuration
- `PORT` - Server port

## Notes

- All timestamps use ISO 8601 format
- Geospatial queries use GeoJSON coordinates
- Photo uploads have 6 photo max per user
- Compatible test required before chat
- Story/dare/WYR sessions bound to playroom matches
- Rewards system tracks streaks (reset after 48h gap)
- Game difficulty scales 1-5
- Playroom spice level 1-5 with unlockable features
