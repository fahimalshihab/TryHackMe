# Network Security
### whois
```
whois tryhackme.com
```
- Registrar: Via which registrar was the domain name registered?
- Contact info of registrant: Name, organization, address, phone, among other things. (unless made hidden via a privacy service)
- Creation, update, and expiration dates: When was the domain name first registered? When was it last updated? And when does it need to be renewed?
- Name Server: Which server to ask to resolve the domain name?
### nslookup
```
nslookup -type=A tryhackme.com 1.1.1.1
nslookup -type=AAAA tryhackme.com 1.1.1.1
```
- Find the IP address of a domain name
```
nslookup -type=MX tryhackme.com
```
- learn about the email servers and configurations for a particular domain
### dig
```
dig tryhackme.com MX
```
- For more advanced DNS queries and additional functionality, you can use dig
-  dig returned more information thn nslookup

### For sub domains

[DNSDumpster](https://dnsdumpster.com/)
## Shodan
[Shodan.io](https://www.shodan.io/)
- Shodan.io collects information related to any device it can find connected online
- Shodan.io is a search engine for the Internet of Things.
#### Banners
 Devices run services, and Shodan stores information about them. The information is stored in a banner. It’s the most fundamental part of Shodan.
An example banner looks like:
```
{
		"data": "Moxa Nport Device",
		"Status": "Authentication disabled",
		"Name": "NP5232I_4728",
		"MAC": "00:90:e8:47:10:2d",
		"ip_str": "46.252.132.235",
		"port": 4800,
		"org": "Starhub Mobile",
		"location": {
				"country_code": "SG"
		}
 }
```
### Filters
#### Autonomous System Numbers(ASN)
- (ASN) is a global identifier of a range of IP addresses.
- We can put the IP address into an ASN lookup tool such as [ASNLOOKUP](https://asnlookup.com/) and it give us a asn number and On Shodan.io,we can search using the ASN filter. The filter is ASN:[number]
- Knowing the ASN is helpful, because we can search Shodan for things such as coffee makers or vulnerable computers within our ASN, which we know (if we are a large company) is on our network.
- example ```asn:AS14061```

#### MYSQL
If we look at the search, we can see it is another filter product:MySQL
Knowing this, we can actually combine 2 searches into 1.
On TryHackMe’s ASN, let’s try to find some MYSQL servers.
We use this search query

```asn:AS14061 product:MySQL```
#### vuln
The "vuln" filter is only available to Academic users or Small Business API subscription and higher.
#### Examples
```
asn:AS15169 product:"MySQL"
asn:AS15169 product:"nginx"
asn:AS15169 country:"US"
asn:AS15169 country:"US" city:"Los Angeles"
```
#### Shodan Monitor
Shodan Monitor is an application for monitoring your devices in your own network. In their words:
Keep track of the devices that you have exposed to the Internet. Setup notifications, launch scans and gain complete visibility into what you have connected.
https://monitor.shodan.io/dashboard
#### Shodan Dorking
Shodan has some lovely webpages with Dorks that allow us to find things. Their search example webpages feature some.
Some fun ones include:
- Which uses optical character recognition and remote desktop to find machines compromised by ransomware on the internet. 
```
has_screenshot:true encrypted attention
```
```
screenshot.label:ics
```
more here [shodan-dorks.txt](shodan-dorks.txt)

### Ping
 - The primary purpose of ping is to check whether you can reach the remote system and that the remote system can reach you back.
 - In simple terms, the ping command sends a packet to a remote system, and the remote system replies. This way, you can conclude that the remote system is online and that the network is working between the two systems.
 - If you prefer a pickier definition, the ping is a command that sends an ICMP Echo packet to a remote system. If the remote system is online, and the ping packet was correctly routed and not blocked by any firewall, the remote system should send back an ICMP Echo Reply.
```
ping -c 5 104.22.54.228
```
example :
```
└─$ ping -c 5 104.22.54.228       
PING 104.22.54.228 (104.22.54.228) 56(84) bytes of data.
64 bytes from 104.22.54.228: icmp_seq=1 ttl=55 time=9.65 ms
64 bytes from 104.22.54.228: icmp_seq=2 ttl=55 time=118 ms
64 bytes from 104.22.54.228: icmp_seq=3 ttl=55 time=27.7 ms
64 bytes from 104.22.54.228: icmp_seq=4 ttl=55 time=11.5 ms
64 bytes from 104.22.54.228: icmp_seq=5 ttl=55 time=26.6 ms

--- 104.22.54.228 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4008ms
rtt min/avg/max/mdev = 9.646/38.641/117.775/40.263 ms

```
### Traceroute
- The purpose of a traceroute is to find the IP addresses of the routers or hops that a packet traverses as it goes from your system to a target host.
- This command also reveals the number of routers between the two systems.
- It is helpful as it indicates the number of hops (routers) between your system and the target host.
- TTL is a value in a packet header that limits the lifespan of data in a computer or network.It prevents infinite loops or long-lived packets in a network.
- Each router or hop along the route decrements the TTL value of the packet by 1.If the TTL reaches 0, the packet is discarded.
- -m: Set the maximum TTL (time to live).
```
traceroute tryhackme.com
traceroute -m 4  tryhackme.com
```
example :
```
┌──(iftx㉿kali)-[~]
└─$ traceroute -m 4  tryhackme.com
traceroute to tryhackme.com (104.22.54.228), 4 hops max, 60 byte packets
 1  192.168.0.1 (192.168.0.1)  1.312 ms  2.057 ms  2.011 ms
 2  10.10.10.10 (10.10.10.10)  2.675 ms  2.626 ms  2.863 ms
 3  10.110.241.57 (10.110.241.57)  4.480 ms  4.433 ms  4.392 ms
 4  10.110.240.102 (10.110.240.102)  4.891 ms  4.847 ms  4.804 ms
                                                                                                                                                  
┌──(iftx㉿kali)-[~]
└─$ traceroute tryhackme.com     
traceroute to tryhackme.com (104.22.55.228), 30 hops max, 60 byte packets
 1  192.168.0.1 (192.168.0.1)  2.069 ms  2.018 ms  1.998 ms
 2  10.10.10.10 (10.10.10.10)  3.230 ms  3.212 ms  3.192 ms
 3  10.110.241.57 (10.110.241.57)  3.813 ms  4.548 ms  4.529 ms
 4  10.110.240.102 (10.110.240.102)  4.998 ms  4.979 ms  4.958 ms
 5  172.17.17.21 (172.17.17.21)  8.285 ms  8.264 ms  8.244 ms
 6  172.16.250.198 (172.16.250.198)  8.225 ms  5.112 ms  5.035 ms
 7  172.16.249.114 (172.16.249.114)  7.126 ms  5.182 ms  5.103 ms
 8  10.8.0.213 (10.8.0.213)  3.788 ms  3.750 ms  3.713 ms
 9  172.21.66.37 (172.21.66.37)  9.021 ms  10.406 ms  10.369 ms
10  103.21.42.18.earth.net.bd (103.21.42.18)  10.331 ms  10.290 ms  10.252 ms
11  104.22.55.228 (104.22.55.228)  9.336 ms  9.297 ms  8.571 ms

```
### Telnet
- Telnet stands for "Telecommunication Network." It is a network protocol that allows a user to interact with another computer or device over a network, typically the internet.
- Telnet provides a command-line interface to communicate with a remote device. It was commonly used for remote access and management of devices before more secure protocols like SSH (Secure Shell) became prevalent.
- Telnet typically uses port 23 for communication.
```
telnet 104.22.54.228 80
```
### Netcat
Netcat or simply nc has different applications that can be of great value to a pentester. Netcat supports both TCP and UDP protocols. It can function as a client that connects to a listening port; alternatively, it can act as a server that listens on a port of your choice. Hence, it is a convenient tool that you can use as a simple client or server over TCP or UDP.

```
netcat as server :$ nc -lvnp 1337
```
```
netcat as client :$ nc 192.168.0.176 1337
```
### Nmap
![745e0412b319d324352c7b29863b74f4 (3)](https://github.com/fahimalshihab/TryHackMe/assets/97816146/a1cec004-2c12-4fa4-94d4-f71b5d95ee00)

``` nmap -sL -n 10.10.0-255.101-125 ```
 to check the list of hosts that Nmap will scan

#### ARP

- An ARP (Address Resolution Protocol) request is a message sent out by a device on a local network to discover the MAC (Media Access Control) address associated with a particular IP (Internet Protocol) address. ARP is used in IPv4 networks to map an IP address to the corresponding hardware (MAC) address on a local network.

- When a device wants to communicate with another device on the same local network, it needs to know the MAC address of the target device. It broadcasts an ARP request to all devices on the local network, asking for the MAC address associated with a specific IP address.

- The ARP request includes the IP address for which the device is seeking the MAC address. The device with that specific IP address replies to the ARP request with its MAC address, allowing the requesting device to update its ARP cache. Once the ARP cache is updated, the devices can communicate with each other using their MAC addresses.

- ARP is a critical protocol for the functioning of local networks, helping devices discover and communicate with each other at the link layer.
```
sudo arp-scan 10.10.210.6/24
sudo nmap -PR -sn 10.10.210.6/24
```
Example :-
```
pentester@TryHackMe$ sudo arp-scan 10.10.210.6/24
Interface: eth0, datalink type: EN10MB (Ethernet)
WARNING: host part of 10.10.210.6/24 is non-zero
Starting arp-scan 1.9 with 256 hosts (http://www.nta-monitor.com/tools/arp-scan/)
10.10.210.75	02:83:75:3a:f2:89	(Unknown)
10.10.210.100	02:63:d0:1b:2d:cd	(Unknown)
10.10.210.165	02:59:79:4f:17:b7	(Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.9: 256 hosts scanned in 2.726 seconds (93.91 hosts/sec). 3 responded

.............................................................................................
pentester@TryHackMe$ sudo nmap -PR -sn 10.10.210.6/24

Starting Nmap 7.60 ( https://nmap.org ) at 2021-09-02 07:12 BST
Nmap scan report for ip-10-10-210-75.eu-west-1.compute.internal (10.10.210.75)
Host is up (0.00013s latency).
MAC Address: 02:83:75:3A:F2:89 (Unknown)
Nmap scan report for ip-10-10-210-100.eu-west-1.compute.internal (10.10.210.100)
Host is up (-0.100s latency).
MAC Address: 02:63:D0:1B:2D:CD (Unknown)
Nmap scan report for ip-10-10-210-165.eu-west-1.compute.internal (10.10.210.165)
Host is up (0.00025s latency).
MAC Address: 02:59:79:4F:17:B7 (Unknown)
Nmap scan report for ip-10-10-210-6.eu-west-1.compute.internal (10.10.210.6)
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 3.12 seconds
```

