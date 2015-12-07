---
layout: post
title: Cartography
date: 2015-12-10
category: advent-calendar
---

Oh AutoLayout... We all love it how powerful the engine is, and we've all been close to a table flip when the constarints don't seem to work. (Hint: it's almost certainly your falut).

One thing is for sure, constraints can easily get out of control in Interface Builder, specially we you are describing complex layout.

Describing your layout in code could help managing the constraints, but the APIs are so verbose!

[Cartography](https://github.com/robb/Cartography), by [Robert Böhnke](https://twitter.com/dlx), brings an end to this, providing a concise Swift DSL for coding AutoLayout constraints. This library is also an example of custom operators done right.

Using Cartography you can set the constraint for your view in and handful of lines of code.

Here's how to make a view squared, and position it in the bottom right corner, with a certain margin.

```swift
let size = 48
let margin = 8

constrain(aView) { view in
  view.height  == size
  view.width   == view.height
  view.right   == view.superview!.right - margin
  view.bottom  == view.superview!.bottom - margin
}
```

Note that we are implicitly unwrapping the `superview` property. Did you know that every time you use a `!` in Swift a kitten dies? A better but longer implementation would be:

```swift
let size = 48
let margin = 8

constrain(aView) { view in
  guard let superview = view.superview else {
    return
  }

  view.height  == size
  view.width   == view.height
  view.right   == superview.right - margin
  view.bottom  == superview.bottom - margin
}
```

Most of the times you'll want to define constraints for views in relation to each others, for example:

```swift
let margin = 8

constrain(descriptionLabel, titleLabel, imageView) { label, titleLabel, image in
  guard let superview = label.superview else {
    return
  }

  label.leading == image.trailing + margin
  label.top == titleLabel.bottom + margin
  label.trailing == superview.trailing - margin
  label.bottom == superview.bottom - margin
}
```

Notice how you can use `leading` and `trailing` as well as `left` and `rigth`.

This is one of my favourite Cartography features:

```swift
constrain(someView) { view in
  guard let superview = view.superview else {
    return
  }

  view.edges == inset(superview.edges, 10)
}
```

With one line of code you can pin a view to the edges of its superview, with or without padding.

## Next Steps

There is really not much left to say... Cartography is awesome and it can definitely help you control your constraints.

On top of that there is a case to be made to not use Interface Builder and only draw views in code, but that's the topic for another post. [Ping me if you are interested](http://twitter.com/home?status=Hey @mokagio on @_theiostimes you say you don't like IB, wanna talk about it?).

If you want to see more coding example of have a look at [the project repo on GitHub](https://github.com/robb/Cartography) and at the [example project for this tutorial](https://github.com/mokacoding/AdventCalendar2015).

---

That's it for today. See you tomorrow with a tool that will help you manage resources identifiers in your project in a Swifty way. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅
