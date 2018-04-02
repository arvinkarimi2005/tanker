* [disk](#disk)
   * [scsi](#scsi)
      * [lsscsi](#lsscsi)
         * [ubuntu](#ubuntu-1)
         * [centos](#centos)
      * [list scsi devices](#list-scsi-devices)
      * [list scsi hosts](#list-scsi-hosts)
      * [rescan scsi](#rescan-scsi)
      * [remove disk from sci](#remove-disk-from-sci)
   * [fdisk](#fdisk)
      * [list](#list)
      * [format and partition](#format-and-partition)
   * [mount new disk using the mount command](#mount-new-disk-using-the-mount-command)
   * [LVM](#lvm)
      * [install on ubuntu](#install-on-ubuntu)
      * [install on centos](#install-on-centos)
      * [enable](#enable)
      * [pv ( LVMs physical volume )](#pv--lvms-physical-volume-)
         * [create](#create)
         * [list , display](#list--display)
      * [vg (volume group)](#vg-volume-group)
         * [create vg](#create-vg)
         * [list VGs or get vg details](#list-vgs-or-get-vg-details)
      * [lv ( logical volume )](#lv--logical-volume-)
         * [create lv in existing vg](#create-lv-in-existing-vg)
         * [Display](#display)
# disk
## scsi
### lsscsi
#### ubuntu
```bash
cat /proc/scsi/scsi
```
#### centos
```bash
yum install lsscsi
```
### list scsi devices
```bash
cat /proc/scsi/scsi
```
### list scsi hosts
```bash
ls /sys/class/scsi_host
```
### rescan scsi
```bash
echo "- - -" > /sys/class/scsi_host/host#/scan
fdisk -l
tail -f /var/log/message
```
### remove disk from sci
```bash
echo 1 > /sys/block/sdc/device/delete
```

## fdisk
### list
```bash
fdisk -l
```
### format and partition
```bash
fdisk /dev/sdx
```
options :
* n – create a new partition
* p – print the partition table
- can specify size , start and endpoint
* d – delete a partition
* w – write the new partition table and exit
* q – quit without saving changes

## mount new disk using the mount command
```bash
mkdir /disk1
mount /dev/sdx /disk1
df -H
```
* mount automaticaly on reboot ( add to /etc/fstab)
```bash
/dev/sdx               /disk1           ext3    defaults        1 2
```
## LVM
### install on ubuntu
```bash
apt install lvm2
```
### install on centos
```bash
yum install lvm2*
```

### enable
```bash
systemctl enable lvm2-lvmetad.service
systemctl enable lvm2-lvmetad.socket
systemctl start lvm2-lvmetad.service
systemctl start lvm2-lvmetad.socket
```
### pv ( LVMs physical volume )
#### create
pvcreate used to prepare a disk partition or a whole disk for use by LVM
```bash
pvcreate /dev/sd
```
#### list , display
To get LVM's internal view and details, use LVM commands
Lists : use the lvs and pvs commands with option --segments
Detailed view: use the lvdisplay and pvdisplay commands with option -m

1- lvs
* List the physical segments used by a logical volume :
```
lvs --segments /dev/vg_ssd/lv_videos -o +lv_size,devices
```
* List the physical extents of a given LV. Useful to move those segments (using pvmove):
```
lvs  /dev/vg_ssd/lv_videos -o seg_pe_ranges
```

3- lvdisplay
* List the physical segments of a given logical volume, among other information:
```
lvdisplay -m /dev/vg_ssd/lv_videos
```
4- pvdisplay
* Display the Logical volume associated with a given physical volume , among other information:
```
pvdisplay -m /dev/sdb1
```

4- pvs
* list the logical volume (segments) inside a given Physical Volume:
```
pvs  /dev/sdb1  --segments  -o +lv_name,lv_size
```
* Complex command, but full list :
```
 pvs   --segments  -o pv_name,pv_size,seg_size,vg_name,lv_name,lv_size,seg_pe_ranges
```

###  vg (volume group)
At least one physical volume is required to create a volume group.
#### create vg
vgcreate <volume group name> <device name>
```
 vgcreate testvg /dev/sdx1 /dev/sdx2 ...
```
#### list VGs or get vg details
```bash
vgdisplay testvg #testvg is optional
```

### lv ( logical volume )
#### create lv in existing vg
```
lvcreate -n lv_data1 --size 12G testvg
```
* options:
-n : volume name
--size[-L] : volume lv size
-p{r|rw} : permision
-M{y|n} : persistent

#### Display
```
lvdisplay
```
