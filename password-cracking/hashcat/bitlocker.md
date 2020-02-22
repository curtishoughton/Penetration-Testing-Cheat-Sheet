# Bitlocker Drive Password Cracking (No TPM)

This section will show how to crack the password of a Bitlocker enrypted volume/drive that is NOT using a hardware-backed encryption key, such as a Trusted Platform Module (TPM), to encrypt the drive.  If you're in the situation that you have a Bitlocker device encrypted using a TPM, you will need the bitlocker recovery key that is usually either printed, or saved directly to the Microsoft Office account in the cloud, as Microsoft will offer to back up the key on encrypting the drive.

## Bitlocker Encrypted volume
To follow along, you will need access to a Bitlocker encrypted volume with a password you have set. Once you have created a Bitlocker volume, you will need to extract the "hash" from the container using the `bitcracker_hash.c` that can be found within the e-ego's Github https://github.com/e-ago/bitcracker. 
