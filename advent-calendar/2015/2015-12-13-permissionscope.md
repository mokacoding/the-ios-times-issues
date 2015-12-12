---
layout: post
title: PermissionScope
date: 2015-12-13
category: advent-calendar
---

Being respectful of the user's privacy and building apps that cannot be exploited to access its data is very important. For this reason iOS requires apps to request user's authorization for a number of different things.

While somehow similiar in the way they behave each authorization type have they're own set of APIs that we need to familiarise with.

One other important thing to consider when building apps that access user's data like location, photos, HealthKit, etc. is to be clear on why we need them, so that our users will be likely to gran the permission.

[PermissionScope](https://github.com/nickoneill/PermissionScope), by [Nick O'Neill](https://twitter.com/nickoneill) helps prompting the user for permissions in a nice and structured way, and also provides a common interface for all the permissions type, making it quicker to write code to ask for different types of permissions.

![PermissionScope Demo](http://raquo.net/images/permissionscope.gif)

Let's see how to use this library.

### How to prompt for a simple permission

```swift
func presentSimplePermissionsAlert() {
	let permissionScope = PermissionScope()

	// Set up permissions
	permissionScope.addPermission(ContactsPermission(),
		message: "This way it is going to be easier to find your friends"
	)
	permissionScope.addPermission(NotificationsPermission(notificationCategories: nil),
		message: "So that we can send you the latests updates straightaway"
	)

	// Show permission dialog with callbacks
	permissionScope.show(
		{ finished, results in
			print("Permissions state changed, results: \(results)")
		},
		cancelled: { results in
			print("User closed the permission alert, results: \(results)")
		}
	)
}
```

As you can see it's pretty simple.

1. Instantiate a `PermissionScope` object.
1. Add a `Permission` object, which you can configure with an explanatory message for the user
1. Prompt the user for permissions.

Showing an alert is not the only way you have to request the user for permissions, for example you could use a switch with an explanatory label.

### How to toggle permissions using a switch

```swift
func togglePhotosPermissions(basedOnSwitch switch: UISwitch) {
	if switch.on {
		// turn on photos access
		if PermissionScope().statusPhotos() == .Authorized {
			print("Photos have been already authorized")
		} else {
			print("Photos need to be reauthorized")
			PermissionScope().requestPhotos()
		}
	} else {
		// turn off notifications
		print("Photos need to be de-authorized")
	}
}
```

### Next Steps

As with all the other iOS Advent Calendar posts I don't presume to teach you everything about a library, but rather show you enough to make you curious about it, or decide its not for you.

If you think PermissionScope could help you in your projects head over to [the library's repository on GitHub](https://github.com/nickoneill/PermissionScope), where you'll find information on all the supported permissions types, other ways to request for permissoins, and how to configure the permission's alert.

If you are looking for some ready to use code to play with the check out the [example project for this post](https://github.com/AdventCalendar2015).

---

That's it for today. See you tomorrow with a Swift friendly mobile database. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

