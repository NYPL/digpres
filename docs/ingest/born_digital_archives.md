---
layout: page
title: Born-Digital Archives
parent: Ingest
---

## Package Requirements

A Born-Digital Archives package must conform to the folder and file structure shown in Figure. 1. Figure 2 to Figure 5 are example packages for different types of materials.

![General DA Package]({{site.baseurl}}/assets/img/DA_package_general.svg)
^ Figure 1. General Born-Digital Archives package

![Finding Aid Component from FTK Package]({{site.baseurl}}/assets/img/DA_package_example_FA_Component_FTK.svg)
^ Figure 2. Example package of Finding Aid Component from FTK

![Disk Image from optical media Package]({{site.baseurl}}/assets/img/DA_package_example_DI_optical_media.svg)
^ Figure 3. Example package of Disk Image from optical media

![Disk Image from floppy disk Package]({{site.baseurl}}/assets/img/DA_package_example_DI_floppy_disk.svg)
^ Figure 4. Example package of Disk Image from Floppy Disk

![File Transfer Package]({{site.baseurl}}/assets/img/DA_package_example_File_Transfer.svg)
^ Figure 5. Example package from File Transfer

## Data Model

Born-Digital Archives Data Model, as shown in Figure. 6, is created to accommodate a wide range of content collected by the NYPL [Digital Archives program](https://nypl.github.io/digarch/). It is designed to be adaptable to legacy collections as well as Digital Archives' future acquisitions.

### Data Model Description

Born-Digital Archives content contains different types of components, including but not limited to finding aid components, disk images, and file transfers. Each component forms a Structural Object (SO), [NAMED] , which can be understood as a folder. [NAME SO] must have one metadata SO, [NAMED] , and one contents SO, [NAMED] . Within the [NAMED (metadata) SO], there may not have any file, or it may have metadata file(s). Within the [NAMED (contents) SO], there can be Information Object(s) (IO), also known as asset(s), and/or file and folder hierarchy, depending on the original content structure.

![Born-Digital Archives Data Model]({{site.baseurl}}/assets/img/svg_data_model_born_digital_archives.svg)
^ Figure 6. Born-Digital Archives Data Model

## Process

Coming soon.