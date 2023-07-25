# SysAdmin

This repo documents things that i had to learn for my job as a System
Administrator. Here is the description of each sub-directory contents:

## raid-zfs

Mostly about RAID and it's types. Explains how each RAID system {0,1,6,10}
stores data accross the disk array. Also introduces ZFS and it's capabilities, 
passing through RAID support in ZFS and how ZFS leads with read and write cache
disks.

## c3sl-configs

Documents any configuration of a new service, OS or file system done in a
machine that i manage in the laboratory i work on. Currently present documents:

|         Doc         |      Description     |
|---------------------|----------------------|
|  c3sldb1_zfs_setup  | Zfs setup in our database machine, raid10 and cache
configs included 														 |

