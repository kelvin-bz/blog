---
title: "MERN - MongoDB Fundamental"
categories: [mern, mongodb]
date: 2024-07-18 00:00:00
tags: [mern, mongodb]
image: "/assets/images/mongodb.png"
---


## MongoDB Architecture

MongoDB is a popular NoSQL database designed for high performance, high availability, and easy scalability. It stores data in flexible, JSON-like documents, making it easy to work with structured, semi-structured, and unstructured data.

- **Database**: A container for collections.
- **Collection**: A group of MongoDB documents.
- **Document**: A set of key-value pairs (similar to JSON objects).

```mermaid
graph 
  subgraph mongoDB["fa:fa-database MongoDB"]
    database["fa:fa-folder Database"]
    database --> collection["fa:fa-folder-open Collection"]
    collection --> document["fa:fa-file Document"]
  end

  style mongoDB stroke:#333,stroke-width:2px
  style database fill:#ccf,stroke:#f66,stroke-width:2px
  style collection fill:#add8e6,stroke:#333,stroke-width:2px
  style document fill:#9cf,stroke:#333,stroke-width:2px
```


## CRUD Operations

CRUD stands for Create, Read, Update, and Delete. These are the basic operations for interacting with data in MongoDB.

```mermaid
graph
  subgraph crudOperations["fa:fa-tools CRUD Operations"]
    create["fa:fa-plus-circle Create"] --> read["fa:fa-eye Read"]
    read --> update["fa:fa-edit Update"]
    update --> delete["fa:fa-trash Delete"]
  end

  style crudOperations stroke:#333,stroke-width:2px
  style create fill:#ccf,stroke:#f66,stroke-width:2px
  style read fill:#add8e6,stroke:#333,stroke-width:2px
  style update fill:#9cf,stroke:#333,stroke-width:2px
  style delete fill:#faa,stroke:#333,stroke-width:2px
```

### Create

To insert a new document into a collection, you use the `insertOne()` or `insertMany()` methods.

```javascript
db.collection('users').insertOne({ name: 'Alice', age: 25 });
```

### Read

To read documents from a collection, you use the `find()` method.

```javascript
db.collection('users').find({ name: 'Alice' });
```


### Update

To update existing documents, you use the `updateOne()` or `updateMany()` methods.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $set: { age: 26 } });

db.collection('users').updateMany({ city: 'New York' }, { $set: { city: 'San Francisco' } });
```

#### Update Operators
Operators in MongoDB are used to perform specific operations on fields in documents.

```mermaid

graph LR
  subgraph updateOperators["fa:fa-sync Update Operators"]
    set["fa:fa-equals $set"]
    inc["fa:fa-plus $inc"]
    mul["fa:fa-times $mul"]
    unset["fa:fa-trash-alt $unset"]
    rename["fa:fa-i-cursor $rename"]
  end

  style updateOperators stroke:#333,stroke-width:2px
  style set fill:#ccf,stroke:#f66,stroke-width:2px
  style inc fill:#add8e6,stroke:#333,stroke-width:2px
  style mul fill:#9cf,stroke:#333,stroke-width:2px
  style unset fill:#faa,stroke:#333,stroke-width:2px
  style rename fill:#f9f,stroke:#333,stroke-width:2px
```


```javascript
// $set operator to update fields
db.collection('users').updateOne({ name: 'Alice' }, { $set: { age: 26, city: 'New York' } });

// $inc operator to increment a field
db.collection('users').updateOne({ name: 'Alice' }, { $inc: { age: 1 } });

// $mul operator to multiply a field value
db.collection('users').updateOne({ name: 'Alice' }, { $mul: { age: 2 } });

// $unset operator to remove a field
db.collection('users').updateOne({ name: 'Alice' }, { $unset: { city: '' } });

// $rename operator to rename a field
db.collection('users').updateOne({ name: 'Alice' }, { $rename: { city: 'location' } });

```



### Delete

To delete documents from a collection, you use the `deleteOne()` or `deleteMany()` methods.

```javascript
db.collection('users').deleteOne({ name: 'Alice' });

db.collection('users').deleteMany({ city: 'New York' });
```


## Indexing

Indexing in MongoDB improves query performance by creating efficient data structures for faster data retrieval.

```mermaid
graph LR
  subgraph indexing["fa:fa-search Indexing"]
    singleField["fa:fa-file $singleField"]
    compound["fa:fa-layer-group $compound"]
    multikey["fa:fa-list $multikey"]
    text["fa:fa-font $text"]
  end

  style indexing stroke:#333,stroke-width:2px
  style singleField fill:#ccf,stroke:#f66,stroke-width:2px
  style compound fill:#add8e6,stroke:#333,stroke-width:2px
  style multikey fill:#9cf,stroke:#333,stroke-width:2px
  style text fill:#faa,stroke:#333,stroke-width:2px
```

- **Single Field Index**: Index on a single field of a document.
- **Compound Index**: Index on multiple fields.
- **Multikey Index**: Index on array fields.
- **Text Index**: Supports text search queries on string content.
  
## Single Field Index

To create an index, you use the `createIndex()` method.

```javascript
db.collection('users').createIndex({ name: 1 });
```

## Compound Index

A compound index in MongoDB is an index on multiple fields in a document.

```mermaid
graph LR
  subgraph compoundIndex["fa:fa-layer-group Compound Index"]
    field1["fa:fa-file Field 1"]
    field2["fa:fa-file Field 2"]
    field3["fa:fa-file Field 3"]
  end

  style compoundIndex stroke:#333,stroke-width:2px
  style field1 fill:#ccf,stroke:#f66,stroke-width:2px
  style field2 fill:#add8e6,stroke:#333,stroke-width:2px
  style field3 fill:#9cf,stroke:#333,stroke-width:2px
```


```javascript
db.collection('users').createIndex({ name: 1, age: 1, city: 1, country: 1, hobbies: 1 });
```

Prefixes of a compound index can be used to satisfy queries that match the index fields from left to right.

```javascript
db.collection('users').find({ name: 'Alice', age: 25 });
db.collection('users').find({ name: 'Alice', age: 25, city: 'New York' });
```


## Multikey Index

A multikey index in MongoDB is an index on an array field.



```mermaid
graph LR
  subgraph multikeyIndex["fa:fa-list Multikey Index"]
    arrayField["fa:fa-list Array Field"]
  end

  style multikeyIndex stroke:#333,stroke-width:2px
  style arrayField fill:#ccf,stroke:#f66,stroke-width:2px
```



```javascript
db.collection('users').createIndex({ hobbies: 1 });
```

### Limitations of Multikey Indexes

- you cannot create a compound index if more than one field is an array.

```javascript
db.collection('users').createIndex({ "hobbies": 1, "tags": 1 }); // Not allowed if both hobbies and tags are arrays
```


## Text Index

A text index in MongoDB is used for text search queries on string content.

```javascript
db.collection('articles').createIndex({ content: 'text' });
```

Querying with a text index:

```javascript
db.collection('articles').find({ $text: { $search: 'database' } });
```


## Array Operations

MongoDB supports a variety of array operations for working with arrays in documents.

```mermaid

graph 

  subgraph  
    each["fa:fa-list $each"]
    in["fa:fa-rectangle-list $in"]
    gt["fa:fa-angle-double-right $gt"]
    ne["fa:fa-angle-double-left $ne"]
  end


  subgraph     
    pull["fa:fa-trash-alt $pull"]
    pop["fa:fa-trash-alt $pop"]
    pullAll["fa:fa-trash-alt $pullAll"]
  end

    subgraph arrayOperations["fa:fa-list Array Operations"]
    elementMatch["fa:fa-equals $elementMatch"]
    push["fa:fa-plus $push"]
    set["fa:fa-hammer $set"]
    addToSet["fa:fa-plus-square $addToSet"]
  end


  style elementMatch fill:#add8e6, stroke:#333,stroke-width:2px
  style arrayOperations stroke:#333,stroke-width:2px
  style push fill:#ccf,stroke:#f66,stroke-width:2px
  style addToSet fill:#add8e6,stroke:#333,stroke-width:2px
  style each fill:#9cf,stroke:#333,stroke-width:2px
  style pull fill:#9cf,stroke:#333,stroke-width:2px
  style pop fill:#faa,stroke:#333,stroke-width:2px
  style pullAll fill:#ccf,stroke:#f66,stroke-width:2px
  style in fill:#add8e6,stroke:#333,stroke-width:2px
  style gt fill:#9cf,stroke:#333,stroke-width:2px
  style ne fill:#faa,stroke:#333,stroke-width:2px

```


### Adding Elements to an Array


To add elements to an array in a document, you use the `$push` operator.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $push: { hobbies: 'Reading' } });
```

### Querying Arrays with `$elemMatch`

Given a collection of students with scores in different subjects:

```javascript
{
  "_id": ObjectId("64d39a7a8b0e8c284a2c1234"),
  "name": "Alice",
  "scores": [
    { "subject": "Math", "score": 95 },
    { "subject": "English", "score": 88 }
  ]
},
{
  "_id": ObjectId("64d39a808b0e8c284a2c1235"),
  "name": "Bob",
  "scores": [
    { "subject": "Math", "score": 78 },
    { "subject": "English", "score": 92 }
  ]
}
```

**Querying with $elemMatch:**

To find students who specifically scored above 90 in Math, we need `$elemMatch`:

```javascript
db.students.find({ 
  scores: { 
    $elemMatch: { subject: "Math", score: { $gt: 90 } } 
  } 
})
```

**Result:** This will return only Alice, as she is the only student with a score above 90 in the "Math" subject. `$elemMatch` ensures that *all* the conditions within the array element must be met.


Without `$elemMatch` - This will return both Alice and Bob, as they both have scores above 90 in different subjects and not necessarily in the "Math" subject.



```mermaid
graph 
  subgraph query["Query"]
    elemMatchMath["scores { $elemMatch: { subject: 'Math', score: { $gt: 90 } } }"]
  end

  subgraph documents["Documents"]
    alice["Alice: { scores: [{ subject: 'Math', score: 95 }, { subject: 'English', score: 88 }] }"]
    bob["Bob: { scores: [{ subject: 'Math', score: 78 }, { subject: 'English', score: 92 }] }"]
  end



  elemMatchMath -->  alice --> found["✅"]
  elemMatchMath --> bob 
  bob --> |With $elemMatch|notFound["❌"]
   bob --> |Without $elemMatch|found2["✅"]

  style found fill:#90EE90
  style notFound fill:#FFCCCC

```

### Add Unique - `$addToSet`

The `$addToSet` operator in MongoDB is used to add elements to an array only if they are not already present. This prevents duplicate entries in the array.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $addToSet: { hobbies: 'Reading' } });
```


### Add Multiple  `$push` and `$each`

The `$push` operator in MongoDB is used to add elements to an array. The `$each` modifier allows you to add multiple elements to the array.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $push: { hobbies: { $each: ['Reading', 'Swimming'] } } });
```



### Add Sorted -  `$push` and `$sort`

The `$push` operator in MongoDB is used to add elements to an array. The `$sort` modifier allows you to sort the array elements.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $push: { scores: { $each: [85, 90], $sort: -1 } } });
```


### Add Limited -  `$push` and `$slice`

The `$push` operator in MongoDB is used to add elements to an array. The `$slice` modifier allows you to limit the number of elements in the array.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $push: { scores: { $each: [85, 90], $slice: -3 } } });
```


### Removing Elements from an Array

To remove elements from an array in a document, you use the `$pull` operator.

```javascript

db.collection('users').updateOne({ name: 'Alice' }, { $pull: { hobbies: 'Reading' } });
```


### Remove the first or last element - `$pop` 

The `$pop` operator in MongoDB is used to remove the first or last element from an array.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $pop: { hobbies: 1 } });
```


### Remove Multiple -  `$pullAll`

The `$pullAll` operator in MongoDB is used to remove all occurrences of specified values from an array.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $pullAll: { hobbies: ['Reading', 'Swimming'] } });
```

### Remove Multiple - `$pull` and `$in`

The `$pull` operator in MongoDB is used to remove elements from an array. The `$in` modifier allows you to specify multiple values to remove.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $pull: { hobbies: { $in: ['Reading', 'Swimming'] } } });
```



### Remove Condition - `$pull` and `$gt`

The `$pull` operator in MongoDB is used to remove elements from an array. The `$gt` modifier allows you to specify a condition for removing elements.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $pull: { scores: { $gt: 85 } } });
```
### Remove Not Equal - `$pull` and `$ne`

The `$pull` operator in MongoDB is used to remove elements from an array. The `$ne` modifier allows you to specify a condition for removing elements that are not equal to a value.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $pull: { scores: { $ne: 85 } } });
```

### Update Condition - `$set` and `$`

The `$set` operator in MongoDB is used to update fields in a document. The `$` positional operator allows you to update the first element that matches a condition in an array.

```javascript
db.collection('users').updateOne({ name: 'Alice', 'scores.subject': 'Math' }, { $set: { 'scores.$.score': 90 } });
```

### Update All - `$[]`

The `$[]` operator in MongoDB is used to update all elements in an array that match a condition.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $set: { 'scores.$[].score': 90 } });
```

To increment a field in all elements of an array, you can use the `$[]` operator with the `$inc` operator.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $inc: { 'scores.$[].score': 5 } });
```
### Update Filtered - `$[<identifier>]`

The `$[<identifier>]` operator in MongoDB is used to update elements in an array that match a condition.

```javascript
db.collection('users').updateOne({ name: 'Alice' }, { $set: { 'scores.$[elem].score': 90 } }, { arrayFilters: [{ 'elem.subject': 'Math' }] });
```



## Aggregation

Aggregation operations process data records and return computed results. Aggregation allows you to perform complex data processing and transformation.

### Aggregation Pipeline

The aggregation framework in MongoDB uses a pipeline approach, where multiple stages transform the documents.

```javascript
db.collection('orders').aggregate([
  { $match: { status: 'A' } },
  { $group: { _id: '$cust_id', total: { $sum: '$amount' } } },
  { $sort: { total: -1 } }
]);
```

```mermaid
graph
  subgraph aggregation["fa:fa-chart-line Aggregation"]
    stage1["fa:fa-filter Match"] --> stage2["fa:fa-layer-group Group"]
    stage2 --> stage3["fa:fa-sort Sort"]
  end

  style aggregation stroke:#333,stroke-width:2px
  style stage1 fill:#ccf,stroke:#f66,stroke-width:2px
  style stage2 fill:#add8e6,stroke:#333,stroke-width:2px
  style stage3 fill:#9cf,stroke:#333,stroke-width:2px

```




### Join Collections

MongoDB does not support joins like relational databases. Instead, you can use the `$lookup` operator to perform a left outer join between two collections.

```javascript
db.collection('orders').aggregate([
  {
    $lookup: {
      from: 'customers',
      localField: 'cust_id',
      foreignField: '_id',
      as: 'customer'
    }
  }
]);
```


```mermaid

graph
  subgraph joinCollections["fa:fa-link Join Collections"]
    orders["fa:fa-folder-open Orders"]
    customers["fa:fa-folder-open Customers"]
  end

  orders --> |$lookup| customers

  style joinCollections stroke:#333,stroke-width:2px
  style orders fill:#ccf,stroke:#f66,stroke-width:2px
  style customers fill:#add8e6,stroke:#333,stroke-width:2px
```


### Unwind Arrays

The `$unwind` operator in MongoDB is used to deconstruct an array field into multiple documents.

```javascript
db.collection('orders').aggregate([
  { $unwind: '$items' }
]);
```

### Group Documents

The `$group` operator in MongoDB is used to group documents by a specified key.

```javascript
db.collection('orders').aggregate([
  { $group: { _id: '$cust_id', total: { $sum: '$amount' } } }
]);
```

### Project Fields

The `$project` operator in MongoDB is used to include, exclude, or rename fields in the output documents.

```javascript
db.collection('orders').aggregate([
  { $project: { _id: 0, cust_id: 1, amount: 1 } }
]);
```

### Run Multiple Aggregations

You can run multiple aggregation pipelines in a single query using the `$facet` operator.

```javascript
db.collection('orders').aggregate([
  {
    $facet: {
      totalAmount: [
        { $group: { _id: null, total: { $sum: '$amount' } } }
      ],
      averageAmount: [
        { $group: { _id: null, average: { $avg: '$amount' } } }
      ]
    }
  }
]);
```




## Replica Sets

A replica set is a group of MongoDB instances that maintain the same data set. Replica sets provide redundancy and high availability.

### Components of a Replica Set

- **Primary**: Receives all write operations.
- **Secondary**: Replicates data from the primary. Can be used for read operations.
- **Arbiter**: Participates in elections for primary but does not hold data.

```mermaid
graph
  subgraph replicaSet["fa:fa-server Replica Set"]
    primary["fa:fa-database Primary"]
    secondary1["fa:fa-database Secondary"]
    secondary2["fa:fa-database Secondary"]
    arbiter["fa:fa-balance-scale Arbiter"]
  end

  primary --> secondary1
  primary --> secondary2
  primary --> arbiter

  style replicaSet stroke:#333,stroke-width:2px
  style primary fill:#ccf,stroke:#f66,stroke-width:2px
  style secondary1 fill:#add8e6,stroke:#333,stroke-width:2px
  style secondary2 fill:#add8e6,stroke:#333,stroke-width:2px
  style arbiter fill:#9cf,stroke:#333,stroke-width:2px
```

### Replica Set Configuration

To configure a replica set, you use the `rs.initiate()` method.

```javascript
rs.initiate({
  _id: 'rs0',
  members: [
    { _id: 0, host: 'mongo1:27017' },
    { _id: 1, host: 'mongo2:27017' },
    { _id: 2, host: 'mongo3:27017', arbiterOnly: true }
  ]
});
```

### Read Preference

Read preference in MongoDB determines how read operations are distributed across the replica set.

```mermaid
graph LR
  subgraph readPreference["fa:fa-eye Read Preference"]
    primary["fa:fa-database Primary"]
    secondary["fa:fa-database Secondary"]
    primaryPreferred["fa:fa-database PrimaryPreferred"]
    secondaryPreferred["fa:fa-database SecondaryPreferred"]
    nearest["fa:fa-database Nearest"]
  end

  style readPreference stroke:#333,stroke-width:2px
  style primary fill:#ccf,stroke:#f66,stroke-width:2px
  style secondary fill:#add8e6,stroke:#333,stroke-width:2px
  style primaryPreferred fill:#9cf,stroke:#333,stroke-width:2px
  style secondaryPreferred fill:#9cf,stroke:#333,stroke-width:2px
  style nearest fill:#9cf,stroke:#333,stroke-width:2px
```




- **Primary**: Reads from the primary.
- **Secondary**: Reads from the secondary.
- **PrimaryPreferred**: Reads from the primary if available, otherwise from the secondary.
- **SecondaryPreferred**: Reads from the secondary if available, otherwise from the primary.
- **Nearest**: Reads from the nearest member of the replica set.

```javascript

db.collection('users').find().readPref('secondary');
```


### Write Concern

Write concern in MongoDB determines the level of acknowledgment for write operations.

```mermaid
graph LR
  subgraph writeConcern["fa:fa-sync Write Concern"]
    w0["fa:fa-times w: 0"]
    w1["fa:fa-check w: 1"]
    wMajority["fa:fa-check w: majority"]
  end

  style writeConcern stroke:#333,stroke-width:2px
  style w0 fill:#ccf,stroke:#f66,stroke-width:2px
  style w1 fill:#add8e6,stroke:#333,stroke-width:2px
  style wMajority fill:#9cf,stroke:#333,stroke-width:2px
```



- **w: 0**: No acknowledgment.
- **w: 1**: Acknowledgment from the primary.
- **w: majority**: Acknowledgment from the majority of the replica set.

```javascript

db.collection('users').insertOne({ name: 'Alice' }, { writeConcern: { w: 'major

```

### Automatic Failover

MongoDB uses a heartbeat mechanism to detect the availability of replica set members. If the primary becomes unavailable, a new primary is elected.

- `Primary` is elected based on the number of votes from the replica set members.
- `Secondary` can be promoted to primary if the primary is unavailable.
- `Arbiter` is used to break the tie in elections.



```mermaid
graph
  subgraph failover["fa:fa-sync Failover"]
    primary["fa:fa-database Primary"]
    secondary1["fa:fa-database Secondary"]
    secondary2["fa:fa-database Secondary"]
    arbiter["fa:fa-balance-scale Arbiter"]
  end

  primary --> secondary1
  primary --> secondary2
  primary --> arbiter

  style failover stroke:#333,stroke-width:2px
  style primary fill:#ccf,stroke:#f66,stroke-width:2px
  style secondary1 fill:#add8e6,stroke:#333,stroke-width:2px
  style secondary2 fill:#add8e6,stroke:#333,stroke-width:2px
  style arbiter fill:#9cf,stroke:#333,stroke-width:2px
```

### Manual Failover

You can initiate a manual failover in MongoDB by forcing a replica set member to become the primary.

```javascript
rs.stepDown();
```


## Sharding

Sharding is a method for distributing data across multiple machines. It allows you to scale horizontally by adding more machines to your system.

A Collection is divided into chunks, and each chunk is stored on a different shard.

Each Shard is a subset of the data in a sharded cluster.


```mermaid
graph
  subgraph sharding["fa:fa-database Sharding"]
    collection["fa:fa-folder-open Collection"]
    chunk1["fa:fa-cube Chunk 1"]
    chunk2["fa:fa-cube Chunk 2"]
    chunk3["fa:fa-cube Chunk 3"]
  end

  collection --> chunk1
  collection --> chunk2
  collection --> chunk3

  style sharding stroke:#333,stroke-width:2px
  style collection fill:#ccf,stroke:#f66,stroke-width:2px
  style chunk1 fill:#add8e6,stroke:#333,stroke-width:2px
  style chunk2 fill:#9cf,stroke:#333,stroke-width:2px
  style chunk3 fill:#faa,stroke:#333,stroke-width:2px
```


### Components of Sharding

- **Shard**: A subset of the data in a sharded cluster.
- **Config Server**: Stores metadata and configuration settings for the cluster.
- **Query Router**: Routes queries to the appropriate shard.

```mermaid
graph
  subgraph sharding["fa:fa-database Sharding"]
    shard1["fa:fa-database Shard 1"]
    shard2["fa:fa-database Shard 2"]
    shard3["fa:fa-database Shard 3"]
    configServer["fa:fa-cogs Config Server"]
    queryRouter["fa:fa-route Query Router"]
  end

  shard1 --> configServer
  shard2 --> configServer
  shard3 --> configServer
  queryRouter --> shard1
  queryRouter --> shard2
  queryRouter --> shard3

  style sharding stroke:#333,stroke-width:2px
  style shard1 fill:#ccf,stroke:#f66,stroke-width:2px
  style shard2 fill:#add8e6,stroke:#333,stroke-width:2px
  style shard3 fill:#9cf,stroke:#333,stroke-width:2px
  style configServer fill:#ccf,stroke:#f66,stroke-width:2px
  style queryRouter fill:#add8e6,stroke:#333,stroke-width:2px

```

### Sharding Key

The sharding key is the field used to distribute data across the shards. It should be chosen carefully to ensure a balanced distribution of data.

```javascript
db.collection.createIndex({ _id: 'hashed' });
```


When selecting a shard key, consider the following factors:

- **Cardinality**: The number of unique values in the shard key.
- **Write Scaling**: The ability to distribute write operations across shards.
- **Query Isolation**: The ability to target specific shards for read operations.

### Shard Key Strategies

- **Hashed Sharding**: Distributes data evenly across the shards using a hash function.
- **Range Sharding**: Distributes data based on a range of values in the shard key.
- **Compound Sharding**: Distributes data based on multiple fields in the shard key.

```javascript
db.collection.createIndex({ _id: 'hashed' });
db.collection.createIndex({ date: 1 });
db.collection.createIndex({ country: 1, city: 1 });
```

## Mongoose

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a schema-based solution to model your application data.

```mermaid

graph 
  subgraph mongoose["fa:fa-database Mongoose"]
    connect["fa:fa-plug Connect"]
    schema["fa:fa-file Schema"]
    model["fa:fa-cube Model"]
    insert["fa:fa-plus-circle Insert"]
    find["fa:fa-eye Find"]
    update["fa:fa-edit Update"]
    delete["fa:fa-trash Delete"]
  end

  connect --> schema
  schema --> model
  model --> insert
  model --> find
  model --> update
  model --> delete

  style mongoose stroke:#333,stroke-width:2px
  style connect fill:#ccf,stroke:#f66,stroke-width:2px
  style schema fill:#add8e6,stroke:#333,stroke-width:2px
  style model fill:#9cf,stroke:#333,stroke-width:2px
  style insert fill:#ccf,stroke:#f66,stroke-width:2px
  style find fill:#add8e6,stroke:#333,stroke-width:2px
  style update fill:#9cf,stroke:#333,stroke-width:2px
  style delete fill:#faa,stroke:#333,stroke-width:2px
```


### Connecting to MongoDB

To connect to MongoDB using Mongoose, you use the `connect()` method.

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/myapp', { useNewUrlParser: true, useUnifiedTopology: true });
```


### Defining a Schema

A Mongoose schema defines the structure of the documents in a collection.

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  age: Number
});
```

### Creating a Model

A Mongoose model is a class that represents a collection in MongoDB.

```javascript
const User = mongoose.model('User', userSchema);
```

### Inserting Documents

To insert a document into a collection, you create an instance of the model and call the `save()` method.

```javascript
const user = new User({ name: 'Alice', age: 25 });

user.save();
```

### Querying Documents

To query documents from a collection, you use the `find()` method.

```javascript
User.find({ name: 'Alice' });
```

With Projection:

```javascript
User.find({ name: 'Alice' }, { name: 1, age: 1 });
```

### Updating Documents

To update documents in a collection, you use the `updateOne()` method.

```javascript
User.updateOne({ name: 'Alice' }, { age: 26 });
```

### Deleting Documents

To delete documents from a collection, you use the `deleteOne()` method.

```javascript
User.deleteOne({ name: 'Alice' });
```


### Middleware

Mongoose middleware are functions that are executed before or after certain operations.

```javascript
userSchema.pre('save', function(next) {
  console.log('Saving user...');
  next();
});
```

### Virtuals

Mongoose virtuals are document properties that you can get and set but that do not get persisted to MongoDB.

```javascript

userSchema.virtual('fullName').get(function() {
  return this.name + ' ' + this.age;
});
```

### Plugins

Mongoose plugins are reusable pieces of schema middleware that can be added to any schema.

```javascript
const timestampPlugin = require('./plugins/timestamp');

userSchema.plugin(timestampPlugin);
```

### Transactions

Mongoose transactions allow you to perform multiple operations on multiple documents in a single transaction.

```javascript

const session = await mongoose.startSession();
session.startTransaction();

try {
  await User.create({ name: 'Alice' }, { session });
  await User.create({ name: 'Bob' }, { session });

  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
} finally {
  session.endSession();
}
```

### Aggregation

Mongoose provides a fluent API for building aggregation pipelines.

```javascript
const result = await User.aggregate([
  { $match: { name: 'Alice' } },
  { $group: { _id: '$name', total: { $sum: '$age' } }
]);
```

### Indexes

Mongoose allows you to define indexes on your schemas.

```javascript
userSchema.index({ name: 1 });
```

### Population

Mongoose population allows you to reference documents in other collections.

```javascript
const userSchema = new mongoose.Schema({
  name: String,
  posts: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Post' }]
});
```

### Validation

Mongoose provides built-in validation for schema fields.

```javascript
const userSchema = new mongoose.Schema({
  name: { type: String, required: true }
});
```
