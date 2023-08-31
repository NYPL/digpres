---
layout: page
title: Born-Digital Archives
parent: Ingest
---

# Born-Digital Archives

{: .no_toc }
&nbsp;
{: .no_toc .text-delta }

1. TOC
{:toc}

## Package Requirements

A Born-Digital Archives package must conform to the folder and file structure shown in the General Born-Digital Archives Package.
An example package for the Finding Aid Component is also included on this page, as it is the most common Born-Digital Archives package type.

### General Born-Digital Archives Package

![alt text]({{site.baseurl}}/assets/img/DA_package_general.svg "Diagram showing the file and folder structure of a General
Born-Digital Archives Package")

All Born-Digital Archives package conforms to this structure. The top-level folder, named "Finding Aid component", must have two
folders, namely, a "metadata" folder and an "objects" folder. Within the metadata folder, it may have a "submissionDocumentation" folder.
Both the metadata and submissionDocumentation folders may or may not have any files. When there are files in the metadata folder, it could
include CSV files, Digital Forensics XML files, or any other types of metadata relevant to the object(s). In the "objects" folder, it may
have more files and folders structures, depending on what types of content it is, and its original arrangement.

### Example Package of Finding Aid Component from Forensic Toolkit

![alt text]({{site.baseurl}}/assets/img/DA_package_example_FA_Component_FTK.svg "Diagram showing the file and folder structure of an
Example Package of Finding Aid Component from FTK")

This diagram shows how a package from the Finding Aid Component exported from FTK (Forensic Toolkit) may look like. The top-level folder should
be named after the finding aid component ID, aka the Electronic Records Identifier, e.g. M1234_ER_0001. This top-level folder should also
include a "metadata" and an "objects" folder. Within the "metadata" folder, there should be a CSV file, named with the same finding aid component
ID. Within the "objects" folder, it should have one or more files.

## Data Model

Born-Digital Archives Data Model is created to accommodate a wide range of content collected by the NYPL
[Digital Archives program](https://nypl.github.io/digarch/). It is designed to be adaptable to legacy collections as well as Digital
Archives' future acquisitions.

### Data Model Description

Born-Digital Archives content contains different types of components, including but not limited to finding aid (FA) components, disk images,
and file transfers. This data model is created based on FA component packages, and describes how the package will be structured after ingested
into Preservica.

Each component forms a Structural Object (SO), named as "DI/EM/ER Container", which can be understood as a folder. DI stands for Digital Image;
EM stands for Email; and ER stands for Electronic Record. DI/EM/ER Container must have one metadata SO, named "(original folder title)_metadata",
 and one contents SO, "(original folder title)_contents". Within the metadata SO, there may not have any file, or it may have metadata file(s).
 Within the contents SO, there can be Information Object(s) (IO), also known as asset(s), and/or file and folder hierarchy, depending on the original
 content structure.

![alt text]({{site.baseurl}}/assets/img/svg_data_model_born_digital_archives.svg "Diagram using the Unified Modeling Language showing the Data Model of
the Born-Digital Archives, including the data classification and its relationships, folder names, metadata fragments, security tags")

## Process

The Born-Digital Archives Ingest Process documents how Digital Preservation (DP) staff move
Born-Digital Archives packages from temporary storage locations to the Library’s digital
repository hosted on Preservica. A Born-Digital Archives package is the package arranged
by the archivist after they have processed the collection. More information on the
Processing steps can be found on [Digital Archives documentation website](https://nypl.github.io/digarch/staging/processing.html).

### Step-by-step ingest instructions

1. Locate packages in two locations: Isilon and DigArchDiskStation

    1. Choose a collection to work with
    2. Create a Trello ticket to log the work
    3. Use compare_paths.py to confirm the copies on Isilon and DigArchDiskStation
       are the same

        ```sh
        python3 compare_paths.py --directory_one isilon/path --directory_two diskstation/path
        ```

2. Confirm with Digital Archives (DA) team what collections are ready for ingest. If there are
   issues about the collections, discuss them with DA team.

3. Log in to a virtual machine (VM)

4. Create a folder within the "DA_Incoming" with the collection ID as its folder name

5. Upload the collection to the folder that you just created, using rsync

    ```sh
    rsync -arP /source/folder/collectionID/folder/ DA_Incoming/collectionID/folder
    ```

    Argument explanation:
    * a is archive mode
    * r is recursive
    * P is progress

6. Validate and update packages

    Normally, a linter is a static program that catches errors, bugs and flags potential problems
    in the source code. In our context, [lint_er.py](https://github.com/NYPL/prsv-tools/blob/main/src/prsv_tools/ingest/lint_er.py)
    is a Python script that confirms each Electronic Record (ER) package conforms to the structure
    expected by the packaging and ingest processes.

    1. While in the VM, run the linter on all of the packages from the collection.

        ```sh
        python3 lint_er.py -... /source/digarch/path/to/collection ...
        ```

    2. After the linting process, go through the log file
       1. Fix each package that has error(s) individually
       2. If a repair can be carried out, do so. For example, use [flatten_er_metadata_folder.py](https://github.com/NYPL/prsv-tools/blob/main/src/prsv_tools/ingest/flatten_er_metadata_folder.py) to restructure the metadata folder
       3. If further help is needed, contact other Digital Preservation or Digital Archives staff
       4. Document common issues found and what we perform on them
    3. Continue linting the packages until all packages pass
    4. Move valid packages to DA_Source folder under the DigArch folder hierarchy

        ```sh
        mv DA_Incoming/folder/* DA_Source/DigArch/
        ```

7. Repackage and ingest

    Packages that conform to the data model structure are ready to be ingested into Preservica.
    First, they must be repackaged according to Preservica's expectations.

    1. Log in to a VM
    2. Switch user to preservica

        ```sh
        su preservica
        ```

    3. Change directory to `DA_Scripts`

    4. Run the packaging script

        ```sh
        python3 DigArch_NYPL_Uploader.py
        ```

    5. Follow the instructions to create pre-ingest containers for all packages
       1. Select `1` to ingest content to PRODUCTION tenant or select `2` to ingest content to TEST tenant
       2. Clear the process list? Choose `N`
       3. Select the process you would like to run. Select `1` to Create New Container
       4. Select the workflow type. Enter `1` for DigArch

    6. After all containers are created, ingest all packages to the instance of choosing
       1. Select `1` to ingest content to PRODUCTION tenant or select `2` to ingest content to TEST tenant
       2. Clear the process list? Choose `N`
       3. Select the process you would like to run. Select `2` to Ingest Container
       4. Enter `ALL` to upload all packages, unless for other purposes, specify which container to ingest

    7. Monitor the ingest progress on the Preservica user interface

### Ingest confirmation

Confirm packages are ingested correctly on the Preservica website.

### Clear out working area

Once the collection is ingested correctly, the working folders need to be cleaned up
before the next ingest.

1. `rm -r isilon/path/opex_DigArch/Container_*`
2. `rm -r vm/path/DA_Source/DigArch/M*`
3. `rm -r vm/path/DA_Target/Container_M*`
4. `rm -r vm/path/DA_Incoming/*`
