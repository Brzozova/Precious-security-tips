# TOR
Tor is a free and open-source software for enabling anonymous communication. 
Tor directs Internet traffic through a free, worldwide, volunteer overlay network consisting of more than seven thousand relays to 
conceal a user's location and usage from anyone conducting network surveillance or traffic analysis.
Tor browser, as seen from its name, is a browser that transfers all its traffic through TOR and by using firefox headers makes all Tor users look the same.

## Key terms

Proxychains - a tool that forces any TCP connection made by any given application to follow through proxy like TOR or any other SOCKS4, SOCKS5 or HTTP(S) proxy.
Link -> https://github.com/haad/proxychains

---
## Installation

TOR
```
apt-get install tor && service tor start && service tor status 
```


Proxychains

A tool that forces any TCP connection made by any given application to follow through proxy like TOR or any other SOCKS4, SOCKS5 or HTTP(S) proxy.
Link -> https://github.com/haad/proxychains

```
apt-get install proxychains
```

Configure:
```
vim /etc/proxychains.conf
```

Uncomment dynamic_chain & proxy_dns in order to prevent DNS leak. 


Run:
```
proxychains firefox
```

* After the firefox has loaded, check if your IP address has changed with any website that provides such information. 
* Try running a test on dnsleaktest.com and see if your DNS address changed too.

**NOTE: All other web browser windows should be closed before opening firefox through proxychains!**

---




