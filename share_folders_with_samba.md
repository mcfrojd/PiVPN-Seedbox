#Installera SAMBA#

~~~
sudo apt-get install samba -o Acquire::ForceIPv4=true
~~~

**Configure the settingsfile for samba**
~~~
sudo nano /etc/samba/smb.conf
~~~

**Replace the content in that file with this:**
~~~
[global]
netbios name = SeedBox
server string = The SeedBox File Center
workgroup = WORKGROUP
hosts allow =
remote announce =
remote browse sync =

[SEEDBOX]
path = /media/pi/SeedBox
comment = No comment
browsable = yes
read only = no
valid users =
writable = yes
guest ok = yes
public = yes
create mask = 0777
directory mask = 0777
force user = root
force create mode = 0777
force directory mode = 0777
hosts allow =
~~~

**We need to create a user that can access the share**
   * Here we create a user named "pi" (use what ever you want)
~~~
sudo smbpasswd -a pi
~~~
   * Enter a password for the user. (Remember these credentials)

**Restart the samba service**
~~~
sudo service smbd restart
~~~

**Now you can look at your computer, refresh the network and the share "SEEDBOX" should appear**
   * Login to the share with your credentials from above.

