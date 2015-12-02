---
layout: post
title: JSONCodable
category: advent-calendar
date: 2015-12-09
---

Soon after Swift was released a war started, a war that will be remembered as the JSON parsing libraries war.

The war is not over yet, if you've been following The iOS Times you might have noticed that every couple pf weeks a new parsing library is published.

If you ask me though, the war is over. And there is a winner[*](#pagenote).

[JSONCodable](https://github.com/matthewcheok/JSONCodable), by [Matthew Cheok](https://twitter.com/matthewcheok), provides decode and encode of JSON dictionaries to Swift objects, using Swift 2's `throw` for proper error handling, and it uses of sharped focused protocols.

Let's bring up the example app we used in the [Alamofire tutorial](http://theiostimes.com/advent-calendar/2015/alamofire.html) and see how to manipulate JSON data using JSONCodable.

### How to decode a JSON to a Swift object

In the toy app made for the Alamofire example we connected to the [JSON placeholder APIs](http://jsonplaceholder.typicode.com/) and get a list of posts.

We could write a model object representing the post using a struct for example.

```swift
struct Post {
    let userId: Int
    let id: Int
    let title: String
    let body: String
}
```

To be able to parse a JSON dictionary into a `Post` all we have to do is conform to the `JSONDecodable` protocol, which defines the `init?(JSONDictionary: JSONObject)` method, and describe the keys mappings.

```swift
extension Post: JSONDecodable {
    init?(JSONDictionary: JSONObject) {
        let decoder = JSONDecoder(object: JSONDictionary)
        do {
            id = try decoder.decode("id")
            userId = try decoder.decode("userId")
            title = try decoder.decode("title")
            body = try decoder.decode("body")
        } catch {
            return nil
        }
    }
}
```

That's it. Now we can replace the silly manual parsing in the app with a clearer and safer:

```swift
Alamofire.request(.GET, "http://jsonplaceholder.typicode.com/posts")
		.responseJSON { response in

				if let data = response.result.value as? [[String: AnyObject]] {
						for postJSON in data {
								if let post = Post(JSONDictionary: postJSON) {
										self.posts.append(post)
								}
						}
				} else {
						// TODO: Handle errors
		}
```

### How to decode a JSON with nested objects

The library's parsing power doesn't end here. We can decode nested objects as well.

Consider this payload for a "user" resource:

```json
{
	"id": 1,
	"name": "Leanne Graham",
	"username": "Bret",
	"address": {
		"street": "Kulas Light",
		"suite": "Apt. 556",
		"city": "Gwenborough",
		"zipcode": "92998-3874"
	}
}
```

We can model the user resource like this:

```swift
struct User {
    let id: Int
    let name: String
    let username: String
    let address: Address
}

struct Address {
    let street: String
    let suite: String
    let city: String
    let zipCode: String
}
```

JSONCodable won't break a sweat when parsing a user JSON dictionary, as long as we defined the mappings for both `User` and `Address`:

```swift
extension Address: JSONDecodable {
    init?(JSONDictionary: JSONObject) {
        let decoder = JSONDecoder(object: JSONDictionary)
        do {
            street = try decoder.decode("street")
            suite = try decoder.decode("suite")
            city = try decoder.decode("city")
            zipCode = try decoder.decode("zipcode")
        }
        catch {
            return nil
        }
    }
}

extension User: JSONDecodable {
    init?(JSONDictionary: JSONObject) {
        let decoder = JSONDecoder(object: JSONDictionary)
        do {
            id = try decoder.decode("id")
            name = try decoder.decode("name")
            username = try decoder.decode("username")
            address = try decoder.decode("address")
        } catch {
            return nil
        }
    }
}
```

Ok you might say, but what about types that are not your usual `String` or `Int`, what if I had an `NSURL`?. Well, you can decode a string into an `NSURL` like this:

```swift
website = try decoder.decode("website", transformer: JSONTransformers.StringToNSURL)
```

`JSONTransformer<EncodedType, DecodedType>` is an object you can initialize passing two closures, one to go from `EncodedType` to `DecodedType`, the other to do the opposite.

In the case of the `String` to `NSURL` the transformer is created this way:

```swift
public struct JSONTransformers {
    public static let StringToNSURL = JSONTransformer<String, NSURL>(
        decoding: {NSURL(string: $0)},
        encoding: {$0.absoluteString})
```

I'm sure you can already see how easy it is to create your custom transformers.

So far we've talked about how to decode a dictionary into an object, but how can we go the other way around?

### How to encode a model into a JSON dictionary

The same way you can conform to the `JSONDecodable` protocol to go from JSON dictionary to Swift object, by conforming to `JSONEncodable` you can do the opposite.

```swift
extension Post: JSONEncodable {
    func toJSON() throws -> AnyObject {
        return try JSONEncoder.create { encoder in
            try encoder.encode(id, key: "id")
            try encoder.encode(userId, key: "userId")
            try encoder.encode(title, key: "title")
            try encoder.encode(body, key: "body")
        }
    }
}
```

#### Pro Tip

Writing strings values for keys multiple times is always dangerous as it is easy to make typos. We can have a stronger and easier to change system by leveraging Swift's enum's raw value feature:

```swift
extension Post {
    enum JSONKeys: String {
        case id = "id"
        case userId = "userId"
        case title = "title"
        case body = "body"
    }
}

```

Then get the key values like this:

```swift
extension Post: JSONEncodable {
    func toJSON() throws -> AnyObject {
        return try JSONEncoder.create { encoder in
            try encoder.encode(id, key: Post.JSONKeys.id.rawValue)
            try encoder.encode(userId, key: Post.JSONKeys.userId.rawValue)
            try encoder.encode(title, key: Post.JSONKeys.title.rawValue)
            try encoder.encode(body, key: Post.JSONKeys.body.rawValue)
        }
    }
}
```

There is a bit more typing involved, but it results in dryer code that is easier to maintain.

### Next Steps

The iOS Times Advent Calendar's post certainly don't aim to be comprehensive tutorials on how to use a library, but rather provide a little taste of it, to ignite your curiosity.

I suggest you keep JSONCodable in mind when looking for a JSON parsing solution for your next project. Head over to [the project's repo on GitHub](http://jsonplaceholder.typicode.com/), or feel free to play around with [the code from this tutorial](https://github.com/mokacoding/AdventCalendar2015).

---

That's it for today. See you tomorrow with a library to describe layouts in code the right way. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

<p id="pagenote">
  <i>
    To be honest with you my very favourite JSON parsing library is <a href="https://github.com/thoughtbot/Argo">Argo</a>, but that's because I like to feel smart and be using monadic operators.
  </i>
</p>
