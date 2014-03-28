---
layout: post
title:  "It's OK to Shave the Yak"
date:   2014-03-28 17:00:00
categories: culture technical
---

[Yak Shaving](http://sethgodin.typepad.com/seths_blog/2005/03/dont_shave_that.html) is a term I really like because
it's so relatable to everyone... see the following (low quality) example of a form of Yak Shaving, from Malcolm in the
Middle:

<p style="text-align:center;">
  <iframe width="560" height="315" src="//www.youtube.com/embed/RHpJFROEOmg" frameborder="0" allowfullscreen></iframe>
</p>

Now, I have some counterintuitive advice for beginning software engineers. I say **Go Ahead and Shave the Yak**, and
here's why...

Software development is a process of discovery and creativity, as well as conformance to standards and specifications.
By Shaving **a single** Yak, you will learn about new systems and new ways of doing things. By Shaving **many** Yaks,
you will improve your rate of learning. You will start to notice patterns which you will be able apply to your own
work.

Let's say you're a fairly new developer, building a web app and you have some inter-dependent models, let's say...
<code>Parent</code> and <code>Child</code>. For performance' sake, you've de-normalized the last name field so that
each model has a <code>last\_name</code> property. Therefore, if the <code>Parent</code>'s <code>last\_name</code>
changes, the <code>Child</code>'s last_name needs to change as well.

The quick-and-dirty solution to this problem is to think "I've only got one controller action in my app where
<code>parent.last\_name</code> can change, so I'll update <code>child.last\_name</code> there as well." However,
further thinking would reveal that on any non-trivial project, you really can't be certain that this "one action" will forever remain the only applicable code path that could change the field value.

So -- once you realize that the "right" solution is actually to push this logic down to the model level, then you
start to think, "Well, now I'm going to have to learn all about after-save hooks... what once was a one-line change
could take me all day to do the 'right' way!"

<p style="text-align:center;">
  <img src="http://www.codebestowed.com/images/yak-shaving.jpg" width="400" />
</p>

Well, that's exactly what you should do -- you should spend the day learning these skills which will pay dividends in
the future. Additionally, take your time and be sure you learn it right, don't just rush through it to try to
accomplish this one specific goal that you happen to be tackling at the time.

Now, obviously this is a pretty clear-cut example that some could argue is less yak-shaving and more just "doing things
right." In reality, I have experienced many scenarios in which I could've gotten away with a "right" solution but there
was a "more right" solution that involved a bit of Yak Shaving. Often I took the first road, but sometimes I took the
second. Which brings me to...

**Don't Always Shave the Yak**. Good developers have an understanding of when it makes sense to work on honing their
craft, and when it makes sense to **Just Get Things Done**. Finding that balance comes with experience and is only
achievable through experimentation.
