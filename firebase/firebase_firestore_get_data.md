# Cloud Firestore: Get Data

There are three primary ways to retrieve data stored in Cloud Firestore:

1. Call a method to get the data once
2. Set a listener to receive data-change events
3. Bulk-load Firestore snapshot data from an external source via data bundles

## Get a Document

To retrieve the contents of a single document:

### Web (Modular API - v9+)

```javascript
import { doc, getDoc } from "firebase/firestore";

const docRef = doc(db, "cities", "SF");
const docSnap = await getDoc(docRef);

if (docSnap.exists()) {
  console.log("Document data:", docSnap.data());
} else {
  console.log("No such document!");
}
```

### Web (Namespaced API - Compat)

```javascript
var docRef = db.collection("cities").doc("SF");

docRef.get().then((doc) => {
    if (doc.exists) {
        console.log("Document data:", doc.data());
    } else {
        console.log("No such document!");
    }
}).catch((error) => {
    console.log("Error getting document:", error);
});
```

### iOS (Swift)

```swift
let docRef = db.collection("cities").document("SF")

docRef.getDocument { (document, error) in
    if let document = document, document.exists {
        let dataDescription = document.data().map(String.init(describing:)) ?? "nil"
        print("Document data: \(dataDescription)")
    } else {
        print("Document does not exist")
    }
}
```

### Android (Kotlin)

```kotlin
val docRef = db.collection("cities").document("SF")
docRef.get()
    .addOnSuccessListener { document ->
        if (document != null) {
            Log.d(TAG, "DocumentSnapshot data: ${document.data}")
        } else {
            Log.d(TAG, "No such document")
        }
    }
    .addOnFailureListener { exception ->
        Log.d(TAG, "get failed with ", exception)
    }
```

### Flutter

```dart
final docRef = db.collection("cities").doc("SF");
final snapshot = await docRef.get();

if (snapshot.exists) {
  print("Document data: ${snapshot.data()}");
} else {
  print("No such document!");
}
```

## Source Options

You can specify the `source` option to control where the document is retrieved from:

### Web (Modular API - v9+)

```javascript
import { doc, getDocFromCache, getDocFromServer } from "firebase/firestore";

// Get from cache only
try {
  const doc = await getDocFromCache(docRef);
  console.log("Cached document data:", doc.data());
} catch (e) {
  console.log("Error getting cached document:", e);
}

// Get from server only
const doc = await getDocFromServer(docRef);
console.log("Server document data:", doc.data());
```

### Web (Namespaced API - Compat)

```javascript
// Get from cache
docRef.get({ source: 'cache' })
  .then((doc) => {
    console.log("Cached document data:", doc.data());
  })
  .catch((error) => {
    console.log("Error getting cached document:", error);
  });

// Get from server
docRef.get({ source: 'server' })
  .then((doc) => {
    console.log("Server document data:", doc.data());
  })
  .catch((error) => {
    console.log("Error getting document from server:", error);
  });
```

## Custom Objects

You can convert Firestore documents to custom objects using data converters:

### Web (Modular API - v9+)

```javascript
// Define a city class
class City {
    constructor(name, state, country) {
        this.name = name;
        this.state = state;
        this.country = country;
    }
    
    toString() {
        return this.name + ', ' + this.state + ', ' + this.country;
    }
}

// Define a converter
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

// Get a document with the converter
const ref = doc(db, "cities", "LA").withConverter(cityConverter);
const docSnap = await getDoc(ref);
if (docSnap.exists()) {
    const city = docSnap.data(); // City object
    console.log(city.toString());
}
```

### Web (Namespaced API - Compat)

```javascript
// Get with a custom object converter
db.collection("cities").doc("LA")
    .withConverter(cityConverter)
    .get()
    .then((doc) => {
        if (doc.exists) {
            const city = doc.data(); // City object
            console.log(city.toString());
        }
    });
```

## Get Multiple Documents from a Collection

To retrieve multiple documents from a collection based on a query:

### Web (Modular API - v9+)

```javascript
import { collection, query, where, getDocs } from "firebase/firestore";

const q = query(collection(db, "cities"), where("capital", "==", true));
const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

### Web (Namespaced API - Compat)

```javascript
db.collection("cities").where("capital", "==", true)
    .get()
    .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
            console.log(doc.id, " => ", doc.data());
        });
    })
    .catch((error) => {
        console.log("Error getting documents: ", error);
    });
```

### iOS (Swift)

```swift
db.collection("cities").whereField("capital", isEqualTo: true)
    .getDocuments() { (querySnapshot, err) in
        if let err = err {
            print("Error getting documents: \(err)")
        } else {
            for document in querySnapshot!.documents {
                print("\(document.documentID) => \(document.data())")
            }
        }
}
```

### Android (Kotlin)

```kotlin
db.collection("cities")
    .whereEqualTo("capital", true)
    .get()
    .addOnSuccessListener { documents ->
        for (document in documents) {
            Log.d(TAG, "${document.id} => ${document.data}")
        }
    }
    .addOnFailureListener { exception ->
        Log.w(TAG, "Error getting documents: ", exception)
    }
```

### Flutter

```dart
final citiesRef = db.collection("cities");
final query = citiesRef.where("capital", isEqualTo: true);
final querySnapshot = await query.get();

for (var doc in querySnapshot.docs) {
  print("${doc.id} => ${doc.data()}");
}
```

## Get All Documents in a Collection

To retrieve all documents in a collection without filtering:

### Web (Modular API - v9+)

```javascript
import { collection, getDocs } from "firebase/firestore";

const querySnapshot = await getDocs(collection(db, "cities"));
querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

### Web (Namespaced API - Compat)

```javascript
db.collection("cities").get().then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
        console.log(doc.id, " => ", doc.data());
    });
});
```

## Get All Documents in a Subcollection

To access documents in a subcollection:

### Web (Modular API - v9+)

```javascript
import { collection, getDocs } from "firebase/firestore";

const querySnapshot = await getDocs(collection(db, "cities", "SF", "landmarks"));
querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
});
```

### Web (Namespaced API - Compat)

```javascript
db.collection("cities").doc("SF").collection("landmarks").get()
    .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
            console.log(doc.id, " => ", doc.data());
        });
    });
```

## List Subcollections of a Document

The `listCollections()` method is only available in server client libraries, not in mobile/web clients. If you need to list subcollections in a client application, consider using a different data structure or maintaining a list of collection names elsewhere in your database.