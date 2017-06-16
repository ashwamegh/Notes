# MongoDB University Notes

These are my personal notes for MongoDB University course: **M101JS: MongoDB for Node.js Developers**

<br/>

## Week : 1

### Queries:

- `db.movie.find({})`
- `hasNext()`
- `next()`

## Week : 2

### CRUD:

#### Create

- `insertOne()`
- `insertMany([], ordered: false)`

#### Read

- `find({})`
- `find({}).count()`

##### Cursors
> The cursors work as default container of batch files when the database is queried. It acts as a pointer to the 

##### Projections
> Filter the number of result keys that are shown, and reduces processing speed & memory.

```shell
find({display : "true"},{ title:1, _id: 0} )
```


##### Comparison Operators

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$eq`			|Matches values that are equal to a specified value.|
|`$gt`			|Matches values that are greater than a specified value.|
|`$gte`			|Matches values that are greater than or equal to a specified value.|
|`$lt`			|Matches values that are less than a specified value.|
|`$lte`			|Matches values that are less than or equal to a specified value.|
|`$ne`			|Matches all values that are not equal to a specified value.|
|`$in`			|Matches any of the values specified in an array.|
|`$nin`			|Matches none of the values specified in an array.|

For Eg:

```shell
db.moviesScratch.find({ year: {$gt: 1976, $lt: 2000}}).pretty()
```


##### Element Query Operators

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$exists`		|Matches documents that have the specified field.|
|`$type`		|Selects documents if a field is of the specified type.|

For Eg;

```shell
db.movieDetails.find({"tomato.meter": {$exists: false}})
```

```shell
db.moviesScratch.find({"_id": {$type: "string"}})
```


##### Logical Operators**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$or`			|Joins query clauses with a logical OR returns all documents that match the conditions of either clause.|
|`$and`			|Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.|
|`$not`			|Inverts the effect of a query expression and returns documents that do not match the query expression.|
|`$nor`			|Joins query clauses with a logical NOR returns all documents that fail to match both clauses.|

Query Eg:

```shell
db.movieDetails.find({ $and: [{ "metacritic" : { $ne:null }}, { "metacritic":{ $exists: true } } ]  })
```
```shell
db.movieDetails.find({ $or: [{ "tomato.meter" : { $gt:90 }}, { "metacritic":{ $gt: 70 } } ]  })
```

##### **Regex Operators**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$regex` 		|Provides regular expression capabilities for pattern matching strings in queries.|

Query Eg:

```shell
db.movieDetails.find({"awards.text" : { $regex: /^Won\s.*/}}, {"awards.text":1, "title":1, "_id":0})
```

##### **Array Operators**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$all`			|Matches arrays that contain all elements specified in the query.|
|`$elemMatch`		|Selects documents if element in the array field matches all the specified $elemMatch conditions.|
|`$size`			|Selects documents if the array field is a specified size.|

Query Eg:

```shell
db.movieDetails.find({"genres": {$all : ["Action","Adventure","Drama"]}}, {"title":1, "_id":0})
```
```shell
db.movieDetails.find({"countries": {$size: 1}}, {"title":1, "_id":0})
```
```shell
db.movieDetails.find({ boxOffice: {$elemMatch: { country: "UK", revenue: { $gt: 15 } } } })
```


### **Updating Documents**

#### **Update Operators**

Thses operators update the document based on the inputs provided with the function (i.e. updateOne(), updatemany()..).

#### **Field Operators**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$inc`	|Increments the value of the field by the specified amount.
|`$mul`	|Multiplies the value of the field by the specified amount.
|`$rename`|	Renames a field.
|`$setOnInsert`	|Sets the value of a field if an update results in an insert of a document. Has no effect on update operations |`that` |modify existing documents.
|`$set`	|Sets the value of a field in a document.
|`$unset`	|Removes the specified field from a document.
|`$min`	|Only updates the field if the specified value is less than the existing field value.
|`$max`	|Only updates the field if the specified value is greater than the existing field value.
|`$currentDate`	|Sets the value of a field to current date, either as a Date or a Timestamp.


#### **Array Operators**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$`	|Acts as a placeholder to update the first element that matches the query condition in an update.|
|`$addToSet`|	Adds elements to an array only if they do not already exist in the set.|
|`$pop`|	Removes the first or last item of an array.|
|`$pullAll`|	Removes all matching values from an array.|
|`$pull`|	Removes all array elements that match a specified query.|
|`$pushAll`|	Deprecated. Adds several items to an array.|
|`$push`|	Adds an item to an array.|

#### **Modifiers**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$each`|	Modifies the $push and $addToSet operators to append multiple items for array updates.|
|`$slice`|	Modifies the $push operator to limit the size of updated arrays.|
|`$sort`	|Modifies the $push operator to reorder documents stored in an array.|
|`$position`|	Modifies the $push operator to specify the position in the array to add elements.|

#### **Bitwise Operator**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$bit`|	Performs bitwise AND, OR, and XOR updates of integer values.|

#### **Isolation Operator**

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$isolated` |	Modifies the behavior of a write operation to increase the isolation of the operation.|


Query Eg:

```shell
db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $push: {
    reviews: {
      $each: [{
        rating: 0.5,
        date: ISODate("2016-01-13T07:00:00Z"),
        reviewer: "Shannon B.",
        text: "Enjoyed watching with my kids!"
      }],
      $position: 0,
      $slice: 5
    }
  }
})
```

```shell
// Could do this, but it's probably the wrong semantics.
db.movieDetails.updateMany({
  rated: null
}, {
  $set: {
    rated: "UNRATED"
  }
})


// Better to do this.
db.movieDetails.updateMany({
  rated: null
}, {
  $unset: {
    rated: ""
  }
})
```

#### **Upserts:**

> The operations in which , no document were found for our operations, this build the document we queried for.

Query Eg:

```shell
db.movieDetails.updateOne({
  "imdb.id": detail.imdb.id
}, {
  $set: detail
}, {
  upsert: true
});
```

#### **replaceOne():**

It replaces the previous document with the new one, without duplicating it.

Query Eg:

```shell
db.movies.replaceOne({
    "imdb": detail.imdb.id
  },
  detail);
```


