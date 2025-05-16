# Authenticate Using Facebook Login with JavaScript

Firebase allows users to authenticate with their Facebook accounts through two primary methods:

## Prerequisites

1. Add Firebase to your JavaScript project
2. Obtain Facebook App ID and App Secret
3. Enable Facebook Login in Firebase Console

## Method 1: Handle Sign-in with Firebase SDK

### Create Facebook Provider

```javascript
// Modern Firebase
import { FacebookAuthProvider } from "firebase/auth";
const provider = new FacebookAuthProvider();

// Legacy Firebase
var provider = new firebase.auth.FacebookAuthProvider();
```

### Optional Configuration

```javascript
// Add OAuth scopes
provider.addScope('user_birthday');

// Set language
auth.languageCode = 'it';

// Custom OAuth parameters
provider.setCustomParameters({
  'display': 'popup'
});
```

### Sign-in Methods

#### Popup Sign-in

```javascript
signInWithPopup(auth, provider)
  .then((result) => {
    const user = result.user;
    const credential = FacebookAuthProvider.credentialFromResult(result);
    const accessToken = credential.accessToken;
  })
  .catch((error) => {
    // Handle errors
  });
```

#### Redirect Sign-in

```javascript
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

## Method 2: Manual Sign-in Flow

1. Integrate Facebook Login SDK
2. Setup auth state listener
3. Exchange Facebook token for Firebase credential

**Note:** Facebook enforces HTTPS and requires development mode for localhost testing.

## Handling Existing Account Errors

When a user attempts to sign in with an email already associated with another provider, handle the `account-exists-with-different-credential` error by linking credentials.