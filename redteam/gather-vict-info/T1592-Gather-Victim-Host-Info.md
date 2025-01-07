# T1592 Gather Victim Host Information

## T1592.001 Hardware

A number of techniques can be used to grab victim host hardware information from an external recon position

### T1592.001 Procedure

#### Public Asset Inventory

Review public documentation and websites that may expose hardware inventories on websites such as Github, Gitlab or IT management portals. 

Google dorks can be used to search for leaked data, which may include hardware assets:

```shell
site:company.com inurl:inventory
site:company.com filetype:xlsx hardware
```

Websites like shodan and Censys can be used to search devices/IP addresses/domains related to an organisation and can footprint devices, often providing insights into Makes/Models/Versons:

![image](https://github.com/user-attachments/assets/ba35ba12-5589-4301-ae5c-c0c135a7addc)

#### Network Footprinting

Network footprinting using tools like `nmap` for banner grabbing or `snmpwalk` often gives information on the underlying hardware and technology in use.

##### Nmap Banner Granning

Nmap can be used to grab the "banners" from ports, which may indicate what technologies or hardware is in use:

```shell
nmap -sS -sV --script=banner -p <port> <IP>
```

The banner may leak hardware information, such as Make/Model/Serial number.

##### Simple Network Management Protocol (SNMP) Port: 161/UDP

The Simple Network Management Prococol (SNMP) operates on UDP port 161 and can give extremely detailed information about a device and its running configuration. A common misconfiguration for SNMP is having the Read/Write or Read-Only community string(s) set to the default values:

```shell
Read-Only Community String: public
Read/Write Community String: private
```

If the device uses the default community strings along with SNMP version 1 or v2c, it is possible to dump configuration data from the device. This can include Make/Model/Serial Number/Network Interfaces and much more.

To use snmpwalk on a Linux system such as Kali Linux, install SNMP Mibs using:

```shell
sudo apt update
sudo apt install snmp snmp-mibs-downloader -y
```

Once the SNMP mibs are installed the tool `snmpwalk` can be used to query SNMP over port 161/UDP to reveal critical system information. 

snmpwalk command explanation:

```shell
-v 1: SNMP version 1.
-c public: Community string (public). Or -c "private" for private.
192.168.1.10: Target device IP.
system: Fetch system information OID (e.g., system description, uptime, etc.).
```

Example 1: SNMP Version 1 (Public Community String)

```shell
snmpwalk -v 1 -c public 192.168.1.10 system
```

Example 2: SNMP Version 1 (Private Community String)

```shell
snmpwalk -v 1 -c private 192.168.1.10 system
```

Example 3: SNMP Version 2c (public Community String)

```shell
snmpwalk -v 2c -c public 192.168.1.10 system
```

Example 4: SNMP Version 2c (private Community String)

```shell
snmpwalk -v 2c -c private 192.168.1.10 system
```

#### IPMI & BMC Interfaces

IPMI (Intelligent Platform Management Interface) and BMC (Baseboard Management Controller) allow out-of-band management of servers and hardware, often used for remote monitoring, rebooting, and troubleshooting. These systems are attractive targets because misconfigurations or default credentials can expose detailed hardware information and even control over the hardware.

IPMI or BMC interfaces typically run on ports 623/UDP and 664/TCP

Nmap can be used to scan for 
