# Open Source Intelligence (OSINT)

## Passive

### WHOIS

The following command can be used to perform a WHOIS lookup on a domain:

`whois example.com`


### DNS Zone Transfers

The following command can be used to attempt a DNS zone transfer:

`dig axfr example.com @ns1.example.com`

This will attempt to access replicate/copy records contained within the DNS database for example.com from the primary DNS server.  This can give an accurate picture of how both internal and external domains are set up, along with what IP addresses are associated with those domains.  Attempt a DNS zone transfer if TCP port 53 is exposed on the webserver.  Note that typically DNS utilises UDP on port 53.


