# Simple Mail Transfer Protocol (SMTP)

SMTP is a cleartext protocol designed to send, receive and relay email to its intended recipient. In a penetration test SMTP can be used for username enumeration, in order to find potential usernames/email addresses belonging to an organisation.

Default Port: **25**

## SMTP Enumeration

The VRFY, EXPN and RCPT commands can all be used to aid username enumeration from an SMTP mail server. This can be done manually using netcat or telnet, or automated, using Metasploit or smtp-user-enum.

### Banner Grabbing

The first stage of identifying the SMTP service is to grab the banner, to ensure it is the SMTP service running.

Netcat:

`nc -nv <IP Address> <Port>`

Telnet:

`telnet <IP Address> <Port>`

This should result in the server responding with a `220` status code and potentially other useful server information.

### Identifying Supported Commands

As stated above, the server may allow for username enumeration using the VRFY, EXPN and RCPT commands. Supported commands can be checked using NMAP. The following command assumes the server is using the default port of 25:

`nmap -vv -p25 -n --script smtp-commands <IP Address>`

This will return a list of commands that are accepted by the server and will assist in deciding what commands can be used to enumerate users.

### SMTP-USER-ENUM Script

The smtp-user-enum tool, built into Kali Linux, can be used to automate username enumeration via SMTP:

`smtp-user-enum -U /path/to/usernames.txt -t <IP Address> -m 150 -M <mode>`

The `-M` parameter can be set to either VRFY, EXPN or RCPT, depending upon what commands the server allows.
