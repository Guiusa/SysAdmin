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

## Raid setup

To setup the raid arrangement on monetdb machine i had forstly to install mdadm
package:

`
apt update && apt install mdadm
`

Then, i runned the command to create a RAID0 setup with all four 250Gb disks i
added to the VM

`
mdadm --create /dev/md0 --level=0 --raid-devices=4 /dev/vdb /dev/vdc /dev/vdd /dev/vde
`

### Configuration problem
I forgot to add an entry in /etc/mdadm/mdadm.conf to specify that those disks 
where arranged in a RAID0 array with a given name. So i added /dev/md0 in 
/etc/fstab and after the first reboot, mdadm changed the arrangement name from
md0 to md127, wich broke fstab and made impossible for the system to boot
correctly and accept ssh conections.

Luckily, we have resources in Abbey to fix this, i had to take the following
steps:

1. Map the root disk of vm monetdb into a file located in /dev on Abbey:

`
rbd map vm-268-disk-0
`

Wich returned me **/dev/rbd0**

2. Then, mount the generated virtual disk in /mnt:

`
mount /dev/rbd0 /mnt
`

3. Chroot into the disk to make changes on mdadm.conf:

`
chroot /mnt
`

4. Make the necessary changes in mdadm.conf, than umount de disk and unmap it.
After all this, i could reboot correctly the system and proceed with
configuration 

`
/etc/mdadm/mdadm.conf:

ARRAY /dev/md0 metadata=1.2 UUID=xyzw1234:xyzw1234:xyzw1234:xyzw1234
`

`
umount /dev/rbd0

rbd unmap vm-268-disk-0
`

## MonetDB install

I decided to compile manually MonetDB following it's howto, as present in
**https://github.com/MonetDB/MonetDB**.

Firstly, i created the source directories:

`
git clone https://github.com/MonetDB/MonetDB
`
Than, i runned:

`
cmake /root/MonetDB/
`

In the first run, cmake returned an error because BISON wasn't present, so i 
simply installed bison  package with apt and runned again. Then i runned:

`
cmake --build . --target install
`
