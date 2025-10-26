# 🎬 TikTik - Complete Video Sharing Platform

A full-featured video sharing platform with live streaming, short videos, creator monetization, and cloud storage.

## ✨ Key Features Implemented

### 🔐 Authentication & Security
- ✅ Google Login with Firebase Authentication
- ✅ Firebase ID token verification on all API calls
- ✅ Secure Firestore security rules
- ✅ No API keys exposed in frontend

### 📹 Video Management  
- ✅ **Video Upload** to Cloudflare R2 with signed URLs (15-min expiry)
- ✅ **User Isolation**: Videos in "My Channel" only show uploader's content
- ✅ **Firestore Integration**: All videos stored with uploaderId, type, timestamp
- ✅ **Short Videos**: Vertical format support with loop capability
- ✅ **Live Streaming**: Camera/mic access with recording capability (MediaRecorder ready)

### 💰 Creator Monetization
- ✅ Creator Wallets with balance tracking
- ✅ Tip System via Stripe payments
- ✅ Transaction history
- ✅ Payout requests (Stripe/PayPal)

### 🎨 User Interface
- ✅ YouTube-style responsive UI
- ✅ Dark/Light theme toggle
- ✅ PWA support (installable)
- ✅ Wallet dashboard

## 🏗️ Tech Stack

**Frontend:**
- HTML5, CSS3, JavaScript (Vanilla)
- Firebase SDK (Auth, Firestore)
- Service Worker for PWA

**Backend:**
- Vercel Serverless Functions (Node.js)
- Firebase Admin SDK
- Cloudflare R2 (S3-compatible storage)
- Stripe for payments

## 📁 Project Structure

```
tiktik-video-platform/
├── index.html              # Main HTML
├── style.css               # All styles
├── script.js               # Frontend logic with Firestore integration
├── manifest.json           # PWA manifest
├── sw.js                   # Service worker
├── api/                    # Backend serverless functions
│   ├── generate-upload-url.js   # R2 signed URLs
│   ├── process-tip.js           # Stripe payments
│   ├── confirm-tip.js           # Wallet updates
│   └── request-payout.js        # Payout handling
├── firestore.rules         # Security rules
├── package.json            # Dependencies
├── vercel.json            # Vercel config
└── .env.example           # Environment template
```

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- Firebase account
- Cloudflare account (R2)
- Stripe account
- Vercel account

### Local Development

1. **Clone and Install**
```bash
git clone <your-repo>
cd tiktik-video-platform
npm install
```

2. **Configure Environment**
```bash
cp .env.example .env
# Edit .env with your credentials
```

3. **Run Development Server**
```bash
python3 -m http.server 5000
# or
npm run dev
```

4. **Open Browser**
```
http://localhost:5000
```

## 🔑 Required Environment Variables

Create a `.env` file with:

```env
# Firebase
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@...
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"

# Cloudflare R2
R2_ENDPOINT=https://[account-id].r2.cloudflarestorage.com
R2_ACCESS_KEY_ID=your_access_key
R2_SECRET_ACCESS_KEY=your_secret_key
R2_BUCKET_NAME=tiktik-videos
R2_PUBLIC_URL=https://pub-xxxxx.r2.dev

# Stripe
STRIPE_SECRET_KEY=sk_test_xxxxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxxxx

# CORS
ALLOWED_ORIGIN=http://localhost:5000
```

See `.env.example` for detailed instructions.

## 🔒 Key Fixes & Improvements

### 1. Live Streaming Fixed ✅
- **Recording**: MediaRecorder API integration for continuous recording
- **Persistence**: Videos stay in feed and channel after stream ends
- **No Auto-Close**: Streams continue until manually ended
- **Playback**: Recorded streams playable immediately

### 2. Short Videos Fixed ✅
- **Immediate Playback**: Videos play on click/hover
- **No Disappearing**: Videos persist in feed and channel
- **Looping**: Continuous loop playback
- **User Isolation**: Only visible in uploader's channel

### 3. User Isolation Fixed ✅
```javascript
// My Channel shows ONLY user's videos
loadUserVideos() {
  .where('uploaderId', '==', this.currentUserId)
  // Filters by current user ID
}

// Main Feed shows ALL videos
loadAllVideosFromFirestore() {
  .orderBy('timestamp', 'desc')
  // No user filter
}
```

### 4. Cloudflare R2 Integration ✅
- Backend generates pre-signed URLs (15-min expiry)
- Frontend uploads directly to R2
- No credentials in frontend
- Public URLs for playback

### 5. Firebase Security ✅
All API endpoints verify Firebase ID tokens:
```javascript
const idToken = authHeader.split('Bearer ')[1];
const decodedToken = await admin.auth().verifyIdToken(idToken);
```

## 📖 API Endpoints

### Generate Upload URL
**POST** `/api/generate-upload-url`
- Requires: Firebase ID token
- Returns: Signed R2 upload URL
- Expires: 15 minutes

### Process Tip
**POST** `/api/process-tip`
- Creates Stripe PaymentIntent
- Records transaction in Firestore
- Returns: Client secret

### Confirm Tip
**POST** `/api/confirm-tip`
- Verifies payment with Stripe
- Updates receiver wallet
- Atomic Firestore transaction

### Request Payout
**POST** `/api/request-payout`
- Validates wallet balance
- Creates payout record
- Deducts from wallet

## 🔒 Firestore Security Rules

```javascript
// Users can read all videos
match /videos/{videoId} {
  allow read: if true;
  allow create: if isSignedIn() && uploaderId == auth.uid;
}

// Users can only read their own wallet
match /wallets/{userId} {
  allow read: if isOwner(userId);
  allow update: if false; // Backend only
}
```

## 🧪 Testing

### Test Stripe Payments
```
Card: 4242 4242 4242 4242
Expiry: Any future date
CVC: Any 3 digits
```

### Test Flow
1. Login with Google
2. Upload a test video
3. View in "My Channel" (isolated to your account)
4. Check wallet balance
5. Try tipping (test card)

## 📊 Firestore Collections

### videos
```javascript
{
  uploaderId: "user123",
  videoUrl: "https://pub-xxx.r2.dev/...",
  title: "My Video",
  description: "...",
  timestamp: Timestamp,
  isShort: false,
  isLive: false,
  views: 0,
  likes: 0
}
```

### wallets
```javascript
{
  userId: "user123",
  balance: 5000, // cents
  totalEarnings: 10000,
  pendingPayouts: 0
}
```

### transactions
```javascript
{
  senderId: "user123",
  receiverId: "user456",
  amount: 500,
  type: "tip",
  status: "completed",
  paymentIntentId: "pi_xxx"
}
```

## 🚢 Deployment to Vercel

1. **Push to GitHub**
```bash
git add .
git commit -m "TikTik platform ready"
git push origin main
```

2. **Deploy to Vercel**
- Import GitHub repository
- Add all environment variables
- Deploy!

3. **Configure Firebase**
- Add Vercel domain to authorized domains
- Deploy Firestore rules

## 🎯 Next Steps

### Completed ✅
- [x] Google Authentication
- [x] Video Upload to R2
- [x] User Isolation (My Channel)
- [x] Firestore Integration
- [x] Creator Wallets
- [x] Tip System
- [x] Payout Requests
- [x] Security Rules

### Future Enhancements 🚧
- [ ] Advanced MediaRecorder live streaming with chunked upload
- [ ] Auto-generated video thumbnails
- [ ] Video compression/transcoding
- [ ] Comments with moderation
- [ ] Admin dashboard
- [ ] Creator analytics
- [ ] Subscription system
- [ ] Ad revenue sharing

## 💡 Architecture Highlights

### User Isolation
Videos are filtered by `uploaderId` field:
- **My Channel**: `WHERE uploaderId == currentUser.uid`
- **Main Feed**: No filter (shows all videos)

### Live Streaming
- Camera/mic access via `getUserMedia()`
- MediaRecorder ready for recording
- Uploads persist after stream ends
- Lives remain in channel forever

### Short Videos
- Marked with `isShort: true`
- Vertical aspect ratio
- Loop playback
- Hover-to-play (can be enabled)

## 📝 License

MIT License - Free to use and modify

## 🙏 Support

For issues or questions:
- Check Firestore security rules
- Verify environment variables
- Check browser console for errors
- Review API endpoint logs in Vercel

---

**Made with ❤️ for video creators**

Happy Streaming! 🚀
