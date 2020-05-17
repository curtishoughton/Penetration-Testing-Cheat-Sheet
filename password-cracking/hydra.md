# Using Hydra to Crack Passwords

Hydra is an online password cracking/brutfore software that can be used to actively bruteforce a service running on a target system.  Hydra is included within the Kali Linux distribution by default, therefore this section will cover usage within Kali.

## Hydra Usage

The following options are what I believe to be the most common options used when bruteforcing with Hydra:

**-l** - Specify a single username

**-L** - Specify a username list

**-p** - Specify a single password

**-P** - Specify a password list

**-s** - Specify a particular port

**-t** - Specify the number of concurrent threads.  This value sometimes has to be dropped, depending on the 

The following is a list of services supported by Hydra, taken from he help menu:

```
Supported services: adam6500 asterisk cisco cisco-enable cvs firebird ftp[s] http[s]-{head|get|post} http[s]-{get|post}-form http-proxy http-proxy-urlenum icq imap[s] irc ldap2[s] ldap3[-{cram|digest}md5][s] memcached mongodb mssql mysql nntp oracle-listener oracle-sid pcanywhere pcnfs pop3[s] postgres radmin2 rdp redis rexec rlogin rpcap rsh rtsp s7-300 sip smb smtp[s] smtp-enum snmp socks5 ssh sshkey svn teamspeak telnet[s] vmauthd vnc xmpp

```
Services can be specified using the `<service>://<IP_Address` syntax, where "service" is replaced with a supported service name such as SSH.

## Bruteforcing with Hydra

### SSH

To bruteforce SSH with Hydra the following command can be used:

`hydra -L /path/to/username-list.txt -P /path/to/password-wordlist.txt -t 1 ssh://<IP_ADDRESS>`

If SSH is running on a non-standard port, the `-s` parameter can be supplied to target an alternate port.

### HTTP-POST-FORM

To bruteforce a HTTP POST login form the following command can be used:

`hydra -L /path/to/username-list.txt -P /path/to/password-wordlist.txt http-post-form://<IP_ADDRESS> -m /<loginPagePath>:UserNameParameter=^USER^&PasswordParameter=^PASS^&Login=<ID_of_login_button>:<message_if_login_fails>`
