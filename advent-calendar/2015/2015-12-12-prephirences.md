---
layout: post
title: Prephirences
date: 2015-12-12
category: advent-calendar
---

Today's library is [Prephirences](https://github.com/phimage/Prephirences), and I will start by saying that not everyone should use it.

What Prephirences does is putting an abstraction layer on top of common iOS data storage systems like [NSUserDefaults](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSUserDefaults_Class/), [NSBundle](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/), [iCloud](https://developer.apple.com/library/ios/documentation/Foundation/Reference/NSUbiquitousKeyValueStore_class/index.html), and even [Core Data](https://developer.apple.com/library/tvos/documentation/Cocoa/Conceptual/CoreData/index.html), with a clever usage of protocols and extensions.

What this abstraction layer allows you to do is read and write data from any of these stores without worrying about its specific APIs, and with using subscript. Even more importantly, you can now exchange stores without having to update the code that consumes them.

### How to access the NSUserDefaults using Prephirences

```swift
let userDefaults = NSUserDefaults.standardUserDefaults()

value = userDefaults["key"]

userDefaults["key"] = newValue
```

Easier and less verbose 😊 

This is made possible by Prephirences defining the `PreferencesType` and `MutablePreferencesType` protocols, and by extending `NSUserDefaults` making it conform to them.

### How to access preferences regardless of their storage

The library extends other types like `NSUbiquitousKeyValueStore`, and defines wrappers for types like `Dictionary` or stores like the Keychain that conform to `PreferencesType`.

This means that you can write code like this:

```swift
func configure(withPreferences preferences: PreferencesType) {
  self.foo = preferences["foo"]
  self.bar = preferences["bar"]
}

let prefsFromUserDefaults = NSUserDefaults.standardUserDefaults()
let prefsFromDictionary: DictionaryPreferences = ...

configure(withPreferences: prefsFromUserDefaults)
configure(withPreferences: prefsFromDictionary)
```

This kind of abstraction can be very valuable when prototyping, or in projects where configurations are stored in different locations (_and you can't afford to refactor or rewrite the system to be centralised_).

### Next Steps

The iOS Times Advent Calendar's posts certainly don't aim to be comprehensive tutorials on how to use a library, but rather provide a little taste of it, to ignite your curiosity.

Prephirences has much more to offer, head over to [the project's page on GitHub](https://github.com/phimage/Prephirences) to learn about all the supported types and the project's roadmap, and feel free to git clone [this tutorial example code](https://github.com/mokacoding/AdventCalendar20154).

---

That's it for today. See you tomorrow with a tool to help you manage the permissions requests your apps need to make. Subscribe to the email list to avoid missing out.

If you found this post useful and want to support the project please consider sharing it with your colleagues, [click here to tweet about The iOS Advent Calendar](). Thanks, it means a lot for me, and Santa won't bring you coal if you do it 🎅
