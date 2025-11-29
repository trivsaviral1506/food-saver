# FoodSaver Backend API

Complete Node.js/Express backend for the FoodSaver food waste reduction platform.

## 🚀 Features

- **Authentication**: JWT, Google OAuth, GitHub OAuth
- **Food Listings**: CRUD operations with image upload to Cloudinary
- **Claims System**: QR code-based food claiming and verification
- **Real-time Notifications**: Socket.io for live updates
- **Geolocation**: MongoDB geospatial queries for nearby food
- **Image Management**: Cloudinary integration

## 📋 Prerequisites

- Node.js 16+
- MongoDB (local or Atlas)
- Cloudinary account
- Google OAuth credentials (optional)
- GitHub OAuth credentials (optional)

## 🛠️ Setup

### 1. Install Dependencies

```bash
npm install
```

### 2. Environment Variables

Copy `config.example.env` to `.env` and fill in your credentials:

```bash
cp config.example.env .env
```

**Required variables:**
- `MONGODB_URI` - Your MongoDB connection string
- `JWT_SECRET` - Random secret key for JWT tokens
- `CLOUDINARY_*` - Your Cloudinary credentials

**Optional (for OAuth):**
- `GOOGLE_CLIENT_ID` & `GOOGLE_CLIENT_SECRET`
- `GITHUB_CLIENT_ID` & `GITHUB_CLIENT_SECRET`

### 3. Start Server

```bash
# Development
npm run dev

# Production
npm start
```

Server runs on `http://localhost:5000`

## 📚 API Endpoints

### Authentication (`/api/auth`)
- `POST /register` - Register new user
- `POST /login` - Login with email/password
- `GET /me` - Get current user (protected)
- `PUT /profile` - Update profile (protected)
- `GET /google` - Google OAuth login
- `GET /github` - GitHub OAuth login

### Food Listings (`/api/listings`)
- `GET /` - Get all listings (with filters)
- `GET /:id` - Get single listing
- `POST /` - Create listing (protected, with images)
- `PUT /:id` - Update listing (protected)
- `DELETE /:id` - Delete listing (protected)
- `GET /my/listings` - Get my listings (protected)

### Claims (`/api/claims`)
- `POST /` - Create claim (protected)
- `GET /my-claims` - Get my claims as receiver (protected)
- `GET /received` - Get claims on my listings as donor (protected)
- `GET /:id` - Get claim details (protected)
- `PUT /:id/confirm` - Confirm claim (donor, protected)
- `POST /:id/verify` - Verify pickup with code (donor, protected)
- `PUT /:id/cancel` - Cancel claim (protected)
- `POST /:id/rate` - Rate completed claim (protected)

### Notifications (`/api/notifications`)
- `GET /` - Get notifications (protected)
- `GET /unread-count` - Get unread count (protected)
- `PUT /:id/read` - Mark as read (protected)
- `PUT /read-all` - Mark all as read (protected)
- `DELETE /:id` - Delete notification (protected)

## 🔌 Socket.io Events

### Client → Server
- `join-room` - Join user's notification room

### Server → Client
- `new-listing` - New food listing posted nearby
- `new-claim` - Someone claimed your listing
- `claim-confirmed` - Your claim was confirmed
- `claim-completed` - Pickup was verified
- `claim-cancelled` - Claim was cancelled

## 📦 Project Structure

```
server/
├── config/
│   ├── database.js         # MongoDB connection
│   ├── cloudinary.js       # Cloudinary setup
│   └── passport.js         # OAuth strategies
├── controllers/
│   ├── authController.js   # Auth logic
│   ├── foodListingController.js
│   ├── claimController.js
│   └── notificationController.js
├── middleware/
│   ├── auth.js             # JWT protection
│   ├── upload.js           # Multer config
│   └── error.js            # Error handlers
├── models/
│   ├── User.js
│   ├── FoodListing.js
│   ├── Claim.js
│   └── Notification.js
├── routes/
│   ├── authRoutes.js
│   ├── foodListingRoutes.js
│   ├── claimRoutes.js
│   └── notificationRoutes.js
├── utils/
│   ├── qrcode.js           # QR code generation
│   └── cloudinaryUpload.js # Image upload helpers
├── index.js                # Main server file
└── package.json
```

## 🧪 Testing API

Use tools like Postman or Thunder Client:

1. **Register**: `POST /api/auth/register`
   ```json
   {
     "name": "John Doe",
     "email": "john@example.com",
     "password": "password123",
     "role": "both"
   }
   ```

2. **Login**: `POST /api/auth/login`
   ```json
   {
     "email": "john@example.com",
     "password": "password123"
   }
   ```
   Save the returned `token`.

3. **Create Listing**: `POST /api/listings`
   - Add `Authorization: Bearer YOUR_TOKEN` header
   - Use form-data with images

## 🔐 Security

- Passwords hashed with bcrypt
- JWT tokens for stateless auth
- Protected routes with middleware
- Input validation
- CORS configured

## 📝 Notes

- Images are auto-optimized by Cloudinary
- Listings auto-expire based on `expiresAt`
- Notifications auto-delete after 30 days
- Geospatial queries use MongoDB 2dsphere indexes

## 🤝 Contributing

This is the backend for FoodSaver. The frontend is in the `../client` directory.

