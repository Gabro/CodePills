---
layout: post
title: "Haskell data declaration constraints, the right way"
date: 2012-12-17 14:21
comments: true
categories: haskell, functional-languages
---

Recently I've been using the Haskell language to implement the solutions to the [Coursera's Algorithms 2](https://class.coursera.org/algo2-2012-001/class/index) online course.

The idea of using Haskell came talking with [Mario](http://blog.mariosangiorgio.com/) who was already using it for the very same purpose.

In such context Mario started developing a purely functional Union-Find data structure an he eventually run into an [interesting problem](http://blog.mariosangiorgio.com/2012/12/17/let-the-type-inferencer-work-for-you/) concerning algebraic data types that inspired this post.

<!-- more -->

In order to define a parametric data type in Haskell you usually write something like follows. (I'll take as example the very same data structure mentioned above.)

{% codeblock lang:haskell %}
data UnionFindElement valueType =
  RootElement valueType |
  ElementWithParent valueType (UnionFindElement valueType)
  deriving (Eq, Show)
{% endcodeblock %}

Everything looks fine and we try to use the new data type declaring a new function and its signature

{% codeblock lang:haskell %}
holds :: UnionFindElement valueType -> valueType -> Bool
holds (RootElement v) value = v == value
holds (ElementWithParent v root) value = v == value
{% endcodeblock %}

What we are doing here is to perform an equality check on an argument whose type is a generic `valueType`. It seems reasonable but the compiler is not very happy about that: in fact we never promised that `valueType` will be an instance of the class `Eq` therefore it cannot determine whether the `==` operation can be performed or not.

The obvious fix here seems to put a constraint on the data constructor, meaning something: "*look the parameter can be any type, but I guarantee that such type is an instance of Eq*".

{% codeblock lang:haskell %}
data (Eq valueType) => UnionFindElement valueType = ...
{% endcodeblock %}

Perfect right? Well, unfortunately it doesn't work as expected (I won't go through the details here) and this fact is so well known that such construct has been deprecated and it will be removed soon, as stated in the [official documentation](http://www.haskell.org/ghc/docs/latest/html/users_guide/data-type-extensions.html).
{% blockquote %}
This is widely considered a misfeature, and is going to be removed from the language.
{% endblockquote %}

Damn! This means that we need to manually specify for every signature that our `valueType` is an instance of `Eq`, doing something like:

{% codeblock lang:haskell %}
holds :: (Eq valueType) => UnionFindElement valueType -> valueType -> Bool
{% endcodeblock %}

Really annoying indeed...

Can we do better? Fortunately we can! It turns out that in Haskell is possible to put constraints on the data type constructors using something called **Generalized Algebraic Data Types** (GADTs). According to the [doc](http://www.haskell.org/ghc/docs/latest/html/users_guide/data-type-extensions.html#gadt):

{% blockquote %}
Generalised Algebraic Data Types generalise ordinary algebraic data types by allowing constructors to have richer return types.
{% endblockquote %}

This roughly means that we can specify a custom signature for the data constructors, and therefore we can also specify the necessary type constraints.

How can we do that? Take a look at the following code:
{% codeblock lang:haskell %}
data UnionFindElement valueType where
  	RootElement 	  :: (Eq valueType, Show valueType) => valueType -> UnionFindElement valueType
  	ElementWithParent :: (Eq valueType, Show valueType) => valueType -> UnionFindElement valueType -> UnionFindElement valueType
deriving instance Eq (UnionFindElement valueType)
deriving instance Show (UnionFindElement valueType)
{% endcodeblock %}

As already anticipated, with this new syntax we are allowed to specify the constructor signature. Therefore we are putting a couple of constraints (`Eq` and `Show`) on valueType. After that we want to make our new data type an instance of `Eq` and `Show`, but it turns out that we cannot use the 'normal' syntax for deriving, since the instance declaration would have a non-standard context. Fortunately Haskell comes with a mechanisms called **stand-alone deriving declaration** which allows us to specify the instance derivation separately from the data type definition (if you want to know more about it, [the doc](http://www.haskell.org/ghc/docs/6.12.2/html/users_guide/deriving.html) is there waiting for you).

The last thing I need to tell you is that the two cool features that we just discussed, GADTs and stand-alone deriving, both need a compiler flag in order to be enabled, respectively `-XGADTs` and `-XStandaloneDeriving`. Just add them to your build command and you're all set.
If you use Sublime Text 2, you can change your `cmd` parameter in `Haskell.sublime-build` to:

```
"cmd": ["runhaskell", "-XGADTs", "-XStandaloneDeriving", "$file"]
```

So we made it! We can now write any signature using our newly defined `UnionFindElement` and the compiler will know that its type parameter will be an instance of `Eq`, allowing us to perform (dis)equality operations on it!

It's all for today,
stay tuned for more pills!

**UPDATE**

As Jason pointed out in the comments you can use a pragma instead of adding compiler flags. In this way you can enable this features in a more flexible way only when you need them. Just add this pragma at the top of your source file and you're done

{% codeblock lang:haskell %}
{-# LANGUAGE GADTs, StandaloneDeriving #-}
{% endcodeblock %}