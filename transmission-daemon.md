#Step 4 - Install and configure the transmission-daemon.#

**1 - Install transmission-daemon**
~~~
sudo apt install transmission-daemon
~~~
~~~
sudo nano /etc/init.d/transmission-daemon
~~~
   * Change USER from
~~~
USER=debian-transmission
~~~
   * to
~~~
USER=pi
~~~
   * Close and save with **ctrl-x** - **Y** - **enter**





**2 - Use [SD Card Formatter 4.0 for SD/SDHC/SDXC](https://www.sdcard.org/downloads/formatter_4/index.html) to format your MicroSD card.**

**7 - Login to your Raspberry Pi with [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)**
   * Logi as: pi
   * Username: raspberry

##Set up your Pi.##
~~~
sudo raspi-config
~~~

###Return to the [Main guide](https://github.com/mcfrojd/PiVPN-Seedbox) and proceed with step 5.###
