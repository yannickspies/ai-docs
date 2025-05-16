# Authenticate with Firebase using Password-Based Accounts in JavaScript

## Before You Begin

1. Add Firebase to your JavaScript project
2. Connect your app to Firebase project
3. Enable Email/Password sign-in in Firebase console

## Create a Password-Based Account

To create a new user account with a password:

```javascript
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const auth = getAuth();
createUserWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    const user = userCredential.user;
  })
  .catch((error) => {
    const errorCode = error.code;
    const errorMessage = error.message;
  });
```

## Sign In a User

To sign in a user with email and password:

```javascript
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const auth = getAuth();
signInWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    const user = userCredential.user;
  })
  .catch((error) => {
    const errorCode = error.code;
    const errorMessage = error.message;
  });
```

## Password Policy Recommendations

Firebase supports configuring password policies with requirements like:
- Lowercase characters
- Uppercase characters
- Numeric characters
- Non-alphanumeric characters
- Minimum/maximum password length

## Sign Out a User

```javascript
import { getAuth, signOut } from "firebase/auth";

const auth = getAuth();
signOut(auth)
  .then(() => {
    // Sign-out successful
  })
  .catch((error) => {
    // An error happened
  });
```

## Next Steps

- Set an observer on the `Auth` object to track user authentication status
- Use Firebase Security Rules to control data access
- Consider enabling email enumeration protection