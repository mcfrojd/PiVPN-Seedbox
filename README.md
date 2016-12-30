#PiVPN-Seedbox#

##The hardware and software used in this guide is:##
   * Raspberry Pi 3
   * MicroSD: SanDisk Ultra MicroSDHC UHS-I 8gb 
   * Raspbian Jessie with Pixel. Version: November 2016
   * USB Drive: OCZ Vertex 2 Sata II 2.5" SSD drive in a portable USB enclosure (no extra power needed)

##Lets start the guide##

**1 - Latest raspberry Jessie. Local settings + update & upgrade.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/LatestRaspbianJessie.md)

**2 - Prepare usb disk / drive and mount it in your Raspberry Pi.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/MountUSBDrive.md)

**3 - Samba sharing of the mounted seedbox folder "SeedBox"**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/share_folders_with_samba.md)

**4 - Install OpenVPN and add the settings for Private Internet Access. Check ip to verify.**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/raspberry-pi-vpn-router.md)

**5 - install Transmission and all settings.**
   * [Link to guide]()

**6 - Open transmission port throu VPN**
   * [Link to guide](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/open_port_on_vpn.md)

**7 - Autostart and restart to make trasmission start and keep active.**
   * [Link to guide]()

**8 - DynDNS to get ip to seedbox?**
   * [Link to guide]()

##Sources##

**1 - Följ guiden [raspberry-pi-vpn-router.md](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/raspberry-pi-vpn-router.md) för att sätta upp en fungerande VPN anslutning till PrivateInternetAccess (PIA)**

**2 - Följ guiden [Raspberry Pi Seedbox (w/ Transmission WebUI) | Linux Tutorial](https://www.youtube.com/watch?v=flhGmgbAqZA&t=346s) för att sätta upp Transmission**

**3 - Följ guiden [open_port_on_vpn.md](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/open_port_on_vpn) för att öppna en port på VPN anslutnigen och rapportera denna till transmission.**

**4 - Följ guiden [How2SetUp a Raspberry Pi Windows NAS storage server](http://www.simonthepiman.com/how_to_setup_windows_file_server.php) från punkt 6 för att dela ut mappar med samba**

Källor

https://www.privateinternetaccess.com/forum/discussion/180/port-forwarding-without-the-application-advanced-users/p13

