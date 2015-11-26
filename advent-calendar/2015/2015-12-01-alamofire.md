---
layout: post
title: Alamofire
date: 2015-12-01
category: advent-calendar
---

[Alamofire](https://github.com/Alamofire/Alamofire) is the Swift cousin of [AFNetworking](https://github.com/Alamofire/Alamofire), the most popular iOS development library.

This library's tag line is "Elegant networking in Swift", and it delivers on the promise, with neat, clean, Swifty APIs for your everyday networking needs, as well as more advanced features.

Let's have a look at how to get and send data to a server with Alamofire. You can find all the code for this example in the [Advent Calendar repo](https://github.com/mokacoding/AdventCalendar2015/01-Alamofire).

### How to do a GET request with Alamofire

```swift
Alamofire.request(.GET, "http://jsonplaceholder.typicode.com/posts")
  .responseJSON { response in

    if let JSON = response.result.value as? [[String: AnyObject]]! {
      // Deserialise JSON and "do stuff"
    } else if let error = response.result.error {
      print("Error: \(error)")
      // Notify user of the error in the appropriate way
    }

}
```

It is as simple as that. Can you guess how we would send data to the server then?

One of the nicest features of Alamofire is its chainable APIs. For example we can check that the network response status is in the valid HTTP range, 200...299, by adding a `validate()` step.

```swift
Alamofire.request(.GET, "http://jsonplaceholder.typicode.com/posts")
  .validate()
  .responseJSON { response in
    // ...
}
```

If validation fails, subsequent calls to response handlers will have an associated error.

### How to do a POST request with Alamofire

A POST request with JSON parameters is as easy as a GET request, just use `.POST` instead, and pass the parameters' dictionary.

```swift
let parameters = [
  "title": "a cool title",
  "body": "a meaningful content",
  "userId": "42"
]

Alamofire.request(.POST, "http://jsonplaceholder.typicode.com/posts", parameters: parameters, encoding:  .JSON)
  .validate()
  .responseJSON { [weak self] response in

    switch response.result {
    case .Success(let responseContent):
      // Handle success case...
      break
    case .Failure(let error):
      // Handle failure case...
      break
    }
}
```

Notice how we are switching on the `response.result` enum, and using its associated valuer. This really is elegant networking in Swift.

### Where to go from here

The iOS Times Advent Calendar's post certainly don't aim to be comprehensive tutorials on how to use a library, but rather provide a little taste of it, to ignite your curiosity.

If you find Alamofire interesting and think you could benefit from using it in your projects, or want to read very good Swift code, head over to the [project's repo](https://github.com/Alamofire/Alamofire) on GitHub. You might also want to have a look at [AlamofireImage](https://github.com/Alamofire/AlamofireImage) an image component library for Alamofire.

---

That's it for today. See you tomorrow with a colorful library. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅
