# Aircrack-ng

Link: https://www.aircrack-ng.org/doku.php?id=newbie_guide

## Aircrack-ng suite

* airbase-ng -- Multi-purpose tool aimed at attacking clients as opposed to the Access Point (AP) itself.
* aircrack-ng -- 802.11 WEP and WPA/WPA2-PSK key cracking program.
* airdecap-ng -- Decrypt WEP/WPA/WPA2 capture files.
* airdecloak-ng -- Remove WEP Cloaking™ from a packet capture file.
* airdrop-ng -- A rule based wireless deauthication tool.
* aireplay-ng -- Inject and replay wireless frames.
* airgraph-ng -- Graph wireless networks.
* airmon-ng -- Enable and disable monitor mode on wireless interfaces.
* airodump-ng -- Capture raw 802.11 frames.
* airolib-ng -- Precompute WPA/WPA2 passphrases in a database to use it later with aircrack-ng.
* airserv-ng -- Wireless card TCP/IP server which allows multiple application to use a wireless card.
* airtun-ng -- Virtual tunnel interface creator.
* packetforge-ng -- Create various type of encrypted packets that can be used for injection.

---
## Standard steps

1. Put your wireless card into monitor mode:
```
$ airmon-ng start wlan0
```
Monitor mode - allows your computer to listen to every wireless packet & optionally inject packets into a network
What the command actually does is it creates another interface called mon0, which you can see when you do iwconfig. 
This is the command that you probably didn't execute. If you did run it but you can't see mon0 afterwards, let us know what was the command's output.


2. Start packets capturing:
```
$ airodump-ng <options> mon0
```

3. Attempt to increase the number of IVs being captured:
```
$ aireplay-ng <options> mon0
```

4. Cracks the capture to extract the password:
```
$ aircrack-ng <options> <file>
```

---
### Useful commands

Kill other processes that are currently trying to use that network adapter
```
$ airmon-ng check kill
```

Capture packets to a file:
```
$ airodump-ng -w <file_name> --output-format <format_like_csv> mon0
```







