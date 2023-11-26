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
### Shodan
[Shodan.io](https://www.shodan.io/)
- Shodan.io collects information related to any device it can find connected online
