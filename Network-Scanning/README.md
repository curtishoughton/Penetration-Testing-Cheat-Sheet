# Network Scanning

In a nutshell, the objective is to identify open ports on a host(s), identify what services are running on the open ports and then find if there are any vulnerabilities present in the services that are exposed.  The most common network mapping software within penetration testing for achieving this is **nmap**, which is available from https://nmap.org/download.  

## Overt NMAP Scans

### Full TCP Connect Scan

A full TCP connect scan of all 65535 ports on a specified host can be initiated using the following nmap command:

`nmap -vv -sT -p- 192.168.X.X`

Nmap will perform a full three-way TCP handshake with each port and return whether the port(s) are open or closed, along with an assumption of the service running on that port.  The `-sV` flag can be added to this command and will probe open ports further to determine more accurate service/version information. Obviously performing a scan against all ports on a target host(s) is noisy on the network, so be mindful of this if you are on an test where stealth is of importance (such as a Red Team engagement).

### Full TCP Syn Scan
A full TCP Syn scan of all 65535 ports on a specified host can be initiated using the following nmap command:

`nmap -vv -sS -p- 192.168.X.X`

The command will use nmap to scan all TCP ports on the target system and return whether the port(s) are open or closed.  A Syn scan will only complete two stages of the three-way TCP handshake.  This scan is classed as being stealthier, as a full TCP connection is never made, therefore some devices may not log the scan attempt if configured to only log full TCP connections.  It must be noted that most - if not all - modern IDS/IPS and firewall devices will detect a TCP Syn scan. 

### Full UDP Scan

A full UDP scan of all 65535 ports on a specified host can be initiated using the following nmap command:

`nmap -vv -sU -p- 192.168.X.X`

The command will use nmap to scan all UDP ports on the target system and return whether the port(s) are open or closed.  Note that the UDP protocol is a stateless protocol and therefore scanning can take a long time, especially if there are multiple hosts.  It would be better to obtain a list of the most common UDP ports and feed that into nmap using `-p 161,1434` etc. If no ports are specified, nmap will default to scanning the top 1000 most common UDP ports.  This will be fine in most situations.

## Covert NMAP Scans

### NMAP with TOR and Proxychains

NMAP can be used in conjunction with TOR and proxychains to carry out a covert NMAP scan over the TOR network.  TOR can be installed within Kali Linux using the command `apt-get install tor`.  Once installed, TOR can be started within Kali Linux wihin a terminal by typing:

`tor`

This will establish a connection to the TOR network using port 9050.  Proxychains can then be configured to route all traffic through port 9050.  The configuration file for proxychains is located at `/etc/proxychains.conf`.  The configuration file should have the following set:

```[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks4  127.0.0.1 9050
```

Proxychains, by default, is set to route traffic through TOR using a SOCKS4 proxy.  Once TOR is started and the proxychains configuration file has been configured, nmap scans can now be made using the proxychains command:

`proxychains nmap -sT -p 80,443,3389 -vv <IP ADDRESS>`

The command above route the nmap scan, using proxychains over the TOR network.

### Socat HTTP Relay Over TOR

Socat can be used to create a persistent connection to a server over TOR, which will anonymise all traffic.  The following command can be used to open a local listening port with SOCAT on 8080, which will route all traffic over TOR to the destination server:

`socat TCP4-LISTEN:8080,fork SOCKS4a:127.0.0.1:<server_IP>:80,socksport=9050 &`

Once the connection has been established, any additional scans, such as Nikto, can be ran against 127.0.0.1 port 8080.  Any traffic sent will be routed over the TOR network to the destination server.
