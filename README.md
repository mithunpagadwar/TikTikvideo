# 🎬 TikTik - Complete Video Sharing Platform

एक पूर्ण YouTube/TikTok जैसा video sharing platform with **payments**, **creator wallets**, and **secure cloud storage**.

---

## ✨ Features

### 🔐 Authentication & Security
- ✅ **Google Login** with Firebase Authentication
- ✅ **Secure API** - सभी sensitive operations backend में
- ✅ **Firestore Security Rules** - unauthorized access prevention
- ✅ **No API keys in frontend** - सभी secrets server-side

### 📹 Video Management
- ✅ **Video Upload** to Cloudflare R2 (secure cloud storage)
- ✅ **Video Playback** with custom controls
- ✅ **Shorts** - vertical short videos support
- ✅ **Live Streaming** placeholder
- ✅ **Video Metadata** in Firebase Firestore

### 💰 Creator Monetization
- ✅ **Creator Wallets** - हर creator का personal wallet
- ✅ **Tip System** - viewers can tip creators via Stripe
- ✅ **Transaction History** - सभी payments की tracking
- ✅ **Payout Requests** - creators can withdraw earnings
- ✅ **Secure Payments** - Stripe integration

### 🎨 User Interface
- ✅ **YouTube-style UI** - familiar और professional
- ✅ **Dark/Light Mode** - theme toggle
- ✅ **Responsive Design** - सभी devices पर काम करता है
- ✅ **PWA Support** - Progressive Web App
- ✅ **Wallet Dashboard** - earnings tracking

---

## 🏗️ Tech Stack

### Frontend
- **HTML5** - semantic markup
- **CSS3** - modern styling with CSS variables
- **JavaScript (ES6+)** - vanilla JS, no frameworks
- **Firebase SDK** - auth और firestore
- **Font Awesome** - icons

### Backend (Serverless)
- **Vercel Serverless Functions** - Node.js API endpoints
- **Firebase Admin SDK** - server-side Firestore access
- **Stripe API** - payment processing
- **AWS SDK** - S3-compatible R2 uploads

### Storage & Database
- **Cloudflare R2** - video storage (S3-compatible)
- **Firebase Firestore** - metadata, wallets, transactions
- **Firebase Authentication** - user management

---

## 📁 Project Structure

```
tiktik-video-platform/
├── index.html              # Main HTML file with UI
├── style.css               # Complete styles (3200+ lines)
├── script.js               # All JavaScript logic (3700+ lines)
├── manifest.json           # PWA manifest
├── sw.js                   # Service worker
├── api/                    # Backend serverless functions
│   ├── generate-upload-url.js   # Cloudflare R2 signed URLs
│   ├── process-tip.js           # Process tip payments
│   ├── confirm-tip.js           # Confirm and update wallet
│   └── request-payout.js        # Handle payout requests
├── firestore.rules         # Firestore security rules
├── vercel.json            # Vercel configuration
├── package.json           # Dependencies
├── .env.example           # Environment variables template
├── DEPLOYMENT.md          # Detailed deployment guide (Hindi)
└── README.md              # This file
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ installed
- Firebase account
- Cloudflare account (for R2)
- Stripe account (for payments)
- Vercel account (for deployment)

### Local Development

1. **Clone Repository**
```bash
git clone https://github.com/YOUR_USERNAME/tiktik-video-platform.git
cd tiktik-video-platform
```

2. **Setup Environment Variables**
```bash
cp .env.example .env
# Edit .env and fill in your credentials
```

3. **Install Dependencies**
```bash
npm install
```

4. **Run Development Server**
```bash
python3 -m http.server 5000
# या
npm run dev
```

5. **Open in Browser**
```
http://localhost:5000
```

---

## 🔑 Environment Variables

Create a `.env` file:

```env
# Firebase Configuration
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@...
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"

# Cloudflare R2
R2_ENDPOINT=https://xxxxx.r2.cloudflarestorage.com
R2_ACCESS_KEY_ID=your_access_key
R2_SECRET_ACCESS_KEY=your_secret_key
R2_BUCKET_NAME=tiktik-videos
R2_PUBLIC_URL=https://pub-xxxxx.r2.dev

# Stripe
STRIPE_SECRET_KEY=sk_test_xxxxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxxxx

# App Configuration
ALLOWED_ORIGIN=http://localhost:5000
```

पूरी setup guide के लिए देखें: **[DEPLOYMENT.md](./DEPLOYMENT.md)**

---

## 📖 API Endpoints

### 1. Generate Upload URL
**POST** `/api/generate-upload-url`

Generates a signed URL for uploading videos to Cloudflare R2.

**Request:**
```json
{
  "fileName": "myvideo.mp4",
  "fileType": "video/mp4",
  "fileSize": 50000000
}
```

**Response:**
```json
{
  "uploadUrl": "https://...",
  "fileKey": "videos/123456-myvideo.mp4",
  "publicUrl": "https://pub-xxx.r2.dev/videos/123456-myvideo.mp4",
  "expiresIn": 900
}
```

### 2. Process Tip
**POST** `/api/process-tip`

Process a tip payment via Stripe.

**Request:**
```json
{
  "receiverId": "user123",
  "amount": 500,
  "videoId": "video456",
  "message": "Great video!"
}
```

**Response:**
```json
{
  "success": true,
  "clientSecret": "pi_xxx_secret_xxx",
  "transactionId": "trans123"
}
```

### 3. Confirm Tip
**POST** `/api/confirm-tip`

Confirm payment and update wallets.

### 4. Request Payout
**POST** `/api/request-payout`

Request payout to Stripe/PayPal.

---

## 🔒 Security Features

### 1. **No API Keys in Frontend**
सभी sensitive keys (R2, Stripe Secret, Firebase Admin) केवल backend में हैं।

### 2. **Firebase ID Token Verification**
हर API request में Firebase ID token verify होता है।

### 3. **Firestore Security Rules**
```javascript
// Users can only read their own wallet
match /wallets/{userId} {
  allow read: if request.auth.uid == userId;
  allow update: if false; // Only backend can update
}
```

### 4. **Signed Upload URLs**
R2 upload URLs expire in 15 minutes और backend से generate होते हैं।

### 5. **CORS Protection**
Only allowed domains can make API requests.

---

## 💸 Payment Flow

### Tip Flow:
```
1. User clicks "Tip" button
   ↓
2. Frontend calls /api/process-tip
   ↓
3. Backend creates Stripe Payment Intent
   ↓
4. User completes payment
   ↓
5. Frontend calls /api/confirm-tip
   ↓
6. Backend updates receiver's wallet
   ↓
7. Transaction saved in Firestore
```

### Payout Flow:
```
1. Creator clicks "Request Payout"
   ↓
2. Backend verifies wallet balance
   ↓
3. Creates payout request
   ↓
4. Deducts from wallet (pending)
   ↓
5. Processes via Stripe Connect
   ↓
6. Updates payout status
```

---

## 📊 Database Schema

### Firestore Collections:

#### `videos`
```javascript
{
  uploaderId: "user123",
  videoUrl: "https://...",
  title: "My Video",
  description: "...",
  timestamp: Timestamp,
  views: 0,
  likes: 0
}
```

#### `wallets`
```javascript
{
  userId: "user123",
  balance: 5000, // in cents
  totalEarnings: 10000,
  pendingPayouts: 0,
  createdAt: Timestamp,
  lastUpdated: Timestamp
}
```

#### `transactions`
```javascript
{
  senderId: "user123",
  receiverId: "user456",
  amount: 500, // in cents
  type: "tip",
  status: "completed",
  videoId: "video789",
  paymentIntentId: "pi_xxx",
  createdAt: Timestamp
}
```

#### `payouts`
```javascript
{
  userId: "user123",
  amount: 10000, // in cents
  method: "stripe",
  status: "pending",
  requestedAt: Timestamp
}
```

---

## 🎯 Features Roadmap

### Currently Implemented ✅
- [x] Google Authentication
- [x] Video Upload to R2
- [x] Creator Wallets
- [x] Tip System
- [x] Payout Requests
- [x] Transaction History
- [x] Firestore Security

### Coming Soon 🚧
- [ ] Video Thumbnails Generation
- [ ] Video Compression
- [ ] Comments System (with moderation)
- [ ] Admin Dashboard
- [ ] Analytics for Creators
- [ ] Subscription System
- [ ] Ad Revenue Sharing

---

## 🧪 Testing

### Test Credentials

**Stripe Test Card:**
```
Card Number: 4242 4242 4242 4242
Expiry: Any future date
CVC: Any 3 digits
```

**Test Firebase:**
Use your Google account for testing.

### Test Flow:
1. Login with Google
2. Go to "Your Channel" → "Wallet"
3. Check wallet balance (should be $0.00)
4. Upload a test video (small file)
5. Try tipping yourself (use test card)
6. Verify transaction appears

---

## 📝 License

MIT License - Free to use and modify

---

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create pull request

---

## 📧 Support

Issues या questions के लिए:
- GitHub Issues: [Create Issue](https://github.com/YOUR_USERNAME/tiktik-video-platform/issues)
- Email: support@tiktik.com

---

## 🙏 Credits

Built with ❤️ for video creators

### Technologies Used:
- Firebase by Google
- Cloudflare R2
- Stripe Payments
- Vercel Hosting
- Font Awesome Icons

---

**Made in India 🇮🇳**

Happy Coding! 🚀
