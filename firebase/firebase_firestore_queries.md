# Cloud Firestore: Simple and Compound Queries

Cloud Firestore provides powerful query functionality for specifying which documents to retrieve from a collection or collection group. These queries can be used with both single-time reads via `get()` or real-time listeners with `addSnapshotListener()`.

## Query Operators

Firestore supports the following comparison operators:

- `<` less than
- `<=` less than or equal to
- `==` equal to
- `>` greater than
- `>=` greater than or equal to
- `!=` not equal to
- `array-contains`
- `array-contains-any`
- `in`
- `not-in`

## Simple Queries

### Basic Equality Queries

#### Web (Modular API - v9+)

```javascript
import { collection, query, where, getDocs } from "firebase/firestore";

const citiesRef = collection(db, "cities");
const q = query(citiesRef, where("state", "==", "CA"));
const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
  console.log(doc.id, " => ", doc.data());
});
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("cities").where("state", "==", "CA")
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

### Query for Boolean Values

#### Web (Modular API - v9+)

```javascript
const q = query(citiesRef, where("capital", "==", true));
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("cities").where("capital", "==", true);
```

## Compound Queries

You can chain multiple `where()` clauses to create more specific queries. These clauses are combined using AND logic.

### Example: Multiple Where Clauses

#### Web (Modular API - v9+)

```javascript
// Cities in Colorado named Denver
const q1 = query(citiesRef, 
  where("state", "==", "CO"), 
  where("name", "==", "Denver")
);

// Cities in California with population less than 1 million
const q2 = query(citiesRef, 
  where("state", "==", "CA"), 
  where("population", "<", 1000000)
);
```

#### Web (Namespaced API - Compat)

```javascript
// Cities in Colorado named Denver
db.collection("cities")
  .where("state", "==", "CO")
  .where("name", "==", "Denver");

// Cities in California with population less than 1 million
db.collection("cities")
  .where("state", "==", "CA")
  .where("population", "<", 1000000);
```

## OR Queries

Use the `or()` function to combine clauses with OR logic.

### Example: OR Conditions

#### Web (Modular API - v9+)

```javascript
import { collection, query, where, or, getDocs } from "firebase/firestore";

const q = query(citiesRef,
  or(
    where('capital', '==', true),
    where('population', '>=', 1000000)
  )
);
```

### Example: Combining AND and OR Operations

#### Web (Modular API - v9+)

```javascript
import { collection, query, where, and, or, getDocs } from "firebase/firestore";

const q = query(collection(db, "cities"), 
  and(
    where('state', '==', 'CA'),   
    or(
      where('capital', '==', true),
      where('population', '>=', 1000000)
    )
  )
);
```

## Array Queries

### Query for Array Membership

Use `array-contains` to find documents where the array field contains a specific value.

#### Web (Modular API - v9+)

```javascript
const q = query(citiesRef, where("regions", "array-contains", "west_coast"));
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("cities").where("regions", "array-contains", "west_coast");
```

### Query for Multiple Array Values

Use `array-contains-any` to find documents where the array field contains any of the specified values.

#### Web (Modular API - v9+)

```javascript
const q = query(citiesRef, 
  where('regions', 'array-contains-any', ['west_coast', 'east_coast'])
);
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("cities")
  .where('regions', 'array-contains-any', ['west_coast', 'east_coast']);
```

## IN and NOT-IN Queries

### IN Queries

Use `in` to find documents where the field matches any of the specified values.

#### Web (Modular API - v9+)

```javascript
const q = query(citiesRef, where('country', 'in', ['USA', 'Japan']));
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("cities").where('country', 'in', ['USA', 'Japan']);
```

### NOT-IN Queries

Use `not-in` to find documents where the field does not match any of the specified values.

#### Web (Modular API - v9+)

```javascript
const q = query(citiesRef, where('country', 'not-in', ['USA', 'Japan']));
```

#### Web (Namespaced API - Compat)

```javascript
db.collection("cities").where('country', 'not-in', ['USA', 'Japan']);
```

## Collection Group Queries

Query across multiple collections with the same ID using `collectionGroup()`.

#### Web (Modular API - v9+)

```javascript
import { collectionGroup, query, where, getDocs } from "firebase/firestore";

const museums = query(collectionGroup(db, 'landmarks'), where('type', '==', 'museum'));
const querySnapshot = await getDocs(museums);
querySnapshot.forEach((doc) => {
  console.log(doc.id, ' => ', doc.data());
  // Note: doc.ref.path will reveal the full path including parent documents
});
```

#### Web (Namespaced API - Compat)

```javascript
db.collectionGroup('landmarks').where('type', '==', 'museum')
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, ' => ', doc.data());
    });
  });
```

## Query Limitations

When creating queries, be aware of these limitations:

1. **Array Queries**
   - Only one `array-contains` clause per query
   - Cannot combine `array-contains` with `array-contains-any`

2. **Compound Queries**
   - Cannot combine `not-in` with `in`, `array-contains-any`, or `or`
   - Maximum 30 disjunctions (conditions OR'd together) in disjunctive normal form
   - `not-in` supports up to 10 comparison values
   - `in` and `array-contains-any` support up to 30 values

3. **OR Queries**
   - OR queries require appropriate composite indexes
   - Queries with inequality filters imply ordering by that field

## Executing Queries

Once you've defined your query, you can execute it to retrieve the matching documents:

#### Web (Modular API - v9+)

```javascript
import { collection, query, where, getDocs } from "firebase/firestore";

const q = query(collection(db, "cities"), where("capital", "==", true));
const querySnapshot = await getDocs(q);
querySnapshot.forEach((doc) => {
  console.log(doc.id, " => ", doc.data());
});
```

#### Web (Namespaced API - Compat)

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