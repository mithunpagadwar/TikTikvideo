# TikTik Video Platform - Complete Deployment Guide

हिंदी में Full Deployment Instructions

## 🎯 Overview

TikTik एक complete video sharing platform है जो YouTube/TikTok जैसा है, जिसमें:
- ✅ Google Authentication (Firebase)
- ✅ Video Upload to Cloudflare R2
- ✅ Creator Wallets और Payments (Stripe/PayPal)
- ✅ Tip System
- ✅ Payout Management
- ✅ Firebase Firestore Database

---

## 📋 Prerequisites (जरूरी चीजें)

आपको ये accounts चाहिए:

1. **Firebase Account** (FREE)
   - https://firebase.google.com/
   
2. **Cloudflare Account** (FREE tier available)
   - https://www.cloudflare.com/
   
3. **Stripe Account** (for payments)
   - https://stripe.com/
   
4. **Vercel Account** (FREE)
   - https://vercel.com/

5. **GitHub Account** (FREE)
   - https://github.com/

---

## 🚀 Step-by-Step Deployment

### Step 1: Firebase Setup

#### 1.1 Create Firebase Project
1. [Firebase Console](https://console.firebase.google.com/) पर जाएं
2. "Add project" क्लिक करें
3. Project name: `tiktik-videos` (या कोई भी नाम)
4. Google Analytics चालू रखें (optional)
5. प्रोजेक्ट बनाएं

#### 1.2 Enable Firebase Authentication
1. Firebase Console में अपने प्रोजेक्ट को खोलें
2. बाईं तरफ "Build" → "Authentication" पर क्लिक करें
3. "Get started" क्लिक करें
4. "Sign-in method" tab में
5. "Google" को enable करें
6. Support email चुनें
7. Save करें

#### 1.3 Enable Cloud Firestore
1. "Build" → "Firestore Database" पर जाएं
2. "Create database" क्लिक करें
3. **Production mode** चुनें
4. Location चुनें (India: asia-south1 recommended)
5. Enable करें

#### 1.4 Deploy Firestore Security Rules
1. Firebase Console में "Firestore Database" → "Rules" पर जाएं
2. `firestore.rules` file की content copy करें
3. Firebase Rules editor में paste करें
4. "Publish" क्लिक करें

#### 1.5 Get Firebase Config
1. Project Overview → Project Settings
2. "Your apps" section में Web app add करें
3. App nickname: `TikTik Web`
4. Firebase SDK configuration copy करें:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  // ... etc
};
```

5. इन values को अपनी `script.js` में update करें (lines 5-9)

#### 1.6 Get Firebase Admin SDK Credentials
1. Project Settings → "Service accounts" tab
2. "Generate new private key" क्लिक करें
3. JSON file download होगी
4. इस file को सुरक्षित रखें (Git में upload न करें!)
5. `.env` file बनाएं और ये values डालें:

```env
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=firebase-adminsdk@your-project.iam.gserviceaccount.com
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
```

---

### Step 2: Cloudflare R2 Setup

#### 2.1 Create R2 Bucket
1. [Cloudflare Dashboard](https://dash.cloudflare.com/) login करें
2. बाईं sidebar में "R2" पर क्लिक करें
3. "Create bucket" पर क्लिक करें
4. Bucket name: `tiktik-videos`
5. Location: Automatic (recommended)
6. Create करें

#### 2.2 Enable Public Access (CORS)
1. अपनी bucket खोलें
2. "Settings" tab → "CORS Policy" section
3. Add CORS policy:

```json
[
  {
    "AllowedOrigins": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
    "AllowedHeaders": ["*"],
    "ExposeHeaders": []
  }
]
```

4. Save करें

#### 2.3 Get R2 Access Keys
1. R2 overview page पर "Manage R2 API Tokens" क्लिक करें
2. "Create API token" क्लिक करें
3. Token name: `TikTik Upload`
4. Permissions: "Object Read & Write"
5. TTL: Forever (या custom)
6. Create करें

7. Keys copy करें (ये बाद में नहीं मिलेंगे!):
   - Access Key ID
   - Secret Access Key

8. `.env` file में add करें:

```env
R2_ENDPOINT=https://[account-id].r2.cloudflarestorage.com
R2_ACCESS_KEY_ID=your_access_key_id
R2_SECRET_ACCESS_KEY=your_secret_access_key
R2_BUCKET_NAME=tiktik-videos
R2_PUBLIC_URL=https://pub-xxxxx.r2.dev
```

#### 2.4 Setup Public URL (Domain)
1. R2 bucket settings → "Public access"
2. "Connect domain" या "Enable R2.dev subdomain"
3. Public URL को `.env` में `R2_PUBLIC_URL` के रूप में save करें

---

### Step 3: Stripe Payment Setup

#### 3.1 Create Stripe Account
1. [Stripe Dashboard](https://dashboard.stripe.com/register) पर signup करें
2. Business details fill करें
3. Verify email

#### 3.2 Get API Keys
1. Stripe Dashboard → "Developers" → "API keys"
2. **Test mode** में start करें
3. Secret key और Publishable key copy करें

4. `.env` में add करें:

```env
STRIPE_SECRET_KEY=sk_test_xxxxxxxxxxxxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxxxxxxxxxxxx
```

#### 3.3 Setup Webhook (Optional for production)
1. "Developers" → "Webhooks"
2. "Add endpoint"
3. Endpoint URL: `https://your-app.vercel.app/api/stripe-webhook`
4. Events to listen: `payment_intent.succeeded`
5. Webhook secret copy करें और `.env` में add करें

---

### Step 4: GitHub Repository Setup

#### 4.1 Create Repository
1. GitHub पर login करें
2. "New repository" क्लिक करें
3. Repository name: `tiktik-video-platform`
4. Public या Private चुनें
5. Repository बनाएं

#### 4.2 Push Code to GitHub
अपने project folder में terminal खोलें:

```bash
# Git initialize करें
git init

# सभी files add करें
git add .

# First commit
git commit -m "Initial commit - TikTik Video Platform"

# Remote repository add करें
git remote add origin https://github.com/YOUR_USERNAME/tiktik-video-platform.git

# Push करें
git branch -M main
git push -u origin main
```

---

### Step 5: Vercel Deployment

#### 5.1 Connect to Vercel
1. [Vercel Dashboard](https://vercel.com/) पर login करें
2. "New Project" क्लिक करें
3. GitHub repository import करें: `tiktik-video-platform`
4. "Import" क्लिक करें

#### 5.2 Configure Environment Variables
1. "Environment Variables" section में:
2. `.env.example` file की सभी variables add करें:

**Required Variables:**
```
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=...
FIREBASE_PRIVATE_KEY=...
R2_ENDPOINT=...
R2_ACCESS_KEY_ID=...
R2_SECRET_ACCESS_KEY=...
R2_BUCKET_NAME=tiktik-videos
R2_PUBLIC_URL=...
STRIPE_SECRET_KEY=...
STRIPE_PUBLISHABLE_KEY=...
ALLOWED_ORIGIN=https://your-app.vercel.app
```

3. **Important:** Private key को exactly quotes के साथ paste करें:
```
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
```

#### 5.3 Deploy
1. सभी environment variables check करें
2. "Deploy" button क्लिक करें
3. Wait करें deployment complete होने तक (2-3 minutes)
4. Deployment successful होने पर URL मिलेगा: `https://your-app.vercel.app`

#### 5.4 Update Firebase Authorized Domains
1. Firebase Console → Authentication → Settings
2. "Authorized domains" में अपना Vercel URL add करें:
   - `your-app.vercel.app`

---

## ✅ Testing the Deployment

### Test 1: Frontend Access
1. अपनी Vercel URL खोलें: `https://your-app.vercel.app`
2. Website load होना चाहिए
3. UI properly display होनी चाहिए

### Test 2: Google Login
1. "Login with Google" button क्लिक करें
2. Google account select करें
3. Login successful होना चाहिए
4. Profile pic और name display होना चाहिए

### Test 3: Wallet Access
1. Login करने के बाद
2. "Your Channel" page पर जाएं
3. "Wallet" tab क्लिक करें
4. Wallet balance `$0.00` show होना चाहिए

### Test 4: Video Upload (Optional - requires more setup)
1. R2 setup complete होने पर
2. "Upload Video" option try करें
3. Small test video upload करें
4. Success message आना चाहिए

---

## 🔒 Security Best Practices

### 1. Never Commit Secrets
`.gitignore` file check करें:
```
.env
.env.local
.env.production
```

### 2. Use Environment Variables
- सभी API keys `.env` में रखें
- Frontend में कभी secret keys न डालें
- Vercel environment variables में ही secrets रखें

### 3. Firestore Rules Active रखें
- Default rules से कभी न चलाएं
- Production में proper authentication check करें
- Regular security audit करें

### 4. CORS Configuration
- केवल अपने domains को allow करें
- `*` (all origins) production में avoid करें
- Regular CORS policy review करें

---

## 🐛 Common Issues & Solutions

### Issue 1: Firebase Authentication Failed
**Error:** "Firebase not initialized"

**Solution:**
1. Firebase SDK scripts check करें `index.html` में
2. Firebase config में apiKey verify करें
3. Authorized domains में Vercel URL add करें

### Issue 2: R2 Upload Failed
**Error:** "Failed to generate upload URL"

**Solution:**
1. R2 credentials verify करें (Access Key ID, Secret Key)
2. Bucket name correct है check करें
3. CORS policy configured है verify करें

### Issue 3: Stripe Payment Failed
**Error:** "Invalid API key"

**Solution:**
1. Test mode में `sk_test_` key use करें
2. Vercel environment variables में key verify करें
3. Stripe Dashboard में key active है check करें

### Issue 4: Vercel Build Failed
**Error:** "Build failed"

**Solution:**
1. `package.json` present है check करें
2. Node version 18+ use करें
3. Build logs में error details check करें

---

## 📱 Features Guide

### For Users:
1. **Google Login**: Secure authentication via Firebase
2. **Video Browsing**: Watch videos without login
3. **Tip Creators**: Send tips to favorite creators (Stripe)
4. **Transaction History**: View all your tip transactions

### For Creators:
1. **Upload Videos**: Upload to secure Cloudflare R2 storage
2. **Creator Wallet**: Track earnings and balance
3. **Request Payouts**: Withdraw earnings via Stripe/PayPal
4. **Transaction History**: Monitor all incoming tips

---

## 🔄 Updating the App

### Update Code
```bash
# Make changes
git add .
git commit -m "Update: description"
git push origin main
```

Vercel automatically redeploys on git push!

### Update Environment Variables
1. Vercel Dashboard → Project → Settings
2. Environment Variables tab
3. Edit/Add variables
4. Redeploy trigger करें

---

## 💰 Cost Breakdown

### Free Tier Services:
- **Firebase**: 50k reads, 20k writes/day FREE
- **Cloudflare R2**: 10GB storage, 100k requests/month FREE
- **Vercel**: Unlimited deployments FREE (hobby)

### Paid Services:
- **Stripe**: 2.9% + ₹2 per transaction
- **Firebase** (if exceeded): Pay-as-you-go
- **R2** (if exceeded): $0.015/GB storage

**Expected Cost for 1000 users:**
- ~₹500-1000/month (primarily Stripe fees)

---

## 📞 Support & Help

### Documentation:
- Firebase: https://firebase.google.com/docs
- Cloudflare R2: https://developers.cloudflare.com/r2/
- Stripe: https://stripe.com/docs
- Vercel: https://vercel.com/docs

### Common Commands:
```bash
# Local development
python3 -m http.server 5000

# View logs
vercel logs your-app

# Redeploy
vercel --prod
```

---

## ✨ Next Steps

1. **Custom Domain**: Add custom domain in Vercel settings
2. **Analytics**: Add Google Analytics
3. **SEO**: Optimize meta tags
4. **PWA**: Test progressive web app features
5. **Monitoring**: Set up error monitoring (Sentry)

---

**Congratulations! 🎉** आपका TikTik video platform successfully deploy हो गया है!

Questions या issues के लिए GitHub repository में issues create करें।

**Happy Building! 🚀**
