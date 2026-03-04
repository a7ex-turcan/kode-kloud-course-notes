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