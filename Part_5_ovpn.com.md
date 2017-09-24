#Step 5 - Install ovpn.com

###Setup VPN Client###

**1. Uppdatera & starta om Pi:n**
~~~
sudo rpi-update && sudo reboot
~~~
**2. Installera OpenVPN**
~~~
sudo apt-get install openvpn unzip
~~~
**3. Säkerställ att tidzonen är korrekt**

Kör nedanstående kommando och gå igenom konfigurationen för att välja rätt tidzon.
~~~
sudo dpkg-reconfigure tzdata
~~~
**4. Ladda ner konfigurationen du vill använda**
~~~
cd /tmp && wget https://files.ovpn.com/raspbian/ovpn-se.zip && unzip ovpn-se.zip && mv config/* /etc/openvpn && rm -rf config && rm ovpn-se.zip
~~~
**5. Ange dina inloggningsuppgifter**
~~~
echo "mcfrojd" >> /etc/openvpn/credentials
~~~
~~~
echo "ÄNDRA TILL DITT LÖSENORD" >> /etc/openvpn/credentials
~~~
**6. Testa att starta OpenVPN för att se så att allt fungerar**
~~~
sudo openvpn --config /etc/openvpn/ovpn.conf --daemon
~~~
**7. Verifiera att anslutningen fungerade**

Vänta i cirka en minut från det att du körde det tidigare kommandot, kör därefter:
~~~
curl https://www.ovpn.com/v1/api/client/ptr | python -m json.tool
~~~
Du borde se något i stil med:
~~~
{
    "ip": "46.227.67.132",
    "ptr": "cliXXX.ovpn.com",
    "status": true
}
~~~
**8. Färdig**

###Return to the [Main guide](https://github.com/mcfrojd/PiVPN-Seedbox) and proceed with step 6.###
