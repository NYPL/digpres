---
layout: post
title:  "exFAT: A File System for Your Portable Drives"
date:   2022-02-14 10:00:00 -0400
categories: file-systems
author: Nick Krabbenhoeft
---

# exFAT: A File System for Your Portable Drives

We use a lot of portable drives at the library.
They range in size from 500 GB solid state disks the size of a credit card to a 126 TB RAID 5 array with its own rollaway case.
All of them use the same file system, exFAT.
And if you are also using a lot of portable drives, you should consider exFAT as well.

## What is a file system?

A file system is the way that data is structured on a storage device.
To better understand it, think about some of the things that happens when you open a file on your desktop named `video.mov`

1. The computer figures out where the Desktop folder is in the folder hierarchy.
2. It checks if there is a file in that folder named `video.mov`.
3. It checks to see if you have permission to open that file.
4. It finds out the sector of the device where the first part of that file is written.

The rules that create the architecture are part of the file system.
The File Allocation Table (FAT12) file system, first published by Microsoft in 1977, is a good example to use for illustration.

1. Each folder is defined as its own table with a row for every file or folder that is a member of that folder.
2. The first 11 bytes of the row define the file or folder name, represented as `(8 characters).(3 characters)`.
3. The 13th byte of the row specifies whether the file requires a password to read, modify, or delete.
4. The 20th and 21st byte specify which sector of the device the first chunk of the file is stored in.

As long as your computer system knows those rules and can follow them, it can use a storage device formatted with that file system.
If it can't read those rules, your computer will be able to see the storage device but not the contents of it.

## What are other examples of file systems?

A file system developed in 1977 will have the biases of a computing in 1977.
For example, a 5.25-inch floppy disk only held about 360 thousand bytes, so the designers didn't want to consume too much space with the file system.
With that perspective 11 characters for the file name and 2 bytes for the sector position was fine.
On a device with 5 billion bytes of capacity, 11 characters for a file name is miserly, and 2 bytes isn't enough for the sector positions.
FAT12 was extended to deal with some of these issues, first to FAT16 (1984) and then to FAT32 (1996).

But file systems can do even more than just organize a storage device.
They can have features like data integrity checks, on-the-fly compression, and storing metadata about the files.
The implementation of these features could be commercially advantageous and vendors created their own file systems to best interact with their systems.

Apple has used HFS (1986), HFS+ (1998), and APFS (2017).
Microsoft created NTFS (1993).
In Linux, there is ext2 (1993), ext3 (2001), ext4 (2008), and btrfs (still in development).

Take note of the relationship between operating systems and file systems.
Every OS will best support in-house developed file systems.
Supporting others' file systems is not guaranteed.
Windows can't read HFS+.
macOS can read from an NTFS-formatted device, but it can't write to it.
Linux can work with nearly everything, if someone released software to do it and you installed it.


## Why does this matter?

For those of us that work with old storage media, identifying a file system and having a computer with the appropriate software to read it is important.
That's just another example of needing to maintain an array of options to access data from the harried years of the 80s and 90s.

For those of us writing data to storage devices, we need to choose file systems that meet the needs of our computing environment.
A common need is cross-platform compatibility.
Because, if my digitization vendor uses Windows workstations and I use Macs, the hard drive we shuttle data with must be compatible with both.

You can install software to add additional file system compatibility.
And I do this when I have open-source options available or budget to buy licenses.
But, it's easier if I don't have to worry about what software is on what workstation.
Isn't there a cross-platform file system?

## exFAT: The Cross-Platform Choice

Microsoft released another file system in 2006, the extensible File Allocation Table (exFAT).
exFAT addresses FAT32's critical flaw in modern computing environments.
The FAT table records the size of a file using 4 bytes.
That means a storage device using FAT can only store files that are 4,294,967,295 bytes (4.29 GB) large.
In digitization work, this is smaller than most video files and even some audio files.

exFAT uses 8 bytes to store the size of a file, which means the maximum file size is 1.84 quintillion bytes (1.84 exabytes).
At this point, that's plenty of space for our entire collection, much less a single file.

exFAT is natively supported in Windows, macOS, and Linux.
That means no software installation needed.

It also supports all unicode characters, meaning `视频.mov`, `ویڈیو.mov`, `this-file-name-is-really-very-long-,-probably-too-long-to-be-useful_.mov`and `📹 .mov` are all legal.

What's the catch?
Well, it doesn't have support for useful features like data integrity checks and on-the-fly compression.
Don't use this as your day-to-day file system.

The cross-platform support also isn't perfect.
macOS only supports exFAT drives formatted with a sector size of 128-1024 kiB.
Either format the drive on a Mac or use be careful when choosing option during formatting on Windows and Linux.
However, those are small catches.

If you're looking for more validation, take a look at SD cards.
They are the last major removable media format that needs to be accessible on a variety of computers.
The default file system for all cards larger than 32 GB is exFAT.
