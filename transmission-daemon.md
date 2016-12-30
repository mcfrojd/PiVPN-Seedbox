#Step 5 - Install and configure the transmission-daemon.#

**1 - Install transmission-daemon**
~~~
sudo apt install transmission-daemon
~~~

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
