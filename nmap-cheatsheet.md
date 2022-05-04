# NMAP

## Types of statuses

* Open - a service is listening on the specified port.
* Closed - no service is listening on the specified port, although the port is accessible. 
  By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs.
* Filtered - cannot determine if the port is open or closed because the port is not accessible. 
  This state is usually due to a firewall preventing Nmap from reaching that port. 
  Nmap’s packets may be blocked from reaching the port; alternatively, the responses are blocked from reaching Nmap’s host.
* Unfiltered - cannot determine if the port is open or closed, although the port is accessible. This state is encountered when using an ACK scan -sA.
* Open|Filtered - cannot determine whether the port is open or filtered.
* Closed|Filtered - cannot decide whether a port is closed or filtered.

---
## Types of TCP headers

* URG - Urgent flag indicates that the urgent pointer filed is significant. 
  The urgent pointer indicates that the incoming data is urgent, and that a TCP segment with the URG flag set is processed immediately 
  without consideration of having to wait on previously sent TCP segments.
* ACK - Acknowledgement flag indicates that the acknowledgement number is significant. It is used to acknowledge the receipt of a TCP segment.
* PSH - Push flag asking TCP to pass the data to the application promptly.
* RST - Reset flag is used to reset the connection. Another device, such as a firewall, might send it to tear a TCP connection. 
  This flag is also used when data is sent to a host and there is no service on the receiving end to answer.
* SYN - Synchronize flag is used to initiate a TCP 3-way handshake and synchronize sequence numbers with the other host. 
  The sequence number should be set randomly during TCP connection establishment.
* FIN: The sender has no more data to send.

---
## Types of nmap scanning

### ARP scan - in  Link Layer
  - Sending ARP requests to discover live hosts.
  - It's sending a frame to the broadcast address on the network segment and asking the computer with a specific IP address to respond 
    by providing its MAC (hardware) address. 
  - It works only inside the subnetwork. Scanner will be routed via the default gateway (router) to reach the systems on another subnet. 
  - However, the ARP queries won’t be routed and hence cannot cross the subnet router. 
  - ARP is a link-layer protocol, and ARP packets are bound to their subnet.
   Link: http://www.royhills.co.uk/wiki/index.php/Arp-scan_Documentation
  
   #### Send ARP queries to all valid IP addresses on your local networks
    ```
    $ arp-scan --localnet
    ```
   Similar nmap command:
    -PE discover live fosts -sn without port scanning
    ```
    $ nmap -PR -sn
    ```
   #### Send ARP queries for all valid IP addresses on the eth0 interface
    ```
    $ sudo arp-scan -I eth0 -l
    ```

### ICMP scan - in Network Layer
  - Uses ICMP requests to identify live hosts
  - Uses Type 8 (Echo) and Type 0 (Echo Reply)
  - ARP request is send & APR responde is received before sending ping
  - Nmap didn’t need to send ICMP packets as it can confirme that hosts are up based on the ARP responses it received
  - ARP query will precede the ICMP request if your target is on the same subnet
  
  #### Standard echo request
   (ICMP Type 8/Echo) requests with a ping reply (ICMP Type 0)
   Wireshark -> Info: Echo (ping) request
    ```
    $ nmap -PE -sn
    ```
  
  #### In case of blocked ICMP
  * ICMP Timestamp request(ICMP Type 13) & Timestamp reply (ICMP Type 14)
    Wireshark -> Info: Timestamp request
    ```
    $ nmap -PP -sn
    ```
  
  * ICMP Address Mask
    Address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18)
    If getting no results, then ICMP Address Mask is blocked on firewall
    Wireshark -> Info: Address mask request
    ```
    $ nmap -PM -sn
    ```   
  
### TCP/UDP ping scan - in Transport Layer.
  - Sends packets to TCP ports and UDP ports to determine live hosts.
  - Used when ICMP is blocked
  
  #### TCP SYN Ping
   1. Send a packet with the SYN (Synchronize) flag set to a TCP port (80 by default)
   2. SYN/ACK (Acknowledge) if port is open || RST (Reset) is port is closed
   Wireshark -> Info: SYN
  
   Scan using port range:
    ```
    $ nmap -PS21-25 
    ```
  #### TCP ACK Ping
  Only for privileged user.
  1. Send a packet with an ACK flag set
  2. Respond with the RST flag set because the TCP packet with the ACK flag is not part of any ongoing connection.
  Wireshark -> Info: ACK
  
  Scan using port range:
  ```
  $ sudo nmap -PA21-25 
  ```
  NOTE: Nmap sends each packet twice. The systems that don’t respond are offline or inaccessible.
  
  #### UDP Ping
  Sending a UDP packet to an open port is not expected to lead to any reply. 
  If we send a UDP packet to a closed UDP port, we expect to get an ICMP port unreachable packet; 
  this indicates that the target system is up and available.
  
  ```
  $ sudo nmap -PU -sn
  ```
 
---
### TCP connect scan

 TCP 3-way handshake
 SYN -> SYN/ACK -> ACK
 
 ```
 $ nmap -sT
 ```
 
 ### TCP SYN scan
 
  Does not need to complete the TCP 3-way handshake
  SYN -> SYN/ACK -> RST
  *NOTE: The default scan mode when running Nmap as a privileged user*
  
  ```
  $ sudo nmap -sS
  ```
  
 ### UDP scan
 
  Does not require any handshake for connection establishment
  *NOTE: If a UDP packet is sent to a closed port, an ICMP port unreachable error (type 3, code 3) is returned.*
  * Port is open:
    UDP packet -> No response
  * Port is clodes:
    UDP packet <- ICMP, Type 3, Code 0
  
  ```
  $ sudo nmap -sU
  ```
---
### Scans behinde the firewall

A stateless firewall will check if the incoming packet has the SYN flag set to detect a connection attempt. 
Using a flag combination that does not match the SYN packet makes it possible to deceive the firewall and reach the system behind it.
A stateful firewall will practically block all such crafted packets and render this kind of scan useless.

### Null Scan

 * does not set any flag
 * all six flag bits are set to zero
 * lack of reply in a null scan indicates that either the port is open or a firewall is blocking the packet
 * Port is open:
   NULL -> No response
 * Port is clodes:
   NULL <- RST/ACK
   
   ```
   $ sudo nmap -sN
   ```
 
 *NOTE: Because the null scan relies on the lack of a response to infer that the port is not closed, 
 it cannot indicate with certainty that these ports are open; there is a possibility that the ports 
 are not responding due to a firewall rule.*

### FIN scan

 * TCP packet with the FIN flag set

 * Port is open:
   FIN -> No response
 * Port is clodes:
   FIN <- RST/ACK
   
   ```
   $ sudo nmap -sF
   ```
  *NOTE: Some firewalls will 'silently' drop the traffic without sending an RST*

### Xmas scan
 
 * Sets the FIN, PSH, and URG flags simultaneously

 * Port is open:
   FIN/PSH/URG -> No response
 * Port is clodes:
   FIN/PSH/URG <- RST/ACK
   
   ```
   $ sudo nmap -sX
   ```

---
ACK scan and the window scan were very efficient at helping us map out the firewall rules.\b
It is vital to remember that just because a firewall is not blocking a specific port, \b
it does not necessarily mean that a service is listening on that port. \b
There is a possibility that the firewall rules need to be updated to reflect recent service changes. \b
ACK and window scans are exposing the firewall rules, not the services.\b

### TCP ACK scan

 * Send a TCP packet with the ACK flag set
 * Checking ACK packets resulted in responses, you will learn which ports were not blocked by the firewall.
 * This type of scan is more suitable to discover firewall rule sets and configuration
 
 ```
 $ sudo nmap -sA
 ```

### TCP window scan
 
 * Like the ACK scan, but it examines the TCP Window field of the RST packets returned

 ```
 $ sudo nmap -sW
 ```

### Custom scan

Example:
To set SYN, RST, and FIN simultaneously:
```
$ nmap --scanflags RSTSYNFIN
```

---
## IP SPOOTING

It requires you to be in a position where you can monitor the traffic. 

Process:
1. Attacker sends a packet with a spoofed source IP address to the target machine.
2. Target machine replies to the spoofed IP address as the destination.
3. Attacker captures the replies to figure out open ports.


```
$ nmap -S SPOOFED_IP MACHINE_IP
```

Define network interface to use (-e) and not to expect to receive a ping reply (-Pn)
```
$ nmap -e NET_INTERFACE -Pn -S SPOOFED_IP MACHINE_IP
```

## MAC spoofing

It requires you to be in a position where you can monitor the traffic.

When you are on the same subnet as the target machine, you would be able to spoof your MAC address.
This address spoofing is only possible if the attacker and the target machine are on the same Ethernet (802.3) network or same WiFi (802.11).

```
--spoof-mac SPOOFED_MAC
```

## Decoy

Make the scan appears to be coming from many IP addresses so that the attacker’s IP address would be lost among them.


```
$ nmap -D 10.10.0.1,10.10.0.2,ME MACHINE_IP
```

-D - specifying a specific or random IP address
ME - indicate that your IP address should appear in the third order

```
$ nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME MACHINE_IP
```
RND - source IP addresses are assigned randomly

---
## Fragmented Packets


-f - fragment packets, the IP data will be divided into 8 bytes or less
-f -f or -ff - will split the data into 16 byte-fragments
--mtu - change the default value, choose a multiple of 8


*Note: If you added -ff (or -f -f), the fragmentation of the data will be multiples of 16. In other words, the 24 bytes of the TCP header, in this case, would be divided over two IP fragments, the first containing 16 bytes and the second containing 8 bytes of the TCP header.*

Q: If the TCP segment has a size of 64, and -ff option is being used, how many IP fragments will you get? A: 4

---
## Zombie/IDLE scan

Nmap will make each probe appear as if coming from the idle (zombie) host, then it will check for indicators whether the idle (zombie) host received any response to the spoofed probe. 
This is accomplished by checking the IP identification (IP ID) value in the IP header. 

Process:
1. Trigger the idle host to respond so that you can record the current IP ID on the idle host.
2. Send a SYN packet to a TCP port on the target. The packet should be spoofed to appear as if it was coming from the idle host (zombie) IP address.
3. Trigger the idle machine again to respond so that you can compare the new IP ID with the one received earlier.

```
$ nmap -sI ZOMBIE_IP MACHINE_IP
```
ZOMBIE_IP - the IP address of the idle host 



---
## How to be less visible using Nmap?

* Use fragmented packets
* Use decoy

---
#### Subnet scanning
```
$ nmap -sL -n 10.10.12.13/29
```
#### Range scanning
```
$ nmap -sL -n 10.10.0-255.101-125
```
#### Discover online hosts without port-scanning
Nmap, by default, uses a ping scan to find live hosts, then proceeds to scan live hosts only. 
```
$ nmap -sn TARGETS
```
---
## Useful Nmap flags

-A	- equivalent to -sV -O -sC --traceroute\b

-n - don't send DNS queries\b
-R - query the DNS server even for offline hosts\b
--dns-servers <dns_address> - use a specific DNS server\b

-p-	- all ports\b
-p1-1023 - scan ports 1 to 1023\b
-F	- 100 most common ports\b
-r	- scan ports in consecutive order\b
-T<0-5>	-T0 - being the slowest and T5 the fastest\b
--max-rate 50	- rate <= 50 packets/sec\b
--min-rate 15	- rate >= 15 packets/sec\b
--min-parallelism 100	- at least 100 probes in parallel\b

## Flags to get more details
--reason - gives us the explicit reason why Nmap concluded that the system is up or a particular port is open. 

Host is up, received arp-response - system is considered online
“syn-ack” - port is open

-vv - verbose\b
-dd - debugging mode\b

## Output flags

-oN - normal, like in terminal\b
-oG - grepable\b
-oX - XML\b


---
# Services detection

-sV - collect and determine service and version information for the open ports\b
--version-intensity LEVEL - where the level ranges between 0, the lightest, and 9, the most complete\b
-sV --version-light - an intensity of 2\b
-sV --version-all - an intensity of 9\b

*Note: It is important to note that using -sV will force Nmap to proceed with the TCP 3-way handshake and establish the connection. \b
The connection establishment is necessary because Nmap cannot discover the version without establishing a connection fully and communicating with the listening service. In other words, stealth SYN scan -sS is not possible when -sV option is chosen.*


-O
--traceroute - find the routers between you and the target
```
$ nmap -sS --traceroute <IP>
```

*Note: Standard traceroute starts with a packet of low TTL (Time to Live) and keeps increasing until it reaches the target. Nmap’s traceroute starts with a packet of high TTL and keeps decreasing it.*


#### Case

Command: 
```
$ nmap -sS --traceroute <IP>
```
Output: 
```
TRACEROUTE
HOP RTT     ADDRESS
1   1.48 ms MACHINE_IP
```
Effect:
There are no routers/hops between the two as they are connected directly.\b
Many routers are configured not to send ICMP Time-to-Live exceeded, which would prevent us from discovering their IP addresses.

---
## Nmap Scripting Engine

Nmap Scripting Engine (NSE) is a Lua interpreter that allows Nmap to execute Nmap scripts written in Lua language.\b
Scripts to use are located under the path `/usr/share/nmap/scripts`, but you can install other user’s scripts and use them for your scans.

Specify the script by name using --script "SCRIPT-NAME" or a pattern such as --script "ftp*"

```
$ sudo nmap -sS -n --script "http-date" <IP>
```
`http-date` - retrieve the http server date and time







