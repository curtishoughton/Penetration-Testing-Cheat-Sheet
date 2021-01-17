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
### Manual Testing

Manual testing of the supported commands can be done using netcat or telnet. Replace the `<mode>` with VRFY, EXPN or RCPT, to enumerate users:

Netcat:
```
nc -nvv <IP> <Port>
<mode> <username>
```
Telnet:

```
telnet <IP> <Port>
<mode> <username>
```
This will return either a `250` or `252` response code for any valid users that are discovered. An error message will appear for any invalid users. When enumerating, it is pretty clear when a valid user has been found using this method.

### Automated Testing

#### NMAP

Nmap has various NSE scripts that can be used for enumerating SMTP, bruteforcing and checking for known vulnerabilities against SMTP servers.  The current list of SMTP script supported is below:

```
smtp-brute.nse
smtp-commands.nse
smtp-enum-users.nse
smtp-ntlm-info.nse
smtp-open-relay.nse
smtp-strangeport.nse
smtp-vuln-cve2010-4344.nse
smtp-vuln-cve2011-1720.nse
smtp-vuln-cve2011-1764.nse
```
##### Enumerate Users

The snmp-enum-users nse script can be used to enumerate users. The mode is provided within curly braces, such as `{VRFY}`. Replace `{MODE}` with either `{VRFY}`,`{EXPN}` or `{RCPT}`

`nmap -v -p25 -n --script smtp-enum-users --script-args smtp-enum-users.methods={MODE} <IP Address>`

##### SMTP Open Relay

Nmap can also be used to check is the server is configured to relay emails from any location with the following command:

`nmap -v -p25 --script smtp-open-relay <IP Address>`

Nmap will return whether the server is configured to relay emails.

#### SMTP-USER-ENUM Script

The smtp-user-enum tool, built into Kali Linux, can be used to automate username enumeration via SMTP:

`smtp-user-enum -U /path/to/usernames.txt -t <IP Address> -m 150 -M <mode>`

The `-M` parameter can be set to either VRFY, EXPN or RCPT, depending upon what commands the server allows.

#### Metasploit Framework

The metasploit framework can also be used to enumerate SMTP users.  If a port is not set, it will default to port 25:

```
use auxiliary/scanner/smtp/smtp_enum
set RHOSTS <IP ADDRESS>
set RPORT <Port>
run 
```
