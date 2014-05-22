To Create a Whole Backup of a Drive 

```

dd if=/dev/vda | ssh babinlonston@192.168.1.100 'gzip - > /home/babinlonston/Desktop/backup.gz'

```

To restore, you have to take the server down and manually image the disk. Perhaps a hard drive swap or something of the sort.
copy data over with a filemanager from a live CD.. grsync is an easy GUI for using rsync

To place the image on the 
[new] drive:

```

gzip -d < image.gz | dd of=/dev/sda2


```

well to recover, you cannot just do it "live", ie, while the system is running off of that hard disk. You would need to either to boot from a live medium (cd/etc) and do the disk image there, or perhaps pull out the hard drives and put them in another computer.