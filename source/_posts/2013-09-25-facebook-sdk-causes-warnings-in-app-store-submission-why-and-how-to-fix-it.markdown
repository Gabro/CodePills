---
layout: post
title: "Facebook SDK may cause your app to be rejected from the App Store."
date: 2013-09-25 14:51
comments: true
categories: 
---
##Scenario
You just finished coding the coolest app ever. A well-deserved cold beer is awaiting for you. Archive... Validate....

Validation passed with warnings:

{% blockquote %}
The app references non-public selector in id
{% endblockquote %}

<!-- more -->

##Why?
Facebook SDK defines several protocols which define a property named `id`. Example

{% codeblock lang:objc FBGraphUser.h %}
@protocol FBGraphUser<FBGraphObject>
...
@property (retain, nonatomic) NSString *id;
... 
@end
{% endcodeblock %}

Needless to say, it's a *poor design choice*&trade;.

Any programmer with a minimal experience (and by minimal I mean any programmer who has ever made a fart app) easily recognizes this as a bad idea, since `id` is a keyword of the language (ok, I'm lying to keep things simple. `id` is just a predefined type, defined as a pointer to `struct objc_object`.)

It's no surprise then that Apple warns you whenever you do something like *user.id* and you submit it to the App Store.

##How to fix it
Easily enough, in any case I'm aware of, the objects conforming to the aforementioned protocol the Facebook SDK uses are all `NSDictionary` instances.
You can therefore access the `id` field using `objectForKey:` or the literal notation as follows.

{% codeblock lang:objc %}
NSString * userId = user[@"id"];
{% endcodeblock %}

This fixes the warnings and it's not too terrible syntactically-wise.

##Which Facebook SDKs?
According to [this bug report](https://developers.facebook.com/bugs/143882255765923/), all versions < 3.5.3 are affected.
It appears to have been fixed in version 3.5.3, and then it reappeared from some 3.7.x version on. At the time of writing the latest version is 3.8 and the issue is still there.

##Will my app be rejected out of this?
Apple review process is far away from being clear, but a non-public selector reference (even though is indeed a false positive) is very likely to trigger an automatic rejection of your app. So yes, it **may** be.

##What should Facebook do?
Change the property name to `ID` or `userId` (`placeId`, `objectId`, whatever appropriate).

##Why don't they do it?
Because they are lazy.

[^1]: Ok, I lied to keep things simple. `id` is just a predefined type, defined as a pointer to `struct objc_object`.