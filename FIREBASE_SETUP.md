# Firebase Setup Instructions (हिंदी)

## Firebase Firestore Security Rules Setup

आपकी TikTik website पर videos upload और display करने के लिए, आपको Firebase Console में Firestore security rules को configure करना होगा।

### Steps:

1. **Firebase Console खोलें**
   - https://console.firebase.google.com पर जाएं
   - अपना project "tiktik-video-2de07" select करें

2. **Firestore Database में जाएं**
   - Left sidebar में "Firestore Database" पर click करें
   - अगर Firestore enable नहीं है, तो "Create database" पर click करें

3. **Rules Tab खोलें**
   - Firestore page पर "Rules" tab पर click करें

4. **Security Rules को Update करें**
   
   **Development/Testing के लिए (अभी के लिए recommended):**
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
   
   **या Authenticated Users के लिए (More Secure):**
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

5. **Publish करें**
   - "Publish" button पर click करें

## Google Authentication Setup

Google Sign-in enable करने के लिए:

1. **Firebase Console में जाएं**
2. **Authentication** section में जाएं
3. **Sign-in method** tab पर click करें
4. **Google** provider को enable करें
5. अपना **Project public-facing name** और **Support email** add करें
6. **Save** करें

## Indexes (Optional - अगर complex queries की जरूरत हो)

अगर आपको videos को filter करने की जरूरत है, तो आपको composite indexes create करने की जरूरत हो सकती है:

1. **Firestore** → **Indexes** tab में जाएं
2. जब भी console में index error आए, वहां दिए गए link पर click करके automatically index create हो जाएगा

## Current Status

✅ Firebase credentials configured (Environment Variables में)
✅ Server running and serving Firebase config
✅ Sample videos loading on home page
⚠️ Firestore security rules को configure करना बाकी है (videos upload के लिए जरूरी)
⚠️ Google Sign-in enable करना बाकी है (Firebase Console में)

## Testing

Website को test करने के लिए:
1. "Login with Google" button पर click करें
2. अगर Firebase Authentication enable है, तो Google sign-in popup खुलेगा
3. Sign-in करने के बाद, आप videos upload कर सकेंगे
