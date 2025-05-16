# Cloud Firestore Data Model

Cloud Firestore is a NoSQL, document-oriented database with the following characteristics:

- No tables or rows like in SQL databases
- Data stored in **documents** organized into **collections**
- Each document contains key-value pairs
- Optimized for storing large collections of small documents

## Documents

Documents are the basic units of storage in Cloud Firestore:

- Lightweight records containing fields mapped to values
- Each document is identified by a name
- Limited to 1MB in size
- Similar to JSON objects with some additional data types

### Example Document

A document representing a user might look like this:

```
// Document ID: alovelace
{
  first: "Ada",
  last: "Lovelace",
  born: 1815
}
```

### Nested Objects (Maps)

Documents can contain complex, nested objects called maps:

```
// Document ID: alovelace
{
  name: {
    first: "Ada",
    last: "Lovelace"
  },
  born: 1815
}
```

## Collections

Collections are containers for documents:

- Simple containers with no fields of their own
- Cannot directly contain raw fields with values or other collections
- Document names within a collection must be unique
- Collections are created implicitly when the first document is created
- Collections disappear when all documents are removed

### Example Collection

A `users` collection might contain multiple user documents:

```
// Collection: users
{
  // Document ID: alovelace
  alovelace: {
    first: "Ada",
    last: "Lovelace",
    born: 1815
  },
  
  // Document ID: aturing
  aturing: {
    first: "Alan",
    last: "Turing",
    born: 1912
  }
}
```

### Schemaless Design

Cloud Firestore is schemaless, meaning:

- Complete freedom over what fields are in each document
- No restrictions on data types for those fields
- Documents within the same collection can have different fields or data types

However, it's recommended to use consistent fields and data types across documents to simplify queries.

## References

Each document and collection has a unique location in the database that can be referenced in code.

### Document References

```javascript
// Web v9 (Modular)
import { doc } from "firebase/firestore";
const alovelaceDocumentRef = doc(db, 'users', 'alovelace');

// Web v8 (Namespaced)
const alovelaceDocumentRef = db.collection('users').doc('alovelace');

// Swift
let alovelaceDocumentRef = db.collection("users").document("alovelace")

// Android (Kotlin)
val alovelaceDocumentRef = db.collection("users").document("alovelace")

// Flutter
final alovelaceDocumentRef = db.collection("users").doc("alovelace");
```

### Collection References

```javascript
// Web v9 (Modular)
import { collection } from "firebase/firestore";
const usersCollectionRef = collection(db, 'users');

// Web v8 (Namespaced)
const usersCollectionRef = db.collection('users');

// Swift
let usersCollectionRef = db.collection("users")

// Android (Kotlin)
val usersCollectionRef = db.collection("users")

// Flutter
final usersCollectionRef = db.collection("users");
```

### Path References

You can also create references using path strings with forward slashes:

```javascript
// Web v9 (Modular)
import { doc } from "firebase/firestore"; 
const alovelaceDocumentRef = doc(db, 'users/alovelace');

// Web v8 (Namespaced)
const alovelaceDocumentRef = db.doc('users/alovelace');

// Swift
let aLovelaceDocumentReference = db.document("users/alovelace")

// Android (Kotlin)
val alovelaceDocumentRef = db.document("users/alovelace")

// Flutter
final aLovelaceDocRef = db.doc("users/alovelace");
```

## Hierarchical Data

Cloud Firestore supports hierarchical data structures through subcollections.

### Subcollections

A subcollection is a collection associated with a specific document. This allows for hierarchical data organization without storing everything in a single document.

Example: Chat app with rooms and messages

```
// Structure
rooms (collection)
  |
  +-- roomA (document)
  |     |
  |     +-- name: "my chat room"
  |     |
  |     +-- messages (subcollection)
  |           |
  |           +-- message1 (document)
  |           |     |
  |           |     +-- from: "alex"
  |           |     +-- msg: "Hello World!"
  |           |
  |           +-- message2 (document)
  |                 |
  |                 +-- ...
  |
  +-- roomB (document)
        |
        +-- ...
```

### Accessing Subcollections

To reference a document in a subcollection:

```javascript
// Web v9 (Modular)
import { doc } from "firebase/firestore"; 
const messageRef = doc(db, "rooms", "roomA", "messages", "message1");

// Web v8 (Namespaced)
const messageRef = db.collection('rooms').doc('roomA')
                .collection('messages').doc('message1');

// Swift
let messageRef = db
  .collection("rooms").document("roomA")
  .collection("messages").document("message1")

// Android (Kotlin)
val messageRef = db
    .collection("rooms").document("roomA")
    .collection("messages").document("message1")

// Flutter
final messageRef = db
    .collection("rooms")
    .doc("roomA")
    .collection("messages")
    .doc("message1");
```

### Important Notes on Hierarchical Data

- Collections and documents must always follow an alternating pattern
- You cannot have a collection within a collection or a document within a document
- Subcollections can contain their own subcollections, allowing for deeper nesting
- Data can be nested up to 100 levels deep
- Subcollections make data easier to access hierarchically