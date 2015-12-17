---
layout: post
title: OHHTTPStubs
date: 2015-12-18
category: advent-calendar
---

If you ever tried to write unit tests for a _real world_ application you will have noticed that testing asynchronous and network related code, can be tricky.

There are two factors that make code hitting the network harder to test, it is asynchronous, and relies on a server which you don't control to get results.

XCTest provides the [`XCTestExpectation`](http://nshipster.com/xctestcase/) class, and [Nimble](https://github.com/QuickNimble), which we saw on [day 7](http://theiostimes.com/advent-calendar/quick-nimble.html), offers `waitUntil` to deal with asynchronous code. But what about the network responses? How can we remove that dependency, and run our tests in isolation and in a deterministic way?

[OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs) by [Olivier Halligon](https://twitter.com/aligatr) is a neat library that allows us to stub any network request and provide a response ourselves, rather than hitting the real network.

This approach is very useful when unit testing, as it removes any kind of dependency from the server being up, or having a slow connection, and it allows us to control the value returned, which makes for deterministic tests.

Let's see how we could stub the network requests made by the example app of day 1, using [Quick and Nimble](http://theiostimes.com/advent-calendar/quick-nimble.html).

### How to stub network requests using OHHTTPStubs

OHHTTPStubs works in a very simple way. Use `stub` to define a request pattern to match, and provide the stubbed response to return.

```switf
stub(isHost("theiostimes.com") && isPath("/advent)) {
    return OHHTTPStubsResponse(
        JSONObject: ["key": "value"],
        statusCode: 200,
        headers: [ "ContentType": "application/json" ]
    )
}
```

The code is very readable, any request with host "theiostimes.com" and path "/advent" will be stubbed returning a 200 response with body `{ "key": "value" }`.

Constructing the JSON manually can be quite a task when the stubbed response are rich of data. Luckily OHHTTPStubs offers a neat way to load JSON data directly from a file.

### How to load the response data from a file

Given that we had a `success_response.json` file in the test target bundle, we could rewrite the code above like this:

```swift
guard let path = OHPathForFile("success_response.json", self.dynamicType) else {
    preconditionFailure("Could not find expected file in test bundle")
}

return OHHTTPStubsResponse(
    fileAtPath: path,
    statusCode: 200,
    headers: [ "ContentType": "application/json" ]
)
```

### Setup and tear down

If you care about doing testing right then you won't just write the test for the _happy path_, but also for every possible error outcome you can imagine.

This raises a question, how to you update the stub for a request so that for a test it returns a success, while returning a failure for another?

We can do it by hooking up into the setup and tear down logic of our test class, or in Quick terminology the `beforeEach` and `afterEach` of our spec's examples:

```swift
describe("APIClient get posts") {
    let client = APIClient()

    context("when the network returns an error") {
        beforeEach {
            stub(isHost("jsonplaceholder.typecode.com")) { _ in
                return OHHTTPStubsResponse(error: NSError(domain: "test", code: 42, userInfo: [:]))
            }
        }

        it("executes the completion closure passing the error recieved from the server") {
            // Write your async test and expectation here
        }

        afterEach {
             OHHTTPStubs.removeAllStubs()
        }
    }

    context("when the network returns a successful response") {
        beforeEach {
            // Stub successful request
        }

        it("executes the completion closure passing domain models build from the response") {
            // Write your async test and expectation here
        }

        afterEach {
             OHHTTPStubs.removeAllStubs()
        }
    }
}
```

### Next Steps

I think OHHTTPStubs is a very useful and elegant library, and there's much more it can do for you that this post hasn't shown.

Visit the [project's repo on GitHub](https://github.com/AliSoftware/OHHTTPStubs) to learn more about this library, or checkout [the example repo for this tutorial](https://github.com/mokacoding/AdventCalendar2015) to see it in action.

---

That's it for today. See you tomorrow with a library for fetching images in an efficient way. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

