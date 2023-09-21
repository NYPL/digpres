---
layout: page
title: Pre-processed Archives
nav_order: 0
parent: Born-Digital Archives
grand_parent: Ingest
---

# Pre-processed Archives

{: .no_toc }
&nbsp;
{: .no_toc .text-delta }

1. TOC
{:toc}

## Pre-processed Archives: Data from original media carriers

Pre-processed Archives packages are files from the digital carriers. There are a number of techniques used to transfer files including,
disk images, logical file transfers, and cloud transfers. Pre-processed Archives packages may be referred to by these names as well.
The transfer process is conducted by the Digital Archives team.
More information can be found at [Digital Archives Transfers documentation page](https://nypl.github.io/digarch/transfers/transfers.html).

The following diagram show how a typical file transfer package may look like.

![alt text]({{site.baseurl}}/assets/img/bakeoff_DA_package_example_File_Transfer.svg "Diagram showing the file and folder structure of a
Pre-processed Archives package")

The top-level folder should be named after the digital carrier ID, e.g. ACQ_1234_123456.
This top-level folder should also include a "metadata" and an "objects" folder.
Within the "metadata" folder, there may be multiple plaintext log files, which stores information about the transfer process.
Within the "objects" folder, it is a bag structure, including a "data" folder alongside with bag-info.txt, bagit.txt, manifest-md5.txt and tagmanifest-md5.txt.
Within the data folder, it should have one or more files, which can be in folder structures.

## Process

The Pre-processed Archives Ingest instructions document how Digital Preservation (DP) staff move
pre-processed archives packages from temporary storage locations to the Library's digital repository
hosted on Preservica.

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
    in the source code. In our context, [lint_ft.py](https://github.com/NYPL/ipres-package-cloud/blob/main/src/ipres_package_cloud/lint_ft.py)
    is a Python script that confirms each pre-processed archives package conforms to the structure
    expected by the packaging and ingest processes.

    1. Log in to a virtual machine (VM)
    2. Run the linter on all of the packages from the collection.

        ```sh
        python3 lint_ft.py -... /source/digarch/path/to/collection ...
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
        python3 ingest_digarch_ft.py
        ```

    5. Follow the instructions to create pre-ingest containers for all packages
       1. Select `1` to ingest content to PRODUCTION tenant or select `2` to ingest content to TEST tenant
       2. Clear the process list? Choose `N`
       3. Select the process you would like to run. Select `1` to Create New Container
       4. Select the workflow type. Enter `2` for FileTransfer

    6. After all containers are created, ingest all packages to the instance of choosing
       1. Select `1` to ingest content to PRODUCTION tenant or select `2` to ingest content to TEST tenant
       2. Clear the process list? Choose `N`
       3. Select the process you would like to run. Select `2` to Ingest Container
       4. Enter `ALL` to upload all packages, unless for other purposes, specify which container to ingest

    7. Monitor the ingest progress on the Preservica user interface

### Ingest confirmation

Confirm packages are ingested correctly on the Preservica website.