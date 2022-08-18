# WifiStuff
<h1>Wifi Stuff</h1>
**As always, for information only and authorized penetration testing.
**Do not do anything illegal with this or use it for malicious purposes  

This is an ongoing growing page.

<h1>Wifi SSID</h1>
This is a list of free and common used wifi ssid found in New Zealand
An attacker can set these up and create an access point broadcasting these
If a phone or device is set up to auto-connect to these previously used ssid  then it will connect automatically to it, without the user/owner knowing
This is a big risk as now an attacker has access to your device
A proof on consept can be seen using the Wifi Pineapple device's karama attacks.
Public wifi is dangerious, do not use it.
Do not set auto connect on your device, especially mobile devices

<h3> SSID List </h3>
<ul>
<li>AT HOP</li>
<li>WiFi AirNZ_Lounge</li>
<li>Auckland WiFi</li>
<li>BP Free Wifi</li>
<li>Free Facebook</li>
<li>FreeWifi McDonalds</li>
<li>Free Wi-Fi</li>
<li>Mobil Free</li>
<li>Wi-Fi Spark</li>
<li>WiFi Starbucks</li>
<li>Free WiFi</li>
<li>Summer Inn</li>
<li>TheWarehouse-FreeWifi</li>
<li>Vodafone</li>
<li>Wi-Fi@PaknSave</li>

  
<hr>  

  <h1>Capture WPA2 hashes</h1>
WPA2 authentication is the most common found in NZ
It is important that complex passwords be selected.
A lot of “out of the box” manufacturer passwords are strong these days
However, it is still subjected to a data breach.
WPA2 passwords are at a min 8 characters long.
Change you password, mix up upper, lower, numbers and special charters.

An attacker can capture the “WPA handshake” and capture the hash.
A program like hashcat can make use of a GPU against password list.

<h3>WPA2 hash capture</h3>
Nothing complicated.
Stop network services on your linux device:

systemctl stop NetworkManager wpa_supplicant

Start hcxdump tool:

hcxdumptool -i wlan0 -o dumpfile.pcapng --active_beacon --enable_status=15

Leave this running for a while. This will capture all sort of WIFI information.
Ctrl C to exit out

Convert the pcapng file to a hashcat file:
hcxpcapngtool -o hash.hc22000 -E essidlist dumpfile.pcapng

The hash.hc22000 can be used with a GPU enabled OS.
An example:

hashcat -m 22000 hash.hc22000 wordlist.txt

-m 22000 specify dealing with WPA2 encryption 

The hashcat setup is important.
The GPU drivers need to be enabled
You also need to obtain an appropriate wordlist.
The rockyou.txt is good, but not optimal for WPA2

<h3> Denial of service attack</h3>
If you are the average small business an attacker could use a de-authentication attack on your wifi network.
The only real way to prevent the attack is to change to WPA3, or 802.11W standard.
This is not common as of yet in New Zealand.
Simply put this can disconnect any wifi device from the router, for as long the attack is ongoing.
This can stop a service, like a checkout, or..
An attacker can set up a fake access point with the same SSID as he is attacking.
For example, the uneducated checkout assistant may see the device disconnect from the wifi.
When they search for it they find another SSID with the same name. They connect, have access, and continue. Unknown to them they have just connected to an attacker, who has access to the device, and to all the traffic, in a man-in-the-middle type scenario.
Education is the most simple way of prevention.
Don’t connect to networks you don’t know.
Usually this will require an attacker to be close by and have some sort of equipment.
Most common is a laptop with a wifi adapter, that should be suspicious in, for example, a hardware store.
However smaller devices such as the wifi pineapple and the deauth watch exist
Easy setup with linux:
Stop the networking service
systemctl stop NetworkManager wpa_supplicant

Scan for an access point and write down the mac / bssid and channel
airodump-ng wlan0

Start the device in monitor mode, change the channel , send deauth packages:
airmon-ng start wlan0 11 & aireplay-ng -0 100 -a ff:ff:ff:ff:ff:ff wlan0mon

wlan0 is the wifi device
11 is the channel
-0 11 send deauth packets x 100
ff:ff:ff:ff:ff:ff is the bssid / mac of the access point you want to attack
wlan0mon the new wifi device now in monitor mode. Sometimes this may stay the same, wlan0

This will attack every device connected to the acces point
You can add the –c option of you know the device that you want to de-authenticate from the access point.

The other option is to use mdk4 
The last command is similar:
airmon-ng start wlan0 11 & mdk4 wlan0mon d –S ff:ff:ff:ff:ff

These commands cover off de-authentication using linux
This is very much automated on the wifi pineapple.

Linux provide further tool that also create an access point:
<h4>airgeddon</h4>
Will do it all in one. Scan for access points. Capture the hash. Create a fake rogue access point. De-authenticate and capture username and passwords.
Easy to follow wizard. There are a lot more it can do, however, the captive portal is limited to 1 option for inserting the wifi password

<h4>wifipumpkin3</h4>
Similar to airgeddon. There are more options for captive portals. It is harder to set up and has a metasploit type interface.

Bettercap and the airoplay commands can also achieve these in a way, but airgeddon and wifipumpkin is the easier automated options

<hr>

This page is an ongoing work in progress
It is intended to help educate and do lawful penetration testing.
Do not use it illegally, or for any maliciously intend

If I am wrong or missed something please do let me know.
superiorwebnz@gmail.com

