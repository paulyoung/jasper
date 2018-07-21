---
layout: post
cover: false
navigation: true
title: Jade Inheritance
date: 2013-09-20 12:00:00
subclass: 'post'
logo: 'assets/images/logo.png'
author: paulyoung
categories: paulyoung
---

<em>TL;DR I created <a href="https://github.com/paulyoung/jade-inheritance" target="_blank">jade-inheritance</a> to understand the dependencies between <a href="https://github.com/pugjs/jade" target="_blank">Jade</a> templates in order to only compile the files I needed.</em>

In a previous post I described how some much needed updates to our Jade templates at CrowdTwist came at the cost of slowing down our development time.

<img src="http://imgs.xkcd.com/comics/compiling.png" alt="'Are you stealing those LCDs?' 'Yeah, but I'm doing it while my code compiles.'" />

At first I thought, "Sure. We’re doing some pretty complicated stuff now and our app has grown, so it’s bound to take longer."

But then it hit me – our watch task (which monitors changes to our templates) was compiling <strong>everything</strong> everytime I edited something. Even if the edited file lived in total isolation, the entire app’s Jade files were getting compiled into HTML.

And it was taking <strong>57s</strong>.

Even for the smallest change. Fixing a typo, iterating on a feature, anything at all.

<h2>The Problem</h2>
I started by asking, “Why?”.

Why were we compiling all of our templates when potentially only one of them had changed?

Well, there was the possibility that the changes to the modified file could impact the output of other files. Updating a file which was extended or included by other files would require those files to be recompiled too.

<h2>The Solution</h2>
To solve this problem I created <a href="https://github.com/paulyoung/jade-inheritance" target="_blank">jade-inheritance</a>.

Given a Jade file and a directory, it will determine which files within that directory extend or include the file. By passing a modified file to jade-inheritance it’s possible to understand which other files should be recompiled. On the file with the most dependants in our project, this additional processing can be done in around 1.5s.

<h2>Conclusion</h2>
Now, modifying a file which is completely isolated takes only <strong>0.15s</strong>. A 99.7% improvement, with other files of varying dependence compiling in much less time too.
