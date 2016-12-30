#Download and install the latest Raspbian Jessie.

**1 - Download the latest version of Raspbian Jessie. [Direct torrent link](https://downloads.raspberrypi.org/raspbian_latest.torrent) or [Driect Download link](https://downloads.raspberrypi.org/raspbian_latest)**

**2 - Use [SD Card Formatter 4.0 for SD/SDHC/SDXC](https://www.sdcard.org/downloads/formatter_4/index.html) to format your MicroSD card.**

**3 - Use [Win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/) to transfer your .img file to your MicroSD card.**

**4 - Add a file called ssh in the root of your MicroSD card.**

**5 - Insert your MicroSD card in your Raspberry Pi and boot it up.**

**6 - Use [FING](https://play.google.com/store/apps/details?id=com.overlook.android.fing&hl=en_GB) to find your Raspberry Pi ipadress in your network.**

**7 - Login to your Raspberry Pi with [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)**
   * Logi as: pi
   * Username: raspberry

##Set up your Pi.##
~~~
sudo raspi-config
~~~

**1 - Expand Filesystem**

**2 - Change User Password**

**3 - Boot Options**
   * B1 Desktop Autologin
   * B3 Splash Screen - Disable

**4 - Internationalisation Options**
   * I1 Change Locale
   * I2 Change Timezone
   * I3 Change Keyboard Layout
   * I4 Change Wi-fi Country

**5 - Advanced Options**
   * A1 Overscan - No
   * A2 Hostname - SeedBox
   * A3 Memory Split - 16
   * A5 VNC - Yes

**6 - Finish - Reboot**
~~~
sudo reboot
~~~

###Reconnect to your Raspberry Pi with Putty###
   * Logi as: pi
   * Username: yournewpassword

**1 - Update your Pi.**
~~~
sudo apt-get update
~~~
~~~
sudo apt-get upgrade
~~~

**2 - Reboot**
~~~
sudo reboot
~~~
###Reconnect to your Raspberry Pi with Putty###
   * Logi as: pi
   * Username: yournewpassword

###Return to the [Main guide](https://github.com/mcfrojd/PiVPN-Seedbox) and proceed with step 2.
