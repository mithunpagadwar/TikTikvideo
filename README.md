# TikTik - Video Sharing Platform

एक पूर्ण YouTube जैसा वीडियो शेयरिंग प्लेटफॉर्म, Pure HTML, CSS और JavaScript से बना।

## 🎯 Features

✅ **Real Google Authentication** - Firebase के साथ असली Google Sign-in  
✅ **Subscribe Button with Bell Icon** - YouTube-style subscribe करें notification preferences के साथ  
✅ **Video Upload** - अपनी वीडियो upload करें (localStorage के साथ)  
✅ **Shorts** - Short vertical videos create करें  
✅ **Live Streaming** - Real-time live stream करें और save करें  
✅ **Channel Management** - अपना channel बनाएं और customize करें  
✅ **Like/Dislike System** - Videos को like/dislike करें  
✅ **Comments** - Video पर comments add करें  
✅ **Watch History** - आपकी watch history track करता है  
✅ **Subscriptions** - Channels को subscribe करें और manage करें  
✅ **Bell Notifications** - All, Personalized, या None notification settings  
✅ **Search** - Videos search करें  
✅ **Responsive Design** - सभी devices के लिए optimized  
✅ **Dark/Light Theme** - Theme toggle करें  
✅ **PWA Support** - Progressive Web App  

## 🚀 Vercel पर Deploy करें

### Step 1: GitHub पर Push करें

```bash
# अपनी local directory में जाएं
cd tiktik-video-platform

# Git initialize करें (अगर नहीं किया है)
git init

# सभी files add करें
git add .

# Commit करें
git commit -m "Initial commit - TikTik Video Platform"

# GitHub पर repository बनाएं और link करें
git remote add origin https://github.com/YOUR_USERNAME/tiktik-video-platform.git

# Push करें
git push -u origin main
```

### Step 2: Vercel से Connect करें

1. [Vercel](https://vercel.com) पर जाएं और Sign Up/Login करें
2. **New Project** पर क्लिक करें
3. अपनी **GitHub repository** import करें (`tiktik-video-platform`)
4. **Deploy** बटन पर क्लिक करें

बस! आपकी website live हो जाएगी।

## 📁 Project Structure

```
tiktik-video-platform/
├── index.html          # Main HTML file
├── style.css           # All styles
├── script.js           # All JavaScript code
├── manifest.json       # PWA manifest
├── sw.js              # Service Worker
├── vercel.json        # Vercel configuration
└── README.md          # यह file
```

## 🎮 कैसे Use करें

### 1. Login करें (Real Google Authentication)
- **"Login with Google"** बटन पर क्लिक करें
- असली Google account से sign in करें (Firebase authentication)
- या fallback mode में नाम और email enter करें

### 2. Channel बनाएं
- Profile menu खोलें (top-right corner)
- **"Create a channel"** पर क्लिक करें
- Channel name enter करें

### 3. Video Upload करें
- **"+"** बटन पर क्लिक करें → **"Upload Video"** चुनें
- अपनी video file select करें (Max 10MB)
- Title, Description add करें
- **Publish** करें

### 4. Short Create करें
- **"+"** बटन → **"Create Short"**
- Short vertical video upload करें
- Publish करें

### 5. Live Streaming करें
- **"+"** बटन → **"Go Live"**
- Camera और Mic access allow करें
- Stream title और description add करें
- **"Start Live Stream"** पर क्लिक करें

### 6. Subscribe और Bell Notifications
- किसी भी video को play करें
- Channel name के बगल में **Subscribe** बटन क्लिक करें
- Subscribed होने के बाद **Bell icon** दिखेगा
- Bell icon पर क्लिक करके notification preference चुनें:
  - **All** - सभी notifications
  - **Personalized** - selected notifications
  - **None** - no notifications

### 7. My Channel Tabs
- **Videos** - आपकी सभी uploaded videos
- **Shorts** - आपकी सभी shorts
- **Live** - आपकी सभी live streams
- **Playlists** - आपकी playlists
- **About** - Channel information

## 🌐 Local Testing

```bash
# Python 3 के साथ server चलाएं
python3 -m http.server 5000

# Browser में खोलें
http://localhost:5000
```

## 📱 Features Detail

### Channel Tabs (YouTube की तरह)
सभी tabs smooth काम करती हैं:
- ✅ Videos tab - सभी uploaded videos
- ✅ Shorts tab - सभी shorts
- ✅ Live tab - सभी live streams (saved)
- ✅ Playlists tab
- ✅ About tab - Channel details

### Profile Menu (YouTube-style)
सभी menu items functional हैं:
- ✅ Create a channel
- ✅ Google Account (external link)
- ✅ Switch account
- ✅ Sign out
- ✅ TikTik Studio
- ✅ Creator Academy
- ✅ Settings
- ✅ Help
- ✅ Send feedback
- ✅ और भी बहुत कुछ...

### Live Streaming
- ✅ Camera और microphone access
- ✅ Stream preview
- ✅ Stream save होता है automatically
- ✅ Live stream end होने पर video में convert होता है
- ✅ Channel में save होता है

## 💾 Storage

सभी data **localStorage** में save होता है:
- User data
- Uploaded videos
- Shorts
- Live streams
- Comments
- Watch history
- Liked videos
- Channel information

## 🔧 Technical Details

- **Frontend**: Pure HTML5, CSS3, JavaScript (ES6+)
- **Storage**: LocalStorage
- **Media**: DataURL/Blob for video storage
- **PWA**: Service Worker enabled
- **Icons**: Font Awesome 6.0
- **No Framework**: No React, No Vue, Pure JS!

## ⚠️ Important Notes

### Video Upload Limits
- **Max file size**: 10MB (localStorage limitation)
- बड़ी files के लिए error message मिलेगा
- Recommended: छोटी videos upload करें

### Browser Support
- Chrome (Recommended)
- Firefox
- Edge
- Safari
- Mobile browsers

### LocalStorage Quota
- Browser पर depend करता है (usually 5-10MB)
- बहुत सारी videos upload करने से quota exceed हो सकता है

## 🎨 Customization

### Theme बदलें
- Header में moon/sun icon पर क्लिक करें
- Light/Dark mode toggle होगा

### Channel Customize करें
- "Your Channel" page पर जाएं
- **"Edit Channel"** बटन क्लिक करें
- Name, Description, Avatar, Banner update करें

## 📝 License

MIT License - Free to use and modify

## 🙏 Credits

Made with ❤️ for video sharing enthusiasts

---

## 📞 Support

अगर कोई problem हो तो:
1. Browser console check करें (F12)
2. localStorage clear करें और reload करें
3. Different browser try करें

**Enjoy creating and sharing videos! 🎬**
