# Wifi Hacking

Tool: Aircrack-ng

---
## WPA Standard

* WPA and WPA2 use practically the same authentication method,
* WPA2-EAP uses RADIUS servers to authenticate, so if you have to enter a username and password in order to connect then it's probably that
* WEP (Wired Equivalent Privacy) standard was insecure

### Key terms

* SSID - The network "name" that you see when you try and connect
* ESSID - An SSID that *may* apply to multiple access points, eg a company office, normally forming a bigger network. For Aircrack they normally refer to the network you're attacking.
* BSSID - An access point MAC (hardware) address
* WPA2-PSK - Wifi networks that you connect to by providing a password that's the same for everyone
* WPA2-EAP - Wifi networks that you authenticate to by providing a username and password, which is sent to a RADIUS server.
* RADIUS - A server for authenticating clients, not just for wifi.

PSK - wifi code/password/passphrase

---
## Wi-Fi encryption

### Wired Equivalent Privacy (WEP)
* deprecated
* encryption mechanism is the RC4 stream cipher
* employs a single shared key among its users that must be manually entered on an access point device
* small key size resulted in being easier to brute-force, especially when that key doesn’t often change

*Summary: Combined with the use of RC4, this left WEP particularly susceptible to related-key attack. In the case of 128-bit WEP, 
your Wi-Fi password can be cracked by publicly-available tools in a matter of around 60 seconds to three minutes.
While some devices came to offer 152-bit or 256-bit WEP variants, this failed to solve the fundamental problems of WEP’s underlying encryption mechanism.

### Wi-Fi Protected Access (WPA)
* deprecated
* Each packet sent has a unique temporal 128-bit key, that solves the susceptibility to related-key attacks brought on by WEP’s shared key mashing
* Message authentication code (MAC) - known as a checksum, a MAC provides a cryptographic way to verify that messages haven’t been changed.
In Temporal Key Integrity Protocol (TKIP), an invalid MAC can also trigger rekeying of the session key. If the access point receives an invalid MAC twice within a minute, 
the attempted intrusion can be countered by changing the key an attacker is trying to crack.
* encryption mechanism is the RC4 stream cipher

Temporal Key Integrity Protocol (TKIP /tiːˈkɪp/) is a security protocol used in the IEEE 802.11 wireless networking standard.

*Summary: It certainly improved on the weaknesses of WEP, TKIP eventually proved vulnerable to new attacks that extended previous attacks on WEP.

### Wi-Fi Protected Access II (WPA2)
* secure
* encryption mechanism is the block cipher called Advanced Encryption Standard (AES) 


### Wi-Fi Protected Access III (WPA3)
* secure
* required for new devices since July 1, 2020
* provide forward secrecy (protecting previously exchanged information even if a long-term secret key is compromised)

*NOTE: Forward secrecy is already provided by protocols like TLS by using asymmetric keys to establish shared keys.

*Summary: Improve password security by being more resilient to word list or dictionary attacks.

---
## How to improve security

### Use VPN
If you have old devices with WPA & WEP, then use VPN.
VPN provider that offers a feature like a kill switch that blocks your network traffic if your VPN becomes disconnected. 
This prevents you from accidentally transmitting information on an insecure connection like open Wi-Fi or WEP.



