# Simple Network Management Protocol (SNMP)

The Simple Network Management Protocol allows for the collection of data from managed network devices on IP networks.

Default Ports: UDP **161** and **162**

## Installing SNMP Mibs

SNMP utilises management interface base's (MIBS), which are numerical values organised heirarchically.  Each value corresponds to a management value that can be queried in order to obtain information from a system.  Installing the SNMP MIBS allows for automatic parsing of the numeric values and converts them into human-readable text, that describes the output of the MIB.  To install SNMP MIBS run the following Linux command:

`sudo apt install snmp-mibs-downloader download-mibs`

An example of a MIB OID would be:

`1.3.6.1.4.1.77.1.2.25`

If SNMP was running on a Microsoft Windows operating system, querying this specific value would give a list of Windows user accounts.  Installing the MIB's would correlate this value with Windows User Accounts, for example, it may add the following:

`1.3.6.1.4.1.77.1.2.25 - Windows User Accounts`

## Enumeration using snmpwalk

Snmpwalk is a tool built into Kali Linux and a number of other penetration testing distributions that allows for automatic enumeration and parsing of SNMP mibs.  In general, it will give all output, unless just a single SNMP MIB is queried.

One key fact to remember is that to query information a Community string (think of it as a password), must be provided before the information can be queried. A common misconfiguration is leaving the default Community string `public`.  By not changing this value, it allows for any attacker to query data from the given system.  Ideally the community string should be treated like a password, and should be set to something complex and not easily guessable, as it can provide an attacker with a wealth of information about the target system.

SNMP also has several versions, currently `v1`, `v2c` and `v3`, therefore it is necessary to provide the version reference when querying a device.

The following snmpwalk command can be used to extract information from a target device:

`snmpwalk -c <community string> <version> <IP Address or Hostname>`

Optionally a numeric `OID` value can be specified after the hostname/IP Address, however, if not specified it will default to returning all information.

## Enumeration Using snmp-check

A second tool built into Kali Linux named snmp-check can be used to dump out all SNMP inormation providing the community string is known. This can be done using the following command:

`snmp-check <IP address or hostname> -p <port> -c <community String> -v <version>`

##  Enumeration Using Metasploit Framework

Metasploit can also be used to extract information from SNMP.  The following command within metasploit can be used:

```
use auxiliary/scanner/snmp/snmp_enum
set RHOSTS <IP Address or Hostname>
set RPORT <Port>
run
```

## Bruteforcing SNMP Community Strings

### Bruteforcing Using OneSixtyOne




