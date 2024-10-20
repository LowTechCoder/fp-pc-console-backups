# Future Proof PC Console - Backups

### Be sure to read the other guides named fp-pc-console-*


## General Details

This is a guide on how I plan to backup up and recover a Linux system.  The goal is to be able to restore a system 20yrs from now, so this needs to be something that doesn't rely on todays online apt repo's.  So I'll have to do a full backup of the root partition, which also contains the home directory, which I will install all flatpak's to using --user.  This makes it easier to do home directory file backups.  

For file backups, I will backup everything inside of the home directory, which contains user installed flatpaks and config files.

Full efi partition dd backup.  I usually just create 1GB of space in the front of the HD, and have half of that 1gb be empty space, and the last half be the efi partition.

Backup these live tools:
- System Rescue CD for future use
- https://www.system-rescue.org/Download/
- Debian 12 install DVD
- Debian 12 live CD/DVD/USB

Be sure and backup this doc to the blu-ray when complete.

Backup locations:
- spinning giant HD for storing in a closet for 5yrs
- 25gb Blu-ray for storing in a closet forever, basically.

### dd

I'm using status=progress instead of installing a different dd package that shows progress.  I'm using bs=64k, because I feel like I get less problems with that.  Before using dd to do a backup, I'll need to use gparted to shrink the root partition down to a size that will fit on a 25GB Blu-ray.  Keep in mind that gparted uses Gib instead of GB, so make the total be a little less than 23GB since 25gb is equal to 23.2831Gib.

Computers:
- NUC i5 NUC 2015
- NUC i7 NUC 2015
- AMD big computer with 6600 vid card

It does seem like I can just use the same backup for all systems.  No need to do separate backups for Intel and AMD separately, as far as I can tell.

## Backup Steps
Partition your system like this.  It should just be automatic, but make sure efa is sda1 and root is sda2 (the important part is the 1 and 2 is in order.)
- empty space in the beginning of around 349Mib or less
- efa partition: 651Mib (depending on the debian 12 install disk you use, this could be fat32 efi or /boot/efi)
- root partition: 30GiB or more (you'll shrink this down later)

Install flatpak, vim and gparted with apt, and other needed software.

Use --user to install all flatpaks to your home directory.

Get the system working exactly how you like it

Backup the contents of the home directory.  You can use the Debian 12 Live CD/DVD/USB to do this, or if your system boots up fine just run it to do this step.  You may want to compress them, but I haven't done that yet. If it asks you to skip things like symlinks, or what ever else it gets stuck on, that should be fine.

Shrink the /root partition with gparted on the Debian 12 Live CD/DVD/USB less than 25Gb (23Gib) so it can fit on a 25GB blu-ray.  Actually a little less would be better if possible so you can fit both partitions on the same blu-ray.

Backup the /root partition with dd, to a giant spinning hard drive.

Name that dd /root backup something like fp-pc-console-root-2024.iso

example dd command for that.
~~~
sudo dd status=progress bs=64k if=/dev/sdSRC of=/wherever/iso.iso
~~~

Backup your efi partition the same way as you just did the /root, but don't shrink it.  It's small enough.

Both of these /root and efi paritions should fit on a 25gb blu-ray.  Also put it on a giant spinning disk backup drive.  You can optionaly make another blu-ray backup and send it to someone elses house for storage.

Be sure to also include any helpful documents on the blu-ray backup, like this document.



## Restore Steps

The goal is to have the same partition setup as described above, but they have to be exactly the same amout as before, or at least bigger.  For dd, I like to use the Debian Live CD/DVD/USB installer since it auto mounts partitions for you, but you can do all this on the System Rescue Live disk if you know the mount commands, which I will go over below.

Currently using partitions with these minimum sizes:
empty space in the beginning of around 349Mib or less
efa partition: 651Mib 
root partition: 18.8 but feel free to use the entire space left

The Debian 12 Live CD/DVD/USB is much more user friendly at some things, than System Rescue Live CD/DVD/USB.  Here is what I recommend using during each of these tasks, but you can just use System Rescue for most of this instead.
- Installation: Debian
- dd: Debian
- Gparted: System Rescue Live
- Grub/efi repair: System Rescue Live

So use which ever Live CD/DVD/USB you want, and use gparted to create those partitions that are equal or slightly larger than the partitions listed above, in the Backup section of this doc.  Then use dd to restore each partition separately.  Here is an example. Be sure to create both the efi and root partitiion in that order or the labels will be in reverse and annoy you forever (sda1 and sda2 will be reversed).
~~~
#show disks or you can use gparted to see them
fdisk -l
#or
fdisk -l | grep dev
#or
lsblk
#you may need to mount a partition
mount /dev/sdPart /mnt
~~~
~~~
sudo dd status=progress bs=64k if=/wherever/iso.iso of=/dev/sdaPART
~~~

Optionally unmout/eject your big spinning disk just to be safe.
Use gparted, and it will tell you to fix some things on your newly restored drive, just agree/fix/yes to that.  Also use gparted to expand that /root to it's maximum size possible.  Also if you see any exclamations beside a partition in gparted, right click that partition and choose to 'check'.  Then proceed by clicking the green check.  That should fix all issues it finds.

Reboot

If your newly restored system can't boot, you'll need to use System Rescue Live CD/DVD/USB to fix the efi /boot partition.  When System Restore first boots, you'll see the option to "Boot a Linux OS installed on the disk".  Do that.  Now it is temperarilly booted, but you need to fix grub.  Here is an example of that what may look like:

~~~
#show disks or you can use gparted to see them
fdisk -l
#or
fdisk -l | grep '/dev/'
#or
lsblk
#install grub to the drive, NOT the partition in the drive.  So most likely sda.
grub-install /dev/sdDRIVE
~~~


Now everything should boot normally and you can shutdown the system, remove all Live CD/DVD/USB's.  

### Be sure to read the other guides named fp-pc-console-*
