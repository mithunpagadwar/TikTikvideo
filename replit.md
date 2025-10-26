# TikTik Video Sharing Platform

## Overview

TikTik is a complete video sharing platform similar to YouTube/TikTok, built with vanilla JavaScript, HTML5, and CSS3. The platform features video uploads, creator monetization through tips and payouts, Google authentication via Firebase, and cloud storage via Cloudflare R2. It's designed as a serverless application deployed on Vercel with a Progressive Web App (PWA) architecture.

The application provides a full-featured video sharing experience including video uploads, shorts (vertical videos), live streaming placeholders, channel management, subscriptions, comments, likes/dislikes, watch history, and a complete payment system for creator earnings.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Client-Side Framework**: Pure vanilla JavaScript (ES6+) with no frontend frameworks - promotes simplicity and reduces dependencies while maintaining full functionality.

**UI/UX Design Pattern**: YouTube-inspired interface with familiar navigation patterns, header-sidebar-content layout, and responsive grid-based video display. Supports both dark and light themes through CSS custom properties.

**State Management**: Class-based JavaScript architecture (`TikTikApp` class) with localStorage for client-side persistence of user data including watch history, liked videos, saved videos, comments, uploaded videos, channel data, shorts, live streams, and subscriptions.

**Progressive Web App (PWA)**: Implements service worker (`sw.js`) for offline support, caching strategies, and installable app experience. Includes manifest.json for app metadata, icons, and display preferences.

**Authentication Flow**: Firebase Authentication SDK (compat version) integrated for Google Sign-In. Client-side Firebase initialization with token-based authentication that's verified server-side for sensitive operations.

### Backend Architecture

**Serverless Functions**: Vercel Serverless Functions (Node.js runtime) handle all backend logic. This eliminates the need for traditional server infrastructure and provides automatic scaling.

**API Endpoints**:
- `/api/generate-upload-url.js` - Generates pre-signed URLs for secure video uploads to Cloudflare R2
- `/api/process-tip.js` - Handles tip payment transactions via Stripe
- `/api/confirm-tip.js` - Confirms successful payments and updates wallet balances
- `/api/request-payout.js` - Processes creator payout requests

**Security Model**: All sensitive operations (file uploads, payments, wallet updates) are handled server-side. Firebase ID tokens are verified on the backend before executing privileged operations. API keys and secrets are never exposed to the frontend.

**CORS Configuration**: Restricted to specific allowed origins to prevent unauthorized access. Origins are configurable via environment variables.

### Data Storage

**Primary Database**: Firebase Firestore for structured data including:
- User profiles and authentication data
- Video metadata (title, description, channel, views, etc.)
- Creator wallet balances and transaction history
- Comments and engagement metrics
- Channel information and subscriptions

**Video Storage**: Cloudflare R2 (S3-compatible object storage) for video file hosting. Access managed through AWS SDK with pre-signed URLs for secure uploads.

**Client-Side Storage**: Browser localStorage for non-critical data caching:
- User preferences and settings
- Watch history
- Liked/saved videos cache
- Temporary data for offline functionality

**Rationale**: Firebase Firestore provides real-time capabilities and easy integration with Firebase Auth. Cloudflare R2 offers cost-effective video storage with S3 compatibility. localStorage enables offline-first experiences and reduces server load.

### Payment Processing

**Payment Provider**: Stripe for handling monetary transactions (tips and payouts). Chosen for robust API, strong security, and widespread adoption.

**Wallet System**: Each creator has a virtual wallet stored in Firestore. Incoming tips increment wallet balance, payout requests decrement it. All transactions are logged for transparency and auditing.

**Transaction Flow**:
1. User initiates tip payment on frontend
2. Backend creates Stripe payment intent
3. User completes payment through Stripe
4. Webhook or confirmation endpoint updates Firestore wallet balances
5. Transaction history is recorded

**Payout Mechanism**: Creators can request payouts when balance meets minimum threshold. Requests are processed via Stripe Connect or PayPal integration (configurable).

## External Dependencies

### Third-Party Services

**Firebase** (Google Cloud):
- Firebase Authentication - Google Sign-In provider
- Firebase Firestore - NoSQL document database
- Firebase Admin SDK (server-side) - Backend access to Firebase services
- Project ID: `tiktikvideos-4e8e7`

**Cloudflare R2**:
- S3-compatible object storage for video files
- Accessed via AWS SDK (`@aws-sdk/client-s3`, `@aws-sdk/s3-request-presigner`)
- Configured with custom endpoint for R2 compatibility

**Stripe**:
- Payment processing for tips
- Potential Stripe Connect for payouts
- Requires secret key configuration via environment variables

**Vercel**:
- Hosting platform for static frontend
- Serverless function execution
- Automatic deployments from Git
- Environment variable management

### NPM Packages

**Production Dependencies**:
- `@aws-sdk/client-s3` (^3.917.0) - Cloudflare R2 client
- `@aws-sdk/s3-request-presigner` (^3.917.0) - Generate pre-signed URLs
- `firebase-admin` (^12.7.0) - Server-side Firebase operations
- `stripe` (^14.25.0) - Payment processing

### CDN Resources

- Font Awesome 6.0.0 - Icon library
- Firebase SDK 10.7.1 (compat) - Client-side Firebase

### Environment Variables Required

- `STRIPE_SECRET_KEY` - Stripe API secret key
- `FIREBASE_PROJECT_ID` - Firebase project identifier
- `FIREBASE_CLIENT_EMAIL` - Service account email
- `FIREBASE_PRIVATE_KEY` - Service account private key
- `CLOUDFLARE_R2_ACCESS_KEY_ID` - R2 access credentials
- `CLOUDFLARE_R2_SECRET_ACCESS_KEY` - R2 secret key
- `CLOUDFLARE_R2_BUCKET_NAME` - Target bucket name
- `CLOUDFLARE_R2_ENDPOINT` - R2 endpoint URL
- `ALLOWED_ORIGIN` - CORS allowed origin(s)

### Browser APIs

- Service Worker API - PWA offline functionality
- Local Storage API - Client-side data persistence
- MediaDevices API - Camera/microphone access for live streaming
- Fetch API - HTTP requests to backend