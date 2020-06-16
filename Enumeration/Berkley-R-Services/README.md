# Berkeley R Services Enumeration

## Details

Berkeley R Services allow users of one Unix operating system to both login and issue commands to another.  The default ports are:

**Ports**

* **rexecd**: 512

* **rlogind/rwhod**: 513

* **rshd**: 514

To enumerate R Services within Kali linux the rsh-client package must be installed:

`sudo apt install rsh-client`

## Rexec Daemon

The rexec daemon, which by default runs on port 512, allows a user to run shell commands on a remote machine.  To authenticate, a user is required to enter a username and password.  

The metasploit module `auxiliary/scanner/rservices/rexec_login` can be used to bruteforce username/password combinations, in order to gain access to the target system:

```
use auxiliary/scanner/rservices/rexec_login
set RHOSTS <ip address>
set PASS_FILE /path/to/passwordList.txt
set USER_FILE /path/to/userList.txt
run
```

## Rlogin/Rwho Daemon

The rlogin daemon, which by default operates on port 513, allows a user to login to a remote unix system. This can also allow users to login without a password:

` rlogin -l root <IP_Address>`

A list of logged in users can be obtained using the following command:

`rusers -al <IP_Address>`

## RSH Daemon

The Remote SHell (RSH) Daemon allows users to open a shell on a remote machine and execute commands:

`rsh <IP_Address> <shell_command_to_be_ran>`



