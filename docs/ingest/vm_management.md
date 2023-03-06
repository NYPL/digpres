---
layout: page
title: Virtual Machine Management
parent: Ingest
nav_order: 1
---

Ingests should be run from dedicated virtual machines (VMs) to provide optimal bandwidth for file transfers and uploads.
VMs might be dedicated to a specific kind of ingest, such as born-digital archives or digitized audio and moving image.
Additional VMs may be necessary to paralellize the workflow and increase bandwidth utilization or to support a new ingest process.
All VMs are managed by the Information Technology Group (ITG) and should be configured the same when possible.

## Create a New Virtual Machine

1. File a Jira Ticket with ITG to create a new virtual machine.
Include the following:
    * required processor cores, 4-16
    * required RAM, 16+ GB
    * required working storage, 4+ TB if possible
    * required mounts of other storage clusters, such as Isilon or workgroup storage
    * `sudo` privileges for your account
    * a list of users to create
    * a list of software to install (screen, tmux, nano, python3, python3-pip, git)
3. Confirm that the VM meets your needs.
   1. Check connectivity from both the office hardwired connections and wireless VPN connections.
   2. Check your account's sudo privileges, `sudo -v`.
   3. Check all software is installed.
   4. Check all user accounts were created, `ls /home`.
4. Setup additional users, install software, and mounts if required.
5. Contact users to test their connections.
6. Add the VM to the Keeper list of workstations.
7. Close the ticket with ITG.

## User management

### Create new user accounts

1. Create a new user account

   ```sh
   sudo useradd <username> -m -p <pw> -s /bin/bash -G ingest
   ```

   * `<username>` set the username for the account
   * `-p <pw>` set a temporary password of your choosing
   * `-m` create a home directory
   * `-s /bin/bash` set shell
   * `-G ingest` set secondary group to ingest
2. Send the login information to the user.
Ask them to test the connection and also change the password.

### Delete user accounts

1. Delete the account

   ```sh
   sudo userdel -r <username>
   ```

   * `sudo userdel` command to delete the account
   * `-r` deletes the home directory and other user data
   * `<username>` name of the account to delete

## Install software

Most VMs require the same software available either from `apt-get` or `pip`

```sh
sudo apt-get screen tmux nano
sudo apt-get install python3 python3-pip git
sudo pip3 install --system lxml boto3
```

VMs also require the custom Python packaging scripts.
Instructions are being investigated to install these as system-wide scripts.
In the meantime, clone the repository to your home folder and use that version.

```sh
cd ~
git clone https://github.com/NYPL/prsv-tools.git
python3 ~/prsv-tools/path/to/script.py
```

## Mount Drives

### Add new mount

All storage mounts should be made into one of the following directories:

* `/source/` for read-only mounts to data source locations
* `/data/` for read/write mounts to working storage for the script
* `/ifs/preservica/development` for mounts to the upload/storage/download directories for the Preservica test instance
* `/ifs/preservica/production ` for mounts to the upload/storage/download directories for the Preservica production instance

1. Create a directory for mounting and change its ownership to the ingest group.

    ```sh
    sudo mkdir /path/to/mountpoint
    chmod -R ingest /path/to/mountpoint
    ```

2. Update the file system table file with the location and characteristics of the storage to mount. Use existing entries in the file as models.

   ```sh
   sudo nano /etc/fstab
   ```

    * ```sh
      # example read-only mount
      storage.cluster.url:path/to/folder /path/to/mountpoint        nfs4    ro,rsize=65536 1       1
      ```

    * ```sh
      # example read-write mount
      storage.cluster.url:path/to/folder /path/to/mountpoint        nfs4    rw,rsize=65536,wsize=65536 1       1
      ```

3. Mount the drives.
   If drives fail to mount, investigate with IT.

   ```sh
   sudo mount -a
   ```

### Remove a mount

1. Comment out the appropriate line in the file system table.

   ```sh
   sudo nano /etc/fstab
   ```

2. Unmount the source

   ```sh
   sudo umount /path/to/mountpoint
   ```

3. Ensure the source is unmounted by checking that it is not in the list of current mounts.

   ```sh
   sudo mount
   ```

4. Ensure the directory used as the mountpoint is empty and delete it.

   ```sh
   ls /path/to/mountpoint
   rmdir /path/to/mountpoint
   ```

