---
layout: post
title: SDWebImage
date: 2015-12-19
category: advent-calendar
---

Like the most successful apps, the most successful libraries tackle one task that is both common and _painful_, and make it simple and frictionless.

One task that I am sure all of you have done at least once is downloading an image from a server and display it using an `UIImageView`.

Despite sounding like a simple thing, there are a lot of moving parts involved, and it is not a simple to implement.

Today's library is [SDWebImage](https://github.com/rs/SDWebImage) by [Olivier Poitrey](https://twitter.com/olivier_poitrey), and makes downloading images and setting the in `UIImageView`s a oneliner.

### UIImageView and SDWebImage

```swift
cell.imageView.sd_setImageWithURL(url)
```

That's really all you need 😎

If you want more control, say a placeholder and a completion closure for example, SDWebImage has got you covered.

```swift
let placeholder = ...
cell.imageView.sd_setImageWithURL(url, placeholderImage: placeholder) { image, error, cacheType, url in
    // do stuff
}
```

As you can imagine the this functionality is implemented via a category on `UIImageView`. _Category_?! Yes, SDWebImage is written in Objective-C, but is so cool that I decided to include it anyway.

What `sd_setImageWithURL` is doing under the hood is calling `SDWebImageManager.sharedManager().downloadImageWithURL` for you.

If you have some particular needs you can do that yourself, for example if you wanted to reveal the image using a fade in effect.

### Setting images using a fade in effect with SDWebImage

```swift
imageView.alpha = 0
SDWebImageManager.sharedManager().downloadImageWithURL(url, options: [], progress: nil) { downloadedImage, error, cacheType, isDownloaded, url in
        imageView.image = downloadedImage

        UIView.animateWithDuration(1) {
            cell.imageView.alpha = 1
        }
}
```

Very straightforward.

### Cancelling downloads

Whenever dealing network related operation it is important to be a good iOS citizen, and respect our users trying to consume as little of their data as possible.

One important thing you will want to take care of is cancelling image downloads that are not needed anymore. Don't worry, it won't take you long:

```swift
imageView.sd_cancelCurrentImageLoad()

SDWebImageManager.sharedManager().cancelAll()
```

### Next Steps

As you know the tutorials in The iOS Advent Calendar don't aim to me comprehensive, but rather show you just enough code to give you a feel for the library.

If you think SDWebImage could help you in your projects, checkout [the project repository on GitHub](https://github.com/rs/SDWebImage). This library is widely famous and used, and as such has a big community around it. There is even a [Stack Overflow tag](http://stackoverflow.com/questions/tagged/sdwebimage).

Finally, if you'd like to have some code ready to play with, have a look at the [full code from this tutorial](https://github.com/mokacoding/AdventCalendar2015).

---

That's it for today. See you tomorrow with a library that will sweeten your Core Data experience. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅
