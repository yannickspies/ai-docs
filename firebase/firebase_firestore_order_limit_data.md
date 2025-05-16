# Cloud Firestore: Order and Limit Data

This document explains how to order query results and limit the number of documents returned in Cloud Firestore queries.

## Ordering Results

You can use the `orderBy()` method to specify the field to order by and the direction (ascending or descending). By default, an `orderBy()` clause sorts results in ascending order.

### Basic Ordering

#### Web (Modular API - v9+)

```javascript
import { collection, query, orderBy, getDocs } from "firebase/firestore";

// Order by city name (ascending - default)
const q1 = query(collection(db, "cities"), orderBy("name"));
const querySnapshot1 = await getDocs(q1);

// Order by city name (descending)
const q2 = query(collection(db, "cities"), orderBy("name", "desc"));
const querySnapshot2 = await getDocs(q2);
```

#### Web (Namespaced API - Compat)

```javascript
// Order by city name (ascending - default)
db.collection("cities").orderBy("name")
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  });

// Order by city name (descending)
db.collection("cities").orderBy("name", "desc")
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  });
```

### Multiple Field Ordering

You can sort by multiple fields by chaining `orderBy()` clauses. Documents that have the same value for the first ordering field will be sorted by the second field, and so on.

#### Web (Modular API - v9+)

```javascript
import { collection, query, orderBy, getDocs } from "firebase/firestore";

// Order by state, then by population in descending order
const q = query(collection(db, "cities"), 
  orderBy("state"), 
  orderBy("population", "desc")
);
const querySnapshot = await getDocs(q);
```

#### Web (Namespaced API - Compat)

```javascript
// Order by state, then by population in descending order
db.collection("cities")
  .orderBy("state")
  .orderBy("population", "desc")
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  });
```

## Limiting Results

You can use the `limit()` method to limit the number of documents returned in a query.

### Basic Limiting

#### Web (Modular API - v9+)

```javascript
import { collection, query, orderBy, limit, getDocs } from "firebase/firestore";

// Get the 3 largest cities by population
const q = query(collection(db, "cities"), 
  orderBy("population", "desc"), 
  limit(3)
);
const querySnapshot = await getDocs(q);
```

#### Web (Namespaced API - Compat)

```javascript
// Get the 3 largest cities by population
db.collection("cities")
  .orderBy("population", "desc")
  .limit(3)
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  });
```

### Get Last N Documents

The `limitToLast()` method works with `orderBy()` to return the last N results:

#### Web (Modular API - v9+)

```javascript
import { collection, query, orderBy, limitToLast, getDocs } from "firebase/firestore";

// Get the 3 smallest cities by population
const q = query(collection(db, "cities"), 
  orderBy("population"), 
  limitToLast(3)
);
const querySnapshot = await getDocs(q);
```

#### Web (Namespaced API - Compat)

```javascript
// Get the 3 smallest cities by population
db.collection("cities")
  .orderBy("population")
  .limitToLast(3)
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  });
```

## Combining with Where Clauses

You can combine `where()`, `orderBy()`, and `limit()` in a single query.

#### Web (Modular API - v9+)

```javascript
import { collection, query, where, orderBy, limit, getDocs } from "firebase/firestore";

// Get the 2 largest cities with population over 100,000
const q = query(collection(db, "cities"), 
  where("population", ">", 100000), 
  orderBy("population", "desc"), 
  limit(2)
);
const querySnapshot = await getDocs(q);
```

#### Web (Namespaced API - Compat)

```javascript
// Get the 2 largest cities with population over 100,000
db.collection("cities")
  .where("population", ">", 100000)
  .orderBy("population", "desc")
  .limit(2)
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      console.log(doc.id, " => ", doc.data());
    });
  });
```

## Important Limitations and Behaviors

When using ordering and limiting in queries, be aware of these important constraints:

### Field Existence Filtering

When you order by a field, any document that doesn't contain that field is excluded from the results automatically. This is because Firestore needs a value to sort by.

```javascript
// This query won't return documents that don't have a population field
const q = query(collection(db, "cities"), orderBy("population"));
```

### Inequality Filter Ordering Requirements

If your query includes inequality filters (`<`, `<=`, `>`, `>=`), you must order by the same field first:

```javascript
// VALID: Order by the same field used in the inequality
const q = query(collection(db, "cities"),
  where("population", ">", 100000),
  orderBy("population")
);

// INVALID: Different ordering than the inequality field
// This will cause an error
const q = query(collection(db, "cities"),
  where("population", ">", 100000),
  orderBy("name")
);
```

However, you can add additional orderBy clauses after the first one:

```javascript
// VALID: First sort by population (to match the inequality),
// then by name
const q = query(collection(db, "cities"),
  where("population", ">", 100000),
  orderBy("population"),
  orderBy("name")
);
```

### Implicit Ordering

When you use inequality filters, Cloud Firestore automatically orders by that field first, even if you don't explicitly include an `orderBy()`:

```javascript
// This implicitly orders by population
const q = query(collection(db, "cities"),
  where("population", ">", 100000)
);

// This is equivalent to:
const q = query(collection(db, "cities"),
  where("population", ">", 100000),
  orderBy("population")
);
```

### Equality with Ordering

You can use equality filters (`==`) with `orderBy()` clauses on different fields:

```javascript
// Valid: Equality on country, ordering by population
const q = query(collection(db, "cities"),
  where("country", "==", "USA"),
  orderBy("population", "desc")
);
```