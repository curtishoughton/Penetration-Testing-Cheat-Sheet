# Buffer Overflows

## Return Oriented Programming (ROP) Buffer Overflow
The following commands can be used in a Return Oriented Programming (ROP) buffer overflow attack to bypass NX/DEP protection.  The following example makes use of the `ovrflw` ELF binary found on the October Hack The Box machine.  All tools necessary are built into Kali Linux.

The following command can be used to generate a 500 character non repeating pattern to overlow the program:

``/usr/bin/msf-pattern_create -l 500``

