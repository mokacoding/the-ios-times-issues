---
layout: post
title: SwiftLint
date: 2015-12-03
category: advent-calendar
---

[SwiftLint](https://github.com/realm/SwiftLint) is a tool supported by the team of [Realm.io](https://realm.io/) to _lint_ your Swift code, verifying that it conforms to a set of rules syntactic rules defined by you and your teammates.

### How to install and integrate SwiftLint in Xcode

The very first thing you have to do to start using SwiftLint is installing it via [Homebrew](http://brew.sh/).

```
brew update && brew install swiftlint
```

Then you can have Xcode running SwiftLint on every build by adding a "Run Script" build phase.

Use this as the Run Script body:

```bash
if which swiftlint >/dev/null; then
	swiftlint
else
	echo "warning: SwiftLint does not exist, download it from https://github.com/realm/SwiftLint"
fi
```

The code above will also throw a warning if SwiftLint is not installed. If you want to be more aggressive and make sure that all your team will install it you can make fail the build instead:

```bash
if which swiftlint >/dev/null; then
	swiftlint
else
	echo "error: SwiftLint does not exist, download it from https://github.com/realm/SwiftLint"
	exit 1
fi
```

Once this is done SwiftLint will run every time you build the your project, and will show warnings or errors if the Swift code does not comply to the default style rules.

As a matter of fact, Xcode's "Single View" template project **doesn't** pass SwiftLint's validation 😳.

### How to configure SwiftLint's behaviour

To configure the way SwiftLint's behaves you will have to create a `.swiftlint.yml` file in the root of your project.

To **disable a rule** add it to the array of `disabled_rules` keys

```yml
disabled_rules:
  - todo
  - force_cast
```

To **see all the available rules** that SwiftLint exposes run `swiftlint rules` in the terminal. You will see an output like this:

```
Colon (colon): Colons should be next to the identifier when specifying a type.
Comma Spacing (comma): One space before and no after must be present next to any comma.
Control Statement (control_statement): if,for,while,do statements shouldn't wrap their conditionals in parentheses.
Force Cast (force_cast): Force casts should be avoided.
...
```

The value between the parenthesis is the name of the rule that you should use in the `.switlint.yml` file.

Some rules have parameters that you can configure, for example the Type Body Length, how many lines of code the body of a type is allowed to be, can be configured like this:

```yml
type_body_length:
  - 100
  - 200
```

This instructs SwiftLint to throw a warning if the body of a type is longer than 100 lines of code, and an error if it is longer that 200 lines.

You can read more about all the possible configurations for SwiftLint [here](https://github.com/realm/SwiftLint#configuration). At the moment the tool doesn't offer an easy way to discover which rules are parametric and which aren't. The only way to find out is to read the [source of the rule](https://github.com/realm/SwiftLint/tree/master/Source/SwiftLintFramework/Rules).

## Next Steps

Using a linter such as SwiftLint is a great way to remove the cognitive load of having to think about the details of the style, so you can focus on the actual meaning of the code you are writing.

The iOS Times Advent Calendar's post certainly don't aim to be comprehensive tutorials on how to use a library, but rather provide a little taste of it, to ignite your curiosity.

I would recommend you take some times to explore the rules SwiftLint offer, and take a decision with your teammates on which to apply to your current projects. You can use the code [in the example project for this tutorial](https://github.com/mokacoding/AdventCalendar2015) to get started.

---

That's it for today. See you tomorrow with little library that will simplify your asynchronous operations. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe.html).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

