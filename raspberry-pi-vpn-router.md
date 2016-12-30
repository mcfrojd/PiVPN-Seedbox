# Raspberry Pi VPN Router

** Setup VPN Client

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
sudo cp openvpn/Sweden.ovpn /etc/openvpn/Sweden.conf
~~~

You can use a diffrent VPN endpoint if you like. Note the extension change from **ovpn** to **conf**.

Create `/etc/openvpn/login` containing only your username and password, one per line, for example:

~~~
sudo nano /etc/openvpn/login
~~~

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
sudo nano /etc/openvpn/Sweden.conf
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

** Test VPN

At this point you should be able to test the VPN actually works:

~~~
sudo openvpn --config /etc/openvpn/Sweden.conf
~~~

If all is well, you'll see something like:

~~~
$ sudo openvpn --config /etc/openvpn/Sweden.conf 
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

** Enable VPN at boot

~~~
sudo systemctl enable openvpn@Sweden
~~~

** Setup Routing and NAT

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

** VPN Kill Switch

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
sudo systemctl status openvpn@Sweden
sudo journalctl -u openvpn@Sweden
~~~

** Optional: DNS on the Pi

To ensure all your DNS goes through the VPN, you could install `dnsmasq` on the Pi to accept DNS requests from the local LAN and forward requests to external DNS servers.

~~~
sudo apt-get install dnsmasq
~~~

###Return to the [Main guide](https://github.com/mcfrojd/PiVPN-Seedbox) and proceed with step 5.###
