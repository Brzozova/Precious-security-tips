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

---
## Path Traversal attack (Directory traversal/dot-dot-slash attack), LFI, RFI

Allows an attacker to read operating system resources, such as local files on the server running an application

Mitigation for file inclusion vulnerabilities:
* Keep system and services, including web application frameworks, updated with the latest version.
* Turn off PHP errors to avoid leaking the path of the application and other potentially revealing information.
* A Web Application Firewall (WAF) is a good option to help mitigate web application attacks.
* Disable some PHP features that cause file inclusion vulnerabilities if your web app doesn't need them, such as allow_url_fopen on and allow_url_include.
* Carefully analyze the web application and allow only protocols and PHP wrappers that are in need.
* Never trust user input, and make sure to implement proper input validation against file inclusion.
* Implement whitelisting for file names and locations as well as blacklisting.


---
## SSRF (Server-Side Request Forgery)

Allows a malicious user to cause the webserver to make an additional or edited HTTP request to the resource of the attacker's choosing.

### Types

* Regular SSRF - data is returned to the attacker's screen.
* Blind SSRF - vulnerability where an SSRF occurs, but no information is returned to the attacker's screen.

Results:
* Access to unauthorised areas.
* Access to customer/organisational data.
* Ability to Scale to internal networks.
* Reveal authentication tokens/credentials.







