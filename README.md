# TikTik Video Platform - Deployment Guide

यह folder आपकी TikTik website की सभी जरूरी files contain करता है।

## 📁 Files की List:

1. **index.html** - Main webpage
2. **script.js** - JavaScript code (Google Sign-in included)
3. **style.css** - All styling
4. **server.js** - Backend server
5. **package.json** - Node.js dependencies
6. **manifest.json** - PWA manifest
7. **sw.js** - Service Worker (offline support)
8. **.gitignore** - Git ignore file

## 🚀 Vercel पर Deploy कैसे करें:

### Step 1: Vercel Account बनाएं
- https://vercel.com पर जाएं
- Sign up करें (GitHub से भी कर सकते हैं)

### Step 2: Project Upload करें
- "New Project" button पर click करें
- "Upload" option select करें
- इन सभी files को upload करें

### Step 3: Environment Variables Add करें
Vercel dashboard में जाकर ये secrets add करें:

```
FIREBASE_API_KEY=your_api_key_here
FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_STORAGE_BUCKET=your_project.appspot.com
FIREBASE_MESSAGING_SENDER_ID=your_sender_id
FIREBASE_APP_ID=your_app_id
```

### Step 4: Deploy करें!
- "Deploy" button पर click करें
- आपकी website 1-2 minutes में live हो जाएगी

## 🔥 Firebase Setup (जरूरी!):

1. https://console.firebase.google.com पर जाएं
2. अपना project select करें
3. **Authentication** → **Sign-in method** में जाएं
4. **Google** provider को **Enable** करें
5. Vercel domain को **Authorized domains** में add करें
6. Save करें

## 📱 Local Testing:

```bash
npm install
npm start
```

Website http://localhost:5000 पर खुलेगी

## ✅ Features:

- ✅ Google Sign-in
- ✅ Video Upload & Playback
- ✅ Comments System
- ✅ Dark/Light Theme
- ✅ PWA (Offline Support)
- ✅ Responsive Design

## 🆘 Help:

अगर कोई problem हो तो:
1. Firebase में Google Sign-in enable है check करें
2. Environment variables सही हैं check करें
3. Browser console में errors check करें

---

Built with ❤️ for TikTik Platform
