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
### Useful Nmap flags

-n - don't send DNS queries
-R - query the DNS server even for offline hosts
--dns-servers <dns_address> - use a specific DNS server

-p-	- all ports
-p1-1023 - scan ports 1 to 1023
-F	- 100 most common ports
-r	- scan ports in consecutive order
-T<0-5>	-T0 - being the slowest and T5 the fastest
--max-rate 50	- rate <= 50 packets/sec
--min-rate 15	- rate >= 15 packets/sec
--min-parallelism 100	- at least 100 probes in parallel


