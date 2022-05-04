# Types of attacks

## Sniffing attack

Tools: Wireshark, tcpdump, tshark

Mitigation: TLS (Public Key Infrastructure (PKI) and trusted root certificates)

Using a network packet capture tool like Wireshark, tcpdump to collect information about the target.
When a protocol communicates in cleartext like in mail protocols, HTTP or FTP, the data exchanged can be captured.

Requires access to the network traffic, eg. via a wiretap, a switch with port mirroring, MITM attack.

```
$ sudo tcpdump port 110 -A
```

-A - display in ASCII format

---
## Man-in-the-Middle (MITM) attack 

Tools: Ettercap, Bettercap

Mitigation: SSL, TLS (Public Key Infrastructure (PKI) and trusted root certificates)

A victim believes they are communicating with a legitimate destination, but is unknowingly communicating with an attacker.
Thanks to SSL/TLS & public certificates signed by certificate authorities trusted by our systems, browser ensures that it is communicating with the correct server.

---
## Password attack

Tools: Hydra, John the Ripper

Mitigation: 
* Password Policy - Enforces minimum complexity constraints on the passwords set by the user.
* Account Lockout - Locks the account after a certain number of failed attempts.
* Throttling Authentication Attempts - Delays the response to a login attempt.
* Using CAPTCHA - Requires solving a question difficult for machines. It works well if the login page is via a graphical user interface (GUI).
* Requiring the use of a public certificate for authentication.
* Two-Factor Authentication - Ask the user to provide a code available via eg. email, SMS, Google Authenticator.
* IP-based geolocation


