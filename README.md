# 🏠 StayStory — Hostel & PG Review Platform

> A community-driven platform where students share real hostel and PG stay experiences — so the next student doesn't have to guess.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-Online-brightgreen)](https://stay-story-client.onrender.com)
[![GitHub Repo](https://img.shields.io/badge/GitHub-Source%20Code-blue)](https://github.com/RaushanGupta1516/Stay-Story)

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Tech Stack](#-tech-stack)
- [Architecture Overview](#-architecture-overview)
- [Getting Started](#-getting-started)
- [Usage](#-usage)
- [Features](#-features)
- [Deployment](#-deployment)
- [Testing](#-testing)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [FAQs & Known Issues](#-faqs--known-issues)
- [License & Credits](#-license--credits)
- [Contact](#-contact)

---

## 🎯 Project Overview

### The Problem

Students moving to a new city for college face a real struggle: finding reliable, affordable hostels or PG accommodations. Most platforms (99acres, MagicBricks) cater to long-term renters and don't reflect the on-ground reality — actual cleanliness, warden behaviour, WiFi quality, or safety for female students.

Word of mouth exists, but doesn't scale. WhatsApp groups get buried. Google reviews are sparse and unverified.

### The Solution

**StayStory** is a student-first review and discovery platform where:
- Students post detailed reviews of hostels and PGs they've actually lived in
- Others can browse, search, filter, and compare stays before committing
- Community engagement (comments, likes, similar reviews) helps surface the most trusted posts
- Google Maps integration shows exact property locations

### Value Proposition

| For Students Looking | For Students Who've Stayed |
|---|---|
| Search by location, rent range, and rating | Share your experience to help others |
| See real photos, facilities, and honest reviews | Build credibility in the student community |
| View property on map before visiting | Edit or delete your own posts anytime |
| Discover similar stays nearby | Engage with comments and follow-up questions |

---

## 🛠 Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| React.js (v18) | Component-based UI |
| Tailwind CSS | Utility-first responsive styling |
| React Router DOM | Client-side routing |
| React Context API | Global auth and state management |
| Axios | HTTP requests to backend API |
| Google Maps API | Interactive map for property locations |

### Backend
| Technology | Purpose |
|---|---|
| Node.js | JavaScript runtime |
| Express.js | REST API server and middleware |
| MongoDB + Atlas | Document database for users, posts, comments |
| Mongoose | MongoDB ODM and schema validation |
| JWT | Stateless authentication tokens |
| Google OAuth 2.0 | One-click social sign-in |
| Cloudinary | Image upload and hosting |
| bcrypt | Password hashing |
| dotenv | Environment variable management |

### DevOps & Tooling
| Technology | Purpose |
|---|---|
| Render | Cloud hosting for frontend and backend |
| Git + GitHub | Version control and collaboration |
| Postman | API testing during development |
| VS Code | Primary IDE |

---

## 🏗 Architecture Overview

StayStory follows a standard **3-tier MERN architecture** with a clear separation between client, server, and database.

```
┌─────────────────────────────────────────────────────────┐
│                        CLIENT                           │
│   React.js SPA  ──  React Router  ──  Context API       │
│        │                                                │
│   Axios HTTP Requests  ──  Google Maps SDK              │
└──────────────────────┬──────────────────────────────────┘
                       │ REST API (JSON over HTTPS)
┌──────────────────────▼──────────────────────────────────┐
│                        SERVER                           │
│   Express.js Application                               │
│        │                                                │
│   ┌────┴──────────────────────────────────────────┐    │
│   │              Middleware Stack                  │    │
│   │  cors  ──  helmet  ──  JWT Auth  ──  multer   │    │
│   └────┬──────────────────────────────────────────┘    │
│        │                                                │
│   ┌────▼──────────────────────────────────────────┐    │
│   │                Route Handlers                  │    │
│   │  /auth  ──  /posts  ──  /comments  ──  /users │    │
│   └────┬──────────────────────────────────────────┘    │
│        │                   │                            │
│   Mongoose ODM        Cloudinary API                   │
└──────────────────────┬─────┴───────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────┐
│                      DATABASE                           │
│   MongoDB Atlas (Cloud)                                 │
│                                                         │
│   Collections:                                          │
│   ├── users     { name, email, passwordHash, avatar }   │
│   ├── posts     { title, location, rent, review, ... }  │
│   ├── comments  { postId, userId, body, createdAt }     │
│   └── likes     { postId, userId }                      │
└─────────────────────────────────────────────────────────┘
```

### Data Flow — Posting a Review
```
User fills form
     │
     ▼
Frontend validates input  ──►  Sends POST /api/posts (with JWT header)
     │
     ▼
Server middleware verifies JWT token
     │
     ▼
Image uploaded to Cloudinary  ──►  URL stored in MongoDB document
     │
     ▼
Post document saved to MongoDB Atlas
     │
     ▼
201 response returned  ──►  Frontend updates feed via Context state
```

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

```bash
node -v        # v18.x or higher
npm -v         # v9.x or higher
git --version  # any recent version
```

You'll also need accounts on:
- [MongoDB Atlas](https://www.mongodb.com/atlas) — free tier works
- [Cloudinary](https://cloudinary.com) — free tier works
- [Google Cloud Console](https://console.cloud.google.com) — for OAuth credentials
- [Google Maps Platform](https://developers.google.com/maps) — for Maps API key

---

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/RaushanGupta1516/Stay-Story.git
cd Stay-Story
```

**2. Install server dependencies**
```bash
cd server
npm install
```

**3. Install client dependencies**
```bash
cd ../client
npm install
```

**4. Set up environment variables**

Create a `.env` file in the `/server` directory:
```env
PORT=5000
MONGO_URI=your_mongodb_atlas_connection_string
JWT_SECRET=your_jwt_secret_key
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
GOOGLE_CLIENT_ID=your_google_oauth_client_id
GOOGLE_CLIENT_SECRET=your_google_oauth_client_secret
CLIENT_URL=http://localhost:3000
```

Create a `.env` file in the `/client` directory:
```env
REACT_APP_API_URL=http://localhost:5000
REACT_APP_GOOGLE_MAPS_API_KEY=your_google_maps_api_key
REACT_APP_GOOGLE_CLIENT_ID=your_google_oauth_client_id
```

---

### Running Locally

**Start the backend server:**
```bash
cd server
npm run dev       # uses nodemon for hot-reload
```
Server runs at `http://localhost:5000`

**Start the frontend (in a new terminal):**
```bash
cd client
npm start
```
App runs at `http://localhost:3000`

---

## 💡 Usage

### Basic Workflows

**Signing up / Logging in**
```
1. Visit http://localhost:3000
2. Click "Sign In with Google" for one-click OAuth login
   — OR —
   Register with email and password
3. You're redirected to the home feed
```

**Posting a Review**
```
1. Click "Write a Review" in the navbar
2. Fill in: Property Name, Location, Rent, Facilities, Your Review
3. Upload photos (optional, hosted on Cloudinary)
4. Submit — your post appears in the feed instantly
```

**Finding a Stay**
```
1. Use the search bar to search by location or property name
2. Apply filters: rent range, rating, property type
3. Click a post to view full details and the location on Google Maps
4. Read comments from other students
```

**Engaging with Posts**
```
- Like a post to save it as helpful
- Comment with questions or follow-ups
- View "Similar Reviews" panel on each post for nearby alternatives
```

---

## ✅ Features

### Currently Implemented

- 🔐 **Authentication** — JWT-based login + Google OAuth one-click sign-in
- 📝 **Full CRUD** — Create, read, update, and delete your own review posts
- 💬 **Comments** — Nested comment threads on every post
- ❤️ **Likes** — Like/unlike posts to surface the most helpful reviews
- 🔍 **Search & Filter** — Search by location/name; filter by rent range and rating
- 🗺️ **Google Maps** — Interactive map showing exact hostel/PG location on each post
- 🔄 **Similar Reviews** — Related listings surfaced by location and property type
- 🖼️ **Image Uploads** — Photos hosted via Cloudinary CDN
- 🛡️ **Protected Routes** — Edit/delete only available to the post owner
- 📱 **Responsive UI** — Works cleanly on mobile, tablet, and desktop

### Planned Enhancements

- [ ] Verified student badge (college email verification)
- [ ] Bookmark / saved posts list
- [ ] Direct owner/landlord contact form
- [ ] Price trend analytics over time
- [ ] PWA support for offline browsing
- [ ] Email notifications for comments on your posts

---

## ☁️ Deployment

The app is deployed on **Render** (free tier).

| Service | URL |
|---|---|
| Frontend | https://stay-story-client.onrender.com |
| Backend API | Deployed as a separate Render web service |

### Build & Deploy Steps (Render)

**Backend:**
```
Build Command:  npm install
Start Command:  node server.js
Environment:    Add all variables from the .env section above
```

**Frontend:**
```
Build Command:  npm install && npm run build
Publish Dir:    build/
Environment:    Add REACT_APP_* variables
```

> ⚠️ **Note:** Render free tier spins down after inactivity. First load may take 30–60 seconds to cold-start.

---

## 🧪 Testing

> Automated test coverage is currently minimal — this is an honest disclosure.

Manual API testing was done throughout development using **Postman**.

To run any existing tests:
```bash
cd server
npm test
```

**Planned:**
- Unit tests for auth middleware using Jest
- API integration tests for CRUD routes
- Frontend component tests using React Testing Library

Contributions adding test coverage are especially welcome — see [Contributing](#-contributing).

---

## 🗺 Roadmap

### Short-Term (Next 1–2 Months)
- [ ] Add bookmark/save feature for posts
- [ ] Improve search with autocomplete
- [ ] Add pagination to the main feed
- [ ] Mobile UI polish pass

### Mid-Term (3–6 Months)
- [ ] College email verification for student badge
- [ ] Landlord profile pages with response capability
- [ ] Rent price trend graphs per location
- [ ] Push notifications for comment activity

### Long-Term
- [ ] React Native mobile app
- [ ] Multi-language support (Hindi, regional languages)
- [ ] AI-powered review summarizer for quick TL;DRs
- [ ] Integration with college portals for verified listings

---

## 🤝 Contributing

Contributions are welcome. Here's how to get started:

**1. Fork the repo**
```bash
git clone https://github.com/your-username/Stay-Story.git
```

**2. Create a feature branch**
```bash
git checkout -b feature/your-feature-name
```

**3. Make your changes and commit**
```bash
git add .
git commit -m "feat: add your feature description"
```

**4. Push and open a Pull Request**
```bash
git push origin feature/your-feature-name
```

### Guidelines
- Follow the existing code style (no formatter enforced yet, but be consistent)
- Keep PRs focused — one feature or fix per PR
- Write clear commit messages (prefer conventional commits: `feat:`, `fix:`, `docs:`)
- If adding a new feature, update this README accordingly
- Be respectful — this is a student project and a learning environment

### Code of Conduct
- Be constructive in code reviews
- No gatekeeping — beginners are welcome to contribute
- Report any issues via GitHub Issues, not DMs

---

## ❓ FAQs & Known Issues

**Q: The live demo is slow to load.**
> A: The app is hosted on Render's free tier which cold-starts after inactivity. Give it 30–60 seconds on first load.

**Q: Google Maps isn't showing on local setup.**
> A: Make sure you've added a valid `REACT_APP_GOOGLE_MAPS_API_KEY` in your client `.env` and enabled the Maps JavaScript API in Google Cloud Console.

**Q: Image upload fails.**
> A: Double-check your Cloudinary credentials in the server `.env`. Ensure the cloud name, API key, and secret are all correct.

**Q: OAuth login redirects to an error page.**
> A: Verify that `http://localhost:3000` is added as an authorised redirect URI in your Google Cloud Console OAuth 2.0 credentials.

### Known Issues
- Free tier Render deployment has occasional cold-start delays
- No email/password reset flow implemented yet
- Search is basic string matching — not fuzzy or typo-tolerant

---

### Built With
- [React.js](https://reactjs.org)
- [Node.js](https://nodejs.org)
- [Express.js](https://expressjs.com)
- [MongoDB Atlas](https://www.mongodb.com/atlas)
- [Tailwind CSS](https://tailwindcss.com)
- [Cloudinary](https://cloudinary.com)
- [Google Maps Platform](https://developers.google.com/maps)

---

## 📬 Contact

**Raushan Gupta**
- 📧 work.raushangupta@gmail.com
- 💼 [LinkedIn](https://www.linkedin.com/in/raushangupta16/)
- 🐙 [GitHub](https://github.com/RaushanGupta1516)

---

<p align="center">Built with ❤️ by a student, for students.</p>
