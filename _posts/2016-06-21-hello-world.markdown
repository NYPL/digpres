---
layout: post
title:  "hello, world!"
date:   2016-06-21 11:25:44 -0400
categories: update
author: Nick Krabbenhoeft
---
Nothing to see here. Just testing what a blog might look like. Expect posts about preserving fixity, proving trustworthiness, writing documentation, building workflows, and dealing with formats. For example:

{% highlight python %}
function bag_cleaner(bag, system_files):
  for file in bag.payload:
    if match(system_files):
      bag.bad_payload.append(file)

  for file in bag.manifest:
    if match(system_files):
      bag.bad_manifest.append(file)

  for file in bag.bad_payload:
    if file not in bag.bad_manifest:
      os.remove(file)
{% endhighlight %}

If this looks exciting, stay tuned for the first actual next post.
