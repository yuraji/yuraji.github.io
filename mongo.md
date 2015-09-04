---
title: MongoDB
---


# Mongo Reference

[docs.mongodb.org: mongo Shell Quick Reference](http://docs.mongodb.org/manual/reference/mongo-shell/)  
<http://www.mongodbspain.com/wp-content/uploads/2014/03/MongoDBSpain-CheetSheet.pdf>  
<https://dzone.com/refcardz/mongodb>  
<https://blog.codecentric.de/files/2012/12/MongoDB-CheatSheet-v1_0.pdf>  

# Mongo Shell Cheatsheet


## Show Databases

	show dbs

## Switch to database

	use project

## Show Collections

	show collections



# Insert


{% highlight js %}
 doc = { name: "Smith", age: 30, profession: "hacker" }
 db.people.insert(doc)
{% endhighlight %}


# Read

{% highlight js %}
 db.things.find()
 db.things.findOne()
{% endhighlight %}



Iterate with javascript `forEach` and print with `print` and `printjson`:
{% highlight js %}
 db.people.find().forEach(function(doc){
   print( doc.name + ':'); printjson(doc);
 })
{% endhighlight %}




{% highlight js %}
	db.people.findOne( {/*FIND*/}, {/*FILDS*/} )
 db.people.findOne( { name:"Jones"}, {name:true, age:true, _id:false} )
{% endhighlight %}



# Batch random insert


{% highlight js %}
 for(i=0; i<1000; i++){
   names=['exam', 'essay', 'quiz'];
   for(j=0; j<3; j++) {
     db.scores.insert({ student:i, type:names[j], score:Math.round(Math.random()*100) });
   }
 }
 db.scores.find().pretty()
 db.scores.find({student:19, type:'essay'}, {score:true, _id:false})
{% endhighlight %}




## Find: $gt and $lt



{% highlight js %}
 // $gte >=
 // $gt >
 // $lte <=
 // $lt <
 db.scores.find( { score:{ $gt: 95, $lte: 97 } })
{% endhighlight %}




## Find: inequalities

{% highlight js %}
 ['Alice', 'Bob', 'Charlie', 'Dave', 'Edgar', 'Fred'].forEach(function(name,i){
   db.people.insert({ name:name, age: 30+i })
 })
{% endhighlight %}

{% highlight js %}
 db.people.find({ name: { $gt:"B", $lt:"D" }})
 // gets "Bob" and "Charlie"
{% endhighlight %}


## Find: $exists

Find where a given field exists


{% highlight js %}
 db.people.find({ profession: {$exists: true} })
{% endhighlight %}



## Find: $type


1 — number  
2 — string  
4 — array  
5 — Binary Data  
9 — Date  

<http://www.w3resource.com/mongodb/mongodb-type-operators.php>

{% highlight js %}
 db.people.insert({name:42})
 db.people.find({name: { $type: 2 } }) // "string"
 db.people.find({name: { $type: 1 } }) // "number"
{% endhighlight %}




## Find: $regex


{% highlight js %}
 db.people.find({name: { $regex: "e$"  } })
 db.people.find({name: { $regex: "^[A-C]"  } })
{% endhighlight %}




## Find: $or




{% highlight js %}
 db.people.find( { $or:[ 
   { name: { $regex: "e$"} },
   { profession: { $exists: true }}
 ] } )
{% endhighlight %}


## Find in array


{% highlight js %}
 // db.accounts.insert( { name : "George", favorites: [ 'ice cream', 'pretzels' ] } );
 // db.accounts.insert( { name : "Hovard", favorites: ['pretzels', 'coke' ] } );
 db.accounts.find( {favorites: 'coke' } )
{% endhighlight %}


{% highlight js %}
{% endhighlight %}


### Find documents that contain **all** asked values in an array

{% highlight js %}
db.accounts.find( {favorites: { $all: ['pretzels', 'beer'] } } )
{% endhighlight %}

### Find documents that contain **any** asked values in an array

{% highlight js %}
 db.accounts.find( {name: { $in: ['George', 'Hovard'] } } )
 db.accounts.find( {favorites: { $in: ['pretzels', 'coke'] } } )
{% endhighlight %}



## Find in subdocument


{% highlight js %}
 // db.users.insert( { name: "richard", email: { work: "richard@10gen.com", personal: "kreuter@example.com" }  })
 db.users.find( { "email.work": "richard@10gen.com" } )
{% endhighlight %}



## Find: $elemMatch

Matches documents that contain an array field with at least one element that matches all the specified criteria.  

{% highlight js %}
 db.students.find({ scores: { $elemMatch: { type: 'exam', score: {'$gt':99.8} } } })
{% endhighlight %}

It is important to use instead of the `$and` operator, because `$and:[{'$scores.type':'exam'},{'scores.score':{$gt:99.8}}]` will return documents that don't ensure that score is for the exam.




# Shell cursor


{% highlight js %}
 cur = db.accounts.find(); null;

 // manually loop through each result:
 cur.hasNext() && cur.next()

 // print all results:
 while (cur.hasNext()) printjson(cur.next())
{% endhighlight %}

Set limit on the cursor:
{% highlight js %}
 cur.limit(5); null;
{% endhighlight %}


Sort the order:
{% highlight js %}
 cur.sort( { name: -1 }); // sort by `name` DESC

 // Get last 3 names
 cur.sort( { name: -1 }).limit(3); null;
{% endhighlight %}


Skip *n* results (for pagination):
{% highlight js %}
 cur.skip(2); null;
{% endhighlight %}




# Counting

{% highlight js %}
 > db.scores.count()
 3000
 > db.scores.count({ type : 'exam' })
 1000
{% endhighlight %}







# Update


Replace document:
{% highlight js %}
 db.people.update( /*QUERY*/ { name: "Smith" }, /*REPLACE_DOC*/ { name: "Thompson", salary: 5000 } )
{% endhighlight %}

Update fields in a documents `$set`:

{% highlight js %}
 db.people.update( { name: "Alice" }, { $set: {age: 30} } )
{% endhighlight %}

Increment field `$inc`:

{% highlight js %}
 db.people.update( { name: "Alice" }, { $inc: {age: 1} } )
 // if the field for incrementation doesn't exist, it will be created with the value of increment
{% endhighlight %}


Remove field `$unset`:

{% highlight js %}
 db.people.update( { name: "Jones" }, { $unset: {profession: 1} } )
{% endhighlight %}


Update an array items:
{% highlight js %}
 // db.arrays.insert( { _id: 0, a: [ 1, 2, 3, 4 ] } )
 // Update the third item in the array:
 db.arrays.update( { _id:0 }, { $set: { "a.2" : 5 } } )
 // Result: { "_id" : 0, "a" : [ 1, 2, 5, 4 ] }

 // Add item to an array:
 db.arrays.update( { _id:0 }, { $push: { a : 6 } } )
 // Result: { "_id" : 0, "a" : [ 1, 2, 5, 4, 6 ] }

 // Remove last element in an array:
 db.arrays.update( { _id:0 }, { $pop: { a : 1 } } )
 // Result: { "_id" : 0, "a" : [ 1, 2, 5, 4 ] }

 // Remove first element in an array:
 db.arrays.update( { _id:0 }, { $pop: { a : -1 } } )
 // Result: { "_id" : 0, "a" : [ 2, 5, 4 ] }

 // Push multiple elements to end of an array:
 db.arrays.update( { _id:0 }, { $pushAll: { a : [ 7, 8, 9 ] } } )
 // Result: { "_id" : 0, "a" : [ 2, 5, 4, 7, 8, 9 ] }

 // Remove a given value from an array
 db.arrays.update( { _id:0 }, { $pull: { a : 5 } } )
 // Result: { "_id" : 0, "a" : [ 2, 4, 7, 8, 9 ] }

 // Remove given values from an array
 db.arrays.update( { _id:0 }, { $pullAll: { a : [ 2, 4, 8 ] } } )
 // Result: { "_id" : 0, "a" : [ 7, 9 ] }

 // Only add an item to an array if it is not already there:
 db.arrays.update( { _id:0 }, { $addToSet: { a : 5 } } )
 // Result: { "_id" : 0, "a" : [ 7, 9, 5 ] }

{% endhighlight %}




# Upsert

{% highlight js %}
 // Given that no document with { name: "George" } exist
 db.people.update( { name: "George" }, { $set: {age:40} }, { upsert: true }   )
 // New document will be created and will contain { "name" : "George", "age" : 40 }
{% endhighlight %}


# Multi-update

`{}` acts as selector for all documents, but by default `update` will only update the first document. To update all matching documents, use option `{ multi: true }`:


{% highlight js %}
 db.people.update( {}, { $set: { title: "Dr" } }, { multi: true } )
{% endhighlight %}

Multi-update is not an atomic operation, and it is possible that another `find` operation will be processed before all documents are updated.

{% highlight js %}
 // Increment `score` by 20 where `score`<70
 db.scores.update( { score: {$lt:70} }, { $inc: {score:20} }, {multi:true}  )
{% endhighlight %}


# Delete

{% highlight js %}
 db.people.remove( /*QUERY*/{ } )
{% endhighlight %}


Remove one:

{% highlight js %}
 db.people.remove( { }, { justOne: true } )
{% endhighlight %}



Drop whole collection:

{% highlight js %}
 db.people.drop()
{% endhighlight %}






# Sort, Skip and Limit

They always happen in this order:

1. Sort
2. Skip
3. Limit



{% highlight js %}
 var grades = db.collection('grades');
 var cursor = grades.find({});
 cursor.skip(1);
 cursor.limit(4);
 cursor.sort('grade', 1);
{% endhighlight %}


## Multiple sort options

{% highlight js %}
 cursor.sort([['grade', 1], ['student', -1]]);
{% endhighlight %}


## Using options

{% highlight js %}
 var options = { 'skip' : 1,
                 'limit' : 4,
                 'sort' : [['grade', 1], ['student', -1]] };
 var cursor = grades.find(/*query*/{}, /*projection*/{}, options);
{% endhighlight %}




# Explain

{% highlight js %}
 db.students.explain().find({});
 db.students.explain(true).find({});
{% endhighlight %}


# Index
{% highlight js %}
 db.students.createIndex({ username: 1 })
 db.students.getIndexes()
 db.students.dropIndex({ username: 1 })
{% endhighlight %}


## Unique index:

{% highlight js %}
 db.students.createIndex({ username: 1 }, { unique: true })
{% endhighlight %}


## Sparse index:

when the index key is missing from some of the documents. The index will not include documents that are missing the key.

{% highlight js %}
 db.employees.createIndex({ phone: 1 }, { unique: true, sparse: true })
{% endhighlight %}

Such index cannot be used for sorting, because the engine will still have to scan whole collection knowing that some documents are missing from the index.

## Background index:

Slower, but non-blocking for other requests.

{% highlight js %}
 db.students.createIndex({ 'score.score': 1 }, { background: true })
{% endhighlight %}


# Index size


{% highlight js %}
 db.students.stats()
 db.students.totalIndexSize()
{% endhighlight %}



# Explain


{% highlight js %}
 db.foo.explain().find()

 var exp = db.example.explain() /* explainable object */
 exp.help()
 exp.count()
{% endhighlight %}


{% highlight js %}
 var exp = db.example.explain("allPlansExecution")
{% endhighlight %}


# Covered queries

Cueries that indexes can answer without quering collection.

{% highlight js %}
 db.colors.createIndex({ color: 1 })
 db.colors.find({}, { _id: 0 })
{% endhighlight %}





# Geospatial Indexes



{% highlight js %}
 db.stores.ensureIndex({ location: '2d', type:1 })
 db.stores.find({ location: { $near: [50,50] } }).limit(3)
{% endhighlight %}





# Aggregation

{% highlight js %}
 db.products.aggregate([ { $group: {
   _id: "$manufacturer",
   num_products:{ $sum:1 }  
 } } ])
{% endhighlight %}


## Aggregation pipeline

{% highlight js %}
 $project - reshape   - 1:1
 $match   - filter    - n:1
 $group   - aggregate - n:1
 $sort    - sort      - 1:1
 $skip    - skips     - n:1
 $limit   - limit     - n:1
 $unwind  - normalize - 1:n
 $out     - output    - 1:1
{% endhighlight %}


# Compound Grouping

Find number of products that each manufacturer had in each category:

{% highlight js %}
 db.products.aggregate([
   {$group:
     _id: {
       "manufacturer": "$manufacturer",
       "category": "$category"
     },
     num_products: { $sum: 1 }
   }
 ])
{% endhighlight %}


## Aggregation expressions

`$sum` - sum
`$avg` - average
`$min` - minimum
`$max` - maximum
`$push`, `$addToSet` - array
`$first`, `$last` - useful with sorted array


## $sum

Sum population in each state:

{% highlight js %}
 db.zips.aggregate([ { $group:{
   _id: "$state",
   population: { $sum:"$population" }
 } } ])
{% endhighlight %}

## $avg

Average price in a category:

{% highlight js %}
 db.products.aggregate([ { $group:{
   _id: "$category",
   avg_price: { $avg:"$price" }
 } } ])
{% endhighlight %}




## $addToSet

See which categories each brand sells - get array of categories for each brand.

{% highlight js %}
 db.products.aggregate([ { $group:
   {
     _id: { "maker": "$manufacturer" },
     categories:{ $addToSet:"$category" }
   } } ])
{% endhighlight %}



## $push

Doesn't guarantie that the added value will be added only once (that is it won't be unique)

## $max, $min

{% highlight js %}
db.products.aggregate([{$group: {
   _id: {"maker":"$manufacturer"},
   maxprice:{$max:"$price"} 
} } ])
{% endhighlight %}


## Double grouping

Given scores for each students for each class. What's the average class grade in each class?

{% highlight js %}
 db.grades.aggregate([
  {
    '$group':{ /* find avg score of each student in each class */
      _id:{
        class_id: "$class_id",
        student_id: "$student_id"
      },
      'average':{ "$avg":"$score" }
    }  /* returns { _id:{class_id:22, student_id:33}, average:44  }   */
  },
  {
    '$group':{
      _id: "$_id.class_id",
      'average':{ "$avg":"$average" }
    }  /* returns { _id:22, average:44  }   */
  }
 ])
{% endhighlight %}


## $project

- remove keys
- add new keys
- reshape keys
- use some simple functions on keys
  - toUpper
  - $toLower
  - $add
  - $multiply

{% highlight js %}
 db.products.aggregate( [ { $project: 
   {
     _id: 0, /* remove _id from results */
     maker: { $toLower: "$manufacturer" },
     details: {
       category: "$category",
       price: { "$multiply": [ "$price", 10 ] }
     },
     item: "$name"    
   }
 } ] )
 /* Example output:
 {
   "maker" : "apple",
   "details" : {
     "category" : "Tablets",
     "price" : 4990
   },
   "item" : "iPad 16GB Wifi"
 }
 */
{% endhighlight %}








