---
layout: page
title: Processed Archives
nav_order: 0
parent: Born-Digital Archives
grand_parent: Ingest
---

# Processed Archives

{: .no_toc }
&nbsp;
{: .no_toc .text-delta }

1. TOC
{:toc}

## Processed Archives: Finding Aid Component packages from Forensic Toolkit

Processed Archives packages are packages of files created by the Archival Processing Unit using either Forensic Toolkit (FTK) or a file manager.
Subsequently, they are exported by the Digital Archives team.
They are also known as Finding Aid Component (FA component) packages. More information on the Processing
steps can be found on [Digital Archives documentation website](https://nypl.github.io/digarch/staging/processing.html).
The following diagram shows how this type of package may look like.

![alt text]({{site.baseurl}}/assets/img/DA_package_example_FA_Component_FTK.svg "Diagram showing the file and folder structure of an
Example Package of Finding Aid Component from FTK")

The top-level folder should be named after the finding aid component ID, aka the Electronic Records Identifier,
e.g. M1234_ER_0001. This top-level folder should also include a "metadata" and an "objects" folder.
Within the "metadata" folder, there should be a CSV file, named with the same finding aid component ID.
Within the "objects" folder, it should have one or more files.

## Data Model

Born-Digital Archives Data Model is created to accommodate a wide range of content collected by the NYPL
[Digital Archives program](https://nypl.github.io/digarch/). It is designed to be adaptable to legacy collections as well as Digital
Archives' future acquisitions.

### Data Model Description

The following data model describes how a FA component package will be structured after ingested
into the digital repository software, Preservica.

Each component forms a Structural Object (SO), named as "DI/EM/ER Container", which can be understood as a folder. DI stands for Digital Image;
EM stands for Email; and ER stands for Electronic Record. DI/EM/ER Container must have one metadata SO, named "(original folder title)_metadata",
 and one contents SO, "(original folder title)_contents". Within the metadata SO, there may not have any file, or it may have metadata file(s).
 Within the contents SO, there can be Information Object(s) (IO), also known as asset(s), and/or file and folder hierarchy, depending on the original
 content structure.

![alt text]({{site.baseurl}}/assets/img/svg_data_model_born_digital_archives.svg "Diagram using the Unified Modeling Language showing the Data Model of
the Born-Digital Archives, including the data classification and its relationships, folder names, metadata fragments, security tags")

## Process

The Born-Digital Archives Ingest instructions document how Digital Preservation (DP) staff move
FA component packages from temporary storage locations to the Library’s digital
repository hosted on Preservica.

### Step-by-step ingest instructions

1. Locate packages

    1. Choose a collection to work with
    2. Create a Trello ticket to log the work

2. Upload the collection to the source folder with rsync

    ```sh
    rsync -arP /source/folder/* DA_Source/folder
    ```

    Argument explanation:
    * a is archive mode
    * r is recursive
    * P is progress

3. Validate and update packages

    Normally, a linter is a static program that catches errors, bugs and flags potential problems
    in the source code. In our context, [lint_er.py](https://github.com/NYPL/prsv-tools/blob/main/bin/lint_er.py)
    is a Python script that confirms each Electronic Record (ER) package conforms to the structure
    expected by the packaging and ingest processes.

    1. Log in to a virtual machine (VM)
    2. Run the linter on all of the packages from the collection.

        ```sh
        python3 lint_er.py -... /source/digarch/path/to/collection ...
        ```

    3. After the linting process, go through the log file
       1. Fix each package that has error(s) individually
       2. If a repair can be carried out, do so
       3. If further help is needed, contact other Digital Preservation or Digital Archives staff
       4. Document common issues found and what we perform on them
    4. Continue linting the packages until all packages pass

4. Repackage and ingest

    Packages that conform to the data model structure are ready to be ingested into Preservica.
    First, they must be repackaged according to Preservica's expectations.

    1. Log in to a VM
    2. Switch user to preservica

        ```sh
        su preservica
        ```

    3. Change directory to `DA_Scripts`, which has a pyenv environment for Python version control
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
