# TikTik Video Sharing Platform

## Overview

TikTik is a full-featured video sharing platform that combines YouTube and TikTok-style functionality. The platform enables users to upload regular videos, create short-form vertical content, live stream with real-time recording, and monetize their content through creator tips and payouts. Built with vanilla JavaScript on the frontend and serverless functions on the backend, TikTik emphasizes simplicity, security, and scalability while providing a rich user experience with PWA support, dark/light themes, and real-time video management.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack**: Pure vanilla JavaScript (ES6+) with HTML5 and CSS3. No frameworks are used to minimize bundle size and complexity. Firebase SDK (v10.7.1 compat) provides authentication and real-time database capabilities.

**Authentication Flow**: Firebase Authentication handles Google Sign-In. The frontend fetches Firebase configuration securely from `/api/get-config` endpoint rather than hardcoding credentials. After authentication, Firebase ID tokens are included in Authorization headers for all API calls.

**State Management**: Class-based architecture centered on `TikTikApp` class. Application state includes current video, page navigation, sidebar state, user settings (theme), watch history, liked/saved videos, comments, channel data, and subscriptions. State persists to localStorage for non-sensitive data and Firestore for shared/persistent data.

**Video Management**: Three video types are supported - regular uploads, short-form vertical videos, and live streams. All videos upload directly to Cloudflare R2 using pre-signed URLs (15-minute expiration). The MediaRecorder API captures live streams with camera/microphone access. User isolation ensures "My Channel" displays only videos where `uploaderId` matches the authenticated user.

**Progressive Web App**: Service worker (`sw.js`) implements cache-first strategy for static assets (HTML, CSS, JS, fonts). The manifest.json defines app metadata with SVG icons for different resolutions. Supports installation on mobile and desktop with offline viewing capabilities.

**UI Design**: YouTube-inspired responsive layout with fixed header (60px), collapsible sidebar (250px expanded, 70px collapsed), and flexible content grid. CSS custom properties enable dark/light theme switching. All interactions use event delegation for performance.

### Backend Architecture

**Serverless Functions**: Four Node.js serverless functions deployed on Vercel handle all backend operations. Each function is stateless, auto-scaling, and executes independently with 1024MB memory allocation.

**API Endpoints**:
- `/api/get-config.js` - Returns Firebase client configuration (public values only)
- `/api/generate-upload-url.js` - Creates S3-compatible pre-signed URLs for R2 uploads with 15-minute expiration
- `/api/process-tip.js` - Initiates Stripe PaymentIntent for creator tips
- `/api/confirm-tip.js` - Verifies payment success and atomically updates sender/receiver wallet balances
- `/api/request-payout.js` - Processes creator payout requests with $10 minimum threshold

**Authentication Strategy**: Firebase Admin SDK verifies ID tokens on every API request (except `/api/get-config`). Tokens are extracted from `Authorization: Bearer <token>` headers. Invalid/missing tokens return 401 errors. Decoded tokens provide `uid` for user identification.

**Security Model**:
- All secrets stored as environment variables (Firebase service account, R2 credentials, Stripe keys)
- CORS headers restrict access to allowed origins
- Pre-signed URLs provide time-limited, secure storage access
- No API keys exposed in frontend code
- Firestore security rules enforce data isolation at database level

**Error Handling**: All endpoints return structured JSON responses with appropriate HTTP status codes. CORS preflight requests (OPTIONS) are handled separately. Method validation ensures only expected HTTP methods are processed.

### Data Storage

**Cloudflare R2**: S3-compatible object storage for video files. Benefits include no egress fees and global edge distribution. The AWS SDK S3 Client (`@aws-sdk/client-s3`) interacts with R2. Pre-signed URLs enable direct browser-to-storage uploads without proxying through backend.

**Firebase Firestore**: NoSQL document database stores:
- Video metadata (uploaderId, type, timestamp, title, description, URL)
- User profiles and channel data
- Creator wallets with balance tracking
- Transaction history (tips, payouts)
- Comments and engagement metrics

**Data Consistency**: Atomic Firestore transactions ensure wallet balance updates are consistent during tip confirmations. The `runTransaction` method prevents race conditions when multiple tips occur simultaneously.

**Schema Design**: Videos collection uses uploaderId as filterable field for user isolation. Wallets collection uses userId as document ID for fast lookups. Transactions collection includes indexed timestamp for chronological ordering.

## External Dependencies

**Firebase Services**:
- Firebase Authentication - Google OAuth provider for user sign-in
- Firebase Firestore - Real-time NoSQL database for metadata and user data
- Firebase Admin SDK (v12.7.0) - Server-side authentication verification and database access

**Cloudflare R2**:
- S3-compatible object storage for video files
- Accessed via AWS SDK S3 Client (v3.917.0)
- Requires R2 endpoint URL, access key ID, secret access key, and bucket name

**Stripe**:
- Payment processing for creator tips (v14.25.0)
- PaymentIntent API for secure payment flow
- Supports test and live environments via separate API keys

**AWS SDK**:
- `@aws-sdk/client-s3` (v3.917.0) - S3 operations for R2 interaction
- `@aws-sdk/s3-request-presigner` (v3.917.0) - Generates pre-signed URLs with expiration

**Vercel Platform**:
- Serverless function hosting with automatic deployment
- Edge network for global distribution
- Environment variable management for secrets

**Third-Party CDNs**:
- Font Awesome 6.0.0 - Icon library for UI elements
- Firebase SDK 10.7.1 (compat) - Loaded from Google CDN

**Environment Variables Required**:
- `FIREBASE_PROJECT_ID` - Firebase project identifier
- `FIREBASE_CLIENT_EMAIL` - Service account email
- `FIREBASE_PRIVATE_KEY` - Service account private key (newlines escaped)
- `FIREBASE_API_KEY` - Client API key (public)
- `FIREBASE_AUTH_DOMAIN` - Authentication domain (public)
- `R2_ENDPOINT` - Cloudflare R2 endpoint URL
- `R2_ACCESS_KEY_ID` - R2 access credentials
- `R2_SECRET_ACCESS_KEY` - R2 secret credentials
- `R2_BUCKET_NAME` - Target storage bucket
- `STRIPE_SECRET_KEY` - Stripe API key (test or live)
- `ALLOWED_ORIGIN` - CORS allowed origin (optional, defaults to *)