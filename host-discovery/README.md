# Host Discovery

Once on an internal network the first stage is to discover hosts that are live.  The following section will detail software along with associated commands for achieving this task.

## Address Resolution Protocol (ARP) Host Discovery

The ARP protocol can be used on a network to discover live hosts in a subnet.  A tool named **arp-scan** can be used to achieve this task:

`arpscan --interface eth0 --localnet`

This command will generate an ARP broadcast on the localsubnet for the eth0 interface.  Note the interface would need to be changed dependent on the network you are aiming to scan.  E.G for Wireless networks the default interface is often wlan0.  If a host is live it will be shown in the output from the command.

Similar tools that can be used:
`netdiscover -r 192.168.X.X`

## Internet Control Message Protocol (ICMP) Ping Scan

The network scanning tool **Nmap** can also be used to indentify live hosts by conducting a ping scan.  This will send out ICMP (ping) packets to each of the hosts specified.  If a host is live and configured to respond to ICMP ping requests, it will respond with an acknowledgement thus confirming the host is live:

`nmap -sn -vv -192.168.X.X/24`

This command uses nmap to initiate a ping scan (-sn) of all 254 hosts in the /24 (255.255.255.0) subnet for the 192.168.X.X network.  The ip address and subnet would need to be changed to match the network you are scanning.

## Nmap TCP Scan

Nmap can also be used to conduct a TCP scan of a specified port accross an entire subnet.  If any of the hosts are live and listening on the specified port, Nmap will output the host as being live:

`nmap -sP -PT80 192.168.X.X/24`

This command uses nmap to initiate a TCP host discovery scan (-sP) of all 254 hosts in the /24 (255.255.255.0) subnet for the 192.168.X.X network, looking for port 80.  For this scan to be effective, a commonly open port must be chosen - E.G port 445 (SMB) for Windows hosts, or port 22 (SSH) for Linux hosts.


