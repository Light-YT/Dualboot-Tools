# Partitioning

With the introduction of APFS in iOS 10.3, Apple made partitioning much easier. In contrast to HFS+, this uses something called a container. It is much more efficient, and there is no limit to the amount of partitions that can be created. And, unless specified, new volumes are dynamic and do not have a set size; rather they are allowed to share free space within the container itself.
In this case, we are just going to add new volumes to the container, /dev/disk0s1:                     

``newfs_apfs -A -v SystemB /dev/disk0s1``     

``newfs_apfs -A -v DataB /dev/disk0s1``                                                                                                                                                                                                                                                                                                                                                                         
                                                                                     
Done!

Important note: usually new partitions will be disk0s1s3 and disk0s1s4. However some newer devices already use these partitions so the new disks will be different, (for example, disk0s1s5 and disk0s1s6). Make sure to use your new partitions.

Mount new system partition (/dev/disk0s1s3, in this case) to /mnt1:

``mkdir /mnt1 /mnt2``

``mount_apfs /dev/disk0s1s3 /mnt1``

# Restoring RootFS
APFS changed the way Apple handles restoring root filesystem images. It is no longer possible to directly restore images to the target partition. Rather, asr is run in a ramdisk to create something called an inverted asr image, which the apfs_invert binary flashes to the new partition. Run asr on macOS to create an invertible asr image:

``asr -source rootfs.dmg -target out.dmg --embed -erase -noprompt --chunkchecksum --puppetstrings``

Make sure iproxy is running. This can also be done over network instead of iproxy, but it will be much slower:

``iproxy 2222 22``

Then copy the out file to / of new system partition using scp:

``scp -P 2222 out.dmg root@localhost:/mnt1``

Next part → [Making ramdisk](https://light-yt.github.io/modifying-filesystem-partition/)