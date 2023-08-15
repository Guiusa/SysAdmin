# MONETDB VM
I created a VM to run a monetdb instance for my previous project, in this file
i'll document each step i had to take

## VMcommand
Abbey is the machine we use in our lab to create and manage VMs. As root, i
runned:

`
vmcommand clone --cpu 16 --mem 32G --vlan mvc3 -n monetdb
`

After this command, vmcommand initializes an iterativa interface to select the
Vm template used. I selected TemplateBookworm, corresponding to debian 12.

Also is necessary to ask to System administrators to add the MAC address
generated on DNS/DHCP server to point to monetdb name

## ProxMox
On the proxmox interface, i added 5 disks to be used in the following setup:

|      DISK     |              PURPOSE              |
|---------------|-----------------------------------| 
|  (1) 50Gb HD  |            mounted in /           |
| (5) 250Gb Hds | mounted in /home with RAID0 setup |

