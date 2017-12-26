---
layout: post
title:  "Troubleshooting a Siegfried Run"
date:   2017-12-20 13:00:00 -0400
categories: update
author: Nick Krabbenhoeft
---
In the last post, I talked about getting Siegfried installed on a Raspberry Pi and kicking off a process to identify file formats. Easy peasy, except sometimes things don't work out perfectly. Here are a few tips on handling problems with a run.

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

I'll add more tips as I cause more problems for myself.


