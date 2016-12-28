# Raspberry Pi VPN Router

This is a quick-and-dirty guide to setting up a Raspberry Pi as a "[router on a stick](https://en.wikipedia.org/wiki/One-armed_router)" to [PrivateInternetAccess](http://privateinternetaccess.com/) VPN.

## Requirements

Install Raspbian Jessie (`2016-05-27-raspbian-jessie.img`) to your Pi's sdcard.

Use the **Raspberry Pi Configuration** tool or `sudo raspi-config` to:

* Expand the root filesystem and reboot
* Boot to commandline, not to GUI
* Configure the right keyboard map and timezone
* Configure the Memory Split to give 16Mb (the minimum) to the GPU
* Consider overclocking to the Medium (900MHz) setting on Pi 1, or High (1000MHz) setting on Pi 2

## IP Addressing

My home network is setup as follows:

* Internet Router: `192.168.1.1`
* Subnet Mask: `255.255.255.0`
* Router gives out DHCP range: `192.168.100-200`

If your network range is different, that's fine, use your network range instead of mine.

I'm going to give my Raspberry Pi a static IP address of `192.168.1.2` by configuring `/etc/network/interfaces` like so:

~~~
auto lo
iface lo inet loopback

auto eth0
allow-hotplug eth0
iface eth0 inet static
    address 192.168.1.2
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
~~~

You can use WiFi if you like, there are plenty tutorials around the internet for setting that up, but this should do:

~~~
auto lo
iface lo inet loopback

auto eth0
allow-hotplug eth0
iface eth0 inet manual

auto wlan0
allow-hotplug wlan0
iface wlan0 inet static
    wpa-ssid "Your SSID"
    wpa-psk  "Your Password"
    address 192.168.1.2
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
~~~

You only need one connection into your local network, don't connect both Ethernet and WiFi. I recommend Ethernet if possible.

## NTP

Accurate time is important for the VPN encryption to work. If the VPN client's clock is too far off, the VPN server will reject the client.

You shouldn't have to do anything to set this up, the `ntp` service is installed and enabled by default.

Double-check your Pi is getting the correct time from internet time servers with `ntpq -p`, you should see at least one peer with a `+` or a `*` or an `o`, for example:

~~~
$ ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
-0.time.xxxx.com 104.21.137.30    2 u   47   64    3  240.416    0.366   0.239
+node01.jp.xxxxx 226.252.532.9    2 u   39   64    7  241.030   -3.071   0.852
*t.time.xxxx.net 104.1.306.769    2 u   38   64    7  127.126   -2.728   0.514
+node02.jp.xxxxx 250.9.592.830    2 u    8   64   17  241.212   -4.784   1.398
~~~

## Setup VPN Client

Install the OpenVPN client:

~~~
sudo apt-get install openvpn
~~~

Download and uncompress the PIA OpenVPN profiles:

~~~
wget https://www.privateinternetaccess.com/openvpn/openvpn.zip
sudo apt-get install unzip
unzip openvpn.zip -d openvpn
~~~

Copy the PIA OpenVPN certificates and profile to the OpenVPN client:

~~~
sudo cp openvpn/ca.rsa.2048.crt openvpn/crl.rsa.2048.pem /etc/openvpn/
sudo cp openvpn/Japan.ovpn /etc/openvpn/Japan.conf
~~~

You can use a diffrent VPN endpoint if you like. Note the extension change from **ovpn** to **conf**.

Create `/etc/openvpn/login` containing only your username and password, one per line, for example:

~~~
user12345678
MyGreatPassword
~~~

Change the permissions on this file so only the root user can read it:

~~~
sudo chmod 600 /etc/openvpn/login
~~~

Setup OpenVPN to use your stored username and password by editing the the config file for the VPN endpoint:

~~~
sudo nano /etc/openvpn/Japan.conf
~~~

Change the following lines so they go from this:

~~~
ca ca.rsa.2048.crt
auth-user-pass
crl-verify crl.rsa.2048.pem
~~~

To this:

~~~
ca /etc/openvpn/ca.rsa.2048.crt
auth-user-pass /etc/openvpn/login
crl-verify /etc/openvpn/crl.rsa.2048.pem
~~~

## Test VPN

At this point you should be able to test the VPN actually works:

~~~
sudo openvpn --config /etc/openvpn/Japan.conf
~~~

If all is well, you'll see something like:

~~~
$ sudo openvpn --config /etc/openvpn/Japan.conf 
Sat Oct 24 12:10:54 2015 OpenVPN 2.3.4 arm-unknown-linux-gnueabihf [SSL (OpenSSL)] [LZO] [EPOLL] [PKCS11] [MH] [IPv6] built on Dec  5 2014
Sat Oct 24 12:10:54 2015 library versions: OpenSSL 1.0.1k 8 Jan 2015, LZO 2.08
Sat Oct 24 12:10:54 2015 UDPv4 link local: [undef]
Sat Oct 24 12:10:54 2015 UDPv4 link remote: [AF_INET]123.123.123.123:1194
Sat Oct 24 12:10:54 2015 WARNING: this configuration may cache passwords in memory -- use the auth-nocache option to prevent this
Sat Oct 24 12:10:56 2015 [Private Internet Access] Peer Connection Initiated with [AF_INET]123.123.123.123:1194
Sat Oct 24 12:10:58 2015 TUN/TAP device tun0 opened
Sat Oct 24 12:10:58 2015 do_ifconfig, tt->ipv6=0, tt->did_ifconfig_ipv6_setup=0
Sat Oct 24 12:10:58 2015 /sbin/ip link set dev tun0 up mtu 1500
Sat Oct 24 12:10:58 2015 /sbin/ip addr add dev tun0 local 10.10.10.6 peer 10.10.10.5
Sat Oct 24 12:10:59 2015 Initialization Sequence Completed
~~~

Exit this with **Ctrl+c**

## Enable VPN at boot

~~~
sudo systemctl enable openvpn@Japan
~~~

## Setup Routing and NAT

Enable IP Forwarding:

~~~
echo -e '\n#Enable IP Routing\nnet.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
~~~

Setup NAT fron the local LAN down the VPN tunnel:

~~~
sudo iptables -t nat -A POSTROUTING -o tun0 -j MASQUERADE
sudo iptables -A FORWARD -i tun0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o tun0 -j ACCEPT
~~~

Make the NAT rules persistent across reboot:

~~~
sudo apt-get install iptables-persistent
~~~

The installer will ask if you want to save current rules, select **Yes**

If you don't select yes, that's fine, you can save the rules later with `sudo netfilter-persistent save`

Make the rules apply at startup:

~~~
sudo systemctl enable netfilter-persistent
~~~

## VPN Kill Switch

This will block outbound traffic from the Pi so that only the VPN and related services are allowed.

Once this is done, the only way the Pi can get to the internet is over the VPN.

This means if the VPN goes down, your traffic will just stop working, rather than end up routing over your regular internet connection where it could become visible.

~~~
sudo iptables -A OUTPUT -o tun0 -m comment --comment "vpn" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -p icmp -m comment --comment "icmp" -j ACCEPT
sudo iptables -A OUTPUT -d 192.168.1.0/24 -o eth0 -m comment --comment "lan" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -p udp -m udp --dport 1198 -m comment --comment "openvpn" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -p tcp -m tcp --sport 22 -m comment --comment "ssh" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -p udp -m udp --dport 123 -m comment --comment "ntp" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -p udp -m udp --dport 53 -m comment --comment "dns" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -p tcp -m tcp --dport 53 -m comment --comment "dns" -j ACCEPT
sudo iptables -A OUTPUT -o eth0 -j DROP
~~~

And save so they apply at reboot:

~~~
sudo netfilter-persistent save
~~~

If you find traffic on your other systems stops, then look on the Pi to see if the VPN is up or not.

You can check the status and logs of the VPN client with:

~~~
sudo systemctl status openvpn@Japan
sudo journalctl -u openvpn@Japan
~~~

## Configure Other Systems on the LAN

Now we're ready to tell other systems to send their traffic through the Raspberry Pi.

Configure other systems' network so they are like:

* Default Gateway: Pi's static IP address (eg: `192.168.1.2`)
* DNS: Something public like Google DNS (`8.8.8.8` and `8.8.4.4`)

Don't use your existing internet router (eg: `192.168.1.1`) as DNS, or your DNS queries will be visible to your ISP and hence may be visible to organizations who wish to see your internet traffic.

## Optional: DNS on the Pi

To ensure all your DNS goes through the VPN, you could install `dnsmasq` on the Pi to accept DNS requests from the local LAN and forward requests to external DNS servers.

~~~
sudo apt-get install dnsmasq
~~~

You may now configure the other systems on the LAN to use the Pi (`192.168.1.2`) as their DNS server as well as their gateway.
