# Storage

## Block devices

* a type of file found under the `/dev` directory
* a piece of hardware that can store data
* data is stored and read in blocks hence block storage/devices

* To see the block devices

```bash
lsblk
```

```bash
ls -l /dev/ | grep "^b"
```

* Each block has a major and a minor number 
  * The Major number: identifies the type of device
  * The Minor part: distinguish phisical/logical devices

## Disk Partitions

* Allows to segment space
* A disk can be used without partitioning

### Partion Types

* Primary partions: used to boot an operating system
* Extended Partition: cannot be used on it's own but can host logical partitions
* Logical Partitions

### Partition Schemes

#### MBR

* Older
* Only 2TB
* Only 4 primary partitions

#### GUID Partitions Table - GPT 

* More modern
* Number of primary partition is only limited by the OS
* No size limit

### `FDISK` command

* Allows viewing/creating/deleting partitions

```bash
# Print partitions
sudo fdisk -l /dev/sda
```

### `GDISK` command

* improved version of fdisk

```bash
gdisk /dev/sdb
```

## File systems in Linux

* To write to a partition - we must create a file system
* Defines how data is stored on the disk
* once the file system is created we must mount it do a directory

### EXT*2

* There are 3 versions of EXT 2 through 4
* Each imporoves the previous one and is backward compatible

* To create an ext4 files sytem:

```bash
mkfs.ext4 /dev/sdb1
```

* To mount the partition to a directory:

```bash
# First create the directory
mkdir /mnt/ext4;

# Then mount the partition to the directory
mount /dev/sdb1 /mnt/ext4
```

* To check if it mounted correctly:

```bash
mount | grep /dev/sdb1
```

or

```bash
df -hP | grep /dev/sdb1
```

* To make the mount available after a reboot, add entry to `/etc/fstab`:
  * Filesystem - the file system to be used (/dev/vdb1)
  * Mountpoint - directory to be mounted on
  * Type - (ext2, ext3 etc.)
  * Options - (RW - read/write, RO = Readonly)
  * Dump - 0 = ignore, 1 take backup
  * Pass - 0 = ignore, 1 or 2 FSCK filesystem check enforced

## DAS NAS and SAN

### DAS - direct attached storage

* attached directly to the host system
* is seen as block device
* excelent performance
* dedicated to a single host

### NAS - network attached storage

* data goes through the network
* exported via NFS
* centralized shared storage

### SAN - storage area network

* High throughput
* Provided via LAN
* detected by the host as raw disk
* uses FCP or Ethernet
* hosts mission critical applications

## NFS file system

* doesn't store the data in blocks
* stores data as files
* works on a client - server model

* To export nfs directories:

```bash
# all
exportfs -a

# explicit
exortfs -o <client-ip>:/software/repos
```

* Once exported, you can then mount the directory on the client via the `mount` command

## LVM - Logical Volume Manager

* Allows grouping of multiple physical volumes
* From the group - you can then carve logical volumes
* Logical volumes can be resized dynamically

* required pacakge: `lvm2`

* to create a logical volume:
  * create a physical volume on a partition: `pvcreate /dev/sdv`
  * create a volume group: `vgcreate caleston_vg /dev/sdb`
  * `pvdisplay` - displays all the Physical volumes
  * `vgdisplay` - displays all the Volume Groups
  * create logical volumes: 

    ```bash
    # -L = linear volume
    # vol1 - name
    # caleston_vg - group
    lvcreate -L 1G -n vol1 caleston_vg
    ```

  * `lvdisplay` - displays the volume
  * `lvs` - displays all the volumes
  * Create a file system: `mkfs.ext4 /dev/caleston_vg/vol1`
  * mount: `mount -t ext4 /dev/caleston_vg/vol1 /mnt/vol1`

* To resize:
  * `vgs` - displays the volume groups
  * resize logical volume:

    ```bash
    lvresize -L +1G -n /dev/caleston_vg/vol1
    ```
    
  * resize the file system:
    
    ```bash
    resize2fs /dev/caleston_vg/vol1
    ```
