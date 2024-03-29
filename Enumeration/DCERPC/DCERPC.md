# DCERPC Enumeration

## Details

DCE/RPC stands for Distributed Computing Environment/Remote Procedure Call. It is a protocol that allows applications to make remote procedure calls (RPCs) to other applications on a network. RPCs are a way of calling functions on a remote computer as if they were local functions.


**Ports**

* **RPC**: 135 TCP/UDP
* **DYNAMIC RPC PORTS**: 49152-65535/TCP

## RPCCLIENT

The software `rpcclient` is built into many Linux distributions, including Kali Linux, and can be used to interact with Remote Procedure Calls (RPCs) on a remote Windows system. Additionally, if Anonymous login is available, it is possible to obtain details about the domain without user authentication.

### RPCCLIENT - Anonymous Bind

The following command can be used to connect anonymously to RPC without supplying user credentials:

```shell
rpcclient -N -U "" <IP>
```
Once connected you will see the prompt:

```shell
rpcclient $>
```

The `lsaquery` command can be used to get the Domain SID:

```shell
rpcclient $> lsaquery
Domain Name: ZA  
Domain Sid: S-1-5-21-3330634377-1326264276-632209373
```

### RPCCLIENT - Authenticated User

If you have an AD account, the following command can be used to authenticate to a server using rpcclient:

```shell
rpcclient -U "<domain>\<username>" <IP>
```


## Using IOXIDResolver to get IPv6 Address

### rpcmap.py

The rpcmap.py tool can be used to check an IP address to see if the RPC interface UUID for IObjectExporter  `99FCFEC4-5260-101B-BBCB-00AA0021347A` is exposed.

Command:

```shell
rpcmap.py -brute-opnums -opnum-max 5 'ncacn_ip_tcp:10.10.10.213'
```

Successful Response:

```shell
Protocol: [MS-DCOM]: Distributed Component Object Model (DCOM) Remote
Provider: rpcss.dll
UUID: 99FCFEC4-5260-101B-BBCB-00AA0021347A v0.0
Opnum 0: rpc_x_bad_stub_data
Opnum 1: rpc_x_bad_stub_data
Opnum 2: rpc_x_bad_stub_data
Opnum 3: success
Opnum 4: rpc_x_bad_stub_data
Opnum 5: success
```

If opnums 3 and 5 are available it means the `ServerAlive()` function is callable.

### IOXIDResolver.py Python Script

The following python script taken from `https://github.com/mubix/IOXIDResolver` can be used to call the function and retrieve the Hostname, IPv4 and IPv6 addresses of the machine:

```python3
#!/usr/bin/python

import sys, getopt

from impacket.dcerpc.v5 import transport
from impacket.dcerpc.v5.rpcrt import RPC_C_AUTHN_LEVEL_NONE
from impacket.dcerpc.v5.dcomrt import IObjectExporter

def main(argv):

    try:
        opts, args = getopt.getopt(argv,"ht:",["target="])
    except getopt.GetoptError:
        print ('IOXIDResolver.py -t <target>')
        sys.exit(2)

    target_ip = "192.168.1.1"

    for opt, arg in opts:
        if opt == '-h':
            print ('IOXIDResolver.py -t <target>')
            sys.exit()
        elif opt in ("-t", "--target"):
            target_ip = arg

    authLevel = RPC_C_AUTHN_LEVEL_NONE

    stringBinding = r'ncacn_ip_tcp:%s' % target_ip
    rpctransport = transport.DCERPCTransportFactory(stringBinding)

    portmap = rpctransport.get_dce_rpc()
    portmap.set_auth_level(authLevel)
    portmap.connect()

    objExporter = IObjectExporter(portmap)
    bindings = objExporter.ServerAlive2()

    print ("[*] Retrieving network interface of " + target_ip)

    #NetworkAddr = bindings[0]['aNetworkAddr']
    for binding in bindings:
        NetworkAddr = binding['aNetworkAddr']
        print ("Address: " + NetworkAddr)

if __name__ == "__main__":
   main(sys.argv[1:])
```

To run the command use the python script is `python3 IOXIDResolver.py -t <IPv4>`

This will then return the following output:

```shell
[*] Retrieving network interface of <IP>
Address: <hostname>
Address: <IPv4>
Address: <IPv6> (if available)
```


