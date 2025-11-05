# TikTik Video Sharing Platform

## Overview

TikTik is a video sharing platform similar to YouTube, built as a Progressive Web Application (PWA). The application allows users to discover, watch, search, and share videos with Google authentication. It features a responsive design with light/dark theme support, offline capabilities through service workers, and real-time video management through Firebase.

The platform is designed as a single-page application with a Node.js/Express backend serving the frontend and managing Firebase configuration securely through environment variables.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Single Page Application (SPA) Pattern**
- Pure vanilla JavaScript implementation without frameworks
- DOM manipulation for dynamic content rendering
- Client-side routing for navigation between pages (home, search, video player)
- Responsive CSS with CSS custom properties for theming

**Progressive Web App (PWA)**
- Service Worker (`sw.js`) for offline support and caching
- Network-first strategy for HTML/JS files (development-friendly)
- Cache-first strategy for static assets (CSS, fonts, images)
- Web App Manifest for installability on mobile devices
- Version-based cache management (`tiktik-v1.3.0`)

**Design System**
- CSS custom properties for theme variables (light/dark mode support)
- Modular component-based styling
- Mobile-first responsive design
- FontAwesome icons for UI elements

### Backend Architecture

**Express.js Server**
- Minimal Node.js server serving static files
- Single API endpoint (`/api/get-config`) for Firebase configuration
- CORS enabled for cross-origin requests
- Catch-all route serving `index.html` for SPA routing

**Security Pattern**
- Firebase credentials stored in environment variables (never exposed to frontend)
- Backend-only configuration endpoint prevents API key exposure in client code
- Environment variables validated before serving config

### Authentication & Authorization

**Firebase Authentication**
- Google Sign-in provider integration
- Client-side authentication state management through `firebaseAuth`
- Auth state listener for persistent login sessions
- User profile management (display name, photo URL)

**Authentication Flow**
1. Frontend requests Firebase config from backend
2. Firebase SDK initializes with server-provided credentials
3. Google OAuth popup for user authentication
4. Firebase manages authentication tokens and sessions
5. Auth state changes trigger UI updates

### Data Storage Solutions

**Firebase Firestore**
- NoSQL cloud database for video metadata and user data
- Real-time synchronization capabilities
- Collections likely include: videos, users, comments
- Client-side Firestore SDK for direct database access

**Local Storage**
- Browser localStorage for offline data persistence
- Caching user preferences (theme selection)
- Temporary storage for unsynchronized data

**Service Worker Cache**
- Static asset caching for offline functionality
- Versioned cache storage for updates
- Separate caching strategies for different resource types

### Video Management

**Video Data Structure** (inferred)
- Video metadata stored in Firestore
- Video files likely hosted externally (Firebase Storage or CDN)
- Search functionality for video discovery
- Video player integration with standard HTML5 video element

**Content Features**
- Video upload and sharing capabilities
- Comment system integration
- Search with keyword matching
- Video categorization and discovery

## External Dependencies

### Third-Party Services

**Firebase Platform**
- **Firebase Authentication**: Google OAuth sign-in provider
- **Firebase Firestore**: NoSQL database for application data
- **Firebase Storage**: (Likely) Video file hosting and delivery
- **Firebase SDK Version**: 10.7.1 (compat version)

**Required Firebase Configuration**:
- `FIREBASE_API_KEY`: API access key
- `FIREBASE_AUTH_DOMAIN`: Authentication domain
- `FIREBASE_PROJECT_ID`: Project identifier
- `FIREBASE_STORAGE_BUCKET`: Storage bucket URL
- `FIREBASE_MESSAGING_SENDER_ID`: Cloud messaging ID
- `FIREBASE_APP_ID`: Application identifier

### CDN Resources

**FontAwesome 6.0.0**
- Icon library for UI elements
- Loaded from CloudFlare CDN
- Used for menu icons, play buttons, search icons

### Node.js Dependencies

**Express.js (^4.21.2)**
- Web application framework
- Static file serving
- API routing

**CORS (^2.8.5)**
- Cross-Origin Resource Sharing middleware
- Enables frontend-backend communication

**Runtime Requirements**
- Node.js >= 18.0.0

### Browser APIs

**Service Worker API**
- Offline functionality
- Background sync capabilities
- Cache management

**Web App Manifest**
- PWA installation
- App metadata and icons
- Display modes and orientation settings

**LocalStorage API**
- Client-side data persistence
- Theme preferences storage