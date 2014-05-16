---
layout: post
title:  "NSManagedObject Without Inserting"
date:   2014-05-16 15:00:00
categories: technical
---

Ever want to create a Managed Object but **not** have it be part of the Managed Object Context just yet? Possibly a
rare situation, but nevertheless, I found myself needing to do this and was dissappointed to find that most of the
CoreData documentation assumes that you will definitely want to persist newly created model objects.

Here's what I eventually figured out to do:

{% highlight obj-c %}
  NSManagedObjectContext *moc = /* Get managed object context */;
  NSEntityDescription *entity = [NSEntityDescription entityForName:@"ModelName"
      inManagedObjectContext:moc];
  NSModelName *modelInstance = [[NSModelName alloc] initWithEntity:entity
      insertIntoManagedObjectContext:nil];
{% endhighlight %}

If you then determine that you **do** want to persist the object:

{% highlight obj-c %}
  [moc insertObject:modelInstance];
{% endhighlight %}

(Then at some point call ```[moc save:&error]```)
