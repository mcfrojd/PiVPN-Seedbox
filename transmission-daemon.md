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
~~~
sudo nano /var/lib/transmission-daemon/.config/transmission-daemon/settings.json
~~~
   * **ctrl-k to delete all original lines, the paste these settings instead**
~~~
{
    "alt-speed-down": 50,
    "alt-speed-enabled": false,
    "alt-speed-time-begin": 540,
    "alt-speed-time-day": 127,
    "alt-speed-time-enabled": false,
    "alt-speed-time-end": 1020,
    "alt-speed-up": 50,
    "bind-address-ipv4": "0.0.0.0",
    "bind-address-ipv6": "::",
    "blocklist-enabled": false,
    "blocklist-url": "http://www.example.com/blocklist",
    "cache-size-mb": 4,
    "dht-enabled": true,
    "download-dir": "/media/pi/SeedBox/complete",
    "download-limit": 100,
    "download-limit-enabled": 0,
    "download-queue-enabled": true,
    "download-queue-size": 5,
    "encryption": 1,
    "idle-seeding-limit": 30,
    "idle-seeding-limit-enabled": false,
    "incomplete-dir": "/media/pi/SeedBox/incomplete",
    "incomplete-dir-enabled": true,
    "lpd-enabled": false,
    "max-peers-global": 200,
    "message-level": 1,
    "peer-congestion-algorithm": "",
    "peer-id-ttl-hours": 6,
    "peer-limit-global": 200,
    "peer-limit-per-torrent": 50,
    "peer-port": 51413,
    "peer-port-random-high": 65535,
    "peer-port-random-low": 49152,
    "peer-port-random-on-start": false,
    "peer-socket-tos": "default",
    "pex-enabled": true,
    "port-forwarding-enabled": true,
    "preallocation": 1,
    "prefetch-enabled": 1,
    "queue-stalled-enabled": true,
    "queue-stalled-minutes": 30,
    "ratio-limit": 2,
    "ratio-limit-enabled": false,
    "rename-partial-files": true,
    "rpc-authentication-required": false,
    "rpc-bind-address": "0.0.0.0",
    "rpc-enabled": true,
    "rpc-password": "",
    "rpc-port": 9091,
    "rpc-url": "/transmission/",
    "rpc-username": "",
    "rpc-whitelist": "127.0.0.1",
    "rpc-whitelist-enabled": false,
    "scrape-paused-torrents-enabled": true,
    "script-torrent-done-enabled": false,
    "script-torrent-done-filename": "",
    "seed-queue-enabled": false,
    "seed-queue-size": 10,
    "speed-limit-down": 100,
    "speed-limit-down-enabled": false,
    "speed-limit-up": 100,
    "speed-limit-up-enabled": false,
    "start-added-torrents": true,
    "trash-original-torrent-files": true,
    "umask": 2,
    "upload-limit": 100,
    "upload-limit-enabled": 0,
    "upload-slots-per-torrent": 14,
    "utp-enabled": true,
    "watch-dir": "/media/pi/SeedBox/autostart", 
    "watch-dir-enabled": true
}
~~~
   * Close and save with **ctrl-x** - **Y** - **enter**

**Start the transmission-daemon**
~~~
sudo /etc/init.d/transmission-daemon start
~~~

**Test transmission from your browser on your computer http://ipnumber.to.your.pi:9091**

**If you see the webinterface of transmission now the install worked**

~~~
sudo nano /etc/init.d/transmission-daemon-reload
~~~
   * Paste the following into that file:
~~~
#!/bin/sh
### BEGIN INIT INFO
# Provides: transmission-daemon-reload
# Required-Start: $all
# Required-Stop: 
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Reload the transmission-daemon
# Description: Reload the transmission-daemon at boot.
### END INIT INFO

NAME=transmission-daemon-reload

sleep 10
service transmission-daemon reload
~~~
   * Close and save with **ctrl-x** - **Y** - **enter**

**Change permissions to the file so it can be executed**
~~~
sudo chmod 755 /etc/init.d/transmission-daemon-reload
~~~

**Make the script run at startup**
~~~
sudo update-rc.d transmission-daemon-reload defaults
~~~

**Reboot the pi and check if transmission starts automaticly**

###Return to the [Main guide](https://github.com/mcfrojd/PiVPN-Seedbox) and proceed with step 5.###
