---
layout: post
title: ios-charts
date: 2015-12-17
category: advent-calendar
---

As software developers we can build programs that consume and understand huge amount of data, values, and numbers. As humans on the other hand, we can hardly hold 7 information in our memory at the same time, and making sense of large data sets in the form of tables or lists is almost impossible.

This is why we have charts. Charts help us _visualise data_ and make sense out of it.

Drawing charts in mobile and desktop applications is not an easy task, but lucky for us there are a number of libraries that can help a lot. Today's library is one of those.

[ios-charts](https://github.com/danielgindi/ios-charts) by [Daniel Cohen Gindi](https://twitter.com/dcgindi) offers a wide range of charts, and a structured way to build them.

ios-charts is actually inspired by [MPAndroidChart](https://github.com/PhilJay/MPAndroidChart), a chart library for Android, and it is a great example of how we can learn from the experience of other communities, and share innovation for the greater good.

Let's see how to draw useful and beautiful graphs using ios-charts.

### Bar charts

Looking through the code sample and the documentation trying to understand how ios-charts can be a bit daunting, there are a lot of options, and describing graphs is not an easy task per se.

The very first thing you need when building a chart is a data set.

For example let's imagine we are building and app that shows the orders of different types of pizza's for a pizzeria, and we want to use a bar chart, or [histogram](https://en.wikipedia.org/wiki/Histogram).

On the x axis we'll have the type of pizzas, and on the y axis the sold quantities. This way each bar will represent a pizza, and we will have a visual representation of the best and worst selling ones.

This is how our data set might look like:

```swift
let pizzaSales = [
    PizzaSale(pizzaName: "Quattro Stagioni", soldQuantity: 60),
    PizzaSale(pizzaName: "Capricciosa", soldQuantity: 50),
    PizzaSale(pizzaName: "Margherita", soldQuantity: 80),
    PizzaSale(pizzaName: "Pepperoni", soldQuantity: 60),
    PizzaSale(pizzaName: "Marinara", soldQuantity: 30),
]
```

The data for the x and y axes can be generated like this

```swift
let xValues = pizzaSales.enumerate().map { index, element in
    return element.pizzaName
}

let yValues = pizzaSales.enumerate().map { index, element in
    return BarChartDataEntry(value: Double(element.soldQuantity), xIndex: index)
}
```

The next thing you need to do is create a `BarChartData` object combining the values for x and y:

```swift
let dataSet = BarChartDataSet(yVals: yValues, label: "Pizzas Sales")
let data = BarChartData(xVals: xValues, dataSet: dataSet)
```

This is all you need to configure a basic bar chart.

The final step is to pass the `BarChartData` object to a `BarChartView` to be rendered.

```swift
let chartView = BarChartView(frame: view.frame)
chartView.data = data
```

And here's our chart 😊🍕:

![ios-charts bar chart demo](https://s3.amazonaws.com/theiostimes/advent-calendar-2015-ioscharts-bars.jpg)

Actually, to have one _as nice as mine_, you will have to configure some of the objects a bit more. You can have a look at the [full sample for this tutorial](https://github.com/mokacoding/AdventCalendar2015) to find out how.

### Pie charts

When Antonio, the pizzeria owner and head pizzaiolo saw the histogram he told you "Mamma mia! I want a pizza graph not a spaghetti one".

Let's change it into a pie chart.

```swift
chartView = PieChartView(frame: view.frame)
view.addSubview(chartView)

let xValues = pizzaSales.enumerate().map { index, element in
    return element.pizzaName
}

let yValues = pizzaSales.enumerate().map { index, element in
    return ChartDataEntry(value: Double(element.soldQuantity), xIndex: index)
}

let dataSet = PieChartDataSet(yVals: yValues, label: "Pizza Sales")

let data = PieChartData(xVals: xValues, dataSet: dataSet)

chartView.data = data
```

As you can see the difference is minimal. You need to use a `PieChartView`, which expects a `PieChartData` configuration, which can be bulit using a `PieChartDataSet`, made up of `ChartDataEntry` objects.

![ios-charts pie chart demo](https://s3.amazonaws.com/theiostimes/advent-calendar-2015-ioscharts-pie.jpg)

### Next Steps

ios-charts is probably one of the most dense libraries that we have seen so far. There are many configurations and chart types you can use, but the basic principles are the same as what we just saw.

Head over to the [project's repo on GitHub](https://github.com/danielgindi/ios-charts) to see a list of all possible charts. You can also take a look at [APIs documentation](http://cocoadocs.org/docsets/Charts/2.1.6/), although it is still under development.

If you want some simple working code to experiment with feel free to clone the [example project for this tutorial](https://github.com/mokacoding/AdventCalendar2015).

---

That's it for today. See you tomorrow with a library that will help you test networking asynchronous code. [Subscribe to the email list to avoid missing out](http://theiostimes.com/advent-calendar-subscribe).

If you found this post useful and want to support the Advent Calendar please consider sharing it on your favourite social network. Thanks 🎅



