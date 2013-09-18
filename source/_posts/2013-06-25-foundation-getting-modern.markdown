---
layout: post
title: "Type inference for everyone in iOS 7! (yes, I'm breaking Apple's NDA)"
date: 2013-06-25 15:23
comments: true
categories: objective-c ios type-inference
---
##DISCLAIMER
Ok, I'm about to do a crazy thing nobody has ever done before: I'll break Apple's NDA! If you're about to yell me, whining "You cannot talk about this, iOS 7 is not out yet and it's under NDA", well, here's my super politically correct answer for you: "who cares". NDAs are made to be broken, and in Apple's specific case it can also be purchased for 99$/year. On the other hand, if you work as a lawyer at Apple Inc, forget what I just said. Have I ever told you that I LOVE the new icons in iOS 7? We can be best friends!

#`instancetype` for everyone!
I already talked about the `instancetype` keyword in my [previous article](/blog/2013/01/01/obj-c-infer-types-like-a-man-man/).

I'm proud to "announce" that, as per iOS 7.0 API diffs, finally Apple is making use of `instancetype` across the Foundation framework (and not only that). What does it mean?

<!-- more -->

Simple: consider the following code

{% codeblock lang:objc %}
[[NSArray array] viewDidLoad];
{% endcodeblock %}

Despite you would expect the compiler to raise at least a warning, it compiles flawlessly on iOS 6 and below. And that is because the signature of `+array` used to look like

{% codeblock lang:objc %}
+ (id)array
{% endcodeblock %}

Being the return type `id`, the compiler can just check for the existence of a method named `viewDidLoad`, and, since it does, it will consider the above invocation as legitimate.
However, starting from iOS 7, the signature has changed into

{% codeblock lang:objc %}
+ (instancetype)array
{% endcodeblock %}

therefore the compiler will now know that `[NSArray array]` is returning a `NSArray` instance, which of course doesn't feature any `viewDidLoad` method in its interface. This will result in a warning:

{% blockquote %}
NSArray may not respond to viewDidLoad
{% endblockquote %}

To wrap it up, all of this means a simple thing: **the compiler can now be smarter and provide even more helpful typechecking**

You can already see this in action in the preview of Xcode 5, when compiling against iOS SDK 7.0.

# The `instancetype` catalog
As a reference, I'll list here all the classes that are now making use of the `instancetype` return type.

###Foundation framework
- `NSString`
- `NSArray` (and `NSMutableArray`)
- `NSDictionary` (and `NSMutableDictionary`)
- `NSSet` (and `NSMutableSet`)
- `NSDate`
- `NSIndexPath`
- `NSIndexSet`
- `NSNotification`
- `NSOrderedSet` (and `NSMutableOrderedSet`)

###Other frameworks
- `MKPolyline`
- `MPMediaPickerController`
- `UIAppearance`