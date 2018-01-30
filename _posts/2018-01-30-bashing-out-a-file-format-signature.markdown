---
layout: post
title:  "Bashing out a file format signature"
date:   2018-01-30 10:00:00 -0400
categories: raspberry-pi bash file-formats
author: Nick Krabbenhoeft
---

PRONOM is the digital preservation file format registry of choice, but it's only as good as the file format signatures that we put into it. There are already great resources about how to do this. Jenny Mitcham form the University of York included links to most resources in her blog post about [creating a signature](http://digital-archiving.blogspot.com/2016/08/my-first-file-format-signature.html) and there is a [trove OPF blogs](http://openpreservation.org/knowledge/blogs/topic/format-identification/) on the subject by Ross Spencer, Andrea Byrne, Becky McGuiness, and other.

This post will be a very fast flyover of building signature for Microsoft's ACS format, aka Clippy's format.

## Gather some sample data.

The more examples of a format you have, the more confidence you have that you're finding a good signature. Luckily for ACS, the Microsoft Agent tool had a small [community](http://msagentring.org/) that built more of these demonic helpers. I even found one resource with over [300 examples of agents](http://www.ponx.org/msagent/Acs/). So let's download all of them.

{% highlight bash %}
# Make a throwaway directory to keep all these files organized.
> mkdir ~/acs
> cd ~/acs

# Download all the files
# wget, downloading program
# -r, to open all the links on the page
# -nH, to not create a directory for www.ponx.org
# -np, to not create sub-directories for msagent and Acs
# --cut-dirs, to not download anything outside the Acs directory

> wget -r -nH -np --cut-dirs 3 http://www.ponx.org/msagent/Acs/
{% endhighlight %}

## Look around for magic numbers
A lot of formats use a semi-unique series of bytes at the start of the file to identify themselves. We can see if this is the case by printing out the first 16 bytes of all of the ACS files.

{% highlight bash %}
# Display first 16 bytes of each file
# head, prints the start of a data stream
# -c 16, limits head to the first 16 bytes
# -q, makes sure the file name is printed
# *.acs, matches all the acs files in the folder
# It doesn't match

# xxd, prints the hexadecimal interpretation
# defaults to 16 bytes per line

> head -c 16 -q *.acs | xxd
0000000: c3ab cdab 33cd 0300 8707 0000 93c9 0300  ....3...........
0000010: c3ab cdab ebbb 2300 ab0a 0000 c3a2 2300  ......#.......#.
0000020: d0cf 11e0 a1b1 1ae1 0000 0000 0000 0000  ................
0000030: c3ab cdab 5e53 0000 4f06 0000 f251 0000  ....^S..O....Q..
0000040: c3ab cdab 017d 0300 a307 0000 697b 0300  .....}......i{..
0000050: c3ab cdab d1dc 0c00 9106 0000 91d9 0c00  ................
0000060: c3ab cdab e628 0a00 eb09 0000 a219 0a00  .....(..........
0000070: c3ab cdab b224 0100 2d06 0000 7823 0100  .....$..-...x#..
0000080: c3ab cdab cefc 0100 e906 0000 0afc 0100  ................
0000090: c3ab cdab 4ee5 3700 e70a 0000 f2c9 3700  ....N.7.......7.
00000a0: c3ab cdab 437e 1f00 8307 0000 7161 1f00  ....C~......qa..
00000b0: c3ab cdab 437e 1f00 2c07 0000 7161 1f00  ....C~..,...qa..
00000c0: c3ab cdab 8a92 6000 ff0b 0000 7e6c 6000  ......`.....~l`.
{% endhighlight %}


## Test any patterns

The first four bytes (0xC3ABCDAB) look like a good candidate, so let's see how many files have that as the first 4 bytes.

{% highlight bash %}
# Count up how many files start with different 4-byte strings
# cut, extract specified fields from data
# -d " ", fields are delimited by spaces
# -f 2,3, extract the 2nd and 3rd field from xxd output

# sort, alphabetical sort of the extracted lines

# uniq -c, count how many of each line there

> head -c 16 -q *.acs | xxd | cut -d " " -f 2,3 | sort | uniq -c
284 c3ab cdab
22 d0cf 11e0
{% endhighlight %}

You can try cutting more or less fields, but 0xC3ABCDAB looks like the longest common byte string. If you search for 0xD0CF11E0, you'll find that that is the signature for Microsoft OLE Compound File, a general file structure used as the basis for many formats like .doc Word files and .ppt PowerPoint files. These OLE ACS files probably deserve more research.

## Research to backup your hypothesis

It's always good to back up your work with data from other people. One potential source is the TrID format registry. In this case there is a [signature for ACS files](https://github.com/digipres/digipres.github.io/blob/master/_sources/registries/trid/triddefs_xml/ms-acs.trid.xml), and it matches our signature.

There's even someone that wrote an [unofficial spec for the format](http://fileformats.lebeausoftware.org/). Remy Lebeau's work also match our signature.

## Make and test a PRONOM signature file 

Ross Spencer built this [excellent tool](http://www.nationalarchives.gov.uk/pronom/sigdev/index.htm) so that you don't have to write the XML by hand. Both DROID and Siegfried can be loaded with the custom signature for testing

## Submit the signature.

Package up any of the information and resources you have found and [submit them to PRONOM](https://www.nationalarchives.gov.uk/contact-us/submit-information-for-pronom/). The folks at the National Archives will look over the signature and contact you with any comments.