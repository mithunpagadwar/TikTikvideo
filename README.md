# 🎬 TikTikVideo — A Modern Video Sharing Platform

Welcome to **TikTikVideo**, a next-generation video sharing website inspired by YouTube and TikTok —  
built with **React + Firebase + Tailwind CSS** for a fast, responsive, and engaging experience.

---

## 🚀 Live Demo

🔗 [Visit (https://mithunpagadwar.github.io/TikTikvideo/
*(Replace this link with your actual website URL)*

---

## ✨ Features

✅ **User Authentication** (Firebase Login / Signup)  
✅ **Secure Video Upload** (Only logged-in users can upload)  
✅ **Like / Dislike System**  
✅ **Comment Section with Firestore**  
✅ **Search Videos by Title or Tag**  
✅ **Profile Page with User Stats & Uploads**  
✅ **Settings Page for Account Preferences**  
✅ **Shorts (Vertical Video Player)**  
✅ **Responsive Design for all devices**  
✅ **Beautiful Animations using Framer Motion**

---

## 🧠 Tech Stack

| Technology | Purpose |
|-------------|----------|
| **React.js** | Frontend Framework |
| **Firebase** | Authentication + Firestore + Storage |
| **Tailwind CSS** | Styling |
| **React Router DOM** | Navigation |
| **Framer Motion** | Smooth Animations |
| **Lucide Icons** | Icons Library |

---

## ⚙️ Installation (Local Setup)

To run this project locally, follow these simple steps 👇

```bash
# 1️⃣ Clone the repository
git clone https://github.com/yourusername/tiktikvideo.git

# 2️⃣ Move into project folder
cd tiktikvideo

# 3️⃣ Install dependencies
npm install

# 4️⃣ Run the app
npm run dev
```

Now open 👉 **http://localhost:5173**  
to explore TikTikVideo on your browser 🎥

---

## 🔧 Firebase Configuration

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create a new project called **TikTikVideo**
3. Enable the following:
   - Authentication → Email/Password  
   - Firestore Database  
   - Storage
4. Copy your Firebase configuration
5. Paste it inside your project at:  
   `src/firebase/firebaseConfig.js`

Example:
```js
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
};

export const app = initializeApp(firebaseConfig);
```

---

## 📂 Folder Structure

```
tiktikvideo/
├── public/
│   └── favicon.ico
├── src/
│   ├── assets/
│   ├── components/
│   ├── pages/
│   ├── context/
│   ├── firebase/
│   │   └── firebaseConfig.js
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── package.json
└── README.md
```

---

## 📸 Screenshots

| Home Page | Upload Page | Profile Page |
|------------|--------------|--------------|
| 🏠 | ⬆️ | 👤 |

*(You can upload screenshots here later)*

---

## 👨‍💻 Developer Info

**Author:** Mithun Pagadwar  
**Project:** TikTikVideo  
**GitHub:** [https://github.com/yourusername](https://github.com/yourusername)  
**Made with ❤️ in India**

---

## 🪄 Upcoming Features

- 🔴 Live Streaming  
- 🧠 Smart Video Recommendations (AI-based)  
- 💎 Verified User Badges  
- 🌐 Multi-language Support  
- 📊 Video Analytics Dashboard  

---

## 📜 License

This project is licensed under the **MIT License**.  
You are free to use, modify, and distribute this project with proper attribution.

---

> 🎥 *TikTikVideo — Watch. Upload. Share. Enjoy!*
