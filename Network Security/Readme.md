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
more here 
