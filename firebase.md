---
title: Firebase for Web Reference
---

{% highlight html %}
 <script src="https://www.gstatic.com/firebasejs/3.1.0/firebase.js"></script>
  <script>
    // Initialize Firebase
    var config = {
      apiKey: "gibberish", // the KEY
      authDomain: "app-name.firebaseapp.com", // Auth
      databaseURL: "https://app-name.firebaseio.com", // Database
      storageBucket: "app-name.appspot.com", // Storage
    };
    firebase.initializeApp(config);
  </script>
{% endhighlight %}



# Authentication

# Database

## Save by setting

{% highlight js %}
firebase.database().ref('users/' + userId).set({
  username: name,
  email: email
});
{% endhighlight %}


Use the new key, and multiple update.


{% highlight js %}
// A post entry.
  var postData = {
    author: username,
    uid: uid,
    body: body,
    title: title,
    starCount: 0
  };

  // Get a key for a new Post.
  var newPostKey = firebase.database().ref().child('posts').push().key;

  // Write the new post's data simultaneously in the posts list and the user's post list.
  var updates = {};
  updates['/posts/' + newPostKey] = postData;
  updates['/user-posts/' + uid + '/' + newPostKey] = postData;

  return firebase.database().ref().update(updates);
{% endhighlight %}


## Delete

Call `remove()` on a reference to the location of that data.

Or specify `null` as the value for another write operation such as `set()` or `update()`. You can use this technique with `update()` to delete multiple children in a single API call.

## Promise

To know when your data is committed to the Firebase Realtime Database server, you can use a Promise. Both `set()` and `update()` can return a Promise you can use to know when the write is committed to the database.




# Storage


