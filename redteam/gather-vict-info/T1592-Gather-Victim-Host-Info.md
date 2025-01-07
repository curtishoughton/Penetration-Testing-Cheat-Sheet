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
