# DCERPC Enumeration

## Details

DCE/RPC stands for Distributed Computing Environment/Remote Procedure Call. It is a protocol that allows applications to make remote procedure calls (RPCs) to other applications on a network. RPCs are a way of calling functions on a remote computer as if they were local functions.


**Ports**

* **RPC**: 135 TCP/UDP
* **DYNAMIC RPC PORTS**: 49152-65535/TCP

## Using IOXIDResolver to get IPv6 Address

### rpcmap.py

The rpcmap.py tool can be used to check an IP address to see if the RPC interface UUID for IObjectExporter  `99FCFEC4-5260-101B-BBCB-00AA0021347A` is exposed.

Command:

```shell
rpcmap.py -brute-opnums -opnum-max 5 'ncacn_ip_tcp:10.10.10.213'
```

Successful Response:

```
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


