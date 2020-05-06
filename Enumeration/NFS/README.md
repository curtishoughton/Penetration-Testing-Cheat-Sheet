# Network File System (NFS) Enumeration


## Enumeration with Rpcinfo and Showmount

Rpcinfo can be used to show the port NFS is operating on: 

`rpcinfo -p <ip_address>`

Showmount can be used to show a list of NFS exported file shares:

`showmount -e <ip_address>`

## Enumeration with NMAP

The following command will query the RPC Portmapper on port 111 and detail what port NFS is currently running on, which is usually TCP/UDP Port 2049.  The nmap NSE scripts will show a list of the exported NFS shares and output from rpcinfo:

`nmap -p 111 -sV -vv --script=nfs-ls,nfs-statfs,nfs-showmount <ip_address>`

## Mounting NFS Shares

Shares that are listed as being exported can be mounted within Linux using the following command:

`mount -t nfs <ip_address>:/NameOfExportedShare /tmp/LocalMountPoint`
