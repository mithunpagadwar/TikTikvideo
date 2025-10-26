# TikTik Video Sharing Platform

## Overview

TikTik is a full-featured video sharing platform combining elements from YouTube and TikTok. Built with vanilla JavaScript, the platform enables users to upload videos, live stream, create short-form vertical videos, and monetize their content through creator wallets and tips. The application uses Firebase for authentication and data storage, Cloudflare R2 for video hosting, and Stripe for payment processing.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack**: Pure vanilla JavaScript (ES6+) with HTML5 and CSS3. No frontend frameworks are used, promoting simplicity and reducing bundle size while maintaining full functionality.

**UI Design Pattern**: YouTube-inspired interface with a fixed header, collapsible sidebar, and responsive content grid. The application supports dark/light themes through CSS custom properties and data attributes. The layout uses a three-section structure: header (navigation/search), sidebar (menu items), and main content area (video grid/player).

**State Management**: Class-based architecture centered around the `TikTikApp` class. Client-side state is persisted to localStorage for watch history, liked videos, saved videos, comments, channel data, and subscriptions. User-uploaded videos are stored in Firestore with references managed in the application state.

**Authentication**: Firebase Authentication SDK (compat version 10.7.1) handles Google Sign-In. The client initializes Firebase with project configuration and manages auth state. All authenticated API requests include Firebase ID tokens in Authorization headers for server-side verification.

**Progressive Web App**: Service worker (`sw.js`) provides offline capabilities through cache-first strategies for static assets. The manifest.json defines app metadata, icons (SVG-based), screenshots, and PWA features. Users can install the app on mobile and desktop devices.

**Video Features**:
- Regular video uploads with metadata stored in Firestore
- Short videos (vertical format) with loop capabilities
- Live streaming with camera/microphone access via MediaRecorder API
- All videos uploaded to Cloudflare R2 through pre-signed URLs
- User isolation: "My Channel" shows only the uploader's content
- Videos tagged with uploaderId, type (live/short/regular), and timestamps

### Backend Architecture

**Serverless Platform**: Vercel Serverless Functions running Node.js (v18+). Each API endpoint is a separate function deployed automatically, providing automatic scaling and zero infrastructure management.

**API Endpoints**:
- `/api/generate-upload-url.js`: Creates pre-signed URLs for Cloudflare R2 uploads with 15-minute expiration. Verifies Firebase ID token before generating URLs.
- `/api/process-tip.js`: Initiates Stripe payment flow by creating PaymentIntents. Validates tip amounts and user authentication.
- `/api/confirm-tip.js`: Confirms successful Stripe payments and atomically updates both tipper and creator wallet balances in Firestore.
- `/api/request-payout.js`: Processes creator payout requests with minimum threshold validation ($10). Creates payout records in Firestore for administrative processing.

**Authentication Model**: Firebase Admin SDK verifies ID tokens on every API request. Tokens are extracted from Authorization headers (Bearer scheme). Invalid or missing tokens return 401 errors. User IDs from verified tokens are used for authorization checks.

**Security Approach**:
- All sensitive credentials (R2 keys, Stripe keys, Firebase service account) stored as environment variables
- CORS headers restrict API access to specific origins (configurable via ALLOWED_ORIGIN)
- Pre-signed URLs prevent direct storage access
- Firestore security rules enforce user data isolation
- No API keys or secrets exposed in frontend code

**Error Handling**: API endpoints use try-catch blocks with appropriate HTTP status codes (400 for validation, 401 for auth, 405 for wrong methods, 500 for server errors). All responses include JSON with error messages.

### Data Storage

**Firebase Firestore**: NoSQL document database storing:
- **videos collection**: Video metadata including uploaderId, title, description, R2 URL, type (live/short/regular), timestamps, view counts
- **userWallets collection**: Creator wallet balances tracked in cents, initialized on first tip
- **transactions collection**: Payment history including tip amounts, payer/recipient IDs, timestamps, and Stripe payment IDs
- **User profiles**: Authentication data managed by Firebase Auth

**Data Isolation**: Videos filtered by uploaderId when displaying "My Channel". Public feeds show videos based on visibility settings. Security rules prevent unauthorized reads/writes.

**Cloudflare R2**: S3-compatible object storage for video files. Videos uploaded via pre-signed URLs with automatic bucket management. R2 chosen for cost-effectiveness (no egress fees) and S3 compatibility (allows using AWS SDK).

**Local Storage**: Client-side persistence for non-critical data like UI preferences (theme, sidebar state), watch history, and temporary state. Serves as fallback when offline.

### Payment Integration

**Stripe Integration**: 
- PaymentIntent API for secure card payments
- Server-side payment confirmation prevents fraud
- Webhook-ready architecture (confirmation endpoint)
- Currency handled in cents to avoid floating-point errors
- Minimum payout threshold enforced ($10)

**Wallet System**:
- Each creator has a wallet document in Firestore
- Balance atomically updated on successful tips
- Transaction records created for audit trail
- Payout requests decrement balance and create pending records

**Payment Flow**:
1. User initiates tip via frontend
2. `/api/process-tip` creates Stripe PaymentIntent
3. Frontend confirms payment with Stripe.js
4. `/api/confirm-tip` verifies payment and updates wallets
5. Transaction record created in Firestore

## External Dependencies

### Cloud Services

**Firebase (Google Cloud)**:
- Authentication service for Google Sign-In
- Firestore NoSQL database for application data
- Free tier supports initial scaling
- Project ID: `tiktikvideos-4e8e7`
- Requires service account credentials (project ID, client email, private key)

**Cloudflare R2**:
- S3-compatible object storage for video files
- Accessed via AWS SDK v3 (`@aws-sdk/client-s3`, `@aws-sdk/s3-request-presigner`)
- Requires endpoint URL, access key ID, and secret access key
- Region set to "auto" for automatic routing

**Vercel**:
- Hosting platform for static frontend and serverless functions
- Automatic deployments from Git
- Environment variable management
- Free tier available for small projects

### Payment Processing

**Stripe**:
- Payment processing for creator tips
- Node.js SDK version 14.25.0
- Requires secret key for server-side operations
- PaymentIntent API for card payments
- Future support for payouts via Stripe Connect

### NPM Dependencies

**Production**:
- `firebase-admin@^12.7.0`: Server-side Firebase SDK for auth and Firestore
- `@aws-sdk/client-s3@^3.917.0`: S3 client for R2 storage operations
- `@aws-sdk/s3-request-presigner@^3.917.0`: Generates pre-signed URLs
- `stripe@^14.25.0`: Payment processing API

**Development**:
- `vercel@^33.0.0`: CLI for local development and deployment

### Frontend Libraries

**CDN-hosted**:
- Firebase JavaScript SDK 10.7.1 (app-compat, auth-compat, firestore-compat)
- Font Awesome 6.0.0 for icons

### Environment Variables Required

```
FIREBASE_PROJECT_ID
FIREBASE_CLIENT_EMAIL
FIREBASE_PRIVATE_KEY
R2_ENDPOINT
R2_ACCESS_KEY_ID
R2_SECRET_ACCESS_KEY
R2_BUCKET_NAME
STRIPE_SECRET_KEY
ALLOWED_ORIGIN
```

### Browser APIs Used

- MediaRecorder API (live streaming)
- getUserMedia API (camera/mic access)
- Service Worker API (PWA offline support)
- LocalStorage API (client-side persistence)
- Fetch API (HTTP requests)