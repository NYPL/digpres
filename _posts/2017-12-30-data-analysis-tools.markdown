---
layout: post
title:  "Data Analysis Tools"
date:   2017-12-30 10:00:00 -0400
categories: data-analysis python r
author: Nick Krabbenhoeft
---
This is a quick aside from my series on working with Siegfried.

Data is useless unless without tools to analyze that data. To say anything meaningful, we need to do things like group, split, manipulate, and recombine the individual data points. A lot of larger systems have some analysis functions built in, e.g. dashboards or reports. However, when you're not working within a system, or if that system can't do the analysis you need, you'll need a dedicated tool.

## The Spreadsheet Options (Excel, Sheets, etc)

**The Good:** It's installed, you know how to use it, you can see the data, it has a decent user interface

**The Bad:** It slows down with too much data, it transforms data without telling you, it lets you fiddle with data

Spreadsheets can take your analysis pretty far pretty quickly thanks to their graphical user interface, and I often use them for fast analysis. Spreadsheets stop being useful when the interface gets in the way of the analysis. A couple of examples:

* You have 100 spreadsheets that you want collapsed into a single dataset. Open-copy-paste can get it done, but will take a lot of double-checking to make sure nothing was missed.
* Your barcodes are 14-digits long. Your spreadsheet program converts this to a floating-point number (3.43... * 10^13), since 32-bit integers max out at 10 digits.
* You need to add address information to 1000 rows of your spreadsheet. Writing the zip code in the first row and dragging down moves the address to Wyoming, one zip code at-a-time.

All of these problems have workarounds, including bruteforce, cell-by-cell manipulation of data. As you do larger amounts of analysis more routinely, that potential solution becomes a problem. What's to guarantee that someone hasn't fixed up a "few" cells by hand?

Data scientists advocate for [reproducibility](https://en.wikipedia.org/wiki/Reproducibility), the ability for someone else to get the same results using the same data. As if often the case, the most likely someone else is a future you trying to rerun the analysis in the future. The less manual fiddling that's needed for analysis, the more reproducible it can be. That includes messing aroud with `Open file...` menus, as well as cell-by-cell fiddling.

## The Scripting Options

To make analysis more reproducible, the first step is to start recording every step of analysis that you do.

* Step 1: Load the data
* Step 2: Clean/manipulate the data
* Step 3: Run the analysis
* Step 4: Produce a report/dataset/etc

This looks a bit like a computer program, but for it to be a program you need a language to express it in and software to write it in. The following are my two favorite ways of doing that.

### pandas + Jupyter Notebooks
**The Good:** You may already know Python, you can use the rest of Python alongside pandas, you can process Big Data, you can't fiddle with data, you can draft code like writing in a notebook

**The Bad:** The graphing libraries are _okay_, there can be an overwhelming number of _right_ ways to do something, indexes can become very complicated, notebooks can be very drafty

Pandas and R both use a concept called a dataframe, a 2 (or more) dimensional structure that stores data in columns and rows that are named by indexes. Basically, it's a spreadsheet. By abstracting away the graphical representation, you don't have to think about how answers to your questions fit on the sheet.

{% highlight python %}
import pandas as pd

df = pd.read_csv('path/to/siegfried.csv')

# How big are the files in the Siegfried report?
print(df.filesize.sum())

# What 10 file formats have the largest average file size?
print(df.groupby('format').filesize.mean().sort_values(ascending = False).head(10))

# What are the 5 most common file formats with a modified data in the 1980s?
print(df[df['modified']<'1990'].groupby('format').size().sort_values(ascending = False).head(5))
{% endhighlight %}

You can write pandas code in program you prefer, but the most common is Jupyter notebook. The examples above can be saved as Python files and run as scripts (e.g. `python3 analysis.py`), but often analysis requires exploring the data. In the last example, you might want to explore a specific year `df['modified']=='1990'` or bound your filter
`(df['modified']<'1990') & df['modified']>'1980'` while you explore. Jupyter notebooks let you edit and rerun lines of code quickly without have to rerun everything from scratch.

Another great benefit of pandas is that you can pull use the rest of the Python ecosystem. For example, if you want to combine a large set of CSVs into a single file, you can use the glob module to find the filenames, and then read/combine/export the data with pandas.

{% highlight python %}
import pandas as pd
import glob

# Find all the CSV's in a folder
all_files = glob.glob('path/to/folder/of/*.csv'))

# Read them into pandas and glue them together
df_from_each_file = (pd.read_csv(f) for f in all_files)
all_df = pd.concat(df_from_each_file, ignore_index=True)

# Save to a new file
all_df.to_csv('one_big.csv')
{% endhighlight %}



### R + RStudio
**The Good:** The tidyverse grammar is incredibly expressive, ggplot is an amazing visualization library, you develop analyses towards a finished project.

**The Bad:** You probably need to learn R from scratch, R can be slow

R does not come with the full power of a general scripting language like Python. However, it's data analysis functions can often feel much more fluid.  Here's the most common file formats from the 1980s question using the piping feature from tidyverse.

{% highlight r %}
library(tidyverse)

df = read_csv('path/to/siegfried.csv')

# What are the 5 most common file formats with a modified data in the 1980s?
df %>%
  filter(modified < '1990-01-01') %>%
  groupby(format) %>%
  count() %>%
  arrange(n) %>%
  head(5)
{% endhighlight %}

Personally, that is much simpler to read than the pandas version. You can even pipe results into what many consider R's best feature, the ggplot2 graphing library.

{% highlight r %}
df %>%
  mutate(year = year(modified)) %>%
  groupby(format, year) %>%
  count() %>% 
  ggplot() +
  geom_line(x = year, y = n, color = format)
{% endhighlight %}

And now you have a line graph of files by the year of their last modified date and grouped by their file format. Pretty powerful stuff.

## Conclusion

The best tool for any task is the one you can best use. If you don't have experience with pandas or R, there are plenty of free online course, Q&A communities, and colleagues in the field that can help you learn. On this blog, I'll be presenting most of my data analysis work in R or pandas. I hope you can find these posts useful in learning or improving your own data toolset.
