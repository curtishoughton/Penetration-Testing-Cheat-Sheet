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


#### DNS Subdomain Enumeration

The folloiwng tools with associated commands can be used to enumerate subdomains of a target domain.  Amass is provided by OWASP and is one of the most comprehensive domain enumeration tools with lots of extra functionality. To do a simple passive subdomain scan, which will return a list of subdomains, the following command can be used:

`amass enum -d example.com`

Sublist3r is another great tool that can be used to both passively and actively scan a domain.  To run a passive scan the following command can be run:

`sublist3r -d example.com`

If you've outputted all your sublist3r output to a text file, you can use the following command to separate the url's on their own into a new file:

`sublister_output.txt | awk '/-/ {print $1}' > new_output.txt`

#### Checking DNS Bind Version

To disclose DNS bind information the following command can be used:

`host -c chaos -t txt version.bind dns11.domain.com`

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

### Domain Name System (DNS) Enumeration

#### DNS Zone Transfers

The following command can be used to attempt a DNS zone transfer:

`dig axfr example.com @ns1.example.com`

This will attempt to access replicate/copy records contained within the DNS database for example.com from the primary DNS server.  This can give an accurate picture of how both internal and external domains are set up, along with what IP addresses are associated with those domains.  Attempt a DNS zone transfer if TCP port 53 is exposed on the webserver.  Note that typically DNS utilises UDP on port 53.

#### Reverse DNS Sweeping

Reverse DNS sweeping is the process of resolving a block of IP addresses to hostnames.  Once a netblock of interest has been indentified, nmap can be used in conjunction with grep and awk to format the results in "hostname  IP address" format:

`nmap -sL 192.168.1.0/24 | grep "(" | awk '{printf("%s %s\n",$5,$6);}'`

*The command listed above was documented in O'Rileys Network Security Assessment Volume 3*

