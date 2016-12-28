# PiVPN-Seedbox

Montera usb disken först, skapa katalogerna 'autostart' 'complete' 'incomplete'
Installera samba innan openvpn

1. Följ guiden [raspberry-pi-vpn-router.md](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/raspberry-pi-vpn-router.md) för att sätta upp en fungerande VPN anslutning till PrivateInternetAccess (PIA)

2. Följ guiden [Raspberry Pi Seedbox (w/ Transmission WebUI) | Linux Tutorial](https://www.youtube.com/watch?v=flhGmgbAqZA&t=346s) för att sätta upp Transmission

3. Följ guiden [open_port_on_vpn.md](https://github.com/mcfrojd/PiVPN-Seedbox/blob/master/open_port_on_vpn) för att öppna en port på VPN anslutnigen och rapportera denna till transmission.

4. Följ guiden [How2SetUp a Raspberry Pi Windows NAS storage server](http://www.simonthepiman.com/how_to_setup_windows_file_server.php) från punkt 6 för att dela ut mappar med samba
