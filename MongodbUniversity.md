# MongoDB University Notes

These are my personal notes for MongoDB University course: **M101JS: MongoDB for Node.js Developers**

<br/>

### Week : 1

#### Queries:

- `db.movie.find({})`
- `hasNext()`
- `next()`

### Week : 2

#### CRUD:

##### Create

- `insertOne()`
- `insertMany([], ordered: false)`

##### Read

- `find({})`
- `find({}).count()`

###### Cursors
> The cursors work as default container of batch files when the database is queried.

###### Projections
> Filter the number of result keys that are shown, and reduces processing speed & memory.

- `find({display : "true"},{ title:1, _id: 0} )`


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

`db.moviesScratch.find({ year: {$gt: 1976, $lt: 2000}}).pretty()`


##### Element Query Operators

|	Operator	| 		Description		|
|---------------|-----------------------|
|`$exists`		|Matches documents that have the specified field.|
|`$type`		|Selects documents if a field is of the specified type.|

For Eg;

`db.movieDetails.find({"tomato.meter": {$exists: false}})`

`db.moviesScratch.find({"_id": {$type: "string"}})`
