---
layout: post
title:  "Starting Small with Raspberry Pi"
date:   2017-12-20 13:00:00 -0400
categories: file-formats siegfried raspberry-pi bash
author: Nick Krabbenhoeft
---
No better way to start blogging then by starting to blog. To make this less intimidating, I'll try to start small. Specifically with the $35 computer that sits on my desk.

A [$35 Raspberry Pi](https://www.raspberrypi.org/products/) doesn't come with an awful lot of computing power. That's fine by me, because parts of my work don't need an awful lot of power. They just need a reliable little machine that:

* doesn't consume a lot of electricity
* can stay on for weeks at a time
* can connect to our internal networks
* has plenty of USB ports
* gives me admin rights

A Raspberry Pi (and many of the other cheap singleboard computers) fits the bill.

The constant uptime is key. I use a laptop for most of my day-to-day work, which means my main computer is constantly going on- and off-line. That can make it difficult to finish jobs like downloading large files across the network and running the [DSHR Twitter bot](https://twitter.com/dshr_comments). When possible I offload those jobs to my Raspberry Pi.

The rest of this post will be about setting up and running a very specific job, file format identification with Siegfried.

## Siegfried on the Raspberry Pi

There are a lot of tools for file format identification, the most common I see are [DROID](https://digital-preservation.github.io/droid/), [Siegried](https://www.itforarchivists.com/siegfried), and [FIDO](http://fido.openpreservation.org/). They're all great and useful for different purposes, but as a day-to-day tool Siegfried feels faster, and I like it's command-line interface.

### Installing Siegfried

Siegfried's author, Richard Lehane, has included directions for installing the program on many systems. However, none of these instructions work perfectly on a Raspberry Pi. Installing with `apt-get` Ubuntu/Debian instructions fails because the code wasn't compiled for ARM processors.

This isn't an uncommon problem on the Raspberry Pi. Most programs are compiled for x86 processors (the kind in most Mac and PC laptops). A lot have been recompiled for ARM, but not all of them. You can recompile from source, but that's beyond my skillset, and Richard has backup instructions.

Siegfried is written in Go, so like any other scripting language, installing Go should allow you to run any program written in go. But, before you type
```sudo apt-get golang-go```
you should know that the latest version of Go in the Raspberry Pi repository is 1.3.3. Siegfried requires 1.4 or greater, so we'll have to install it ourselves.

{% highlight bash %}
# Download the ARM version of Go 1.9
wget https://storage.googleapis.com/golang/go1.9.linux-armv6l.tar.gz
# Unpack the files to your local application folder
sudo tar -C /usr/local -xzf go1.9.linux-armv6l.tar.gz
# Add the location of the files to your PATH variable via your bash profile
# (so when you type go, the computer knows where to find the code)
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
# Reload your bash profile to update the PATH variable
source ~/.bashrc
# Check your installation
go version
{% endhighlight %}


Whoo! Now we can finally install Siegfried.

{% highlight bash %}
# Install Siegfried directly from the source code on Github
go get github.com/richardlehane/siegfried/cmd/sf
# Download the latest PRONOM signature files for format identification
sf -update
# Test out Siegfried
sf ~/.bashrc
{% endhighlight %}

Everything should be working, so it's finally time to do a file format survey.

{% highlight bash %}
# Generate a list of paths to all the files you want to survey
# -type f, exclude paths to directories
# | grep -v "string", optional, repeatable, exclude paths based on name
# > store paths in a file
find /path/to/files -type f | grep -v "string" > sffiles.txt
# Run Siegfried
# -csv, output results as comma-separated lines
# -f sffiles.txt, use the paths found in the previous step
# &, run in the background
sf -csv -f sffiles.txt > sf.csv &
{% endhighlight %}

Depending on the number of files, the CSV should be complete in a few minutes or days. I'll talk about analysis later.


