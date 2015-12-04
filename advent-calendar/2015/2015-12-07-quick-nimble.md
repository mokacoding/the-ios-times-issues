---
layout: post
title: Quick and Nimble
date: 2015-12-07
category: advent-calendar
---

Writing unit tests is very important. Not only it makes your application code more reliable, it also helps you discover weak spots of your architecture. You probably herd something along the line of "_if something is hard to test, then is going to be hard to work with_".

Today's post will not go into the details of how to write Swift unit tests[*](#testing-blog), but rather showcase [Quick](https://github.com/Quick/Quick) and [Nimble](https://github.com/Quick/Nimble), a pair of frameworks developed by [Brian Gesiak](https://twitter.com/modocache) and [Jeff Hui](https://twitter.com/jeffhui), that will make your experience writing and reading tests way better.

Quick is a framework built on top of Apple's XCTest that provides a [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) to write test in a style similar to Ruby's [RSpec](http://rspec.info/).

Nimble is a libraries of composable matchers wrapping the various `XCTAssert`s.

Together Quick and Nimble allow you to write more expressive tests, with a syntaxt that makes it easier to focus on the behaviour that is tested, and easy to write complex expectations.

### A simple Quick test

Here's how a simple Quick test looks like (_example form [the documentation](https://github.com/Quick/Quick/blob/master/Documentation/QuickExamplesAndGroups.md)_):

```swift
import Quick
import Nimble
import Sea

class DolphinSpec: QuickSpec {
  override func spec() {
    describe("a dolphin") {
      describe("its click") {
        it("is loud") {
          let click = Dolphin().click()
          expect(click.isLoud).to(beTruthy())
        }

        it("has a high frequency") {
          let click = Dolphin().click()
          expect(click.hasHighFrequency).to(beTruthy())
        }
      }
    }
  }
}
```

This test code its easy to read and follow, and describes the behaviour of the `Dolphin` class.

Another great feature that Quick bringt to the table is the `beforeEach` and `afterEach` pair.

### How to use `beforeEach` and `afterEach`

These two methods are great to separate the test setup code form the code that actually tests the behaviour.

```swift
class HealthViewConfigurationSpec: QuickSpec {

    override func spec() {
        describe("HealthViewConfigurationSpec") {
            let configurator = HealthViewConfigurator()

            context("when configuring the HealthView with a Player") {
                context("when the player has full health") {
                    var player:  Player!
                    var view: HealthView!

                    beforeEach {
                        player = Player(health: 100)
                        view = HealthView()
                    }

                    it("sets the view background color to green") {
                        configurator.configureView(view, forPlayer: player)
                        expect(view.backgroundColor).to(equal(UIColor.greenColor()))
                    }
                }
            }
        }
    }
}
```

`beforeEach` and `afterEach` make it easier to [spin up and tear down tests databases](http://www.mokacoding.com/blog/testing-realm-apps/)

### Nimble assertions

Finally, an selection of Nimble of assertion to show you how nice your test code could look like

```swift
expect(anInt) == 42
expect(anInt) != 34

expect(aString) == "Merry Christmas"

expect(foo.barValue).toEventually(equal(42))

expect{ try foo.somethingThatThrows()  }.to(throwError())

waitUntil { done in
	API.getStuff { stuff, error in
		expect(error).to(beNil())
		expect(stuff.uuid) == "1234abcd"
		done()
	}
}
```

### Next Steps

This little tutorial is not in any way a comprehensive list of what you can achieve using Quick and Nimble. If you want to know more about a good place to start is the excellent [Quick's documentation index](https://github.com/Quick/Quick/tree/master/Documentation#documentation), while a great book on the art of testing in general is [Growing Object Oriented Sofware, Guide by Tests](http://geni.us/2Owb
).

If you want to play around with a project with Quick and Nimble already setup using Carthage, checkout [the companion code of this tutorial](https://github.com/mokacoding/AdventCalendar2015).

---

That's it for today. See you tomorrow with a library that will make you _pattern match like a boss_. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅

