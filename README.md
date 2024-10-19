# Future Proof PC Console Backups

### Be sure to read the other guides named fp-pc-console-*


## General Details

This is a guide on how I plan to backup up and recover a Linux system.  The goal is to be able to restore a system 20yrs from now, so this needs to be something that doesn't rely on todays online apt repo's.  So I'll have to do a full backup of the root partition, which also contains the home directory, which I will install all flatpak's to using --user.  This makes it easier to do home directory file backups.  

For file backups, I will backup everything inside of the home directory, which contains user installed flatpaks and config files.

Full efi partition dd backup.  This probably isn't even necissary, but I'll do it since it's only 1gb of space.
I don't fully understand this partition, so I want to give a little breathing room.  I usually just create 1GB of space in the front of the HD, and have half of that 1gb be empty space, and the last half be the efi partition.

Backup these live tools:
- System Rescue CD for future use
- https://www.system-rescue.org/Download/
- Debian 12 install DVD
- Debian 12 live CD/DVD/USB

Be sure and backup this doc to the blu-ray when complete.

Backup locations:
- spinning giant HD for storing in a closet for 5yrs
- 25gb Blu-ray for storing in a closet forever, basically.

example commands:
```
flatpak install flathub org.DolphinEmu.dolphin-emu --user
```
### dd

I'm using status=progress instead of installing a different dd package that shows progress.  I'm using bs=64k, because I feel like I get less problems with that.  Before using dd to do a backup, I'll need to use gparted to shrink the root partition down to a size that will fit on a 25GB Blu-ray.  Keep in mind that gparted uses Gib instead of GB, so make the total be a little less than 23GB since 25gb is equal to 23.2831Gib.

Computers:
- NUC i5 NUC 2015
- NUC i7 NUC 2015
- AMD big computer with 6600 vid card

I can for sure get the same backup from the Intel NUC's, but it may be possible to even have the same for my AMD machine as well.  But I'll probably just do separate backups for the 2 different kinds of computers, just to be on the safe side.

## Backup Steps
Partition your system like this:
- about 500mb of free space
- about 500mb of efi (depending on the debian 12 install disk you use, this could be fat32 efi or /boot/efi)
- about 30gb of /root

Install flatpak, vim and gparted with apt, and other needed software.

Use --user to install all flatpaks to your home directory.

Get the system working exactly how you like it

Backup the contents of the home directory.  You can use the Debian 12 Live CD/DVD/USB to do this, or if your system boots up fine just run it to do this step.  You may want to compress them, but I haven't done that yet.

Shrink the /root partition with gparted on the Debian 12 Live CD/DVD/USB.

Backup the /root partition with dd, to a giant spinning hard drive.

Name that dd /root backup something like fp-pc-console-root-2024.iso

example dd command for that.
~~~
sudo dd status=progress bs=64k if=/dev/sdSRC of=/wherever/iso.iso
~~~

Backup your efi partition the same way as you just did the /root, even though this may not be nessisary.  I suppose this could restore the systems specific grub boot, but I'm not sure yet.  If this doesn't work out, it's not a huge deal.  The recover steps below will show how to do this with a proper efi partition backup or without.

Both of these /root and efi paritions should fit on a 25gb blu-ray.  Also put it on a giant spinning disk backup drive.  You can optionaly make another blu-ray backup and send it to someone elses house for storage.

Be sure to also include any helpful documents on the blu-ray backup, like this document.



## Restore Steps

The goal is to have the same partition setup as described above, but they have to be exactly the same amout as before, or at least slightly bigger.  For dd, I like to use the Debian Live CD/DVD/USB installer, but it doesn't contain gparted on the disk, so if you do a system restore so far into the future, that the debian 12 apt repo's don't exist, you can use the System Rescue Live CD/DVD/USB.  I may need to learn more about mounting partitions in the System Rescue and update this document since it doesn't seem to do that automatically like the Debian 12 Live installer CD/DVD/USB.

So use which ever Live CD/DVD/USB you want, and use gparted to create those partitions that are equal or slightly larger than the partitions listed above, in the Backup section of this doc.  Then use dd to restore each partition separately.  Here is an example. 
~~~
sudo dd status=progress bs=64k if=/wherever/iso.iso of=/dev/sdDEST
~~~

Optionally unmout/eject your big spinning disk just to be safe.
Use gparted, and it will tell you to fix some things on your newly restored drive, just agree/fix/yes to that.  Also use gparted to expand that /root to it's maximum size possible.

Reboot

If your newly restored system can't boot, you'll need to use System Rescue Live CD/DVD/USB to fix the efi /boot partition.  When System Restore first boots, you'll see the option to boot an OS on this drive.  Do that.  Now it is temperarilly booted, but you need to fix grub.  Here is an example of that what may look like:

~~~
grub-install /dev/sdROOOT
~~~

I'm pretty sure the above grub-install is needing the /root partiton, but I will check.

Now everything should boot normally and you can shutdown the system, remove all Live CD/DVD/USB's.  

### Be sure to read the other guides named fp-pc-console-*
