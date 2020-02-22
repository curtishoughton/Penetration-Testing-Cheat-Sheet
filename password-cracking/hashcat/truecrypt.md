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
Hashcat can now be used on the 512 byte container header with a password dictionary of choice using attack mode 0 (dictionary).  The list of attack modes can be found within the Hashcat help menu:

```
- [ Attack Modes ] -

  # | Mode
 ===+======
  0 | Straight
  1 | Combination
  3 | Brute-force
  6 | Hybrid Wordlist + Mask
  7 | Hybrid Mask + Wordlist

```
The command to run hashcat againt the container header - within powershell - would look like:
`.\hashcat64.exe -a 0 -m 6221 container_to_crack512bytes.tc password_list.txt`

Where:

`-a 0` - Attack Mode 0 Dictionary Attack

`-m 6221` - Mode for cracking TrueCrypt 5.0+ SHA512 + AES

`container_to_crack512bytes.tc` - This is the file name you have given to the first 512 bytes you have copied from the container

`password_list.txt` - The password dictionary you have chosen to use.

This method can be applied to all containers, however it may be trial and error with the cracking modes if you do not know the Encryption / Hashing algorithm used.


In this case the password for the container was found to be "hashcat"
```
..\hashcat_sha512_aes.tc: hashcat

Session..........: hashcat
Status...........: Cracked
Hash.Type........: TrueCrypt PBKDF2-HMAC-SHA512 + XTS 512 bit
Hash.Target......: ..\hashcat_sha512_aes.tc
Time.Started.....: Fri Feb 21 23:47:09 2020 (0 secs)
Time.Estimated...: Fri Feb 21 23:47:09 2020 (0 secs)
Guess.Base.......: File (.\pass.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#3.........:       20 H/s (1.87ms) @ Accel:16 Loops:15 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 3/3 (100.00%)
Rejected.........: 0/3 (0.00%)
Restore.Point....: 0/3 (0.00%)
Restore.Sub.#3...: Salt:0 Amplifier:0-1 Iteration:990-999
Candidates.#3....: test -> hashcat
Hardware.Mon.#3..: Temp: 65c
```

## Truecrypt Hidden Container

Now the interesting part, recovering the password for a hidden Truecrypt container.  For this you will need to use Truecrypt (in my case 7.1a), to generate a containter that contains another hidden container within.

To find the header of the hidden container, you will have to count backwards `65536` bytes from the end of the container - in a hex editor - then select 512 bytes from that point and copy them into a new file using the hex editor. I used HxD to do this, but you could use any.  This will give you the header necessary to crack the password of the hidden volume.  The same process can be used as documented within the "Truecrypt Outer Container" section above.

The outer container containing the hidden volume and the hidden volume were were generaed using `PBKDF2-HMAC-RIPEMD160` and `XTS  512 bit pure AES`, which meant mode `6211` was used in this instance.

`.\hashcat64.exe -a 0 -m 6211 hidden_vol_512bytes.tc password_dictionary.txt`

```
..\testhidden.tc:hiddenvol

Session..........: hashcat
Status...........: Cracked
Hash.Type........: TrueCrypt PBKDF2-HMAC-RIPEMD160 + XTS 512 bit
Hash.Target......: ..\testhidden.tc
Time.Started.....: Sat Feb 22 00:26:50 2020 (1 sec)
Time.Estimated...: Sat Feb 22 00:26:51 2020 (0 secs)
Guess.Base.......: File (.\pass.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#3.........:       32 H/s (1.05ms) @ Accel:32 Loops:16 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 5/5 (100.00%)
Rejected.........: 0/5 (0.00%)
Restore.Point....: 0/5 (0.00%)
Restore.Sub.#3...: Salt:0 Amplifier:0-1 Iteration:1984-1999
Candidates.#3....: test -> outerpas
Hardware.Mon.#3..: Temp: 50c

```
