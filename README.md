# PiVPN-Seedbox

---

1. Latest raspberry Jessie. Local settings + update & upgrade.
2. Mount usb disk / drive. Format drive "SeedBox", Make folder Torrents in the root and Complete, Incomplete & Autostart inside Torrents folder. chmod the folders.
3. Samba sharing of the mounted seedbox folder "Torrents"
4. Install OpenVPN and add the settings for Private Internet Access. Check ip to verify.
5. install Transmission and all settings.
6. Open transmission port throu VPN
7. Autostart and restart to make trasmission start and keep active.
8. DynDNS to get ip to seedbox?

---

Montera usb disken först, skapa katalogerna 'autostart' 'complete' 'incomplete'

Installera samba innan openvpn

1. Följ guiden [raspberry-pi-vpn-router.md](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/raspberry-pi-vpn-router.md) för att sätta upp en fungerande VPN anslutning till PrivateInternetAccess (PIA)

2. Följ guiden [Raspberry Pi Seedbox (w/ Transmission WebUI) | Linux Tutorial](https://www.youtube.com/watch?v=flhGmgbAqZA&t=346s) för att sätta upp Transmission

3. Följ guiden [open_port_on_vpn.md](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/open_port_on_vpn) för att öppna en port på VPN anslutnigen och rapportera denna till transmission.

4. Följ guiden [How2SetUp a Raspberry Pi Windows NAS storage server](http://www.simonthepiman.com/how_to_setup_windows_file_server.php) från punkt 6 för att dela ut mappar med samba

Källor

https://www.privateinternetaccess.com/forum/discussion/180/port-forwarding-without-the-application-advanced-users/p13

