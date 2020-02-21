## Truecrypt Container Password Cracking

This section will cover how to recover the password for both an outer Truecrypt password container, along with a hidden container password using hashcat.

## Truecrypt Outer Container
To crack the password of a Truecrypt outer container you will need to copy out the first 512 bytes of the container using a hex editor, or forensic tool such as Access Data's FTK Imager.  Example Truecrypt container headers can be downloaded from https://hashcat.net/wiki/doku.php?id=example_hashes.

Hashcat has a number of modes available for cracking Truecrypt volume headers based upon the encryption and hashing algorithms used.  In this example `TrueCrypt 5.0+ SHA512 + AES ` mode `6221` will be used with a dictionary attack.

