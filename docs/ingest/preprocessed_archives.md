---
layout: page
title: Pre-processed Archives
nav_order: 1
parent: Born-Digital Archives
grand_parent: Ingest
---

# Pre-processed Archives

{: .no_toc }
&nbsp;
{: .no_toc .text-delta }

1. TOC
{:toc}

## Pre-processed Archives: File Transfers from original media carriers

Pre-processed Archives packages that are the direct transfers from the digital carriers. Therefore,
they are also known as File Transfers (FT). The direct transfer process is conducted by the Digital Archives team.
More information can be found at [Digital Archives File Transfers documentation page](https://nypl.github.io/digarch/transfers/file-transfers.html).
The following diagram show how this type of package may look like.

![alt text]({{site.baseurl}}/assets/img/bakeoff_DA_package_example_File_Transfer.svg "Diagram showing the file and folder structure of a
Pre-processed Archives package")

The top-level folder should be named after the digital carrier ID, e.g. ACQ_1234_123456.
This top-level folder should also include a "metadata" and an "objects" folder.
Within the "metadata" folder, there should be a plaintext log file, which stores information about the transfer.
Within the "objects" folder, it is a bag structure, including a "data" folder alongside with bag-info.txt, bagit.txt, rcone-md5.txt and tagmanifest-md5.txt.
Within the data folder, it should have one or more files, which can be in folder structures.

## Data Model

### Data Model Description

## Process

### Step-by-step ingest instructions

### Ingest confirmation