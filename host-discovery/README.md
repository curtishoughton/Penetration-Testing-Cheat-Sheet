# Host / Local Network Discovery

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


## 802.lQ Cisco Dynamic Trunking Protocol (DTP) 

The VLAN protocol DTP is commonly used by Ciso devices to connect switches together via a "trunk". If the switch is in Dynamic Trunking mode, it is possible to analyse the DTP packets, emulate anoher switch, and hop on to a VLAN of the attacers choice.

The dtpscan script found at https://github.com/commonexploits/dtpscan, can be used to detect DTP packets and identify what mode the switches on the network are in, and whether it's possible to perform a VLAN hopping attack. To run the script in Kali Linux use:

`./dtpscan.sh`

If it's found to be possible, Yersinia, which is built into Kali Linux, can be used to enable trunking and attain a list of VLANs with associated IP addresses.

`yersinia -I` will start the program and will allow you to select the interface using the `a and b` keys on the keyboard. Then press `x` for DTP mode and press `1` to enable dynamic trunking. Pressing `g` at this point will show the VLANs and associated IP addresses within the GUI.

Once a VLAN has been identified, a virtual interface can be configured within Kali Linux:

```
modprobe 8021q
vconfig add <interface> <vlan_number>
dhclient <interface>.<vlan_number>
```

To check this is configured correctly the `ifconfig <interface>.<vlan_number>` or `ip a` commands can be ran.

*Reference: Pages 90-91, Network Security Assessment Volume 3, Chris Mcnab, Oreilly Publishers*

## LLMNR, NBT-NS and mDNS

NBT-NS: 137 UDP
LLMNR: 5355 UDP
mDNS: 5353 UDP

Both Link-Local Multicast Name Resolution and Net BIOS Name Service are used by Microsoft systems on the local network when DNS name resolution fails. It is possible to use a tool like `responder` to Man-In-The-Middle (MITM) and intercept failed lookups, in order to obtain Net NTLMv2 hashes:

`responder - <interface> -rdwv`

The responder arguments shown:

```
  -r, --wredir          Enable answers for netbios wredir suffix queries.
                        Answering to wredir will likely break stuff on the
                        network. Default: False
  -d, --NBTNSdomain     Enable answers for netbios domain suffix queries.
                        Answering to domain suffixes will likely break stuff
                        on the network. Default: False
  -w, --wpad            Start the WPAD rogue proxy server. Default value is

  -v, --verbose         Increase verbosity.
```

Any failed lookups will be captured by responder and result in a NTLMv2 hash being captured. Any NTLMv2 hashes can be cracked using either John The Ripper or Hashcat in order to obtain plaintext credentials.  The hash returned will look like:

```
Administrator::WIN-487IMQOIA8E:997b18cc61099ba2:3CC46296B0CCFC7A231D918AE1DAE521:0101000000000000B09B51939BA6D40140C54ED46AD58E890000000002000E004E004F004D00410054004300480001000A0053004D0042003100320004000A0053004D0042003100320003000A0053004D0042003100320005000A0053004D0042003100320008003000300000000000000000000000003000004289286EDA193B087E214F3E16E2BE88FEC5D9FF73197456C9A6861FF5B5D3330000000000000000
```

Hashcat can be used with mode `5600` to crack the hash:

```
hashcat -m 5600 <file-containing-hash.txt> /path/to/password-list.txt
```

