# HTTP/HTTPS Enumeration

## Directory Brute Force

A number of tools can be ued to bruteforce directories on a web server in order to find hidden files and folders that are not publically available.

### Gobuster

Gobuster can be used to bruteforce a web server in order to discover directories and files:

`gobuster dir -u http://website.com/ -w /path/DirectoryWordlist.txt -x php,config`

Gobuster will iterate through the wordlist provided and also append the file extenstions `.php` and `.config` in order to discover directories and files.

### DirSearch



### Common Wordlists Used

A number of wordlists are commonly used and are available in Kali Linux:

``


