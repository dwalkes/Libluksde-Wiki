# Introduction
To mount a Linux Unified Key Setup (LUKS) Disk Encrypted volume you can use luksdemount.

There is support for the following back-ends:
* Dokan library
* fuse
* OSXFuse

To build luksdemount see [Building](https://github.com/libyal/libluksde/wiki/Building).

# Mounting
To mount a LUKSDE volume you can either:
* mount it directly from a device file;
* mount it directly our of a RAW storage media image at a certain offset.

To mount directly from a device file:
```
luksdemount -p password /dev/sda2 /mnt/luksdevolume/
```

To mount directly our of a RAW storage media image at a certain offset:
```
luksdemount -p password -o 524288 image.raw /mnt/luksdevolume/
```

Note that luksdemount takes an offset in bytes if you're copying the output from mmls multiply by the sector size:
```
luksdemount -p password -o $(( 1024 * 512 )) image.raw /mnt/luksdevolume/
```

This will expose a device file that provides the RAW volume data contained in the LUKS volume.
```
/mnt/luksdevolume/luksde1
```

If you get the error:
```
No sub system to mount LUKS volume.
```

That means fuse was not detected when building the luksdetools, check if you have fuse-dev installed and if ./configure is able to detect it.
The last part of the ./configure output shows you this in an overview.

You can now mount the device file as a loopback device:
```
mount -o loop,ro /mnt/luksdevolume/luksde1 /mnt/ntfs_file_system
```
## Mounting on Mac
On Mac system you may see a failure " -o loop: option not supported" during the mount step above.  Use [hdiutil](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/hdiutil.1.html) instead, for instance:
```
hdiutil attach -imagekey diskimage-class=CRawDiskImage -nomount /mnt/luksdevolume/luksde1
```

## Obtaining the volume offset
There are several ways to obtain the volume offset.
* Linux fdisk
* mmls of the [SleuthKit](http://www.sleuthkit.org)

### Linux fdisk
On Linux you can run fdisk with the list option (-l):
```
sudo fdisk -l /dev/sda
```

Or directly on a partitioned RAW storage media image file:
```
fdisk -l image.raw
```

## Why is /mnt/luksdevolume/ not accessible as root
By default fuse prevents root access to the mount point when a LUKSDE volume is mounted.
To enable this functionality first check the fuse documentation.

Make sure the fuse configuration file:
```
/etc/fuse.conf
```

Contains:
```
user_allow_other
```

Pass "allow_root" to the fuse sub system using the luksdemount -X option:
```
luksdemount -X allow_root -p password image.raw /mnt/luksdevolume/
```

## Windows
To mount a LUKSDE volume on Windows:
```
luksdemount -p password -o 524288 image.raw x:
```

At the moment the luksdemount keeps a hold on the console.

This will expose a device file that provides the RAW volume data contained in the LUKS volume.
```
X:\LUKSDE1
```

# Unmounting
You can unmount /mnt/luksdevolume/ using umount:
```
umount /mnt/luksdevolume/
```

Or fusermount:
```
fusermount -u /mnt/luksdevolume/
```

## Windows
At the moment terminate the process running in the console.

# Troubleshooting
First of all make sure to check the output of configure.
If you're seeing something like the following output configure was unable to detect an usable fuse.
```
Building:
   ...
   FUSE support:                                    no
```

On Mac OS X:
* make sure that you only have OSXFuse installed and not another variant, like MacFuse, besides it.
* try adding the C pre processor flags that set the fuse API version, e.g.

```
CPPFLAGS=-DFUSE_USE_VERSION=26 ./configure
```
* if all else fails; file a support issue and attach config.log

On Ubuntu:
```
fusermount – failed to open /etc/fuse.conf – Permission denied
```

Make sure you're part of the group fuse:
```
sudo addgroup <username> fuse
```

If fusermount keeps complaining it cannot open fuse.conf:
```
sudo chmod o+r /etc/fuse.conf
```

