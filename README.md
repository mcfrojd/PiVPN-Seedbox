#PiVPN-Seedbox#

##The hardware and software used in this guide is:##
   * Raspberry Pi 3
   * MicroSD: SanDisk Ultra MicroSDHC UHS-I 8gb 
   * Raspbian Jessie with Pixel. Version: November 2016
   * USB Drive: OCZ Vertex 2 Sata II 2.5" SSD drive in a portable USB enclosure (no extra power needed)

##Lets start the guide##

**1 - Latest raspberry Jessie. Local settings + update & upgrade.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_1_LatestRaspbianJessie.md)

**2 - Prepare usb disk / drive and mount it in your Raspberry Pi.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_2_MountUSBDrive.md)

**3 - Samba sharing of the mounted seedbox folder "SeedBox"**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_3_share_folders_with_samba.md)

**4 - Install Transmission and all settings.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_4_transmission-daemon.md)

**5 - Install OpenVPN and add the settings for Private Internet Access. Check ip to verify.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_5_raspberry-pi-vpn-router.md)

**6 - Open transmission port throu VPN**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_6_open_port_on_vpn.md)

**7 - DuckDNS -  to get ip to seedbox?**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/Part_7_DuckDNS.md)

#Sources#
###These sources have been much help creating this guide###

**For Transmission & USB mounting** -  [Raspberry Pi Seedbox (w/ Transmission WebUI) | Linux Tutorial](https://www.youtube.com/watch?v=flhGmgbAqZA&t=346s)

**For Samba** - [Getting Started with Home Assistant, again!] (https://youtu.be/G8XWsXlfGFQ?t=555)

**For VPN portforwaring** -  [Port Forwarding Without The Application (Advanced Users)](https://www.privateinternetaccess.com/forum/discussion/180/port-forwarding-without-the-application-advanced-users/p13)

**For VPN setup (PIA)** - [Raspberry Pi VPN Router](https://gist.github.com/superjamie/ac55b6d2c080582a3e64)

**For Transmission Blocklist** - [Add Transmission peer Blocklist to guide](https://github.com/drduh/macOS-Security-and-Privacy-Guide/issues/91)

**For DuckDns** - [Getting Started with Home Assistant, again!](https://youtu.be/G8XWsXlfGFQ?t=761)

**For DNS Leak** - [HOW TO FIX OPENVPN DNS LEAK IN LINUX](http://www.ubuntubuzz.com/2015/09/how-to-fix-openvpn-dns-leak-in-linux.html)

**For open port to GUI** - ["STILL LOOKING"](http://#)

---
**Sites to check out**
http://www.htpcguides.com/remote-access-transmission-torrent-behind-vpn-linux/

http://www.htpcguides.com/install-sonarr-raspberry-pi-2-latest-stable-mono/


