

Data loss will be costly. At the very least, critical data loss will have a financial impact on companies of all sizes. In some cases, it can cost your job. I’ve seen cases where sysadmins learned this in the hard way.

There are several ways to backup a Linux system, including rsync and rsnapshot that we discussed a while back.

This article provides 6 practical examples on using dd command to backup the Linux system. dd is a powerful UNIX utility, which is used by the Linux kernel makefiles to make boot images. It can also be used to copy data. Only superuser can execute dd command.

Example 1. Backup Entire Harddisk

To backup an entire copy of a hard disk to another hard disk connected to the same system, execute the dd command as shown below. In this dd command example, the UNIX device name of the source hard disk is /dev/hda, and device name of the target hard disk is /dev/hdb.

```

# dd if=/dev/sda of=/dev/sdb

```

“if” represents inputfile, and “of” represents output file. So the exact copy of /dev/sda will be available in /dev/sdb.

If there are any errors, the above command will fail. If you give the parameter “conv=noerror” then it will continue to copy if there are read errors.

Input file and output file should be mentioned very carefully, if you mention source device in the target and vice versa, you might loss all your data.


In the copy of hard drive to hard drive using dd command given below, sync option allows you to copy everything using synchronized I/O.


```

# dd if=/dev/sda of=/dev/sdb conv=noerror,sync

```

Example 2. Create an Image of a Hard Disk


Instead of taking a backup of the hard disk, you can create an image file of the hard disk and save it in other storage devices.There are many advantages to backing up your data to a disk image, one being the ease of use. This method is typically faster than other types of backups, enabling you to quickly restore data following an unexpected catastrophe.


```

# dd if=/dev/hda of=~/hdadisk.img

```

The above creates the image of a harddisk /dev/hda. Refer our earlier article How to view initrd.image for more details.


Example 3. Restore using Hard Disk Image


To restore a hard disk with the image file of an another hard disk, use the following dd command example.


```

# dd if=hdadisk.img of=/dev/hdb


```

The image file hdadisk.img file, is the image of a /dev/hda, so the above command will restore the image of /dev/hda to /dev/hdb.


Example 5. Backup a Partition


You can use the device name of a partition in the input file, and in the output either you can specify your target path or image file as shown in the dd command example below.

```

# dd if=/dev/hda1 of=~/partition1.img

```

Example 6. CDROM Backup



dd command allows you to create an iso file from a source file. So we can insert the CD and enter dd command to create an iso file of a CD content.


```

# dd if=/dev/cdrom of=tgsservice.iso bs=2048

```


dd command reads one block of input and process it and writes it into an output file. You can specify the block size for input and output file. In the above dd command example, the parameter “bs” specifies the block size for the both the input and output file. So dd uses 2048bytes as a block size in the above command.


Note: If CD is auto mounted, before creating an iso image using dd command, its always good if you unmount the CD device to avoid any unnecessary access to the CD ROM.




To backup my Linux partitions, I combine dd and gzip, e.g. to back up my Ubuntu root partition which is on /dev/sda5:

```

dd if=/dev/sda5 bs=4096 | gzip -c > sda5-root.img.gz


```

Performance of the compression can be improved by creating & deleting a file from /dev/zero before doing the backup, e.g.

```

dd if=/dev/zero of=zero.bin bs=4096


```



