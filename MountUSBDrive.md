#Step 2 - Mount the USB Drive / Disk#

**1 - Install ntfs drivers**
~~~
sudo apt-get install ntfs-3g
~~~
   * Then you will need to power cycle the Raspberry Pi. I'm not yet sure why there is a difference between a reboot and a power cycle, but you must power cycle the Raspberry Pi. That means turn it off, wait 5+ seconds, then turn it back on.

**2 - Format USB Drive / Disk to ntfs format on your computer**
   * Name the disk / drive "SeedBox"
   
**3 - Insert the drive /disk in your Raspberry Pi**
~~~
lsblk
~~~
   * Check for the mountpoint that matches your disk / drive (usually "sda1")
   * Notice that the drive might be automounted at /media/pi/SeedBox, if so unmount first with `sudo umount /media/pi/SeedBox`

~~~
sudo mkdir /mnt/SeedBox
~~~
~~~
sudo mount /dev/sda1 /mnt/SeedBox
~~~
~~~
sudo nano /etc/fstab
~~~
~~~
/dev/sda1       /mnt/SeedBox    ntfs    defaults        0       0
~~~
   * Observe, it's not spaces, it's TAB between the settings above
   * Close and save with **ctrl-x** - **Y** - **enter**

**4 - Create the folders for Transmission**
~~~
sudo mkdir /media/pi/SeedBox/complete
~~~
~~~
sudo mkdir /media/pi/SeedBox/incomplete
~~~
~~~
sudo mkdir /media/pi/SeedBox/autostart
~~~
**5 - Set Permissions on the folders** *(check if this step is nessecery)*
~~~
sudo chmod 744 /media/pi/SeedBox/complete
~~~
~~~
sudo chmod 744 /media/pi/SeedBox/incomplete
~~~
~~~
sudo chmod 744 /media/pi/SeedBox/autostart
~~~

###Return to the [Main guide](https://github.com/mcfrojd/PiVPN-Seedbox) and proceed with step 3.###
