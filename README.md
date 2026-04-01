# TYPO — Developer Social Network

A social platform for developers. Share code, showcase projects, connect with peers.

## Stack

- **React 18** + TypeScript + Vite
- **Firebase** (Auth + Firestore)
- **Tailwind CSS v3**
- **Framer Motion**
- **Deployed on Vercel**

## Deploy to Vercel

1. Push this repo to GitHub
2. Import into [vercel.com](https://vercel.com)
3. Set these **Environment Variables** in the Vercel dashboard:

| Variable | Where to find it |
|---|---|
| `VITE_FIREBASE_API_KEY` | Firebase Console → Project Settings → General |
| `VITE_FIREBASE_AUTH_DOMAIN` | Same |
| `VITE_FIREBASE_PROJECT_ID` | Same |
| `VITE_FIREBASE_STORAGE_BUCKET` | Same |
| `VITE_FIREBASE_MESSAGING_SENDER_ID` | Same |
| `VITE_FIREBASE_APP_ID` | Same |
| `VITE_FIREBASE_DATABASE_ID` | *(only if using a non-default Firestore database)* |

4. Build settings:
   - **Framework**: Vite
   - **Build command**: `npm run build`
   - **Output directory**: `dist`

5. Click **Deploy** ✅

## Firebase Setup

### Auth
Enable **Google** and **Email/Password** providers in Firebase Console → Authentication.

### Firestore
Create a database in **production mode** and add these security rules:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == uid;
    }
    match /posts/{postId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth.uid == resource.data.authorUid;

      match /likes/{likeId} {
        allow read, write: if request.auth != null;
      }
      match /comments/{commentId} {
        allow read: if request.auth != null;
        allow create: if request.auth != null;
        allow delete: if request.auth.uid == resource.data.authorUid;
      }
    }
    match /projects/{projectId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update, delete: if request.auth.uid == resource.data.authorUid;
    }
    match /follows/{followId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
    match /blocks/{blockId} {
      allow read, write: if request.auth != null;
    }
    match /notifications/{notifId} {
      allow read, write: if request.auth != null;
    }
    match /conversations/{convId} {
      allow read, write: if request.auth.uid in resource.data.participants
                         || request.auth != null;
      match /messages/{msgId} {
        allow read, write: if request.auth != null;
      }
    }
  }
}
```

## Local Development

```bash
cp .env.example .env
# Fill in your Firebase values in .env

npm install
npm run dev
```
