* [services and applications](#services-and-applications)
   * [shadowsocks](#shadowsocks)
      * [install shadowsock client](#install-shadowsock-client)
         * [ubuntu](#ubuntu)
* [proxy terminal](#proxy-terminal)
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
* [users and groups](#users-and-groups)
   * [add users](#add-users)
      * [ubuntu](#ubuntu-2)
      * [centos](#centos-1)
   * [modify users](#modify-users)
   * [delete users](#delete-users)
* [networking](#networking)
   * [firewall](#firewall)
      * [centos](#centos-2)
   * [ssh](#ssh)
      * [SSH Login Without Password](#ssh-login-without-password)
      * [change ssh port](#change-ssh-port)
         * [ubuntu](#ubuntu-3)
         * [centos](#centos-3)
* [other useful commands](#other-useful-commands)


# services and applications
## shadowsocks
### install shadowsock client
#### ubuntu
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

# proxy terminal
* add this bash to .bashrc or
create file function.sh and add
`source functions.sh` in .bashrc
```bash
#!/bin/bash

assign_proxy(){
  PROXY_ENV="http_proxy ftp_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY FTP_PROXY ALL_PROXY"
  for envar in $PROXY_ENV
  do
     export $envar=$1
  done
  for envar in "no_proxy NO_PROXY"
  do
     export $envar=$2
  done
}

clr_proxy(){
   PROXY_ENV="http_proxy ftp_proxy https_proxy all_proxy HTTP_PROXY HTTPS_PROXY FTP_PROXY ALL_PROXY"
   for envar in $PROXY_ENV
   do
      unset $envar
   done
}

my_proxy(){
#  read -p "ip: " -s  ip && echo -e " "
#  read -p "Port " -s port &&  echo -e " "
#  read -p "method" -s method && echo -e " "
 PROXY='https://127.0.0.1:1080/';
 if [[ ! -z "$1" ]]; then
   PROXY=$1;
 fi

  echo $PROXY;
  proxy_value="$PROXY"
  no_proxy_value="localhost,127.0.0.1,LocalAddress,LocalDomain.com"
  assign_proxy $proxy_value $no_proxy_value
}

```

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
### create
pvcreate used to prepare a disk partition or a whole disk for use by LVM
```bash
pvcreate /dev/sd
```
### list , display
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

### Display
```
lvdisplay
```

# users and groups
## add users
* create user with home folder and login
`sudo useradd -m username -p PASSWORD`
* add user to sudo group on ubuntu
### ubuntu
`usermod -aG sudo arvin`
### centos
`usermod -aG wheel arvin`
## modify users
* set expiry time for user
```bash
sudo usermod --expiredate 1 arvin
```
* lock user password ( disable ssh )
```bash
sudo passwd -l arvin
```
* enable user password
```bash
sudo passwd -u training
```
* set expiry time for user password
```bash
sudo passwd -e  2013-05-31 arvin
```
* enable root user
```bash
su -
passwd
```
## delete users
* r option removes home directory
```bash
userdel -r arvin
```
# networking
## firewall
### centos
* install semanage
`yum install policycoreutils-python`

## ssh
### SSH Login Without Password
1. create rsa on client
`ssh-keygen -t rsa`
2. copy public key to remote
* make .ssh directory on remote
`ssh user@remote mkdir -p .ssh`
* copy public key to .ssh dir
`cat .ssh/id_rsa.pub | ssh user@remote 'cat >> .ssh/authorized_keys'`
* set permision to directory and files
` ssh user@remote "chmod 700 .ssh; chmod 640 .ssh/authorized_keys"`

 or just do this
`ssh-copy-id user@remote`


### change ssh port
1. open ssh config file  
`nano /etc/ssh/sshd_config`
2. uncommnet "#Port 22" and change it
3. enable port in firewall
#### ubuntu
`sudo ufw allow {your_port}`
#### centos
```
sudo semanage port -a -t ssh_port_t -p tcp {your_port}
sudo firewall-cmd --permanent --zone=public --add-port={your_port}/tcp
sudo firewall-cmd --reload
```

4. restart sshd
`sudo systemctl restart sshd`
5. verify
`ss -tnlp | grep ssh`
6. Login
`ssh user@remote -p {your_port}`


# other useful commands
* copy file to clipboard
```bash
xclip -selection clipboard .ssh/id_rsa.pub
```
