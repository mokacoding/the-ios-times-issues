---
layout: post
title: AnimatedGIFImageSerialization
date: 2015-12-21
category: advent-calendar
---

The Graphics Interchange Format is a bitmap image format that was introduced by CompuServe in 1987 and has since come into widespread usage on the World Wide Web due to its wide support and portability.

![what](https://media.giphy.com/media/sRb7yNtTJAtZS/giphy.gif)

Today we are talking about rendering GIFs 😁

![oh right](https://media.giphy.com/media/U3I5ZJPFJpXRm/giphy.gif)

[AnimatedGIFImageSerialization](https://github.com/mattt/AnimatedGIFImageSerialization) by [Mattt Tomphson](https://twitter.com/mattt), is probably the tinies library we've seen so far, but boy what awesome results it brings.

### How to display GIFs in iOS apps

A simple `import AnimatedGIFImageSerialization` will allow you to display GIF in your `UIImageView`s.

```swift
imageView.image = UIImage(named: "awesome.gif")
```

That easy. Classic Mattt.

If you want to find out more about this little great library, and look into how to gcreate an animated GIF and encode it into `NSData`, head over to [the project's repo on GitHub](https://github.com/mattt/AnimatedGIFImageSerialization).

![cool story](https://media.giphy.com/media/3o85xHktyE0coUN1x6/giphy.gif)

---

That's it for today. See you tomorrow with a forms building library. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅
