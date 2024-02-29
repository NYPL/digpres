---
layout: page
title: Born-Digital Archives Receipt
nav_order: 0
parent: Receipt
---

# Born-Digital Archives Receipt

{: .no_toc }
&nbsp;
{: .no_toc .text-delta }

1. TOC
{:toc}

This page describes how the Digital Repository team receives processed
born-digital packages from the Digital Archives program. For more information on
how the born-digital packages are accessioned, transferred, processed and packaged,
visit [Digital Archives website](https://nypl.github.io/digarch/).

## Process

1. Digital Archives program packages the collection.
2. Once the packaging is complete, Digital Archives program will copy the
   collection packages onto the Isilon cluster A (ICA), a storage system we use
   at the NYPL.
3. Once the copy process is successful, Digital Archive program will mark
   completion on their Trello board, DigArch Project Queue.
4. Digital Archives staff will email the Digital Repository staff at
   <digpres@nypl.org> that this collection is ready to be received at ICA and
   ingested into Preservica.
5. Digital Repository program creates a new card on their Trello board,
   Digital Repository Collections Ingest.
6. Digital Repository staff follows the [Ingest process for "Processed Archives"](https://nypl.github.io/digpres/docs/ingest/processed_archives)
   to ingest the collection.
7. After validating the ingest is successful, following the Disposition steps to
   delete the collection from the staging storage on ICA and DigArchDiskStation,
   a local storage location.
