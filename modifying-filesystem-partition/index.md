# Modifying filesystem
Mount System partition:

``mount_apfs /dev/disk0s1s3 /mnt1``

And modify fstab using nano:

``cp -a /etc/fstab /mnt1/etc/fstab``

``nano /mnt1/etc/fstab``

![Screenshot](https://github.com/Light-YT/light-yt.github.io/blob/master/assets/screen7.png?raw=true)



Change disk0s1s1 to disk0s1s3 and disk0s1s2 to disk0s1s4. (or the new system and data partitions disks based on the device.)

Press CTRL-O to save and CTRL-X to exit.

Now we have to add SEP to the second system:

``cp -a /usr/standalone/firmware/sep* /mnt1/usr/standalone/firmware/``

Also add the baseband firmware:

``cp -av /usr/local /mnt1/usr/local``

The baseband is located inside of /usr/local, and the firmware names vary across devices. We can just copy it since the /usr/local path does not exist on the new system.

# Modifying data partition

Now we have to move /private/var to new Data partition:

``mount_apfs /dev/disk0s1s4 /mnt2

``mv -v /mnt1/private/var/* /mnt2``

Next part →[ no-effaceable-storage](https://light-yt.github.io/no-effaceable-storage)
