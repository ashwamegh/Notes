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


##### Logical Operators

|	Operator	| 		Description		|
|---------------|-----------------------|
|$or			|Joins query clauses with a logical OR returns all documents that match the conditions of either clause.|
|$and			|Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.|
|$not			|Inverts the effect of a query expression and returns documents that do not match the query expression.|
|$nor			|Joins query clauses with a logical NOR returns all documents that fail to match both clauses.|

**Query Eg**

```shell
db.movieDetails.find({ $and: [{ "metacritic" : { $ne:null }}, { "metacritic":{ $exists: true } } ]  })
```
```shell
db.movieDetails.find({ $or: [{ "tomato.meter" : { $gt:90 }}, { "metacritic":{ $gt: 70 } } ]  })
```

##### Regex Operators

|	Operator	| 		Description		|
|---------------|-----------------------|
|$regex 		|Provides regular expression capabilities for pattern matching strings in queries.|

**Query Eg:**

```shell
db.movieDetails.find({"awards.text" : { $regex: /^Won\s.*/}}, {"awards.text":1, "title":1, "_id":0})
```

##### Array Operators

|	Operator	| 		Description		|
|---------------|-----------------------|
|$all			|Matches arrays that contain all elements specified in the query.|
|$elemMatch		|Selects documents if element in the array field matches all the specified $elemMatch conditions.|
|$size			|Selects documents if the array field is a specified size.|

**Query Eg:**

```shell
db.movieDetails.find({"genres": {$all : ["Action","Adventure","Drama"]}}, {"title":1, "_id":0})
```
```shell
db.movieDetails.find({"countries": {$size: 1}}, {"title":1, "_id":0})
```
```shell
db.movieDetails.find({ boxOffice: {$elemMatch: { country: "UK", revenue: { $gt: 15 } } } })
```


