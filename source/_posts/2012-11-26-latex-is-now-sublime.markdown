---
layout: post
title: "LaTeX is now Sublime"
date: 2012-11-26 23:23
comments: true
categories: latex sublime-text
published: false <!-- DRAFT -->
---
Hi everyone!
I can safely guess that many of you use or have used LaTeX, the beautiful markup language by Leslie Lamport.
If you don't and you have some aesthetic taste I suggest you to [check it out](http://en.wikibooks.org/wiki/LaTeX/Introduction) and stop producing ugly Word documents.
If instead you know what I'm talking about, I'd like to share with you all my LaTeX configuration, which makes use of the awesome editor **Sublime Text 2** and its plug-in **LaTeXTools**.

<!-- more -->

# Tools Setup

## LaTeX

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
Wait for the installation to complete (status messages will appear at the bottom of the Sublime Text window) and there you go, easier than ever.

## Skim

# Basic Usage