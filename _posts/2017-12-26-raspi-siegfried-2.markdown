---
layout: post
title:  "Troubleshooting a Siegfried Run"
date:   2017-12-26 13:00:00 -0400
categories: siegfried bash raspberry-pi
author: Nick Krabbenhoeft
---
In the last post, I talked about getting Siegfried installed on a Raspberry Pi and kicking off a process to identify file formats. Easy peasy, except sometimes things don't work out perfectly. Here are a few tips on handling problems with a run.

## Connecting to a Raspberry Pi from another computer
I have a monitor and keyboard for my Pi, but sometimes it's easier to use it through my laptop instead of turning on the monitor. For this, you'll need to [turn on SSH](https://www.raspberrypi.org/documentation/remote-access/ssh/) (Secure SHell) on the Raspberry Pi. 

In order to login to the Pi from another computer, you'll need to know the Pi's IP address.

{% highlight bash %}
# Show all network connections on the Pi, run this on the Pi
# wlan0 shows the wireless connection, eth0 shows the ethernet connection
# The IP address for the connection is on the line that starts `inet address:`
# The IP address probably looks like 192.###.###.### or 10.###.###.###
sudo ifconfig
{% endhighlight %}

Then from the other computer you can login to a command line session on the Pi.

{% highlight bash %}
# Connect to pi from another computer
# The default username is `pi`
ssh username@pi_ip_address
{% endhighlight %}

## Running Siegfried over SSH
Any commands run during an SSH session will typically end when the user ends the session. To keep a long-running process going, use the `nohup` (NO HangUP) command.

{% highlight bash %}
# Make sure a Siegfried doesn't stop when you end the SSH session
nohup sf -csv -f sffiles.txt > sf.csv &
{% endhighlight %}

## Put it in the background
The command from the end of the last post will automatically run Siegfried in the background thanks to the '&' at the end of the command. If you didn't add the '&', you can still put the process in the background like this.

1. Press `ctrl` + `z` to suspend the process
2. Enter the command `bg` to put the process in the background

You should be able to enter new commands agains.

## Restarting a run
If the power turns off, you're working over a flakey network connection, or for any number of reasons, your Siegfried run might get interrupted. Rather than start from scratch, you can pick up from where you left off.

{% highlight bash %}
# Find the name of the last file characterized 
tail -1 sf.csv
# Get the line number of that filename from your list of files
# to figure out how many files you characterized
grep -n 'name_of_the_file' sffiles.txt
# Make a new file list by skipping characterized files
# +, number of lines to skip, use the line number from last command
# if that file is problematic, add one until Siegfried runs again
tail +number sffiles.txt > sffiles2.txt
# Restart Siegfried, adding resuls to your previous output file
# >>, add new output to the end of file instead of overwriting
sf -csv -f sffiles2.txt >> sf.csv &
{% endhighlight %}

Repeat as necessary.

## Clean up the output
In case you had to restart Siegfried, you'll have a couple extra header lines in your csv. This can make it hard to import or analyze the data later, so best to clean it up now.

{% highlight bash %}
# Delete any lines that include the header fields, after the first line
# 2, skip the first line
# ${}, edit to execute
# /.../d, delete lines that match the pattern
sed '2,${/filename,filesize,modified,errors,namespace,id,format,version,mime,basis,warning/d}' sf.csv > sf_clean.csv
{% endhighlight %}

## Copying data over the network
If you can use SSH, you can also copy files over SSH using `scp` (Secure CoPy). That means less fooling around with USB sticks to get data to where you need it.

{% highlight bash %}
# Copy Siegfried output from the Pi
# username@pi_ip_address, login details for Pi
# :..., path to file to copy from Pi
# path/to/..., local paths don't need the login details and colon
scp username@pi_ip_address:sf.csv path/to/copy/file/to/
{% endhighlight %}

{% highlight bash %}
# If you need to copy from your computer to the Pi
scp path/to/a/file.ext username@pi_ip_address:path/to/copy/file/to/
{% endhighlight %}


I'll add more tips as I cause more problems for myself.


