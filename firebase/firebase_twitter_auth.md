# Authenticate Using Twitter in JavaScript

## Before You Begin

1. Add Firebase to your JavaScript project
2. Enable Twitter authentication in Firebase console
3. Register your app on Twitter Developer platform
4. Set OAuth redirect URI in Twitter app settings

## Handle Sign-in Flow with Firebase SDK

### Create Twitter Provider

```javascript
// Modern Firebase
import { TwitterAuthProvider } from "firebase/auth";
const provider = new TwitterAuthProvider();

// Legacy Firebase
var provider = new firebase.auth.TwitterAuthProvider();
```

### Optional: Set Language and Custom Parameters

```javascript
// Set language
const auth = getAuth();
auth.languageCode = 'it';

// Set custom OAuth parameters
provider.setCustomParameters({
  'lang': 'es'
});
```

### Sign-in Methods

#### Sign-in with Popup

```javascript
signInWithPopup(auth, provider)
  .then((result) => {
    const credential = TwitterAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    const secret = credential.secret;
    const user = result.user;
  })
  .catch((error) => {
    // Handle errors
  });
```

#### Sign-in with Redirect

```javascript
// Initiate redirect
signInWithRedirect(auth, provider);

// Handle redirect result
getRedirectResult(auth)
  .then((result) => {
    const credential = TwitterAuthProvider.credentialFromResult(result);
    // Process user and credential
  })
  .catch((error) => {
    // Handle errors
  });
```

## Handling Account Exists with Different Credential

### Popup Mode Example

```javascript
try {
  let result = await signInWithPopup(getAuth(), new TwitterAuthProvider());
} catch (error) {
  if (error.code === "auth/account-exists-with-different-credential") {
    // Handle existing account with different provider
  }
}
```

## Manual Sign-in Flow

1. Integrate Twitter authentication
2. Obtain OAuth access token and secret
3. Create Firebase credential
4. Sign in with credential

```javascript
signInWithCredential(auth, credential)
```