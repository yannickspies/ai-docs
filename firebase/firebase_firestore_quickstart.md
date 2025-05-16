# Cloud Firestore Quickstart Guide

This guide walks you through setting up Cloud Firestore, adding data, and viewing the data in the Firebase console.

## Create a Cloud Firestore Database

1. Create a Firebase project in the [Firebase console](https://console.firebase.google.com/) or add Firebase services to an existing Google Cloud project.
2. Open your project and select **Firestore Database** from the Build section in the left panel.
3. Click **Create database**.
4. Select a location for your database.
5. Choose a starting mode for your Cloud Firestore Security Rules:
   - **Test mode**: Allows anyone to read and write data (suitable for initial development)
   - **Locked mode**: Denies all reads and writes from mobile and web clients (suitable for server-only access)
6. Click **Create**.

Enabling Cloud Firestore also enables the API in the Cloud API Manager.

## Set Up Your Development Environment

### Web (Modular API - v9+)

1. Add Firebase to your web app.
2. Install the Cloud Firestore SDK:
   ```
   npm install firebase@11.7.1 --save
   ```
3. Import Firebase and Cloud Firestore:
   ```javascript
   import { initializeApp } from "firebase/app";
   import { getFirestore } from "firebase/firestore";
   ```

### Web (Namespaced API - Compat)

1. Add Firebase to your web app.
2. Add the Firebase and Cloud Firestore libraries:
   ```html
   <script src="https://www.gstatic.com/firebasejs/11.7.1/firebase-app-compat.js"></script>
   <script src="https://www.gstatic.com/firebasejs/11.7.1/firebase-firestore-compat.js"></script>
   ```
   Alternatively, install via npm:
   ```
   npm install firebase@11.7.1 --save
   ```
   Then import:
   ```javascript
   import firebase from "firebase/compat/app";
   import "firebase/firestore";
   ```

### iOS (Swift Package Manager)

1. Add Firebase to your Apple app.
2. In Xcode, navigate to **File > Swift Packages > Add Package Dependency**.
3. Add the Firebase Apple platforms SDK repository:
   ```
   https://github.com/firebase/firebase-ios-sdk
   ```
4. Choose the Firestore library.

### Android

1. Add Firebase to your Android app.
2. Add the Cloud Firestore dependency in your app-level Gradle file:
   ```kotlin
   dependencies {
       // Import the BoM for the Firebase platform
       implementation(platform("com.google.firebase:firebase-bom:33.13.0"))

       // Declare the dependency for the Cloud Firestore library
       // When using the BoM, you don't specify versions in Firebase library dependencies
       implementation("com.google.firebase:firebase-firestore")
   }
   ```

### Flutter

1. Configure and initialize Firebase in your Flutter app.
2. Install the plugin:
   ```
   flutter pub add cloud_firestore
   ```
3. Rebuild your Flutter application:
   ```
   flutter run
   ```

### Server SDKs

#### Node.js
```
npm install firebase-admin --save
```

#### Python
```
pip install --upgrade firebase-admin
```

#### Java
Using Gradle:
```
compile 'com.google.firebase:firebase-admin:1.32.0'
```
Using Maven:
```xml
<dependency>
  <groupId>com.google.firebase</groupId>
  <artifactId>firebase-admin</artifactId>
  <version>1.32.0</version>
</dependency>
```

#### Go
```
go get firebase.google.com/go
```

#### PHP
```
composer require google/cloud-firestore
```

#### C#
In your .csproj file:
```xml
<ItemGroup>
  <PackageReference Include="Google.Cloud.Firestore" Version="1.1.0-beta01" />
</ItemGroup>
```

#### Ruby
In your Gemfile:
```
gem "google-cloud-firestore"
```

## Initialize Cloud Firestore

### Web (Modular API - v9+)

```javascript
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";

// Replace with your Firebase configuration
const firebaseConfig = {
    // Your config here
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

// Initialize Cloud Firestore
const db = getFirestore(app);
```

### Web (Namespaced API - Compat)

```javascript
import firebase from "firebase/app";
import "firebase/firestore";

// Replace with your Firebase configuration
const firebaseConfig = {
    // Your config here
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);

// Initialize Cloud Firestore
const db = firebase.firestore();
```

### iOS (Swift)

```swift
import FirebaseCore
import FirebaseFirestore

// Configure Firebase
FirebaseApp.configure()

// Initialize Firestore
let db = Firestore.firestore()
```

### Android (Kotlin)

```kotlin
// Access a Cloud Firestore instance
val db = Firebase.firestore
```

### Android (Java)

```java
// Access a Cloud Firestore instance
FirebaseFirestore db = FirebaseFirestore.getInstance();
```

### Flutter

```dart
// Access a Cloud Firestore instance
final db = FirebaseFirestore.instance;
```

### Server SDKs

#### Node.js (Cloud Functions)
```javascript
const { initializeApp } = require('firebase-admin/app');
const { getFirestore } = require('firebase-admin/firestore');

initializeApp();
const db = getFirestore();
```

#### Python
```python
import firebase_admin
from firebase_admin import firestore

# Application Default credentials are automatically created
app = firebase_admin.initialize_app()
db = firestore.client()
```

## Add Data

Cloud Firestore stores data in Documents, which are stored in Collections. Both are created implicitly when you first add data.

### Adding a Document

#### Web (Modular API - v9+)

```javascript
import { collection, addDoc } from "firebase/firestore"; 

try {
  const docRef = await addDoc(collection(db, "users"), {
    first: "Ada",
    last: "Lovelace",
    born: 1815
  });
  console.log("Document written with ID: ", docRef.id);
} catch (e) {
  console.error("Error adding document: ", e);
}
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("users").add({
    first: "Ada",
    last: "Lovelace",
    born: 1815
})
.then((docRef) => {
    console.log("Document written with ID: ", docRef.id);
})
.catch((error) => {
    console.error("Error adding document: ", error);
});
```

#### iOS (Swift)

```swift
do {
  let ref = try await db.collection("users").addDocument(data: [
    "first": "Ada",
    "last": "Lovelace",
    "born": 1815
  ])
  print("Document added with ID: \(ref.documentID)")
} catch {
  print("Error adding document: \(error)")
}
```

#### Android (Kotlin)

```kotlin
// Create a new user
val user = hashMapOf(
    "first" to "Ada",
    "last" to "Lovelace",
    "born" to 1815,
)

// Add a new document with a generated ID
db.collection("users")
    .add(user)
    .addOnSuccessListener { documentReference ->
        Log.d(TAG, "DocumentSnapshot added with ID: ${documentReference.id}")
    }
    .addOnFailureListener { e ->
        Log.w(TAG, "Error adding document", e)
    }
```

#### Flutter

```dart
// Create a new user
final user = <String, dynamic>{
  "first": "Ada",
  "last": "Lovelace",
  "born": 1815
};

// Add a new document with a generated ID
db.collection("users").add(user).then((DocumentReference doc) =>
    print('DocumentSnapshot added with ID: ${doc.id}'));
```

### Adding a Second Document with Different Fields

Documents in a collection can contain different sets of information.

#### Web (Modular API - v9+)

```javascript
import { addDoc, collection } from "firebase/firestore"; 

try {
  const docRef = await addDoc(collection(db, "users"), {
    first: "Alan",
    middle: "Mathison",
    last: "Turing",
    born: 1912
  });
  console.log("Document written with ID: ", docRef.id);
} catch (e) {
  console.error("Error adding document: ", e);
}
```

## Read Data

### Retrieving a Collection

#### Web (Modular API - v9+)

```javascript
import { collection, getDocs } from "firebase/firestore"; 

const querySnapshot = await getDocs(collection(db, "users"));
querySnapshot.forEach((doc) => {
  console.log(`${doc.id} => ${doc.data()}`);
});
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("users").get().then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
        console.log(`${doc.id} => ${doc.data()}`);
    });
});
```

#### iOS (Swift)

```swift
do {
  let snapshot = try await db.collection("users").getDocuments()
  for document in snapshot.documents {
    print("\(document.documentID) => \(document.data())")
  }
} catch {
  print("Error getting documents: \(error)")
}
```

#### Android (Kotlin)

```kotlin
db.collection("users")
    .get()
    .addOnSuccessListener { result ->
        for (document in result) {
            Log.d(TAG, "${document.id} => ${document.data}")
        }
    }
    .addOnFailureListener { exception ->
        Log.w(TAG, "Error getting documents.", exception)
    }
```

#### Flutter

```dart
await db.collection("users").get().then((event) {
  for (var doc in event.docs) {
    print("${doc.id} => ${doc.data()}");
  }
});
```

## Secure Your Data

For web, Android, or Apple platforms SDKs, use Firebase Authentication and Cloud Firestore Security Rules to secure your data.

### Sample Security Rules

#### Auth Required
```
// Allow read/write access to a document keyed by the user's UID
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

#### Locked Mode
```
// Deny read/write access to all users under any conditions
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

For server SDKs, use Identity and Access Management (IAM) to secure your data.

## Next Steps

- **Codelabs**: Follow practical tutorials for [Android](https://codelabs.developers.google.com/codelabs/firestore-android), [iOS](https://codelabs.developers.google.com/codelabs/firestore-ios), or [web](https://codelabs.developers.google.com/codelabs/firestore-web)
- **Data Model**: Learn how data is structured in Cloud Firestore
- **Add Data**: Learn more advanced techniques for creating and updating data
- **Get Data**: Learn more about retrieving data
- **Queries**: Learn how to run simple and compound queries
- **Order and Limit**: Learn how to order and limit query results