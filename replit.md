# TikTik - Video Sharing Platform

## Overview

TikTik is a video sharing platform inspired by YouTube, built as a Progressive Web Application (PWA). The application allows users to discover, watch, upload, and share videos while engaging with content through comments and social features. It's designed as a single-page application with client-side rendering, featuring a responsive interface that works across desktop and mobile devices.

The platform emphasizes offline-first capabilities through service workers, real-time data synchronization via Firebase, and a clean, modern user interface with dark/light theme support.

## User Preferences

Preferred communication style: Simple, everyday language.

## Recent Changes

### November 5, 2025
- **Google Sign-in Integration**: Added Firebase Authentication with Google Sign-in functionality
  - Profile dropdown menu with YouTube-style interface
  - User authentication state management
  - Login/logout functionality with profile display
- **Sample Videos**: Added 8 demo videos to home page for initial content display
- **Server Setup**: Created Node.js Express backend to serve Firebase config securely
  - Firebase credentials stored in environment variables
  - `/api/get-config` endpoint for secure config delivery
- **Service Worker Updates**: Modified caching strategy for development
  - Network-first for HTML and JavaScript files
  - Cache-first for static assets (CSS, images, fonts)
  - Automatic cache invalidation on updates
- **Bug Fixes**: Resolved video loading issues caused by service worker caching

## System Architecture

### Frontend Architecture

**Single-Page Application (SPA) Design**
- Pure JavaScript implementation without frameworks (vanilla JS)
- Client-side routing handled entirely in the browser
- Dynamic DOM manipulation for all UI updates and page transitions
- Component-based thinking applied without a formal framework

**Progressive Web App (PWA) Features**
- Service worker (`sw.js`) for offline support and caching
- Web app manifest for installability on mobile devices
- Cache-first strategy for static assets (CSS, fonts, images)
- Network-first strategy for dynamic content (HTML, JavaScript) to ensure fresh code during development

**Theming System**
- CSS custom properties (CSS variables) for dynamic theming
- Light and dark mode support via `data-theme` attribute
- Persistent theme preference (likely stored in localStorage)
- Smooth transitions between theme changes

**Responsive Design**
- Mobile-first approach with viewport meta tags
- Flexible grid layouts using CSS variables for sizing
- Collapsible sidebar navigation (250px expanded, 70px collapsed)
- Touch-friendly interface optimized for mobile devices

### Backend Architecture

**Express.js Server**
- Minimal Node.js/Express backend serving static files
- Single API endpoint (`/api/get-config`) for Firebase configuration delivery
- Catch-all route that serves `index.html` for client-side routing support
- CORS enabled for cross-origin requests

**Security Pattern**
- Firebase credentials stored as environment variables on the server
- Configuration delivered to client via secure API endpoint
- Prevents exposure of sensitive API keys in frontend code
- Frontend fetches config at runtime rather than having it hardcoded

**Static File Serving**
- Express serves all static assets (HTML, CSS, JS) from the root directory
- No build process or transpilation required
- Direct file serving for development simplicity

### Data Storage

**Firebase Firestore**
- NoSQL document database for storing video metadata, user profiles, and comments
- Real-time data synchronization between clients
- Collection-based data structure (likely collections for videos, users, comments)
- Client-side SDK integration via Firebase JavaScript SDK v10.7.1

**Firebase Authentication**
- User authentication and authorization
- Auth state management in client application
- Persistent sessions across page reloads
- Auth state listener pattern for reactive UI updates

**Local Storage**
- Browser localStorage for client-side data persistence
- Used for caching user preferences (theme, etc.)
- Fallback data storage when offline
- Complements Firestore for hybrid online/offline experience

**Service Worker Cache**
- Cache API for storing static assets offline
- Versioned cache (`tiktik-v1.3.0`) for cache invalidation
- Selective caching strategy based on resource type
- Enables core functionality when network is unavailable

### External Dependencies

**Firebase Services**
- **Firebase App SDK** (v10.7.1): Core Firebase initialization
- **Firebase Auth SDK** (v10.7.1): User authentication and session management
- **Firebase Firestore SDK** (v10.7.1): Real-time NoSQL database
- Configuration includes: API Key, Auth Domain, Project ID, Storage Bucket, Messaging Sender ID, App ID

**CDN Resources**
- **Font Awesome 6.0.0**: Icon library for UI elements
- Loaded from CloudFlare CDN for performance
- Used throughout the interface for navigation, actions, and visual indicators

**Node.js Dependencies**
- **Express** (v4.18.2): Web server framework
- **CORS** (v2.8.5): Cross-Origin Resource Sharing middleware
- Minimal dependency footprint for lightweight deployment

**Browser APIs**
- **Service Worker API**: Offline functionality and caching
- **Cache API**: Asset storage for PWA features
- **LocalStorage API**: Client-side data persistence
- **Fetch API**: HTTP requests to backend and Firebase

**Environment Requirements**
- Node.js >= 18.0.0
- Modern browser with ES6+ support
- Service Worker and PWA capability
- Firebase project with Firestore and Authentication enabled