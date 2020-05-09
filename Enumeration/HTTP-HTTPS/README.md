# HTTP/HTTPS Enumeration

## Directory Brute Force

A number of tools can be ued to bruteforce directories on a web server in order to find hidden files and folders that are not publically available.

### Gobuster

Gobuster can be used to bruteforce a web server in order to discover directories and files:

`gobuster dir -u http://website.com/ -w /path/DirectoryWordlist.txt -x php,config`

Gobuster will iterate through the wordlist provided and also append the file extenstions `.php` and `.config` in order to discover directories and files.

### DirSearch



### Common Wordlists Used

A number of wordlists that are commonly used for directory/file fuzzing are available in Kali Linux:
`/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt`
`/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
`/usr/share/wordlists/dirb/common.txt`


