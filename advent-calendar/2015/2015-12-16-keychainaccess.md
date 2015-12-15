---
layout: post
title: KeychainAccess
date: 2015-12-16
category: advent-calendar
---

Apple products not only distinguish themselves for great design and UX, but also security. The [Keychain](https://en.wikipedia.org/wiki/Keychain_(software)) and [Touch ID](https://en.wikipedia.org/wiki/Touch_ID) are an example of that.

While great from a security point of view, the Keychain is not one of the developer's best friends, with its C APIs and notoriously mysterious error messages.

[KeychainAccess](https://github.com/kishikawakatsumi/KeychainAccess) by [Kishikawa Katsumi](https://twitter.com/k_katsumi) provides a much nicer set of Swift APIs that will make working with the Keychain a pleasure. And as a bouns there is also Touch ID and iCloud support.

### How to read and write values to the Keychain

You can get an instance of the `Keychain` wrapper by using the Bundle Identifier of you application:

```swift
let keychain = Keychain(service: "com.theiostimes.keychainexample")
```

With this instance reading and writing data is very simple:

```swift
do {
    try keychain.set("1234-5678-90AB", key: "token")
} catch {
    // TODO: Handle write failure
}

do {
    let token = try keychain.get("token")
} catch {
    // TODO: Handle read failure
}
```

And there is support for subscript access as well, if you think that style makes your code more readable.

```swift
keychain["token"] = "1234-5678-90AB"
let _ = keychain["token"]
```

Note how with subscripting there is no need to `try`, in fact the values returned are optionals.

### How to delete data from the Keychain

Removing data is as straightforward as writing it:

```swift
do {
    try keychain.remove("token")
    try keychain.removeAll()
} catch {
    // TODO: Handle delete failure
}
```

### Configuring the Keychain

The `Keychain` interface can be configured in this neat chainable way:

```swift
Keychain(service: "com.theiostimes.keychainexample")
    .label("theiostimes.com (mokagio)")
    // Enable iCloud Sync
    .synchronizable(true)
    // Configure data availability
    .accessibility(.AfterFirstUnlock)
```

### Next Steps

There is much more that can be done with KeychainAccess, and with the Keychain itself.

The very first place I would recommend you to visit if you want to find out more is Apple's [Keychain Services Concepts](https://developer.apple.com/library/ios/documentation/Security/Conceptual/keychainServConcepts/02concepts/concepts.html#//apple_ref/doc/uid/TP30000897-CH204-TP9) documentation. Once you'll have a solid understanding of what can be done with the Keychain, head over to [KeychainAccess' project repo on GitHub](https://github.com/kishikawakatsumi/KeychainAccess) and go through the README to learn more about the advanced features this library offers.

As usual all the code used in this tutorial is [available on GitHub](https://github.com/mokacoding/AdventCalendar2015) as well.

---

That's it for today. See you tomorrow with a library to draw charts. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

