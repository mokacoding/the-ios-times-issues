---
layout: post
title: AMScrollingNavbar
date: 2015-12-06
category: advent-calendar
---

Today we are going to have a look at a _"do only one thing but do it well"_ UI library. And as with most UI libraries a picture is better than a 1000 description words:

![Demo](https://raw.githubusercontent.com/andreamazz/AMScrollingNavbar/master/assets/screenshot.gif)

[AMScrollingNavBar](https://github.com/andreamazz/AMScrollingNavbar) is made by [Andrea Mazzini](https://twitter.com/theandreamazz), one of the [most prolific](https://github.com/andreamazz?tab=repositories) open source authors in our community.

This UI effect is most useful in applications presenting long scrolling content that is more important than the navigation bar buttons.

### How to integrate AMScrollingNavbar

Adopting this fancy navigation bar behaviour in your app is very simple, there are only two steps required.

Firstly you need to make the navigation controller a subclass of `ScrollingNavigationController`, here's how to do it in a Storyboard:

![How to set the proper navigation controller subclass from Storyboard](https://s3.amazonaws.com/theiostimes/advent-calendar-2015-scrollingnavicationcontroller-storyboard.png)

The second step consist in configuring the scrolling behaviour at the view controller level:

```swift
override func viewDidLoad() {
	super.viewDidLoad()

	if let navigationController = self.navigationController as? ScrollingNavigationController {
		navigationController.followScrollView(tableView, delay: 50.0)
	}
}
```

As easy as that 😎.

### How to react to the navigation bar changes

If you need to perform some custom logic when the navigation bar changes state you can conform to `ScrollingNavigationControllerDelegate`:

```swift
extension ViewController: ScrollingNavigationControllerDelegate {
  print("Merry Christmas")
}
```

### Next Steps

AMScrollingNavbar is another great example of small and sharp focused library, a trend that is becoming more and more common in Swift and that will definitely result in code that is easier to compose and maintain.

This library is also a great example of how documentation should be written, and how to use Swift's access level to your advantage.

Head over to [the project repository on GitHub](https://github.com/andreamazz/AMScrollingNavbar) to read the code and find out more ways to configure AMScrollingNavbar, or jump to [this tutorial's companion project](https://github.com/mokacoding/AdventCalendar2015) to get your hands dirty in the code.

---

That's it for today. See you tomorrow with a library that will help you when writing unit tests. [Subscribe](http://theiostimes.com/advent-calendar-subscribe) to the email list to avoid missing out.

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network using the buttons below. Thanks 🎅
