---
layout: post
title:  "MaybeBytes: What the '700MB' on a CD-R means"
date:   2019-01-08 10:00:00 -0400
categories: sip audio
author: Nick Krabbenhoeft
---

When you look at an old CD, you might see '700MB' printed in large letters on the label. MB is the SI abbreviation for megabytes or 1,000,000 bytes, but that doesn't mean a '700MB' CD has a maximum capacity of 700,000,000 bytes. It can store 737,280,000 bytes.

![Drawing of 700MiB CD-RW]({{site.baseurl}}/assets/img/cd.png)

And of course that's not really right either.

## Binary and Decimal 1000's
Computer scientists and engineers love binary math. A bit has 2 potential values (2^1). A byte has 8 potential values (2^3). When counting bits and bytes, why not keep using powers of two? That's why you see those numbers so often. The N64 had a processor 64-bit processor (2^6). Extended ASCII encodes 256 characters (2^8). Conveniently 2^10 is 1024, almost a thousand. 

And that's where the problems start. When we started storing lots and lots of bytes, engineers wanted an abbreviation. The SI prefixes for powers of 10, K (thousand, 10^3), M (million, 10^6), G (billion, 10^9), etc, were familiar. Some thought they should be adopted as is, but others believed binary was a more natural for digital counting. They used the following: K (2^10), M (2^20), G (2^30), etc.

So now you have two groups of people using the same word for a different. Errors start to build. You buy a hard drive that can store 250 GB (250x10^9 bytes), but *then your operating sytem says it can only hold 233 GB* (23x32^30 bytes). And it gets worse as you count higher. A decimal K is 2.3% smaller than a binary K. A decimal G is 6.8% smaller than a binary G.

Obviously there was a problem that had to be fixed. The end result is that we have two prefixes: decimal prefixes stayed the same, and binary 1000's are supposed to use binary prefixes. KiB instead of KB. MiB instead of MB. GiB instead of GB. And binary prefixes are pronounced by trading the last two letters of the decimal prefix for 'bi'. So 'kibi', 'mebi', 'gibi', 'tebi', etc.

And since language is simple, this change happened immediately (it was incorporated into standards starting in 1998 and finishing in 2008), and no one ever makes a mistake. Definitely, there is not a fascinating [Wikipedia article](https://en.wikipedia.org/wiki/Binary_prefix).

## Back to CD's
As you can guess, the 'M' in '700MB' refers to a mebibyte, not a megabyte. A CD can store 730MB or 700MiB. And you will find CD's that have '730MB' printed on them. They store the exact same amount of data as those with '700MB'.

### Problem 1
At the beginning, I said a 700MiB CD can store 737,280,000 bytes, so why 730MB not 737.28 MB? Marketing. Round numbers sound nicer, and it would be false advertising to round up, so 730MB it is.

### Problem 2
CD was created as an audio format. Some of the original specifications are for audio playback, like a requirement for 90 seconds of silence so a machine could tell when the CD was finished playing. Some software and drives can write data to these areas, called [overburn](https://www.cdrfaq.org/faq03.html#S3-8-3). That's good for another few MB of data capacity.

## So what is the max capacity of a CD?
Data on a CD is stored as a continuous spiral of dots and dashes. To guide the laser while burning to a writeable disc, the plastic is molded with a spiral called the pre-groove. If you tighten the curve of the spiral, you can actually fit more data onto the disc, as long as your machine is capable of following the tighter groove.

Look around eBay long enough and you'll find some '800MB' and ['900MB' CD-R's](https://www.ebay.com/itm/25-MediaRange-Branded-Blank-CD-R-discs-48x-100-min-900MB-100-minutes-CD-R-MR222-/320993466527), although [not all CD players can play these](https://www.cdrfaq.org/faq03.html#S3-8-2). And of course the '900MB' CD actually holds about 870MiB which is really 912MB...

There are problem even more asides to this question, but it's starting to feel like trying to measure the [coastline of the Great Britain](https://en.wikipedia.org/wiki/Coastline_paradox).

