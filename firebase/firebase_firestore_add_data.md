# Cloud Firestore: Adding and Updating Data

This document explains how to set, add, and update individual documents in Cloud Firestore.

## Overview

You can write data to Cloud Firestore in the following ways:

1. **Set data for a document** with an explicit identifier
2. **Add a document** with an automatically generated identifier
3. **Create an empty document** with an automatically generated identifier and assign data later

## Setting Documents

To create or overwrite a single document, use the `set()` method:

### Web (Modular API - v9+)

```javascript
import { doc, setDoc } from "firebase/firestore"; 

// Add a new document in collection "cities"
await setDoc(doc(db, "cities", "LA"), {
  name: "Los Angeles",
  state: "CA",
  country: "USA"
});
```

### Web (Namespaced API - Compat)

```javascript
// Add a new document in collection "cities"
db.collection("cities").doc("LA").set({
    name: "Los Angeles",
    state: "CA",
    country: "USA"
})
.then(() => {
    console.log("Document successfully written!");
})
.catch((error) => {
    console.error("Error writing document: ", error);
});
```

### iOS (Swift)

```swift
// Add a new document in collection "cities"
do {
  try await db.collection("cities").document("LA").setData([
    "name": "Los Angeles",
    "state": "CA",
    "country": "USA"
  ])
  print("Document successfully written!")
} catch {
  print("Error writing document: \(error)")
}
```

### Android (Kotlin)

```kotlin
val city = hashMapOf(
    "name" to "Los Angeles",
    "state" to "CA",
    "country" to "USA",
)

db.collection("cities").document("LA")
    .set(city)
    .addOnSuccessListener { Log.d(TAG, "DocumentSnapshot successfully written!") }
    .addOnFailureListener { e -> Log.w(TAG, "Error writing document", e) }
```

### Flutter

```dart
final city = <String, String>{
  "name": "Los Angeles",
  "state": "CA",
  "country": "USA"
};

db
    .collection("cities")
    .doc("LA")
    .set(city)
    .onError((e, _) => print("Error writing document: $e"));
```

### Merging Data

If the document exists, you can use the `merge` option to merge the new data with the existing document:

```javascript
// Web v9
const cityRef = doc(db, 'cities', 'BJ');
setDoc(cityRef, { capital: true }, { merge: true });

// Web v8
var cityRef = db.collection('cities').doc('BJ');
cityRef.set({ capital: true }, { merge: true });

// Swift
db.collection("cities").document("BJ").setData([ "capital": true ], merge: true)

// Kotlin
db.collection("cities").document("BJ")
    .set(hashMapOf("capital" to true), SetOptions.merge())

// Flutter
db.collection("cities").doc("BJ").set({"capital": true}, SetOptions(merge: true));
```

## Data Types

Cloud Firestore supports various data types:

- Strings
- Booleans
- Numbers (stored as doubles internally)
- Dates and timestamps
- Arrays
- Maps (nested objects)
- Null values

### Example with Different Data Types

```javascript
// Web v9
import { doc, setDoc, Timestamp } from "firebase/firestore"; 

const docData = {
    stringExample: "Hello world!",
    booleanExample: true,
    numberExample: 3.14159265,
    dateExample: Timestamp.fromDate(new Date("December 10, 1815")),
    arrayExample: [5, true, "hello"],
    nullExample: null,
    objectExample: {
        a: 5,
        b: {
            nested: "foo"
        }
    }
};
await setDoc(doc(db, "data", "one"), docData);
```

## Custom Objects

Most client libraries allow you to store data using custom classes/objects.

### Custom Object Example (Web)

```javascript
class City {
    constructor (name, state, country) {
        this.name = name;
        this.state = state;
        this.country = country;
    }
    toString() {
        return this.name + ', ' + this.state + ', ' + this.country;
    }
}

// Firestore data converter
const cityConverter = {
    toFirestore: (city) => {
        return {
            name: city.name,
            state: city.state,
            country: city.country
        };
    },
    fromFirestore: (snapshot, options) => {
        const data = snapshot.data(options);
        return new City(data.name, data.state, data.country);
    }
};

// Using the converter
import { doc, setDoc } from "firebase/firestore"; 
const ref = doc(db, "cities", "LA").withConverter(cityConverter);
await setDoc(ref, new City("Los Angeles", "CA", "USA"));
```

## Adding Documents

When you don't want to specify an ID for your document, you can use the `add()` method, which will generate a unique ID automatically:

### Web (Modular API - v9+)

```javascript
import { collection, addDoc } from "firebase/firestore"; 

// Add a new document with a generated id
const docRef = await addDoc(collection(db, "cities"), {
  name: "Tokyo",
  country: "Japan"
});
console.log("Document written with ID: ", docRef.id);
```

### Web (Namespaced API - Compat)

```javascript
// Add a new document with a generated id
db.collection("cities").add({
    name: "Tokyo",
    country: "Japan"
})
.then((docRef) => {
    console.log("Document written with ID: ", docRef.id);
})
.catch((error) => {
    console.error("Error adding document: ", error);
});
```

### Creating Document References with Auto-Generated IDs

You can also create a document reference with an auto-generated ID, then use it later:

```javascript
// Web v9
const newCityRef = doc(collection(db, "cities"));
// later...
await setDoc(newCityRef, data);

// Web v8
const newCityRef = db.collection("cities").doc();
// later...
newCityRef.set(data);
```

## Updating Documents

To update specific fields of a document without overwriting the entire document, use the `update()` method:

### Web (Modular API - v9+)

```javascript
import { doc, updateDoc } from "firebase/firestore";

const washingtonRef = doc(db, "cities", "DC");

// Update the capital field
await updateDoc(washingtonRef, {
  capital: true
});
```

### Web (Namespaced API - Compat)

```javascript
var washingtonRef = db.collection("cities").doc("DC");

// Set the "capital" field of the city 'DC'
washingtonRef.update({
    capital: true
})
.then(() => {
    console.log("Document successfully updated!");
})
.catch((error) => {
    console.error("Error updating document: ", error);
});
```

### Server Timestamp

You can set a field to a server timestamp to track when the server receives the update:

```javascript
// Web v9
import { updateDoc, serverTimestamp } from "firebase/firestore";
await updateDoc(docRef, {
    timestamp: serverTimestamp()
});

// Web v8
docRef.update({
    timestamp: firebase.firestore.FieldValue.serverTimestamp()
});

// Swift
db.collection("objects").document("some-id").updateData([
  "lastUpdated": FieldValue.serverTimestamp(),
])

// Kotlin
docRef.update(mapOf(
    "timestamp" to FieldValue.serverTimestamp(),
))
```

## Updating Nested Fields

You can update nested fields using dot notation:

```javascript
// Web v9
await updateDoc(frankDocRef, {
    "age": 13,
    "favorites.color": "Red"
});

// Web v8
db.collection("users").doc("frank").update({
    "age": 13,
    "favorites.color": "Red"
})
```

Without dot notation, you would overwrite the entire nested object:

```javascript
// This will remove favorites.subject and favorites.food
db.collection("users").doc("frank").update({
    favorites: {
        color: "Red"
    }
});
```

## Array Operations

You can use special methods to add and remove elements from arrays:

### Array Union (Add Elements)

Adds elements to an array but only if they're not already present:

```javascript
// Web v9
import { arrayUnion } from "firebase/firestore";
await updateDoc(washingtonRef, {
    regions: arrayUnion("greater_virginia")
});

// Web v8
washingtonRef.update({
    regions: firebase.firestore.FieldValue.arrayUnion("greater_virginia")
});

// Swift
washingtonRef.updateData([
  "regions": FieldValue.arrayUnion(["greater_virginia"])
])

// Kotlin
washingtonRef.update("regions", FieldValue.arrayUnion("greater_virginia"))
```

### Array Remove (Remove Elements)

Removes all instances of each given element:

```javascript
// Web v9
import { arrayRemove } from "firebase/firestore";
await updateDoc(washingtonRef, {
    regions: arrayRemove("east_coast")
});

// Web v8
washingtonRef.update({
    regions: firebase.firestore.FieldValue.arrayRemove("east_coast")
});
```

## Numeric Increment

You can atomically increment or decrement a numeric value:

```javascript
// Web v9
import { increment } from "firebase/firestore";
await updateDoc(washingtonRef, {
    population: increment(50)
});

// Web v8
washingtonRef.update({
    population: firebase.firestore.FieldValue.increment(50)
});

// Swift
washingtonRef.updateData([
  "population": FieldValue.increment(Int64(50))
])

// Kotlin
washingtonRef.update("population", FieldValue.increment(50))
```

This operation is useful for implementing counters without transaction conflicts.