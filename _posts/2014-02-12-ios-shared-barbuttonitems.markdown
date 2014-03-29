---
layout: post
title:  "iOS Shared BarButtonItems"
date:   2014-02-12 02:45:00
categories: ios technical
---
<p></p>

### TL;DR Version
------------

You can use the following UINavigationControllerDelegate method to set up the NavigationItems for all child
ViewControllers:

{% highlight obj-c %}
- (void)navigationController:(UINavigationController *)navigationController
willShowViewController:(UIViewController *)viewController animated:(BOOL)animated
{% endhighlight %}
<p></p>

### Not-Long-Enough; Tell Me More Version
------------

Here's one difficulty I ran into this week that had a less-than-obvious solution...

The Problem
------------

In my iOS app, I wanted every UINavigationController's child ViewController to have the same rightBarButtonItem.

Specifically, I wanted every child to show the "+" (Add) button and for the "+" button to behave the same, regardless
of which screen it was touched in.

<img src="http://codebestowed.com/images/shared-barbuttons.png" width="600" />

Unfortunately, storyboards don't make this very easy to do in a
[DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) manner.

The Solution
------------

The solution basically has two parts:

1. ["Hack" the storyboard](#hacking-the-storyboard) to add the shared BarButtonItem directly to the
NavigationController.
- [Subclass UINavigationController](#subclass-uinavigationcontroller) to programmatically add the shared BarButtonItem
to child ViewControllers.  
<br/>

### <a name="hacking-the-storyboard"></a>Part 1: Hacking the storyboard
------------

In order to display the common BarButtonItem, you will need the NavigationController to have a NavigationItem. However,
by default a NavigationController object will contain a NavigationBar only and won't allow adding any more objects.

To get around this, I discovered the following trick:

1. Create a NavigationController object by dragging from the Object library onto an empty section of the canvas.
2. Select the newly-created RootViewController.
3. Use the Editor menu to select Embed In->Navigation Controller.

    <img src="http://codebestowed.com/images/embed-in.png" width="600" />

4. The **new** NavigationController object will have a NavigationItem.

    <img src="http://codebestowed.com/images/storyboard-hack.png" width="600" />

5. Integrate the new NavigationController into your "actual" storyboard and clean up the unnecessary elements from the
last few steps.

Now, you can drag a BarButtonItem onto the NavigationController's NavigationItem.

<img src="http://codebestowed.com/images/barbutton-added.png" width="600" />

This will help with visualizing how the navigationBar will look, but more importantly, it will also help with achieving
DRYness, as shown in part two below.

### <a name="subclass-uinavigationcontroller"></a>Part 2: Subclassing UINavigationController
------------

Now, you will need to link the BarButtonItem to an IBOutlet and an IBAction:

1. Create a subclass of UINavigationController.
- In the storyboard, set the Custom Class of the NavigationController to be your new class.
- Show the Assistant Editor and control-drag from the BarButtonItem into your new class's .m file to create an outlet
and an action for the BarButtonItem.

    <img src="http://codebestowed.com/images/connect-barbutton.png" width="600" />

The final step is to programmatically add the BarButtonItem to all child ViewControllers. First, tell your new class to
conform to the UINavigationControllerDelegate protocol. Then, add this code to your new class, replacing "addButton"
with the name of your IBOutlet property:

{% highlight obj-c %}
- (void)viewDidLoad
{
    [super viewDidLoad];
    self.delegate = self;
}

- (void)navigationController:(UINavigationController *)navigationController
    willShowViewController:(UIViewController *)viewController animated:(BOOL)animated
{
    if (!viewController.navigationItem.rightBarButtonItem) {
        viewController.navigationItem.rightBarButtonItem = self.addButton;
    }
}
{% endhighlight %}

IMPORTANT: Note the condition inside the "if" statement. This will allow you to override the rightBarButtonItem on a
per-screen basis later, if you want. But for now, make sure you remove any rightBarButtonItems from child
ViewControllers. Therefore, in your storyboard, your navigation path should look like this (only one "+" button):

<img src="http://codebestowed.com/images/final-shared-barbutton-storyboard.png" width="600" />

That's it! Now you can implement whatever you'd like in your IBAction method and the functionality will be the same on
every screen.

