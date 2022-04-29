# NMAP

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
  However, if we send a UDP packet to a closed UDP port, we expect to get an ICMP port unreachable packet; 
  this indicates that the target system is up and available.
  
  ```
  $ sudo nmap -PU -sn
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


-n - don't send DNS queries
-R - query the DNS server even for offline hosts
--dns-servers <dns_address> - use a specific DNS server


