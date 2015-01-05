---
layout: post
title:  "iOS Semi-Transparent Modal Views"
date:   2014-02-28 00:00:00
categories: ios technical
---
<p></p>

### TL;DR Version
------------

Key takeaways:

- Use a Modal-style segue (duh).
- Make the background color of your modal ViewController's main view clearColor.
- Add a full-screen-sized view to the main view and make it semi-transparent by setting it's background color to black
and it's alpha property to 0.5.
- Add another, smaller, view to the main view (as a sibling of the previous view) which will show your actual content.
- Utilize the ```UIModalPresentationCurrentContext``` modalPresentationStyle to have your modal overlay the prior
screen's contents.
- Use the ```dismissViewControllerAnimated:completion:``` method or an unwind segue to dismiss your modal view. You
will probably want to use NO for the animated boolean value.

<p></p>

### Not-Long-Enough; Tell Me More Version
------------

When I first started (re)learning Objective-C, Cocoa, etc., figuring out how to present a
[modal view](http://en.wikipedia.org/wiki/Modal_window) was a great way to learn some of the basics
of iOS programming.

In my case, I wanted to give the appearance of the original screen "dimming" and the modal appearing over it, like so:

<p style="text-align:center;">
	<img src="http://codebestowed.com/images/modal-final.png" width="200" />
</p>

I thought I'd share the concepts for any newbies out there who want to do this by walking through the process, starting
with creating a new project from scratch. I'll leave it to the reader to figure out how to integrate any or all of the
steps into his/her already existing project.

### <a name="showing-the-modal"></a>Showing the Modal
------------

1. Create a new project using the "Single View Application" template, and just choose iPhone (not iPad/universal).
- Open the Main.storyboard file and drag a TextView object into the view. Resize it to fit the view exactly, then
copy and paste the default text ("Lorem ipsum...") a couple times so that the whole view is covered in text. This will
allow you to ensure that the dimming effect is functioning properly.
- Drag a button onto the view and name it "Show Modal" or something similar. Change its background to a solid color
other than white so you can distinguish it from the text.
- Drag a ViewController into the storyboard. This will serve as the modal's ViewController.
- Control-drag from the button to the new ViewController and create a modal segue.
- Change the background color of the new ViewController's view to be clearColor.
- Drag a View into the new ViewController's view. By default, it should fill the entire view. Make this view's
background color black and alpha property 0.5. The alpha property is the transparency factor which causes the dimming
effect.
- Now, open up the Document Outline. Drag another View to the Document Outline, making it a sibling view of the
semi-transparent view you just created. This will contain the actual modal contents. It **must** be listed **after**
the semi-transparent view because views appear in the order they are defined. If you switched the ordering, it would be
"dimmed" along with everything else. Resize the view however you'd like and add a label if you'd like.

At this point your storyboard should look something like this:

<p style="text-align:center;">
  <img src="http://codebestowed.com/images/modal-storyboard.png" width="600" />
</p>

If you run the app as-is, you'll notice that other than the modal view, the screen becomes completely black instead of
dimming. To fix this, edit the ViewController class for the view that contains the "Show Modal" button (the new project
template should have created this class for you), and modify viewDidLoad:

{% highlight obj-c %}
- (void)viewDidLoad
{
    [super viewDidLoad];
    self.modalPresentationStyle = UIModalPresentationCurrentContext;
}
{% endhighlight %}

That's it! Your modal should display properly now.

### <a name="dismissing-the-modal"></a>Dismissing the Modal
------------

There are a couple of ways to dismiss the modal, one involving an unwind segue, and my personal preference, which is
simply to call a method from a button action. Either way requires adding some code. The first answer in this
[StackOverflow post](http://stackoverflow.com/questions/12561735/what-are-unwind-segues-for-and-how-to-use-them)
describes unwind segues really well so I won't go into that. Here's how to do it the way I prefer:

1. Create a new subclass of UIViewController.
- Set the modal's ViewController in your storyboard to be an instance of your newly-created ViewController.
- Add a button to the modal view and name it "Dismiss."
- Open the Assistant Editor and make sure your new ViewController's .m file is in the right pane. Control-drag from the
dismiss button to the .m file to create an IBAction method. Call the method "dismiss."

Now you're ready to have the action method do the dismissing. For this, you'll use the ```presentingViewController```
property and the ```dismissViewControllerAnimated:completion:``` method.

Your new ViewController's .m file should look something like this:

{% highlight obj-c %}
#import "MyModalViewController.h"

@interface MyModalViewController ()

- (IBAction)dismiss:(id)sender;

@end

@implementation MyModalViewController

- (IBAction)dismiss:(id)sender
{
    [self.presentingViewController dismissViewControllerAnimated:NO completion:nil];
}

@end
{% endhighlight %}

Note the use of NO for the animation option. If you specify YES, the animation doesn't seem to "jive" with the idea of
dimming then undimming the initial screen contents. In my opinion, modals behave better without animation when using
semi-transparency.

So that's it, those are the basics of presenting and dismissing semi-transparent modal views!
