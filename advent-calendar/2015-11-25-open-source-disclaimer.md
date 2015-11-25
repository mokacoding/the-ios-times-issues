---
layout: post
category: advent-calendar
---

<p>
Open source libraries and frameworks are great. Code that other people have written to solve your exact problem that you can drop in your project for free.
</p>

<p>
It almost <i>seems</i> possible to build an app entirely out of open source libraries, the dream of the <a href="https://en.wikipedia.org/wiki/Component-based_software_engineering">component-based software</a> movement.
</p>

<p>
You should be very cautious when pulling in a third party library in your code, because even if it is open source it is not <i>really for free</i>.
</p>

<p>
Every library and framework add complexity to your project, more APIs for you to know and understand, and in some cases a mainainance hell when it comes to updating it.

<a href="https://twitter.com/sandofsky">Benjamin Sandfosky</a> wrote <a href="https://sandofsky.com/blog/popular/third-party-libraries.html">a great post on the topic</a>, which I highly recommend you to read.
</p>

<p>
This is my suggestion, when prototyping and bootstrapping an application feel free to use third party libraries. This way you'll be able to deliver more quickly, and receive the real world validation for your product. Once your idea will be validate go back on your steps and start removing libraries, replacing them with code tailored for your domain and that you can control better.
</p>
