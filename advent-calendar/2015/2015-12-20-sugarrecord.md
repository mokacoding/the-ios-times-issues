---
layout: post
title: SugarRecord
date: 2015-12-20
category: advent-calendar
---

One requirement almost every apps has, in one form or the other, is data persistence. Any app with a minimum complexity will probably need a database. We saw this already on [day 14](http://theiostimes.com/advent-calendar/realm.html).

Apple provides us with [Core Data](https://developer.apple.com/library/tvos/documentation/Cocoa/Conceptual/CoreData/index.html), an object model layer framework that is as powerful as it is hard to master. Today's library will make your Core Data experience _sweeter_.

[SugarRecord](https://github.com/SwiftReactive/SugarRecord) by [Pedro Piñera](https://twitter.com/pepibumur) and the [SwiftReactive](http://swiftreactive.github.io/) team, is a Swift wrapper on top of Core Data that provide APIs that are simpler to use and Swiftier.

Let's have a look a what SugarRecord has to offer, using the same example we used to play around with another database library, [Realm](http://theiostimes.com/advent-calendar/realm.html).

### Setting up the Core Data stack

One of Core Data's less appealing features is the amount of boilerplate code needed to do anything. The setup in particular is quite long.

This is how to achieve the same result with SugarRecord:

```swift
func coreDataStorage() -> CoreDataDefaultStorage {
  let store = CoreData.Store.Named("posts-db")
  let bundle = NSBundle(forClass: CoreDataDefaultStorageTests.classForCoder())
  let model = CoreData.ObjectModel.Merged([bundle])
  let defaultStorage = try! CoreDataDefaultStorage(store: store, model: model)

  return defaultStorage
}
```

### How to create a new instance of a model

In our example we had a post resource has an id, a user id, a title, and a body. We can create an instance of `Post` using SugarRecord like this:

```swift
db = coreDataStorage()

db.operation { (context, save) -> Void in
  do {
    let post: Post = try context.create()
    // configure the post...
    save()
  } catch {
    // TODO: handle errors as appropriate
  }
}
```

### How to fetch and update data

We can fetch all our posts from the database like this:

```swift
var posts: [Post]
do {
  posts = try db.fetch(Request<Post>())
} catch {
  // TODO: handle errors as appropriate
  posts = []()
}
```

The fetch operation can be more granular that just "fetch all", for example:

```swift
let myPosts: [Post] = try db.fetch(Request<Post>().filteredWith("userId", equalTo: "..."))

let postsByTitle: [Post] = try db.fetch(Request<Post>().sortedWith("title", ascending: true))

let predicate: NSPredicate = NSPredicate(format: "id == %@", "...")
let thePost: Post? = try db.fetch(Request<Post>().filteredWith(predicate: predicate)).first
```

More often that not the data in your app is not going to be static, and you will have to change it and persist those updates. This is how a SugarRecord write operation looks like:

```swift
let updatedBody = ...
db.operation { (context, save) -> Void in
  post.body = updatedBody
  save()
}
```

### How to delete data

We've seen all the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) functions except for the last one, delete.

The way you delete data using SugarRecord is in line with the rest of the database operation APIs:

```swift
db.operation { (context, save) -> Void in
  do {
    try context.remove([post])
    save()
  } catch {
    // TODO: handle errors as appropriate
  }
}
```

### Next Steps

What we've seen in this tutorial is just enough to get you started with SugarRecord and start writing simpler Core Data code.

SugarRecord has more to offer, in particular a [Functional Reactive Programming](https://en.wikipedia.org/wiki/Functional_reactive_programming) interface. Head over to the [project's repo on GitHub](https://github.com/SwiftReactive/SugarRecord) to find out more about this very nice library.

---

That's it for today. See you tomorrow with a little GIF library. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

