We all at some point in time we all format our USB drives. The question I would like to ask is how do you do it? Is it from GUI or terminal?

I see formatting USB drives from the terminal very handy. Today I am going to show you how to format USB drives using the terminal. Things needed for this tutorial to be useful is just a USB drive, the capacity doesn’t matter.

Let us get started!

1. Insert your USB drive into your system.

2. Open the terminal. (CTRL + ALT + T)

3. Look for the USB drive you want to format, by running:
```
$ df
```
The command above will display the directory path of your various drives. Take note of the drive you wish to format.
In this tutorial, the name of the drive am going to format is Seth and its path under the filesystem is /dev/sdc1.

3. Unmount drive using the syntax below:
```
$ sudo umount /dev/sdc1
```
4. Now run this command to format drive to fat32:
```
$ sudo mkfs.vfat -n 'Ubuntu' -I /dev/sdc1
```

## Understanding the above command

### mkfs

mkfs is used to build a Linux filesystem on a device, usually a hard disk partition. The device argument is either the device name (e.g. /dev/hda1, /dev/sdb2), or a regular file that shall contain the filesystem. The size argument is the number of blocks to be used for the filesystem.

### vfat

Formats the drive to FAT32, other formats available are mkfs.bfs, mkfs.ext2, mkfs.ext3, mkfs.ext4, mkfs.minix, mkfs.msdos, mkfs.vfat, mkfs.xfs, mkfs.xiafs etc.

### -n

Volume-name sets the volume name (label) of the file system. The volume name can be up to 11 characters long. The default is no label. In this tutorial my volume-name is Ubuntu.

### -I

It is typical for fixed disk devices to be partitioned so by default, you are not permitted to create a filesystem across the entire device.


Running $ df after formatting displays this.

You are done and your pen drive has successfully been formatted.
