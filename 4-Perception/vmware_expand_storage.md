# Expanding Storage for vmware

## Issue
As you use your VM, your image may outgrow the space you originally allocated to it. This can cause issues including preventing it from booting. You may get a warning like `root filesystem full`. This is a sign to either delete some files or expand your storage on vm and your ubuntu partition.

## Expanding VM storage
To expand your available storage you must do two things:
1. Expand VM allocated storage
2. Expand Ubuntu Partition

#1 is the equivalent of physically increasing the storage capacity of the machine, almost like installing another storage device. Follow the instructions here to do that: [VMware Docs - Expand a Virtual Hard Disk](https://docs.vmware.com/en/VMware-Workstation-Pro/17/com.vmware.ws.using.doc/GUID-73BEB4E6-A1B9-41F4-BA37-364C4B067AA8.html)

#2 expands the Ubuntu partition to begin using that newly available storage. Follow these instructions: [Ubuntu Docs - Adjust the Size of a Filesystem](https://help.ubuntu.com/stable/ubuntu-help/disk-resize.html.en)

## View your partitions
Open the 'disks' application. Or, via terminal, do `sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL`

## View your storage
Use `df -h` to check your available storage in that partition.

## If your VM crashes because it runs out of storage:
You will need to:
1. Open recovery mode from BIOS
2. Mount your root partition to get access to your normal filesystem
3. Delete some files to make space
4. Exit and boot normally
5. Expand your VM storage, or delete more files you don't need

Follow these instructions: [Ubuntu Docs - Recovery Mode](https://wiki.ubuntu.com/RecoveryMode)