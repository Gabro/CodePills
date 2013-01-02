---
layout: post
title: "Filter a NSArray using a block"
date: 2013-01-02 17:41
comments: true
categories: objective-c
---
A quick post just to point out a minor thing.

If you have ever had the need of filtering an array in Objective-C you know that Apple provides a method called `filteredArrayUsingPredicate:`, that takes a `NSPredicate` and returns a NSArray containing only the objects from the original array that satisfy that predicate.

I personally find the `NSPredicate` format really annoying and error prone.

<!-- more -->

I'd really like to have a way to filter a NSArray in a functional fashion, passing a block (the Objective-C naming for lambdas) as a predicate and receiving my filtered array back.

Surprisingly there isn't a straightforward method to achieve such a result. What you can do is to filter the indexes - using `indexesOfObjectsPassingTest:`, which takes a predicate as a block and returns a `NSIndexSet` - and than you retrieve the objects at those indexes using the `objectsAtIndexes:` method.

I hate doing that all the time so I wrote a simple category over NSArray to condense this operations in a single one, which defines a new method `filteredArrayUsingBlock:`. You can find the (very simple) code on [GitHub](https://github.com/Gabro/NSArray-filter-using-block/).

That's all for today. Stay tuned!