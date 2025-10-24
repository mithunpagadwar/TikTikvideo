# TikTik - Video Sharing Platform

## Overview

TikTik is a modern video sharing platform inspired by YouTube and TikTok, built as a Progressive Web Application (PWA). The application provides a comprehensive video sharing experience with features including video playback, user interactions (likes, comments), channel management, shorts (vertical videos), live streaming capabilities, and search functionality. The platform is designed to work offline and provides a responsive experience across all device types.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Single-Page Application (SPA) Pattern**
- The application uses vanilla JavaScript with a class-based architecture (`TikTikApp` class) to manage all application state and interactions
- No frontend framework dependencies (React-free implementation despite the README mentioning React)
- Client-side routing handled through JavaScript page state management
- DOM manipulation performed directly through vanilla JavaScript methods

**Progressive Web App (PWA) Design**
- Service Worker implementation (`sw.js`) provides offline-first capabilities
- Cache-first strategy for static assets with network fallback
- Web App Manifest defines installability and platform integration
- Supports both mobile and desktop form factors

**Responsive Design System**
- CSS custom properties (CSS variables) enable theme switching (light/dark modes)
- Mobile-first responsive layout with collapsible sidebar
- Flexible grid system for video thumbnails and content layout
- Touch-friendly interface elements

### Data Storage Solutions

**Client-Side Storage with LocalStorage**
- All user data persisted entirely in browser LocalStorage
- No backend database or server-side persistence
- Storage entities include:
  - User settings and preferences
  - Watch history
  - Liked videos
  - Saved videos
  - Comments
  - User-uploaded videos
  - Channel data
  - Shorts content
  - Live stream data
  - Subscriptions
  - User channels

**In-Memory State Management**
- Application state managed through class properties in `TikTikApp`
- State includes current video, current page, sidebar state, and media permissions
- Data loaded from LocalStorage on initialization and saved on updates

### Media Handling

**Video Playback**
- HTML5 native video player for standard videos
- Custom controls and player interface
- Support for multiple video sources (sample videos from external CDN)
- Camera stream integration for live recording capabilities

**Media Capture**
- WebRTC getUserMedia API for camera and microphone access
- Real-time video preview during recording
- In-browser video capture without server upload (local storage)

### User Interface Components

**Theming System**
- CSS custom properties enable dynamic theme switching
- Data attribute `[data-theme="dark"]` toggles color schemes
- Smooth transitions between theme states
- Persistent theme preference via LocalStorage

**Navigation Structure**
- Fixed header with logo, search, and action buttons
- Collapsible sidebar for navigation (250px expanded, 70px collapsed)
- Multi-page simulation through content area swapping
- Search functionality with real-time filtering

## External Dependencies

### Third-Party CDN Resources

**Font Awesome 6.0.0**
- Purpose: Icon library for UI elements
- Source: `cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css`
- Used for: Navigation icons, action buttons, social interaction icons

**Unsplash Images API**
- Purpose: Placeholder images for video thumbnails and user avatars
- Source: `images.unsplash.com`
- Used for: Demo content thumbnails and profile pictures

**Google Cloud Storage**
- Purpose: Sample video hosting
- Source: `commondatastorage.googleapis.com/gtv-videos-bucket/sample/`
- Used for: Demo video files (BigBuckBunny.mp4, ElephantsDream.mp4)

### Browser APIs

**Service Worker API**
- Enables offline functionality and caching strategies
- Lifecycle events: install, fetch, activate
- Cache management for static assets

**LocalStorage API**
- Persistent client-side data storage
- Stores user preferences, history, and uploaded content

**MediaDevices API (WebRTC)**
- Camera and microphone access for video recording
- getUserMedia for media stream capture
- Real-time media preview capabilities

### Deployment Platform

**Vercel** (Configuration Present)
- Static site hosting configuration in `vercel.json`
- Custom headers for caching strategy
- Service Worker routing support
- Clean URLs without trailing slashes