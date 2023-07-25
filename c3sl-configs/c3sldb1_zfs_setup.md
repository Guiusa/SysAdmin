# ZFS config c3sldb1
## Storage pool

Configured 6 disks of 6Tb each on a RAID6 setup (raidz2 as called by ZFS)
 
`zpool create tank raidz2 -f <6TB_DISKS_IDS>`

## Caches
### Read cache devices
Two 300Gb SSDs with striped data

`zpool add tank cache <300GB_SSDS_IDS>`

### Write cache devices
Two 240Gb SSDs mirroed in a l2arc setup. If one fails, there is another to cover the data writes.

`zpool add tank log mirror <240GB_SSDS_IDS>`
