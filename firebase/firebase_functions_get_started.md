# Firebase Cloud Functions: Get Started

Cloud Functions for Firebase allows you to automatically run backend code in response to events triggered by Firebase features and HTTPS requests. This guide walks through creating, testing, and deploying your first functions.

## Overview

In this tutorial, you'll build two related functions:

1. An **"add message"** function that exposes a URL, accepts a text value, and writes it to Cloud Firestore.
2. A **"make uppercase"** function that triggers on Cloud Firestore writes and transforms the text to uppercase.

## Sample Code

### Node.js

```javascript
// The Cloud Functions for Firebase SDK to create Cloud Functions and triggers.
const {logger} = require("firebase-functions");
const {onRequest} = require("firebase-functions/v2/https");
const {onDocumentCreated} = require("firebase-functions/v2/firestore");

// The Firebase Admin SDK to access Firestore.
const {initializeApp} = require("firebase-admin/app");
const {getFirestore} = require("firebase-admin/firestore");

initializeApp();

// Take the text parameter passed to this HTTP endpoint and insert it into
// Firestore under the path /messages/:documentId/original
exports.addmessage = onRequest(async (req, res) => {
  // Grab the text parameter.
  const original = req.query.text;
  // Push the new message into Firestore using the Firebase Admin SDK.
  const writeResult = await getFirestore()
      .collection("messages")
      .add({original: original});
  // Send back a message that we've successfully written the message
  res.json({result: `Message with ID: ${writeResult.id} added.`});
});

// Listens for new messages added to /messages/:documentId/original
// and saves an uppercased version of the message
// to /messages/:documentId/uppercase
exports.makeuppercase = onDocumentCreated("/messages/{documentId}", (event) => {
  // Grab the current value of what was written to Firestore.
  const original = event.data.data().original;

  // Access the parameter `{documentId}` with `event.params`
  logger.log("Uppercasing", event.params.documentId, original);

  const uppercase = original.toUpperCase();

  // You must return a Promise when performing
  // asynchronous tasks inside a function
  // such as writing to Firestore.
  // Setting an 'uppercase' field in Firestore document returns a Promise.
  return event.data.ref.set({uppercase}, {merge: true});
});
```

### Python

```python
# The Cloud Functions for Firebase SDK to create Cloud Functions and set up triggers.
from firebase_functions import firestore_fn, https_fn

# The Firebase Admin SDK to access Cloud Firestore.
from firebase_admin import initialize_app, firestore
import google.cloud.firestore

app = initialize_app()


@https_fn.on_request()
def addmessage(req: https_fn.Request) -> https_fn.Response:
    """Take the text parameter passed to this HTTP endpoint and insert it into
    a new document in the messages collection."""
    # Grab the text parameter.
    original = req.args.get("text")
    if original is None:
        return https_fn.Response("No text parameter provided", status=400)

    firestore_client: google.cloud.firestore.Client = firestore.client()

    # Push the new message into Cloud Firestore using the Firebase Admin SDK.
    _, doc_ref = firestore_client.collection("messages").add({"original": original})

    # Send back a message that we've successfully written the message
    return https_fn.Response(f"Message with ID {doc_ref.id} added.")


@firestore_fn.on_document_created(document="messages/{pushId}")
def makeuppercase(event: firestore_fn.Event[firestore_fn.DocumentSnapshot | None]) -> None:
    """Listens for new documents to be added to /messages. If the document has
    an "original" field, creates an "uppercase" field containg the contents of
    "original" in upper case."""

    # Get the value of "original" if it exists.
    if event.data is None:
        return
    try:
        original = event.data.get("original")
    except KeyError:
        # No "original" field, so do nothing.
        return

    # Set the "uppercase" field.
    print(f"Uppercasing {event.params['pushId']}: {original}")
    upper = original.upper()
    event.data.reference.update({"uppercase": upper})
```

## Setup and Prerequisites

### Create a Firebase Project

1. In the Firebase console, click **Add project**
2. Enter a project name or select an existing Google Cloud project
3. Review and accept the Firebase terms
4. (Optional) Configure Google Analytics
5. Click **Create project**

### Set Up Your Environment

#### Node.js

1. Install Node.js and npm (Node Version Manager recommended)
2. Install the Firebase CLI: `npm install -g firebase-tools`

#### Python

1. Install Python (v3.10 or v3.11 supported)
2. Install the Firebase CLI
3. Use `venv` to isolate dependencies

### Initialize Your Project

1. Run `firebase login` to authenticate
2. Navigate to your project directory
3. Run `firebase init firestore` (accept defaults for rules and indexes)
4. Run `firebase init functions`
   - Choose JavaScript or Python
   - Optionally install dependencies

Your project structure should look like:

#### Node.js

```
myproject
+- .firebaserc
+- firebase.json
+- functions/
      +- .eslintrc.json
      +- package.json
      +- index.js
      +- node_modules/
```

#### Python

```
myproject
+- .firebaserc
+- firebase.json
+- functions/
      +- main.py
      +- requirements.txt
      +- venv/
```

## Understanding the Functions

### The "Add Message" Function (HTTP Trigger)

This function creates an HTTP endpoint that:
1. Accepts a `text` parameter from the query string
2. Stores the text in a new Firestore document
3. Returns a JSON response confirming the write

Key points:
- Uses `onRequest` (Node.js) or `on_request` (Python) to handle HTTP requests
- Is synchronous, so should respond quickly
- Demonstrates accessing request parameters and writing to Firestore

### The "Make Uppercase" Function (Firestore Trigger)

This function:
1. Listens for new documents created in the `/messages` collection
2. Extracts the `original` field value
3. Converts the text to uppercase
4. Writes the uppercase version back to the same document

Key points:
- Uses `onDocumentCreated` (Node.js) or `on_document_created` (Python) to listen for Firestore writes
- Demonstrates accessing document data and path parameters
- Returns a Promise in Node.js for asynchronous operations
- Shows proper error handling in Python

## Testing Locally with Emulators

1. Run `firebase emulators:start`
2. Open the Emulator UI (typically at http://localhost:4000)
3. Find the URL for your HTTP function (e.g., http://localhost:5001/PROJECT_ID/us-central1/addmessage)
4. Test the HTTP function by visiting `[function-url]?text=uppercaseme`
5. Check the Emulator UI:
   - In the **Logs** tab for function execution logs
   - In the **Firestore** tab to see the created documents with original and uppercase text

## Deploying to Production

1. Ensure your project is on the Blaze pricing plan
2. Run `firebase deploy --only functions`
3. The CLI will output the URL for your HTTP function
4. Test the deployed function by visiting `[deployed-function-url]?text=uppercasemetoo`
5. View production logs in the Google Cloud console

## Important Concepts

1. **HTTP Functions**: 
   - Expose an HTTP endpoint
   - Handle request/response objects
   - Should respond quickly (finish processing synchronously)

2. **Event-Triggered Functions**:
   - React to changes in Firebase services
   - Run in the background
   - Must return a Promise when performing async operations (Node.js)

3. **Path Parameters**:
   - Use braces syntax: `{documentId}`
   - Accessible via `event.params` in callback

4. **Local Testing**:
   - Always test with emulators during development
   - Helps prevent costly errors before deployment

## Next Steps

- Explore more [use cases for Cloud Functions](/docs/functions/use-cases)
- Try the [Cloud Functions codelab](https://codelabs.developers.google.com/codelabs/firebase-cloud-functions/#0)
- Review [code samples on GitHub](https://github.com/firebase/functions-samples)
- Learn about [managing functions](/docs/functions/manage-functions) in production
- Explore other event types supported by Cloud Functions