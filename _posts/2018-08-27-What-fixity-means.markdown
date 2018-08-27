---
layout: post
title:  "What Fixity Means"
date:   2018-08-27 10:00:00 -0400
categories: fixity AIP BagIt
author: Nick Krabbenhoeft
---

## Fixity and Data Loss in Libraries
Last week, a [story](https://www.cbc.ca/news/canada/newfoundland-labrador/mun-digital-archives-wiped-out-1.4787960) went out that the Queen Elizabeth II Library at Memorial University in Newfoundland had lost the primary copy of its digital archives in a server crash. The good news is that library has digital preservation practices in place and is restoring the data from backups. Preservation rescues access again!

This and similar stories offer a view into another core digital preservation topic, fixity. 

## What is Fixity?
Fixity is the property of objects to remain fixed, stable, unaltered, etc. Ideally, preservation keeps objects stable, but all things change. Oxides flake from a VHS tape. Dyes fade on a photograph. Pests eat the pages of a book. Fixity is a useful concept for every kind of collection; however, the term fixity is mostly used within digital preservation.

In digital preservation, fixity is often presented alongside the concept of checksums, an amazing piece of math that lets us create a fairly unique signature for files based on the bytes they contain. This piece of fixity information can be used to detect very small changes in an object.

But fixity information can also be other things like the size and location of a file. And these pieces of fixity information can be used to detect much bigger changed such as has a file been partially or completely lost. And frankly, in my experience and others, complete loss is more common than the slow trickle of bit flips detected by checksums. If you want a physical example, think of how much damage is caused by [theft](https://www.archives.gov/research/recover/notable-thefts.html) or [natural disaster](https://blogs.loc.gov/thesignal/2013/02/after-the-flood-digital-art-recovery-in-the-wake-of-hurricane-sandy/).

## Existence Fixity in Practice
Going back to the data loss incident at the Queen Elizabeth II Library, the server failure was likely detected in a fixity check to ensure that the server migration was completed successfully. They may not have called it a fixity check, but from a digital preservation perspective it was. And it's a useful check to incorporate into your own workflows.

For example, if you use bags in your workflows, many tools have a completeness check that compares the expected list of files in the manifest with the files present on the file system.

In Bagger, it's the yellow cube icon.

![Screenshot of Bagger Completeness Check Button]({{site.baseurl}}/assets/img/bagger-complete.png)

And in bagit-python, it's the `--completeness-only` option.

{% highlight bash %}
> bagit.py --validate --completeness-only path/to/bag
{% endhighlight %}

The benefit of these tools is that checking for the existence of a file is much, much faster than validating the checksums of a file. Because checksums are calculated based on every bit of a bytestream, validation requires reading every bit. On the other hand, completeness only requires reading file directory tables. You can't detect bit-level changes, but in the face of a disaster, crime, or another event, you can survey the fixity of an entire collection relatively quickly.

For a more general overview of fixity, I recommend the report [What is Fixity, and When Should I be Checking It?](http://www.digitalpreservation.gov/documents/NDSA-Fixity-Guidance-Report-final100214.pdf) published by the NDSA.
