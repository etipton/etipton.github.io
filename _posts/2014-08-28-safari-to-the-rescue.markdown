---
layout: post
title:  "Safari to the Rescue"
date:   2014-08-23 16:30:00
categories: ui javascript technical
---

Yesterday at work I found myself up against a pretty gnarly UI / javascript bug that was only presenting itself in
Safari on the iPhone. Not having a physical iPhone on hand, I fired up the iOS Simulator provided by XCode.

I proceeded to spend several hours attempting to diagnose the issue by using a combination of desktop Chrome (and its
developer tools), the iOS Simulator, and javascript alerts (yes, ```alert()```s) sprinkled throughout my codebase.

Finally, I got sick of the ```alert()``` strategy and googled for a way to see ```console.log()``` output from the iOS
Simulator. What I found exceeded my expectations and greatly sped up the diagnostic process:

<p style="text-align:center;">
  <img src="http://www.codebestowed.com/images/safari-iphone-simulator.png" width="400">
<p>

With the iOS Simulator launched and running your local site in Safari, open your _desktop_ Safari app and click on
Develop -> iPhone Simulator -> localhost. You'll get the full suite of Safari's development tools at your disposal for
your iOS Simulator's browser!
