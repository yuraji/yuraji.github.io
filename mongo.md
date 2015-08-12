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
{% endhighlight %}


{% highlight js %}
 db.people.find( { $or:[ 
   { name: { $regex: "e$"} },
   { profession: { $exists: true }}
 ] } )
{% endhighlight %}




# Update



# Delete



