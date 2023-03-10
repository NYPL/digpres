---
layout: page
title: User Accounts Audit
parent: Management
---
# User Accounts Audit

Digital Preservation (DP) Program performs seasonal digital repository user accounts audit  
for the software, Preservica. User accounts for the digital repository are created in two  
places, the Preservica user interface and the virtual machine(s). This page discusses how  
the account audit process is conducted, documented and managed.

## User account management

Digital Preservation Program uses comma-separated values (CSV) files to manage user accounts.  
They are stored on the internal departmental NYPL Google Drive.

## Preservica user interface accounts audit instructions

1. Log in to nypl.preservica.com
2. Hover on to the “Administration” tab and click the option “Manage Accounts”
3. Review individual System Users one by one
4. Confirm if the account holder is still active
   1. If it is an NYPL employee account, use Workday to check if they still work at the Library
   2. If it is a contractor, check with user accounts manager in the Information Technology Group (ITG) if the person still works for the Library
   3. If it is a service account, check documented usage for the account, and see if the entity who is responsible for its use still needs it
5. If the account is no longer active, use “Delete User” to remove them from Preservica

## Virtual machine accounts audit instructions

There are three virtual machines that we use for Preservica. Below steps are to be run  
on each individual machine.

1. Log in to the virtual machine (VM)
2. List all users
    ```sh
    getent passwd {1000..60000}
    ```
3. Go through the list of users one by one
4. Confirm if the account holder is still active
   1. If it is an NYPL employee account, use Workday to check if they still work at the Library
   2. If it is a contractor, check with user accounts manager in the Information Technology Group (ITG) if the person still works for the Library
   3. If it is a service account, check documented usage for the account, and see if the entity who is responsible for its use still needs it
5. If the account is no longer active, delete the account
    ```sh
    sudo userdel username
    ```
6. TBD create a record for all VM user accounts for this VM
7. After checking all user accounts, we need to check group access. First, find all groups in the VM
    ```sh
    sudo cat /etc/group
    ```
8. For groups that are created to grant users different access, e.g. ingest, review who are in those
    groups. As an example, check the ingest group with this code:
    ```sh
    getent group ingest
    ```
9. Review individual users listed in the group, and confirm they are still in the library and need this group access.
10. If they no longer need to be in this group, remove their group membership. As an example, below code removes user, bjohnson, from ingest group, and does NOT remove the user account.
    ```sh
    sudo gpasswd -d bjohnson ingest
    ```
11. TBD create a record for all VM group accounts for this VM
