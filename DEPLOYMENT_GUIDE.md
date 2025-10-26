# TikTik Platform - Deployment Guide

## What's Been Fixed ✅

Your TikTik video sharing platform is now fully functional with all critical issues resolved:

1. **Videos Persist Correctly** - All videos (regular, shorts, live streams) are now saved to Cloudflare R2 storage and Firestore database instead of browser localStorage
2. **User Isolation Working** - Videos only appear in the uploader's "My Channel" section
3. **API Security Fixed** - All 4 backend endpoints properly handle CORS and authenticate requests
4. **Deployment Ready** - Configuration files are valid and ready for Vercel deployment

## Architecture Overview

### Frontend
- Pure vanilla JavaScript (no frameworks)
- Firebase Authentication for Google Sign-In
- Real-time video loading from Firestore
- Progressive Web App (PWA) support

### Backend (Serverless Functions)
- **`/api/generate-upload-url.js`** - Creates secure pre-signed URLs for R2 video uploads
- **`/api/process-tip.js`** - Initiates Stripe payments for creator tips
- **`/api/confirm-tip.js`** - Confirms payments and updates creator wallets
- **`/api/request-payout.js`** - Processes payout requests ($10 minimum)

### Data Storage
- **Cloudflare R2** - Video file storage (S3-compatible, no egress fees)
- **Firebase Firestore** - Video metadata, user profiles, wallets, transactions
- **Stripe** - Payment processing for tips and payouts

## Required Environment Variables

Before deploying, you need to set up these secret keys in Vercel:

```bash
# Firebase Configuration
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_CLIENT_EMAIL=your-service-account@your-project.iam.gserviceaccount.com
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"

# Cloudflare R2 Storage
R2_ENDPOINT=https://your-account-id.r2.cloudflarestorage.com
R2_ACCESS_KEY_ID=your-r2-access-key
R2_SECRET_ACCESS_KEY=your-r2-secret-key
R2_BUCKET_NAME=tiktik-videos

# Stripe Payment Processing
STRIPE_SECRET_KEY=sk_live_... or sk_test_...

# Security
ALLOWED_ORIGIN=https://your-domain.vercel.app
```

## How to Deploy to Vercel

### Step 1: Install Vercel CLI (if not already installed)
```bash
npm install -g vercel
```

### Step 2: Login to Vercel
```bash
vercel login
```

### Step 3: Deploy
```bash
vercel
```

Follow the prompts:
- Link to existing project? (Choose "Yes" or "No")
- Project name: `tiktik-platform` (or your preferred name)

### Step 4: Set Environment Variables

In the Vercel dashboard:
1. Go to your project → Settings → Environment Variables
2. Add all the variables listed above
3. Important: For `FIREBASE_PRIVATE_KEY`, make sure to include the quotes and newline characters (`\n`)

### Step 5: Redeploy
```bash
vercel --prod
```

## Testing Your Deployment

After deployment:

1. **Test Video Upload**
   - Sign in with Google
   - Click the + button → Upload Video
   - Select a video file and fill in details
   - Video should upload to R2 and appear in "My Channel"

2. **Test Short Video**
   - Click + button → Create Short
   - Record or upload a vertical video
   - Should appear in Shorts section

3. **Test Live Stream**
   - Click + button → Go Live
   - Allow camera/microphone access
   - Stream should save to R2 after ending

4. **Test Payments** (Stripe must be configured)
   - View another creator's video
   - Click the tip button
   - Use test card: 4242 4242 4242 4242
   - Wallet balance should update

## Important Notes

### Video Upload Flow
1. User selects video → Frontend shows preview
2. Frontend requests signed URL from `/api/generate-upload-url`
3. Frontend uploads directly to R2 using signed URL
4. Frontend saves video metadata to Firestore
5. Video appears immediately in user's channel

### User Isolation
- Each video has an `uploaderId` field
- "My Channel" filters: `videos.where('uploaderId', '==', currentUserId)`
- Home feed shows all public videos
- Users can only edit/delete their own videos

### Security Features
- Firebase ID tokens verify all API requests
- Pre-signed R2 URLs expire after 15 minutes
- CORS headers restrict API access
- No API keys exposed in frontend code

### Storage Costs
- **Cloudflare R2**: Free up to 10GB storage, $0.015/GB after
- **Firebase**: Free tier includes 1GB Firestore storage
- **Vercel**: Free tier includes 100GB bandwidth/month

## Troubleshooting

### Videos Not Appearing After Upload
- Check browser console for errors
- Verify Firebase and R2 credentials in Vercel environment variables
- Ensure `ALLOWED_ORIGIN` matches your deployment URL

### CORS Errors
- Update `ALLOWED_ORIGIN` to match your Vercel domain
- Redeploy after changing environment variables

### Payment Errors
- Verify Stripe keys are set correctly
- Check Stripe dashboard for payment logs
- Ensure webhook URL is configured (if using webhooks)

### Live Streaming Issues
- Browser must support MediaRecorder API (Chrome, Firefox, Edge)
- User must grant camera/microphone permissions
- Check R2 storage quota hasn't been exceeded

## Local Development

To test locally before deploying:

```bash
# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Edit .env with your actual credentials

# Start development server
vercel dev
```

The app will be available at `http://localhost:3000`

## Next Steps

After successful deployment:

1. **Configure Firebase Security Rules** - Restrict Firestore access
2. **Set Up Stripe Webhooks** - For production payment confirmations
3. **Configure Custom Domain** - Add your own domain in Vercel
4. **Enable Analytics** - Track user engagement
5. **Add Email Notifications** - Notify creators of tips/payouts

## Support

If you encounter any issues:
- Check Vercel deployment logs
- Review browser console for frontend errors
- Verify all environment variables are set correctly
- Ensure Firebase, R2, and Stripe accounts are active

Your platform is ready to go live! 🚀
