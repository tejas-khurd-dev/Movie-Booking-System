# 🎬 CineVault

A full-stack movie ticket booking platform with a customer-facing frontend, admin panel, and backend API. Browse movies, view showtimes, select seats, and pay for tickets via Razorpay.

## 🛠 Tech Stack

| Layer | Technology |
| --- | --- |
| **Language** | JavaScript (ES Modules) |
| **Backend** | Node.js, Express 5.2 |
| **Database** | MongoDB (Mongoose 9.9) |
| **Frontend** | React 19, Vite 8, Tailwind CSS 4 |
| **Routing** | react-router-dom 7.x |
| **Payment** | Razorpay |
| **Auth** | JWT, bcrypt, Google OAuth |
| **File Uploads** | Multer + ImageKit |
| **Email** | Nodemailer |

## Features

### 🎟 Customer (Client)
- 🎥 Browse movies with posters and details
- 📅 View showtimes by date
- 💺 Interactive seat selection (max 5 per booking)
- 💳 Razorpay payment integration
- 📋 View active and past bookings
- ❤️ Favorite movies list
- 🔐 Google OAuth login

### 🎛 Admin Panel
- 📊 Dashboard with earnings chart (Recharts)
- 🎬 Add/edit/delete movies with poster & backdrop uploads
- 🕐 Add showtimes per movie
- 🎭 Add cast members with photos
- 📑 View all bookings globally
- 💰 View all shows with earnings

## 📁 Project Structure

```
CineVault/
├── admin/                  # Admin panel (React + Vite)
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── hooks/          # Custom React hooks
│   │   ├── pages/          # Route-level components
│   │   ├── services/       # API calls & React contexts
│   │   ├── lib/            # Utility functions
│   │   ├── constants/      # Static config (nav links)
│   │   └── assets/         # Logos, images
│   └── package.json
├── client/                 # Customer frontend (React + Vite)
│   ├── src/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── lib/
│   │   └── assets/
│   └── package.json
├── server/                 # Backend API (Node.js + Express)
│   ├── src/
│   │   ├── config/         # DB & app config
│   │   ├── controllers/    # Route handlers
│   │   ├── middlewares/     # Auth, admin, upload middleware
│   │   ├── models/         # Mongoose schemas
│   │   ├── routes/         # Express routers
│   │   └── services/       # Email service
│   ├── server.js           # Entry point
│   └── package.json
└── LICENSE
```

## 🚀 Getting Started

### 📋 Prerequisites

- Node.js 18+
- MongoDB instance (local or Atlas)
- Razorpay account (test/live keys)
- ImageKit account
- Google Cloud project (for OAuth)

### 📦 Installation

```bash
# Clone the repo
git clone https://github.com/tejas-khurd-dev/CineVault.git
cd CineVault

# Install server dependencies
cd server && npm install

# Install client dependencies
cd ../client && npm install

# Install admin dependencies
cd ../admin && npm install
```

### 🔑 Environment Variables

Create `.env` files in the respective directories:

**server/.env**
```
PORT=5000
MONGO_URI=your_mongodb_uri
JWT_SECRET=your_jwt_secret
EMAIL=your_email@example.com
EMAIL_PASSWORD=your_email_password
GOOGLE_CLIENT_ID=your_google_client_id
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
IMAGEKIT_PUBLIC_KEY=your_imagekit_public_key
IMAGEKIT_PRIVATE_KEY=your_imagekit_private_key
IMAGEKIT_URL_ENDPOINT=your_imagekit_url_endpoint
```

**client/.env**
```
VITE_API_URL=http://localhost:5000/api
VITE_CURRENCY=Rs.
```

**admin/.env**
```
VITE_API_URL=http://localhost:5000/api
VITE_CURRENCY=Rs.
```

### ▶️ Running

```bash
# Start server (from server/)
npm run dev

# Start client (from client/)
npm run dev

# Start admin (from admin/)
npm run dev
```

## 🌐 API Endpoints

All routes are prefixed with `/api`.

| Method | Path | Auth | Description |
| --- | --- | --- | --- |
| **🔐 Auth** | | | |
| POST | `/auth/register` | No | Register a new user |
| POST | `/auth/login` | No | Login with email/password |
| POST | `/auth/google` | No | Google OAuth login |
| GET | `/auth/logout` | Yes | Logout |
| GET | `/auth/get-me` | Yes | Get current user |
| PUT | `/auth/updateUserInfo` | Yes | Update username/profile image |
| **🎬 Movies** | | | |
| POST | `/movie/add` | Admin | Add a new movie |
| GET | `/movie/all` | No | Get all movies |
| GET | `/movie/:movieId` | No | Get movie by ID |
| DELETE | `/movie/:movieId` | Admin | Delete a movie |
| **🎭 Shows** | | | |
| POST | `/show/add/:movieId` | Admin | Add a show to a movie |
| GET | `/show/all` | Admin | Get all shows |
| GET | `/show/:movieId` | No | Get shows for a movie |
| GET | `/show/single/:showId` | No | Get show by ID |
| DELETE | `/show/:showId` | Admin | Delete a show |
| **🎟 Bookings** | | | |
| POST | `/book-show/create` | Yes | Create booking + Razorpay order |
| POST | `/book-show/verify` | Yes | Verify payment & confirm booking |
| GET | `/book-show/my-bookings` | Yes | Get current user's bookings |
| GET | `/book-show/past-bookings` | Yes | Get archived past bookings |
| GET | `/book-show/admin/all` | Admin | Get all paid bookings |
| **🎭 Cast** | | | |
| POST | `/cast/add` | Admin | Add a cast member |
| GET | `/cast/all` | Admin | Get all cast members |
| GET | `/cast/:movieId` | No | Get cast for a movie |
| DELETE | `/cast/:castId` | Admin | Delete a cast member |
| **📊 Dashboard** | | | |
| GET | `/dashboard/stats` | Admin | Get earnings & booking stats |

## 🔄 Booking Flow

1. 📝 **Create Order** - User selects seats, server validates availability, creates a Razorpay order and a `Booking` document with a TTL expiry (show time + 15 min)
2. 💳 **Payment** - User completes payment on the Razorpay checkout
3. ✅ **Verify** - Server verifies the Razorpay HMAC signature, marks booking as paid, updates show seat availability, creates a past booking archive record, and cleans up unpaid bookings
4. ⏰ **Expiry** - MongoDB TTL index auto-removes unpaid bookings after the expiry time

## 📄 License

MIT License - Copyright 2026 tejas-khurd-dev
