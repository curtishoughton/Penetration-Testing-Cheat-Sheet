# Berkeley R Services Enumeration

## Details

Berkeley R Services allow users of one Unix operating system to both login and issue commands to another.  The default ports are:

**Ports**

* **rexecd**: 512

* **rlogind**: 513

* **rshd**: 514

To enumerate R Services within Kali linux the rsh-client package must be installed:

`sudo apt install rsh-client`

## Rexecd 

The rexec daemon, which by default runs on port 512, allows a user to run shell commands on a remote machine.  To authenticate, a user is required to enter a username and password  
