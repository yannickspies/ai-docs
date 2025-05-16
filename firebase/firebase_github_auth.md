# Authenticate Using GitHub with JavaScript

Firebase allows users to authenticate with their GitHub accounts using two primary methods:

## Prerequisites

1. Add Firebase to your JavaScript project
2. Enable GitHub authentication in Firebase console
3. Register your app on GitHub and obtain:
   - Client ID
   - Client Secret
4. Set Authorization callback URL in GitHub app settings

## Authentication Methods

### Using Firebase SDK

#### Create GitHub Provider

```javascript
// Modern Firebase
import { GithubAuthProvider } from "firebase/auth";
const provider = new GithubAuthProvider();

// Optional: Add OAuth scopes
provider.addScope('repo');

// Optional: Set custom parameters
provider.setCustomParameters({
  'allow_signup': 'false'
});
```

#### Sign-In Options

1. Popup Sign-In:
```javascript
signInWithPopup(auth, provider)
  .then((result) => {
    const credential = GithubAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    const user = result.user;
  })
  .catch((error) => {
    // Handle errors
  });
```

2. Redirect Sign-In:
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

### Manual Sign-In Flow

1. Complete GitHub OAuth 2.0 flow
2. Obtain OAuth access token
3. Create Firebase credential
4. Sign in with credential

```javascript
const credential = GithubAuthProvider.credential(token);
signInWithCredential(auth, credential)
  .then((result) => {
    // Signed in successfully
  })
  .catch((error) => {
    // Handle errors
  });
```

## Special Considerations

- Handle "account-exists-with-different-credential" errors
- For Chrome extensions, use Offscreen Documents guide
- Customize redirect domain if needed