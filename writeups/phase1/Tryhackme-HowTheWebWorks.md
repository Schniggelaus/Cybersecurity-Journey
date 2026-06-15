# How the Web Works

## DNS
- DNS stands for `Domain name System`

## Domain Hierachy
- **TLD** (Top level Domain) is either a `gTLD` (Generic Top Level Domain) or a `ccTLD` (Country Code Top Level Domain)
    - **gTLD** equals to the domain´s name purpose  (e.g: .org = organisation, .edu. = education,...)
    - **ccTLD** equals to the geographical purpose (e.g: .uk = United Kingdom, .ca = Canada,...) 
  
- **SLD** (Second-level Domain) are the characters coming pre TLD (e.g: Tryhackme.com -> Tryhackme = SLD, .com = TLD)
    -   Limited to **63 chars**
    -    Only usable Chars are: `a-z,0-9, "-","_"`
- **Subdomain** sits on left-hand side of SLD
    - can extend Domain-Name (e.g: admin.tryhackme.com)
    - same limitations as SLD
    - infinite-subdomains possible but length must be kept to 253 or chars or less

 ## DNS Record Types
-There are different type of Records since DNS is not just for websites.
### A Record
- Resolves IPv4 addresses
### AAAA Record
- Resolves IPv6 addresses
### CNAME Record
- Resolves to another domain (e.g: Tryhackme´s online shop has the Subdomain name `store.tryhackme.com` which returns a `CNAME` record `shops.shopify.com` -> Another DNS request will be made to `shops.shopify.com`
### MX Record
- Resolves address of servers handeling emails
### TXT Record
- Text fields where any text-based data can be stored
- Usfull in verfifying ownership of domain name when sigining up for third party services


## Making a Request
What happens when i make a DNS request:
```
1. On request, PC checks local cache if address was looked up recently -> if not: Request to Recursive DNS Server will be made
2. DNS Server also checks local cache
    - if result is found locally -> send back to PC and request ends
    - if result is not found -> Journey starts to find correct answer, starting at internet´s root DNS server
3. Root server = backbone of the internet -> It´ll redirect to correct `TLD-Server` (e.g: `www.tryhackme.com` ->
   root server will recognize `.com` and redirect to TLD server that deals with `.com` adresses
4. TLD-Server holds records of where to find the `authoritative server` to answer the DNS request.
5. Authoritative DNS server responsible for storing DNS records for domain name -> DNS records will be sent back to
   `Recursive DNS Server`, where a local copy will be cached for future requests and then relayed back to original client.
   DNS records come with a TTL (Time to Live) value. TTL is a number representing seconds that the response should be saved
   locally until Recursive DNS Server has to look it up again

```

## Practical

### What is the CNAME of shop.website.thm?
- Select CNAME and enter `shops` into the subdomain-field and send the DNS Request
- Terminal will show `shops.myshopify.com` which is the solution

### What is the value of the TXT record of website.thm?
- Select TXT in the DropBox and send DNS Request
- Answer will be displayed in the Terminal

### What is the numerical priority value for the MX record?
- Select MX in the DropBox and send DNS Request
- Answer will be displayed in the Terminal

### What is the IP address for the A record of www.website.thm?
- Select A in the DropBox and send DNS Request
- Answer will be displayed in the Terminal

