---
layout: post
title: "LaTeX is now Sublime"
date: 2012-11-26 23:23
comments: true
categories: latex sublime-text
---
Hi everyone!

I can safely guess that many of you use or have used LaTeX, the beautiful markup language by Leslie Lamport.

If you don't and you have some aesthetic taste I suggest you to [check it out](http://en.wikibooks.org/wiki/LaTeX/Introduction) and stop producing ugly Word documents.
If instead you know what I'm talking about, I'd like to share with you all my LaTeX configuration, which makes use of the awesome editor **Sublime Text 2** and its plug-in **LaTeXTools**.

<!-- more -->

**Disclaimer**: the following article refers to an **OSX** implementation. The information provided usually applies to Windows and Linux systems too, even if minor discrepancies may occur, such as for the LaTeX bundle and the Sublime Text shortcuts.

# Tools Setup

## LaTeX
Installing LaTeX on OSX is pretty straighforward: all it takes is to visit the MacTeX project page and download the installer.
The installation comes with a extensive collection of LaTeX packages and with many useful tools, such as TeXLive, a package manager, and BibDesk, a BibTeX guy.

## Sublime Text 2
[Sublime Text](http://www.sublimetext.com/) is an amazing editor. Period.

I won't go through the several reasons why I love using it, since it would be beyond the scope of this post, but if you are interested you can check out articles such [this one](http://1p1e1.tumblr.com/post/14262857223/9-reasons-you-must-install-sublime-text-2-code-like-a) or [this other one](http://steverandytantra.com/thoughts/three-months-with-sublime-text-2) which are perfectly aligned with my opinion.

The first step is the install this cool editor (which comes with a free unlimited evaluation period, meaning that you'll have to discard an alert every once in a while), a trivial task that can be accomplished [downloading the provided installer](http://www.sublimetext.com/2).

## Sublime Package Control
One of the coolest feature of Sublime Text is its package manager, which has to be installed separately by executing the following command into the Sublime Text shell, accessible by the shortcut `` CTRL+` ``.

{% codeblock Sublime Package Control install script http://wbond.net/sublime_packages/package_control/installation source %}
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'
{% endcodeblock %}

## LaTeXTools
Now that we have the package manager installed it is now incredibly easy to extend the Sublime Text functionalities in a matter of seconds.

{% img alignright http://f.cl.ly/items/3m0v2y44122D2M300k0v/install-package.png 600 %}

Just open the command palette (*Tools -> Command Palette...* or `` Cmd+Shift+P ``) and select *Package Control: Install Package* (tip: start typing the name of the command and the results will be filtered in real time). Press enter, wait a few seconds for the package list to load, select LaTeXTools then press enter.
Wait for the installation to complete (status messages will appear at the bottom of the Sublime Text window) and there you go, quick and easy.

## Skim
Skim is a nice PDF viewer for OSX. I've never had nothing to complain about using the default Preview viewer, but Skim is definitely more advance and one of its feature will come in handy for our purposes.

On OSX the LaTeXTools plug-in will by default for a Skim installation, therefore we just need to install it with the installer provided on its website and that's all there is to it.

# Usage and Features
Great! In a few minutes and without any particular effort we have a nice and productive LaTeX environment. Let's give it a test drive.

## Our first document
Let's create a new LaTeX document called `sublime.tex`.

{% include_code A test document latex/sublime.tex %}

Now let's compile it using the shortcut `` Cmd+B `` (or by selecting *Tools -> Build*)

If everything went as expected, you will obtain a PDF document that will be automatically opened with Skim.

{% img /images/posts/sublime-latex/sublime-tex.png %}

## Forward and backward search
That was nice and easy, but I'd like to go a little more further and show you something slightly more advanced.

If you've ever used a LaTeX-specific editor one of the feature that you have probably loved was the possibility of jumping to the PDF from the source and the other way around from the PDF to the LaTeX source.

You may think (and many people I know actually did) that kind of auto-magic feature must be necessarily included in a custom IDE and that Sublime Text is a too generic editor for handling such a specific feature.
Well, it turns out that you are wrong, my friend. Let's configure our editor in order to achieve such a cool behavior!

### Forward
So you want to jump to the PDF from your LaTeX code? You're lucky, since LaTeXTools already did the work for you (according you're using Skim as a viewer, of course).
Simply use the shortcut `` Cmd+L-J `` (which means press `` Cmd+L ``, release it and then press ``J``) and you'll jump to the PDF at the same row where your cursor is in the source file.

If you pay attention you'll also notice that this is automatically done whenever you compile the document with `` Cmd+B ``, a feature that can be disabled in case it annoys you.

### Backward
Jumping to the PDF is great, but suppose you are reading the PDF and you find a typo. Wouldn't be nice to jump back to the source in order to modify it on the flight?

Here is where Skim reveals its power: all you need is to inform it that you're using Sublime Text as editor and tell it how to communicate with it.

Open the Skim preferences and select the *Sync tab*.
Be sure that the *Check for file changes* is unchecked, select *Custom* as Preset and paste the following path as Command:
```
/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl 
```

Bla