---
layout: post
title: "Obj-C: infer types like a man, man"
date: 2013-01-01 15:45
comments: true
categories: objective-c compiler
---
#The Objective-C type system, in brief
From a type system point of view **Objective-C** can be seen as hybrid language: it both allows dynamic typing, therefore achieving a great flexibility, and static typing, turning the compiler in a precious ally.

When coding you can explicitly declare the types of all your variables or decide to use the keywork `id`, which - in short - is a sort of a hint to the compiler telling him: "*Look, I know what I'm doing. Just trust me and everything will work out just fine.*".

Sometimes using `id` comes in very handy, for instance `id` is the type of any object hold by a generic container (as `NSArray`) eliminating altogether the need for templates or generics (like you see in C++ or Java).

<!-- more -->

# `id` in practice
A very typical usage of the `id` keyword is with constructors. Why can't the type be explicitly specified in such a case? Let's make an example: we have a class `Person` and its subclass `Programmer`. What if we specify the type for their constructors?

{% codeblock lang:objc Person.m %}
-(Person *)init {
	if (self = [super init]) {
		/* initialize stuff */
	}
	return self;
}
{% endcodeblock %}

{% codeblock lang:objc Programmer.m %}
-(Programmer *)init {
	if (self = [super init]) {
		/* special skills for programmers */
	}
	return self; // what's the return type of self?
}
{% endcodeblock %}

We have a conceptual problem here. We initialized the `self` instance of `Programmer` with the `Person` constructur (by calling `[super init]`), so now it's type is `Person`.
Too bad that we our method has a return type of `Programmer`, therefore we cannot definitely return `self`, being of the wrong type.

Using `id` as return type allows subclasses to use their parents constructurs, a widly use technique for neatly solving the issue.

# The compiler is smart...
Being that such a strong convention the **clang** compiler is already smart enough to infer the return type of many methods, by making use of another equally strong convention of the language: every instance method starting with `init`, `copy`, `retain` or `self` and every class method starting with `alloc` or `new` **is known to be a method returning an instance of the receiver class**.

So as an example the compiler knows that the following call will produce a `Person` instance, even if the return type of `init` is `id`:

{% codeblock lang:objc %}
[[Person alloc] init]
{% endcodeblock %}

Suppose then that `Programmer` has defined a method called `codeLikeAMan`. If we try something like follows:

{% codeblock lang:objc %}
[[[Person alloc] init] codeLikeAMan];
{% endcodeblock %}

the compiler will yell at us claiming that there is no visible interface for `Programmer` that declares the selector `codeLikeAMan`.

# ...but not as smart as you may think!
That's great but what if we want to customize our constructors a little, for instance providing our own factory method? We would probably end up writing something like:

{% codeblock lang:objc Person.m %}
+(id)person {
	return [[Person alloc] init];
}
{% endcodeblock %}

Nice, what what about type inference? Let's try

{% codeblock lang:objc %}
[[Person person] codeLikeAMan];
{% endcodeblock %}

Bad news! The compiler doesn't scream anymore: it's not able to infer the type of `[Person person]` since it does not follow any known naming convention, and that's really bad since we all know that not any person can code like a man!

Result: compile and crash at runtime. Not cool at all.

It seems that we just need a way to let the compiler know that our method is returning an instance of the receiver class, being actually a factory method.

# Smart again, with a little help
It turns out that our friend clang has a [language extension](http://clang.llvm.org/docs/LanguageExtensions.html#objc_instancetype) which defines a special contextual keyword `instancetype` that can be used as a return type of an Objective-C method.

The keyword will tell the compiler to infer the return type, by looking at the method implementation. If we are returning something whose type can be inferred by the compiler, that type will become our new return type. `id` is used otherwise.

Let's try with our previous example

{% codeblock lang:objc Person.m %}
+(instancetype)person {
	return [[Person alloc] init];
}
{% endcodeblock %}

The compiler infers that the type `[[Person alloc] init]` is `Person`, according to the standard naming rule we discussed above, but now it will go further and it will change the return type of our method to `Person`. Therefore when we try again to do something as before:

{% codeblock lang:objc %}
[[Person person] codeLikeAMan];
{% endcodeblock %}

the compiler will complain again, since now it knows the type of `[Person person]` to be `Person` and it can finally state without any doubt that a random person is not definitely able to code like a man!

It's all for today,
stay tuned for more pills and happy 2013!