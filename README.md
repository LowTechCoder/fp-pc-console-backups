# linux-os-backups

This is a guide on how I plan to backup up and recover a Linux system.  The goal is to be able to restore a system 20yrs from now, so this needs to be something that doesn't rely on todays online apt repo's.  So I'll have to do a full backup of the root partition, which also contains the home directory, which I will install all flatpak's to using --user.  This makes it easier to do home directory file backups.

Backup locations:
-spinning giant HD for storing in a closet for 5yrs
-25gb Blu-ray for storing in a closet forever, basically.

example commands:
```
sudo dd status=progress bs=64k if=/dev/sdSRC of=/wherever/iso.iso
flatpak install flathub org.DolphinEmu.dolphin-emu --user
```
### dd

I'm using status=progress instead of installing a different dd package that shows progress.  I'm using bs=64k, because I feel like I get less problems with that.  Before using dd to do a backup, I'll need to use gparted to shrink the root partition down to a size that will fit on a 25GB Blu-ray.  Keep in mind that gparted uses Gib instead of GB, so make the total be a little less than 23GB since 25gb is equal to 23.2831Gib.

Computers:
```
NUC i5
NUC i7
AMD big computer with 6600 vid card
```

I can for sure get the same backup from the Intel NUC's, but it may be possible to even have the same for my AMD machine as well.  But I'll probably just do separate backups for the 2 different kinds of computers, just to be on the safe side.

### Other considerations

Backup types

Home directory files backup.  Be sure to include all dot files.

Full root partition dd backup

Full efi partition dd backup.  This probably isn't even necissary, but I'll do it since it's only 1gb of space.
I don't fully understand this partition, so I want to give a little breathing room.  I usually just create 1GB of space in the front of the HD, and have half of that 1gb be empty space, and the last half be the efi partition.

Be sure and backup this doc to the blu-ray when complete.

### Other tools to backup
~~~
System Rescue CD
https://www.system-rescue.org/Download/
#### example grub repair when using System Rescue
grub-install /dev/sdb
burn system rescue with backups?

Debian 12 install DVD
Debian 12 live CD/DVD
~~~





