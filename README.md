<div align="center">

<img src="https://img.shields.io/badge/Production-Live-brightgreen?style=for-the-badge&logo=checkmarx" />
<img src="https://img.shields.io/badge/Node.js-18.x-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
<img src="https://img.shields.io/badge/Express.js-4.x-000000?style=for-the-badge&logo=express&logoColor=white" />
<img src="https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
<img src="https://img.shields.io/badge/Socket.IO-4.x-010101?style=for-the-badge&logo=socketdotio&logoColor=white" />
<img src="https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white" />

# 💍 MarriageSaga — Matrimonial Platform Backend

### A production-grade REST API & real-time backend powering a full matrimonial application

🌐 **Live Platform**: [www.marriagesaga.com](https://www.marriagesaga.com)

</div>

---

## 📖 Table of Contents

- [About the Project](#-about-the-project)
- [Live Product](#-live-product)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Core Features](#-core-features)
- [API Reference](#-api-reference)
- [Database Schema](#-database-schema)
- [Real-Time Chat Flow](#-real-time-chat-flow)
- [Payment Flow](#-payment-flow)
- [Environment Variables](#-environment-variables)
- [Getting Started](#-getting-started)

---

## 🧠 About the Project

MarriageSaga is a **production-deployed matrimonial platform** with thousands of active user profiles. This repository contains the complete backend service — handling everything from secure user onboarding to real-time video calls and astrology-based horoscope matching.

> This is not a tutorial project. It is a live, monetized product used by real users.

**Problems this backend solves:**

- 🔐 Secure user onboarding via Mobile & Email OTP verification
- 💞 Intelligent matchmaking based on user preferences, religion, height, education, and horoscope
- 💬 Real-time one-to-one messaging and customer support chat
- 📹 In-app video calling with Twilio WebRTC token generation
- 💳 Full subscription/payment lifecycle via Razorpay
- 🔔 Offline push notifications via Firebase Cloud Messaging (FCM)

---

## 🌐 Live Product

The backend powers the live matrimonial platform at:

**➡️ [https://www.marriagesaga.com](https://www.marriagesaga.com)**

| Feature | Status |
|---|---|
| User Registration & Login | ✅ Live |
| Profile Matching Engine | ✅ Live |
| Real-time Chat | ✅ Live |
| Video Calling | ✅ Live |
| Subscription Plans | ✅ Live |
| Horoscope Matching | ✅ Live |

---

## 🛠 Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Runtime** | Node.js 18.x | Server-side JavaScript engine |
| **Framework** | Express.js 4.x | RESTful API routing & middleware |
| **Database** | MySQL 8.0 | Primary relational data store |
| **Query Builder** | node-querybuilder | SQL abstraction & query composition |
| **Real-Time** | Socket.IO 4.x | Bidirectional WebSocket communication |
| **Auth** | JWT + bcryptjs | Token-based auth & password hashing |
| **Payments** | Razorpay Node SDK | Order creation & signature verification |
| **Video** | Twilio Video | WebRTC token generation for video calls |
| **Push Notifications** | Firebase Admin (FCM) | Offline user push alerts |
| **Media Storage** | Multer + Cloudinary/AWS S3 | Profile pictures & chat media uploads |
| **Validation** | express-validator | Request input validation |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Mobile / Web Clients                     │
└───────────────────┬─────────────────────┬───────────────────┘
                    │  REST HTTP           │  WebSocket
                    ▼                      ▼
        ┌───────────────────┐   ┌─────────────────────┐
        │  Express API      │   │   Socket.IO Server  │
        │  Server :3000     │   │   Port :7198        │
        └────────┬──────────┘   └──────────┬──────────┘
                 │                          │
        ┌────────▼──────────────────────────▼──────────┐
        │           Controllers & Services              │
        │  (Auth, Profile, Match, Chat, Subscription)  │
        └──┬──────────┬──────────┬──────────┬──────────┘
           │          │          │          │
     ┌─────▼──┐  ┌────▼───┐ ┌───▼────┐ ┌──▼──────────┐
     │ MySQL  │  │Twilio  │ │Razorpay│ │ Firebase    │
     │   DB   │  │ Video  │ │  Pay   │ │   FCM       │
     └────────┘  └────────┘ └────────┘ └─────────────┘
                                │
                        ┌───────▼────────┐
                        │  Cloudinary /  │
                        │    AWS S3      │
                        └────────────────┘
```

---

## ✨ Core Features

### 🔐 Auth & Onboarding
Secure entry into the platform via dual OTP verification (Mobile + Email), plus social login support.

- Mobile OTP send & verify
- Email OTP send & verify
- Native registration / login with bcrypt password hashing
- JWT access token issuance
- Social account login support

### 👤 Profile Management
Rich user profiles covering personal, educational, and professional dimensions, with media gallery support.

- Multi-part form upload for profile picture & photo gallery
- Separate update routes for educational & professional details
- Profile completeness tracking

### 💞 Match Preferences & Discovery
The core matchmaking engine. Stores preference criteria and computes overlapping compatibility across the user base.

- Store preferences: age range, height, religion, education, profession
- Smart query engine to surface compatible profiles
- Like / Unlike / Add to Favourites actions

### 🔯 Horoscope / Kundli Matching
Astrology-based compatibility scoring — a culturally essential feature for the Indian matrimonial market.

- Store birth time, date, and birth place (nakshatra)
- Compare two horoscopes and return a compatibility score

### 💬 Real-Time Chat System
Bidirectional live messaging between matched users, plus a dedicated user-to-support chat channel.

- Connects over Socket.IO on port `7198`
- In-memory active user map keyed by `user_id` → `socketId`
- User-to-User chat stored in `m_chats`
- User-to-Support chat stored in `m_support_chats`
- Auto read-receipt update on message history fetch
- **FCM fallback** if the receiver is offline

### 📹 Video Calling
Secure, in-app video conversations using Twilio's WebRTC infrastructure.

- Generates Twilio access tokens per session
- Tracks call logs (duration, status)
- Handles disconnect/end-call state management

### 💳 Subscriptions & Payments
End-to-end subscription lifecycle: plan discovery → order creation → payment → webhook verification.

- Fetch subscription plans
- Create Razorpay Order ID server-side (never client-side)
- Verify Razorpay webhook HMAC-sha256 signature
- Unlock premium features post-payment confirmation

---

## 📡 API Reference

### Authentication
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/user/register` | Register new user |
| `POST` | `/api/v1/user/login` | Login & receive JWT |
| `POST` | `/api/v1/user/sendOtpMobile` | Send mobile OTP |
| `POST` | `/api/v1/user/verifyOtpMobile` | Verify mobile OTP |
| `POST` | `/api/v1/user/sendOtpEmail` | Send email OTP |
| `POST` | `/api/v1/user/verifyOtpEmail` | Verify email OTP |

### Profile
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/user/createProfile` | Create user profile |
| `PUT` | `/api/v1/user/updateProfileGallary` | Upload gallery images |
| `PUT` | `/api/v1/user/updateEducationProfile` | Update education details |
| `PUT` | `/api/v1/user/updateProfessionalProfile` | Update professional details |
| `GET` | `/api/v1/user/getUserProfile/:id` | Fetch profile by ID |

### Matchmaking
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/user/addMatchPreferences` | Save match criteria |
| `GET` | `/api/v1/user/userMatching` | Fetch matching profiles |
| `POST` | `/api/v1/user/likeProfile` | Like a profile |
| `POST` | `/api/v1/user/addToFavourites` | Add profile to favourites |
| `GET` | `/api/v1/user/matchHoroscope` | Compare horoscopes |

### Video & Payments
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/v1/user/generateVideoToken` | Get Twilio video token |
| `GET` | `/api/v1/subscription/getSubscriptionList` | List plans |
| `POST` | `/api/v1/subscription/getOrderid` | Create Razorpay order |
| `POST` | `/api/v1/payment/verify` | Verify payment webhook |

### Socket.IO Events
| Event | Direction | Payload |
|---|---|---|
| `addUser` | Client → Server | `{ user_id, user_type }` |
| `sendMessage` | Client → Server | `{ receiver_id, message, media }` |
| `sendMessage` | Server → Client | `{ sender_id, message, media, created_on }` |
| `getChatMessages` | Client → Server | `{ user_id, receiver_id }` |
| `disconnect` | Auto | Removes user from active map |

---

## 🗄 Database Schema

### `m_users` — Core Auth Table
```sql
CREATE TABLE m_users (
  user_id       INT PRIMARY KEY AUTO_INCREMENT,
  email         VARCHAR(255) UNIQUE,
  phone         VARCHAR(20) UNIQUE,
  password      VARCHAR(255),          -- bcrypt hash
  deviceType    VARCHAR(50),
  deviceId      VARCHAR(255),          -- FCM token
  chat_with_user_id INT,
  chat_user_type    VARCHAR(50),
  status        ENUM('active','inactive','banned'),
  created_at    DATETIME DEFAULT NOW()
);
```

### `m_userProfile` — Profile Details (1:1 with m_users)
```sql
CREATE TABLE m_userProfile (
  userProfile_id  INT PRIMARY KEY AUTO_INCREMENT,
  user_id         INT REFERENCES m_users(user_id),
  fullName        VARCHAR(255),
  lastName        VARCHAR(255),
  profilePic      VARCHAR(500),
  gender          ENUM('Male','Female','Other'),
  dob             DATE,
  religion        VARCHAR(100),
  height          VARCHAR(20),
  education       VARCHAR(255),
  profession      VARCHAR(255)
);
```

### `m_chats` — User-to-User Messages
```sql
CREATE TABLE m_chats (
  chat_id           INT PRIMARY KEY AUTO_INCREMENT,
  sender_id         INT REFERENCES m_users(user_id),
  receiver_id       INT REFERENCES m_users(user_id),
  message           TEXT,
  media             VARCHAR(500),
  media_type        VARCHAR(50),
  read_status       ENUM('read','unread') DEFAULT 'unread',
  delete_by_sender  ENUM('yes','no') DEFAULT 'no',
  delete_by_receiver ENUM('yes','no') DEFAULT 'no',
  chat_status       ENUM('active','deleted'),
  created_on        DATETIME DEFAULT NOW()
);
```

### `m_support_chats` — User-to-Support Messages
```sql
CREATE TABLE m_support_chats (
  support_chat_id INT PRIMARY KEY AUTO_INCREMENT,
  sender_id       INT REFERENCES m_users(user_id),
  receiver_id     INT,
  sender_type     ENUM('Support','User'),
  message         TEXT,
  media           VARCHAR(500),
  ticketNo        VARCHAR(100),
  read_status     ENUM('read','unread') DEFAULT 'unread',
  chat_status     ENUM('active','closed'),
  created_on      DATETIME DEFAULT NOW()
);
```

---

## 💬 Real-Time Chat Flow

```
Client A                    Socket.IO Server              Client B
   │                               │                          │
   │── connect() ─────────────────►│                          │
   │── addUser({user_id: 1}) ─────►│ stores socketId in map  │
   │                               │                          │
   │── sendMessage({              │                          │
   │     receiver_id: 2,          │                          │
   │     message: "Hi!"           │                          │
   │   }) ───────────────────────►│                          │
   │                               │── write to m_chats DB   │
   │                               │                          │
   │                               │   Is receiver online?   │
   │                               │──── YES ───────────────►│
   │                               │         sendMessage()   │
   │                               │                          │
   │                               │──── NO ─► FCM Push      │
   │                               │           Notification  │
```

---

## 💳 Payment Flow

```
Client                  Backend                    Razorpay
  │                        │                           │
  │── GET /getSubscriptionList ──►│                    │
  │◄── [ plan list ] ────────────│                    │
  │                               │                    │
  │── POST /getOrderid ──────────►│                    │
  │                               │── createOrder() ──►│
  │                               │◄── { order_id } ───│
  │◄── { order_id } ─────────────│                    │
  │                               │                    │
  │── [User pays via Razorpay SDK]──────────────────►  │
  │                               │                    │
  │                               │◄── Webhook POST ───│
  │                               │  /payment/verify   │
  │                               │── HMAC verify ─────│
  │                               │── Unlock Premium   │
```

---

## 🔑 Environment Variables

```env
# App
PORT=3000
SOCKET_PORT=7198
NODE_ENV=production

# Database
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=marriagesaga

# Auth
JWT_SECRET=your_jwt_secret

# Razorpay
RAZORPAY_KEY_ID=rzp_live_xxxx
RAZORPAY_KEY_SECRET=your_secret

# Twilio
TWILIO_ACCOUNT_SID=ACxxxxxx
TWILIO_AUTH_TOKEN=xxxxxxxx
TWILIO_API_KEY=SKxxxxxx
TWILIO_API_SECRET=xxxxxxxx

# Firebase
FIREBASE_SERVER_KEY=AAAAxxxxxxx

# Cloudinary (optional)
CLOUDINARY_CLOUD_NAME=your_cloud
CLOUDINARY_API_KEY=your_key
CLOUDINARY_API_SECRET=your_secret
```

 

## 👨‍💻 Author

Built and deployed to production by **Gyanendra Yadav**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat&logo=linkedin)](https://www.linkedin.com/in/gyanendra-yadav-059725253/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com/GyanendraYadav7715)

> 🌐 Live at [www.marriagesaga.com](https://www.marriagesaga.com) — a real product, real users, real impact.

---

<div align="center">
<sub>Built with ❤️ using Node.js · Express · MySQL · Socket.IO · Twilio · Razorpay · Firebase</sub>
</div>