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
