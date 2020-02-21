## Truecrypt Container Password Cracking

This section will cover how to recover the password for both an outer Truecrypt password container, along with a hidden container password using hashcat.

## Truecrypt Outer Container
To crack the password of a Truecrypt outer container you will need to copy out the first 512 bytes of the container using a hex editor, or forensic tool such as Access Data's FTK Imager.  Example Truecrypt container headers can be downloaded from https://hashcat.net/wiki/doku.php?id=example_hashes.

Hashcat has a number of modes available for cracking Truecrypt containers based upon the encryption and hashing algorithms used.  In this example `TrueCrypt 5.0+ SHA512 + AES ` mode `6221` will be used with a dictionary attack.  The full list of options is available from the hashcat help menu:

``` 
   62XY | TrueCrypt                                        | Full-Disk Encryption (FDE)
     X  | 1 = PBKDF2-HMAC-RIPEMD160                        | Full-Disk Encryption (FDE)
     X  | 2 = PBKDF2-HMAC-SHA512                           | Full-Disk Encryption (FDE)
     X  | 3 = PBKDF2-HMAC-Whirlpool                        | Full-Disk Encryption (FDE)
     X  | 4 = PBKDF2-HMAC-RIPEMD160 + boot-mode            | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure AES                        | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure Serpent                    | Full-Disk Encryption (FDE)
      Y | 1 = XTS  512 bit pure Twofish                    | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure AES                        | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure Serpent                    | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit pure Twofish                    | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded AES-Twofish            | Full-Disk Encryption (FDE)
      Y | 2 = XTS 1024 bit cascaded Serpent-AES            | Full-Disk Encryption (FDE)
```
