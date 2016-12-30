*This guide is not complete yet! it's missing the part where you have to open port 9091 throu the VPN*

##Create a DuckDNS dynamic ip adress and connect it to your Pi.##

**Create a free account at https://www.duckdns.org**

**Add a personal domain XXXXXX.duckdns.org**

**Go back to your Pi and start the setup**
   * Make a folder with a scriptfile in it
~~~
mkdir duckdns
cd duckdns
sudo nano duck.sh
~~~
**Paste this**
    * Instructions can be found at https://www.duckdns.org/install.jsp click "pi" and your domain
~~~
echo url="https://www.duckdns.org/update?domains=XXXXX&token=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX&ip=" | curl -k -o ~/duckdns/duck.log -K -
~~~

**Run** `sudo chmod 777 duck.sh` **to set permissions to the script**

**Run** `crontab -e` **, select the nano option paste this line at the bottom of the script**
~~~
*/5 * * * * ~/duckdns/duck.sh >/dev/null 2>&1
~~~

**Run** `./duck.sh`

**Run** `cat duck.log`**, should return OK. Then run** `sudo service cron start`**to finalice she DuckDNS setup**

###Open port 9091 throu VPN###

**This part is not finish yet**

