---
layout: post
title:  "Bashing out another file format signature"
date:   2018-01-31 10:00:00 -0400
categories: raspberry-pi bash file-formats
author: Nick Krabbenhoeft
---

In the [last post](/2018/01/30/bashing-out-a-file-format-signature.html), I used some bash tools to derive a file format signature for the ACS format used to store [Clippy](https://en.wikipedia.org/wiki/Office_Assistant). During that analysis, 32 of the files had an OLE format signature, 0xD0CF11E0A1B11AE1.

Also known as the Compound File Binary Format, OLE was used as a structure for many different formats from software like 97-2003 Microsoft Office, SPSS, Minitab, and Caseware. It was also used to package the animations and character data used for Microsoft Agents. 

On [this documentation page](https://msdn.microsoft.com/en-us/library/aa227515.aspx), you can see the three formats Microsoft used for agents: ACS, ACF, and AAC. The ACS can store the entire character, but Microsoft also wanted to allow people to build websites and network applications with Agents. ACF and AAC made this easier since an application would only have to download the character data and animations it needed. On the other hand, Clippy.ACS took 6+ minutes to download over a 56k modem.

Examples of ACF and AAC are actually difficult to find on the web. Microsoft used to host them at locations like http://agent.microsoft.com/agent2/chars/genie/genie.acf. As far as I can tell, all of these resources are now down and weren't collected by a web archive. However, the OLE files from http://www.ponx.org/msagent/Acs/ have some promise. 

{% highlight bash %}
# Show bytes 1152-1212 from wade.acs
# tail, print the end of the data stream
# -c +1152, limits to starting at byte 1152

# head -c 160, print the first 160 bytes of the tail

# xxd show the hexcode of the bytes

> tail -c +1152 wade.acs | head -c 160 | xxd

0000000: 0063 0068 0061 0072 002e 0061 0063 0066  .c.h.a.r...a.c.f
0000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
0000020: 0000 0000 0000 0000 0000 0000 0000 0000  ................
0000030: 0000 0000 0000 0000 0000 0000 0000 0000  ................
0000040: 0012 0002 00ff ffff ffff ffff ffff ffff  ................
0000050: ff00 0000 0000 0000 0000 0000 0000 0000  ................
0000060: 0000 0000 0000 0000 0000 0000 0000 0000  ................
0000070: 0000 0000 0000 0000 00e4 0300 0000 0000  ................
0000080: 0061 006e 0069 006d 0031 002e 0061 0061  .a.n.i.m.1...a.a
0000090: 0066 0000 0000 0000 0000 0000 0000 0000  .f..............
{% endhighlight %}

This OLE file has at least one ACF file and AAF file embedded in it. The trick is extracting these files from inside the OLE file.

## Collect all the OLE files from ponx

Honestly, I would probably use python for this, but it was a fun exercise to work out as a shell-scripting 'one-liner'. There should be 33 files in there.

{% highlight bash %}
# Make a directory to store the OLE files
mkdir ole

# Find OLE files based on their first 2 bytes
# for ...;, open a for loop
# file in *.[aA][cC][sS], on each loop process one acs or ACS file
# do ...;, the loop
# bytes=$(head -c 2 $file);, store first two bytes in variable, byte
# test $bytes != 'ë', test if first bytes are not ë, which are true ACS files
# && cp $file ole, if first two bytes are not ë, copy file to ole folder
# done, finish loop
>for file in *.[aA][cC][sS]; do bytes=$(head -c 2 $file); test $bytes != 'ë' && cp $file ole; done

# Check out OLE ACS files
> ls ole
{% endhighlight %}


## Extract ACF and AAC files from OLE

This requires some tools that can read the OLE format. Since it is the basis of the 97-2003 Office formats, there's already a very good Java library to manipulate the formats, Apache POI. Even better, Ross Spencer wrote a nice command-line tool with the Java library to extract the files.



{% highlight bash %}
# Install python java wrapper jython
sudo apt-get install jython

# Download the Apache POI library
wget http://apache.mirrors.ionfish.org/poi/release/bin/poi-bin-3.17-20170915.tar.gz
tar -xzf poi-bin-3.17-20170915.tar.gz

# Download Ross Spencer's OLE tool
git clone git@github.com:exponential-decay/ole2-re-combiner.git

# Create folder for extracted files
mkdir extracted

# Extract all files from the ACS files
# jython, run jython
# -Dpython.path=poi-3.17/poi-3.17.jar, using the POI library
# ole2-re-combiner/ole2recombiner.py, and Ross's Python script
# --extract $file, extract files out of OLE
for file in ole/*; do jython -Dpython.path=poi-3.17/poi-3.17.jar ole2-re-combiner/ole2recombiner.py --extract $file; done
{% endhighlight %}

## Find signatures for ACF and AAC files

The ole folder should be filled with subfolders now, each with the extracted ACF and AAC files. Follow the same process as the [post on the ACS files](/2018/01/30/bashing-out-a-file-format-signature.html) to analyze them for magic numbers. You can use a small tweak to peak into all the subdirectories with one command.

{% highlight bash %}
# Display first 16 bytes of each ACF file
# ole/*/*.acf, all files in subdirectories of ole that have the acf extension

> head -c 16 -q ole/*/*.acf | xxd
{% endhighlight %}

I ended up with 0x1F000100 for AAF and 0xC1ABCDAB for ACF, although I'm doing a little more research before submitting these to PRONOM.