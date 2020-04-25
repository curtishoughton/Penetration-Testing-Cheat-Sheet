# Open Source Intelligence (OSINT)

## Passive

### WHOIS

The following command can be used to perform a WHOIS lookup on a domain:

`whois example.com`

Find names of objects containing example within ARIN database:

`whois -a "z / example*"`

Enumerate email accounts belonging to example objects in ARIN database:

`whois -a "z @ example*"`

Enumerate names of objects containing example in APNIC database:

`whois -A example`

### Domain Name System (DNS) Enumeration

#### DNS Zone Transfers

The following command can be used to attempt a DNS zone transfer:

`dig axfr example.com @ns1.example.com`

This will attempt to access replicate/copy records contained within the DNS database for example.com from the primary DNS server.  This can give an accurate picture of how both internal and external domains are set up, along with what IP addresses are associated with those domains.  Attempt a DNS zone transfer if TCP port 53 is exposed on the webserver.  Note that typically DNS utilises UDP on port 53.

### BGP (Border Gateway Protocol) Enumeration

Often whois queries on an organisations domain will reveal its Autonomous System Numbers (ASN).  BGP is used to route traffic between internet networks using their AS numbers.  The following website can be used to search for details relating to a specific ASN:
`bgp.he.net/ASXXXXX`

Where the X's are replaced with the specicif AS number you're searching.

### Google Hacking

`intitle:`  - Search for text within page title

`intext:`   - Search for text within a webpage

`inurl:`    - Search for hits within page URL

`filetype:` - Return results for specific file type

`site:`     - Show results for a particular domain


## Active
