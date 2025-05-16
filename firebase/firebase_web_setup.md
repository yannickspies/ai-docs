# Add Firebase to Your JavaScript Project

## Overview

This guide helps you add the Firebase JavaScript SDK to your web app or client-side JavaScript project. Here are the key steps:

## Step 1: Create a Firebase Project and Register Your App

1. Visit the [Firebase console](https://console.firebase.google.com/)
2. Click "Add project"
3. Choose an existing Google Cloud project or create a new one
4. Optionally set up Google Analytics
5. Complete project creation

## Step 2: Install SDK and Initialize Firebase

Install Firebase using npm:

```bash
npm install firebase
```

Initialize Firebase in your app:

```javascript
import { initializeApp } from 'firebase/app';

const firebaseConfig = {
  // Your Firebase project configuration
};

const app = initializeApp(firebaseConfig);
```

## Step 3: Access Firebase Services

Example using Cloud Firestore:

```javascript
import { getFirestore, collection, getDocs } from 'firebase/firestore/lite';

const db = getFirestore(app);

async function getCities(db) {
  const citiesCol = collection(db, 'cities');
  const citySnapshot = await getDocs(citiesCol);
  const cityList = citySnapshot.docs.map(doc => doc.data());
  return cityList;
}
```

## Available Firebase Services

The SDK supports multiple services, including:
- Analytics
- App Check
- Authentication
- Cloud Firestore
- Cloud Functions
- Cloud Messaging
- Cloud Storage
- Performance Monitoring
- Realtime Database
- Remote Config
- Vertex AI

## Recommended Next Steps

- Explore [sample Firebase apps](/docs/samples)
- Try the [Firebase Web Codelab](https://codelabs.developers.google.com/codelabs/firebase-web/)
- Review [supported environments](/docs/web/environments-js-sdk)

**Note:** Using a module bundler like webpack is recommended for production apps to reduce bundle size.