# Cloud Firestore

Cloud Firestore is a flexible, scalable NoSQL cloud database from Firebase and Google Cloud designed for mobile, web, and server development. It provides:

- Real-time data synchronization across client apps
- Offline support for mobile and web applications
- Seamless integration with other Firebase and Google Cloud products

## Key Capabilities

### Flexibility
- Supports hierarchical data structures
- Stores data in documents organized into collections
- Documents can contain complex nested objects and subcollections

### Expressive Querying
- Retrieve individual documents or all documents matching query parameters
- Support for multiple chained filters and combined filtering and sorting
- Indexed by default with query performance proportional to result set size

### Realtime Updates
- Uses data synchronization to update data on connected devices
- Optimized for efficient one-time fetch queries

### Offline Support
- Caches actively used data for offline operations
- Synchronizes local changes when device reconnects

### Scalability
- Automatic multi-region data replication
- Strong consistency guarantees
- Atomic batch operations
- Real transaction support
- Designed for the toughest database workloads

## How Does It Work?

Cloud Firestore is a cloud-hosted NoSQL database accessible via:
- Native SDKs for Apple, Android, and web apps
- Native Node.js, Java, Python, Unity, C++, and Go SDKs
- REST and RPC APIs

### Data Model

- **Documents**: Contains fields mapping to values
- **Collections**: Containers for documents to organize data
- **Subcollections**: Enables hierarchical data structures within documents
- **Data Types**: Supports simple types (strings, numbers) and complex nested objects

### Querying

- **Shallow Queries**: Retrieve data at document level without fetching entire collections
- **Query Operations**: Sorting, filtering, limits, and pagination with cursors
- **Realtime Listeners**: Notify apps when data changes, retrieving only the new changes

### Security

- Protect data with Firebase Authentication and Cloud Firestore Security Rules (mobile/web)
- Identity and Access Management (IAM) for server-side languages

## Implementation Path

1. **Integrate SDKs**: Include clients via Gradle, CocoaPods, or script includes
2. **Secure Data**: Set up Security Rules or IAM
3. **Add Data**: Create documents and collections
4. **Get Data**: Create queries or use realtime listeners

## Related Topics

- Getting started with Cloud Firestore
- Cloud Firestore data model
- Differences between Realtime Database and Cloud Firestore