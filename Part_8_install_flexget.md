##Install Flexget to automate your downloads.##
~~~
sudo pip install --upgrade setuptools
sudo pip install flexget
~~~
**Add information to your config.yml**
~~~
templates:
  series:
#    download: /mnt/SeedBox/autostart
    transmission:
      host: 192.168.1.2
      port: 9091
      username: user
      password: passw
      path: /mnt/SeedBox/complete
      addpaused: No
      ratio: 3
    regexp:
      accept:
        - south park
        - prison break
      reject:
        - 2160p
      rest: reject

tasks:
  your_torrent_site:
    rss: http://rss.yourtorrentsite.org/
    headers:
      Cookie: "uid=1234567; pass=dbjdiehfxh205cc033e7cbef12e2251fhf63jst"
    template: series
    priority: 1
~~~
**This part is not finish yet**

