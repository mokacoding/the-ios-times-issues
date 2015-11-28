---
layout: post
title: Chameleon
date: 2015-12-02
category: advent-calendar
---

[Chameleon](https://github.com/ViccAlexander/Chameleon) is a Swift library to work with colors. It comes with a collection of _hand-picked_ colors for you to use, allows the generation of color schemes from both colors and images, provides handy color manipulation methods, and much more.

Let's see have some fun with colors shall we? All the code in the examples below is available in the [Advent Calendar repo on GitHub](https://github.com/mokacoding/AdventCalendar2015/02-Chameleon)

### How to pick a color with Chameleon

Chameleon's colors are all prefixed with `flat`, so you can type `UIColor.flat` and rely on Xcode's autocompletion to show you all the available colors, or have a look in the `UIColor+Chameleon` category for a full list.

```swift
let rainbowColors = [
	UIColor.flatRedColor(),
	UIColor.flatOrangeColor(),
	UIColor.flatYellowColor(),
	UIColor.flatGreenColor(),
	UIColor.flatSkyBlueColor(),
	UIColor.flatPurpleColor(),
	UIColor.flatBlueColor(),
]
```

Yes, I know those are not the _names_ of the rainbow colors, but take a look at the result:

![Rainbow colors with Chameleon](https://s3.amazonaws.com/theiostimes/advent-calendar-2015-chameleon-colors.png)

Chameleons offers more than just nice colors, for example:

### How to manipulate colors with Chameleon

```swift
UIColor.flatOrangeColor().lightenByPercentage(0.1)

UIColor.flatOrangeColor().darkenByPercentage(0.1)

UIColor.orangeColor().flatten()
```

### How to create gradients with Chameleon

```swift
aView.backgroundColor = UIColor.init(
	gradientStyle: UIGradientStyle.LeftToRight,
	withFrame: aView.frame,
	andColors: [ UIColor.flatOrangeColor(), UIColor.flatGreenColor() ]
)
```

The possible style options are `.LeftToRight`, `.TopToBottom`, and `.Radial`. Note that a `CGRect` on which to _spread_ the color needs to be provided.

![Examples of color manipulation with Chameleon](https://s3.amazonaws.com/theiostimes/advent-calendar-2015-chameleon-color-manipulation.png)

Finally here's a pretty handy functionality if you want to adapt your UI colors to an image provided by the user:

### How to create a color using an image with Chameleon

```swift
let image: UIImage = // ...
UIColor.init(averageColorFromImage: image, withAlpha: 1)
```

![Example of colors generated from images with Chameleon](https://s3.amazonaws.com/theiostimes/advent-calendar-2015-chameleon-color-from-images.png)

### Where to go from here

The iOS Times Advent Calendar's posts certainly don't aim to be comprehensive tutorials on how to use a library, but rather provide a little taste of it, to ignite your curiosity.

If you find Chameleon interesting and want to leverage its handy color utilities head over to the [project's repo on GitHub](https://github.com/ViccAlexander/Chameleon), or checkout the [full source of this tutorial](https://github.com/mokacoding/AdventCalendar2015).

---

That's it for today. See you tomorrow with a tool that will help you write consistent Swift code. [Subscribe](http://theiostimes.com/advent-calendar-subscribe) to the email list to avoid missing out.

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network using the buttons below. Thanks 🎅
