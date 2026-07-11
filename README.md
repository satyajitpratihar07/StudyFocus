# 🎓 StudyFocus — The Ultimate AI-Powered Academic Workspace

[![Vite](https://img.shields.io/badge/Vite-8.1-blue.svg?logo=vite)](https://vite.dev/)
[![React](https://img.shields.io/badge/React-19.2-cyan.svg?logo=react)](https://react.dev/)
[![Express](https://img.shields.io/badge/Express-5.2-gray.svg?logo=express)](https://expressjs.com/)
[![Firebase](https://img.shields.io/badge/Firebase-12.1-orange.svg?logo=firebase)](https://firebase.google.com/)
[![Google Gemini](https://img.shields.io/badge/Google_Gemini-API_v2-purple.svg?logo=googlegemini)](https://ai.google.dev/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/satya/StudyFocus/pulls)

**StudyFocus** is a high-performance, responsive, and visually stunning academic productivity portal. Built on a modern glassmorphic theme with smooth micro-animations, it serves as a central hub for students to manage schedules, enter distraction-free focus zones, store private notes behind custom client-side encryption, and collaborate with an intelligent AI study assistant.

---

## 🎨 Visual Showcase & Design System

StudyFocus utilizes a cohesive, tokenized design system built using CSS Custom Properties supporting native **Fluid Transitions**, **Glassmorphism panels**, and a **Real-Time Light/Dark Theme** state.

![StudyFocus Dashboard Banner](./public/studyfocus-banner.png)

---

## ✨ Features Checklist

- [x] **📊 Bento-Style Analytics Dashboard**
  - **3D Analog/Digital Clock**: Built using CSS 3D perspectives, rotating hand layers, and neon drop-shadows.
  - **Dynamic SVG Area & Bar Charts**: Responsive data rendering for daily, weekly, monthly, and yearly study intervals.
  - **Interactive Streaks**: Gamified study streaks tracking consistency.
- [x] **🔒 Cryptographic Private Locker**
  - Secured using an **interactive SVG pattern-drawing lock canvas** (3x3 pattern grid connecting dots).
  - All content inside the locker is **fully client-side encrypted** using AES-256 (`crypto-js`) with key derivation from your drawn pattern. No raw content ever reaches the server.
- [x] **🧑‍💻 Focus Room (Pomodoro)**
  - Distraction-free interface with a configurable timer.
  - YouTube player integration for streaming focus/ambient background tracks and playlists.
  - Session-specific quick tasks checklist.
- [x] **🤖 AI Study Planner & Assistant**
  - Integrated chatbot widget powered by the **Google Gemini API** (`@google/genai`).
  - Implements an automated **12-key round-robin API rotation** on the backend to avoid rate limits and guarantee maximum service uptime.
  - Generates custom study planners and breaks down complex academic subjects into bite-sized tasks.
- [x] **📅 Smart Study Scheduler**
  - Responsive study block schedule planner.
  - Automatically synchronizes client updates to **Cloud Firestore** using a debounced writing algorithm.
- [x] **📧 Medicine & Study Reminders**
  - Custom medicine schedule categorizations (Morning, Afternoon, Night slots).
  - Integrates a Node.js Express server acting as a SMTP client (`Nodemailer`) delivering professional transaction emails via a pool of Gmail SMTP relays.
  - Express security-hardened middleware (`Helmet`, `Express Rate Limit` anti-spam rules).

---

## 🌐 System Architecture

```mermaid
graph TD
    subgraph Client [Client-Side: React 19 + Vite]
        UI[App Shell & Router] --> DB_PAGE[Bento Dashboard]
        UI --> LOCKER[Private Locker]
        UI --> FOCUS[Focus Room]
        UI --> CHAT_WIDGET[AI Chatbot Widget]
        
        LOCKER -->|Client-Side AES Encryption| LOCAL_STORAGE[(Local Storage)]
        DB_PAGE --> LOCAL_STORAGE
    end

    subgraph Backend [Backend-Side: Node + Express]
        API_GATEWAY[Express Server] -->|Rate Limiter & Security Headers| ROUTER[Route Handlers]
        ROUTER -->|Gmail SMTP Host| SMTP[Nodemailer Transporter]
        ROUTER -->|Rotated Key ClientPool| GEMINI[Gemini GenAI Client]
    end

    subgraph Cloud [Cloud Services]
        FIREBASE_AUTH[Firebase Auth]
        FIRESTORE[(Cloud Firestore)]
        GEMINI_API[Google Gemini Models]
    end

    UI -->|onAuthStateChanged| FIREBASE_AUTH
    LOCAL_STORAGE -->|Debounced Auto-Sync| FIRESTORE
    CHAT_WIDGET -->|fetch /api/chat| API_GATEWAY
    ROUTER -->|Firebase Admin SDK| FIRESTORE
    GEMINI -->|Generative Inquiry| GEMINI_API
    SMTP -->|Deliver OTP & Reminders| USER_EMAIL[Student's Email Inbox]
```

---

## ⚙️ Environment Variables Config

Create a `.env` file in the root directory. This contains both frontend (prefixed with `VITE_` for exposure) and backend secure configurations.

```env
# ── Server-Side SMTP Settings (Gmail app password) ──
SMTP_HOST=smtp.gmail.com
SMTP_PORT=465
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-gmail-app-password
API_PORT=3001

# ── Firebase Credentials (Client-Side) ──
VITE_FIREBASE_API_KEY=AIzaSy...
VITE_FIREBASE_AUTH_DOMAIN=your-app.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-app.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=1234567890
VITE_FIREBASE_APP_ID=1:123456:web:abcd...
VITE_FIREBASE_DATABASE_URL=https://your-project-rtdb.firebaseio.com

# ── Google Gemini Rotated API Keys ──
VITE_GEMINI_API_KEY=AIzaSyPrimary...
GEMINI_API_KEY_2=AIzaSySecond...
GEMINI_API_KEY_3=AIzaSyThird...
# Add up to GEMINI_API_KEY_12 for round-robin failover

# ── Firebase Admin SDK Service Account JSON (Private) ──
FIREBASE_SERVICE_ACCOUNT='{
  "type": "service_account",
  "project_id": "your-project-id",
  "private_key_id": "...",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0B...",
  "client_email": "firebase-adminsdk-fbsvc@your-project.iam.gserviceaccount.com"
}'
```

---

## 🚀 Installation & Local Development

### 1. Prerequisites
Make sure you have [Node.js](https://nodejs.org/) installed (v18+ recommended).

### 2. Install Dependencies
Install all package packages for both frontend components and server routes:
```bash
npm install
```

### 3. Run Development Server
StudyFocus runs concurrently. Launching the dev script starts both the **Vite React UI** and the **Express backend API server** together:
```bash
npm run dev
```
* The Client will launch at: `http://localhost:5173`
* The Server will run at: `http://localhost:3001` (Vite routes `/api` proxies to 3001)

### 4. Build for Production
To bundle the frontend application with optimized static assets and oxlint checks:
```bash
npm run build
```

---

## ⚡ Showcase: Micro-Animations & CSS Tricks

### 1. Real-Time 3D Analog Clock Rotations
The bento clock calculates exact geometry coordinates dynamically and applies standard CSS 3D transforms:
```javascript
// Dynamic degree angles calculated inside React rendering
const hrsAngle = (time.getHours() % 12) * 30 + time.getMinutes() * 0.5;
const minsAngle = time.getMinutes() * 6 + time.getSeconds() * 0.1;
const secsAngle = time.getSeconds() * 6;
```
```css
/* Sleek 3D Plate with glow */
.clock-3d-plate {
  transform: rotateX(10deg) rotateY(-5deg);
  box-shadow: 
    -10px 10px 20px rgba(0,0,0,0.3),
    inset 0 0 20px rgba(255,255,255,0.05);
  transition: transform 0.8s cubic-bezier(0.075, 0.82, 0.165, 1);
}
```

### 2. Safe Cryptographic Key Rotation (Backend)
The backend routes chat queries using an automated API key cycling buffer:
```javascript
const GEMINI_API_KEYS = [
  process.env.VITE_GEMINI_API_KEY,
  process.env.GEMINI_API_KEY_2,
  // ... Up to key 12
].filter(Boolean);

let currentKeyIndex = 0;

function getNextGeminiClient() {
  const apiKey = GEMINI_API_KEYS[currentKeyIndex];
  currentKeyIndex = (currentKeyIndex + 1) % GEMINI_API_KEYS.length;
  return new GoogleGenAI({ apiKey });
}
```

---

## 🤝 Contributing
1. Fork this Repository.
2. Create your Feature Branch: `git checkout -b feature/AmazingFeature`
3. Commit your Changes: `git commit -m 'Add some AmazingFeature'`
4. Push to the Branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request.

---

## 📄 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Developed by ❤️ Satyajit Pratihar
