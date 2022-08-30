---
layout: page
title: Born-Digital Archives
parent: Ingest
---

## Package Requirements

A Born-Digital Archives package must conform to the folder and file structure shown in the General Born-Digital Archives Package.
The additional four diagrams in this section are example packages for different types of materials.

### General Born-Digital Archives Package

![alt text]({{site.baseurl}}/assets/img/DA_package_general.svg "Diagram showing the file and folder structure of a General 
Born-Digital Archives Package")

All Born-Digital Archives package conforms to this structure. The top-level folder, named "Finding Aid component", must have two 
folders, namely, a "metadata" folder and an "objects" folder. Within the metadata folder, it may have a "submissionDocumentation" folder.
Both the metadata and submissionDocumentation folders may or may not have any files. When there are files in the metadata folder, it could
include CSV files, Digital Forensics XML files, or any other types of metadata relevant to the object(s). In the "objects" folder, it may
have more files and folders structures, depending on what types of content it is, and its original arrangement.

### Example Package of Finding Aid Component from FTK

![alt text]({{site.baseurl}}/assets/img/DA_package_example_FA_Component_FTK.svg "Diagram showing the file and folder structure of an 
Example Package of Finding Aid Component from FTK")

This diagram shows how a package from the Finding Aid Component exported from FTK (Forensic Toolkit) may look like. The top-level folder should 
be named after the finding aid component ID, aka the Electronic Records Identifier, e.g. M1234_ER_0001. This top-level folder should also 
include a "metadata" and an "objects" folder. Within the "metadata" folder, there should be a CSV file, named with the same finding aid component
ID. Within the "objects" folder, it should have one or more files.

### Example Package of Disk Image from optical media

![alt text]({{site.baseurl}}/assets/img/DA_package_example_DI_optical_media.svg "Diagram showing the file and folder structure of an 
Example Package of Disk Image from optical media")

This diagram shows how a package from optical media disk image may look like. The top-level folder should be named after the disk image ID
, e.g. M1234_DI_0001. This top-level folder should include a "metadata" and an "objects" folder. Within the "metadata" folder, there may or
may not have any files. Within the "objects" folder, it should have one or more files, each of which is a disk image and is accompanied by its CSV
 metadata file. The disk image file is named after the collection ID and underscore, e.g. M1234_0001 and M1234_0002. Disk image files' metadata files 
 will have the same name but must be CSV files.

### Example package of Disk Image from Floppy Disk

![alt text]({{site.baseurl}}/assets/img/DA_package_example_DI_floppy_disk.svg "Diagram showing the file and folder structure of an 
Example package of Disk Image from Floppy Disk")

This diagram shows how a package of disk image from a floppy disk may look like. The top-level folder should be named after the disk image ID
, e.g. M1234_DI_0001. This top-level folder should include a "metadata" and an "objects" folder. Within the "metadata" folder, there may or
may not have any files. Within the "objects" folder, it should have one or more files, each of which is a disk image. The disk image file is named 
after the collection ID and hyphen, e.g. M1234-0001 and M1234-0002. Currently disk image from floppy disk does not contain metadata files.

### Example package from File Transfer

![alt text]({{site.baseurl}}/assets/img/DA_package_example_File_Transfer.svg "Diagram showing the file and folder structure of an
 Example package from File Transfer")

This diagram shows how a package from file transfer may look like. The top-level folder should be named after the collection ID, such as 
M1234_0001. This top-level folder should include a "metadata" and an "objects" folder. Within the "metadata" folder, there should be a CSV file, also
 named after the collection ID. Within the "objects" folder, the structure should be the same as the original arrangement of the storage media. The 
 file name is also the same as the original file name.

## Data Model

Born-Digital Archives Data Model, as shown in Figure. 6, is created to accommodate a wide range of content collected by the NYPL [Digital Archives program](https://nypl.github.io/digarch/). It is designed to be adaptable to legacy collections as well as Digital Archives' future acquisitions.

### Data Model Description

Born-Digital Archives content contains different types of components, including but not limited to finding aid components, disk images, and file transfers. Each component forms a Structural Object (SO), [NAMED] , which can be understood as a folder. [NAME SO] must have one metadata SO, [NAMED] , and one contents SO, [NAMED] . Within the [NAMED (metadata) SO], there may not have any file, or it may have metadata file(s). Within the [NAMED (contents) SO], there can be Information Object(s) (IO), also known as asset(s), and/or file and folder hierarchy, depending on the original content structure.

![alt text]({{site.baseurl}}/assets/img/svg_data_model_born_digital_archives.svg "Diagram using the Unified Modeling Language showing the Data Model of 
the Born-Digital Archives, including the data classification and its relationships, folder names, metadata fragments, security tags")

## Process

Coming soon.