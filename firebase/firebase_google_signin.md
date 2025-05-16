# Authenticate Using Google with JavaScript

Firebase allows users to authenticate using their Google Accounts through two primary methods: using the Firebase SDK or handling the sign-in flow manually.

## Prerequisites

1. Add Firebase to your JavaScript project
2. Enable Google sign-in method in Firebase console

## Handling Sign-In with Firebase SDK

### Create Google Provider

```javascript
// Modern Firebase
import { GoogleAuthProvider } from "firebase/auth";
const provider = new GoogleAuthProvider();

// Legacy Firebase
var provider = new firebase.auth.GoogleAuthProvider();
```

### Optional Configuration

You can customize the authentication flow:

- Add OAuth 2.0 scopes
- Set language preferences
- Add custom OAuth parameters

### Sign-In Methods

#### Popup Sign-In

```javascript
import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";

const auth = getAuth();
signInWithPopup(auth, provider)
  .then((result) => {
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    const user = result.user;
  })
  .catch((error) => {
    // Handle errors
  });
```

#### Redirect Sign-In

```javascript
import { getAuth, signInWithRedirect } from "firebase/auth";

const auth = getAuth();
signInWithRedirect(auth, provider);

// Later, retrieve redirect result
getRedirectResult(auth)
  .then((result) => {
    // Handle successful sign-in
  })
  .catch((error) => {
    // Handle errors
  });
```

## Handling Account Conflicts

When a user attempts to sign in with an email already associated with another account, Firebase provides methods to handle these scenarios, such as linking credentials.

## Manual Sign-In Flow

For advanced scenarios, you can manually handle sign-in using the Google Sign-In library and exchange the ID token for Firebase credentials.

## Node.js Authentication

For Node.js applications, you'll need to obtain a Google ID token and use it to authenticate with Firebase, typically by sending the token from a front-end application.