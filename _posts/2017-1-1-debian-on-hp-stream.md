Ther is excellent write up about installng debian on HP Stream,
I personally have 13 inch stream latpop for browsing and small
activities, but debian makes the laptop close to a development
machine with all the features, I have installed debian on the
sd mmc card and boot from grub2win 

Here is the link:

http://astroelec.blogspot.in/2015/05/installing-debian-on-hp-stream-1113.html

Basically the steps are

1. download debian unoffical iso as it will have the prop firmwares requried for wifi.

2. boot from burned usb and when you get the tasksel(package selection) deselect to choose none as we need to configure wifi before that and that can be dont after the firstboot.

3. connect to he wifi and run tasksel to install desired packages.

  a. ip -a; iwconfig; ip link set wlan0 up; iwlist scan
  
  b. wpa_passphrase myssid my_very_secret_passphrase >> /etc/network/interfaces
  
  c. it should be simillar to be like this
	auto wlan0
	iface wlan0 inet dhcp
    	wpa-ssid [ESSID]
    	wpa-psk ccb290fd4fe6449e050edd02ad44627b16ce0151668f5f53c01b
     

