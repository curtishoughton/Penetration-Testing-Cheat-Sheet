# HTTP/HTTPS Enumeration

## Directory Brute Force

A number of tools can be used to brute force directories on a web server in order to find hidden files and folders that are not publically available.

### Gobuster

Gobuster can be used to bruteforce a web server in order to discover directories and files:

`gobuster dir -u http://website.com/ -w /path/DirectoryWordlist.txt -x php,config`

Gobuster will iterate through the wordlist provided and also append the file extenstions `.php` and `.config` in order to discover directories and files.

### DirSearch

Dirsearch has a built-in wordlist that can be used for bruteforcing directories and files.  Optionally a custom wordlist can be specified using the `-w /path/wordlist.txt` option: 

`python3 dirsearch.py -u http://<ip address>:<port>/ -e <file extensions>`

### Dirb

Dirb can also be used to brute-force directories and is built into Kali Linux by default.  Dirb should be used with smaller wordlists, as large wordlists will often cause the program to hang or close. Note that any wordlists that already have `/` appended to them, would need the forward slash removing, due to dirb appending its own. Adding the `-t ` option will stop dirb automatically adding the `/`. 

The following command can be used to discover directories on a web server:

`dirb -u http://<ip address>:<port> /path/wordlist.txt`

### FFUF 

Fuzz Faster U Fool (FFUF) is a tool written in GO that is also able to bruteforce files and directories. This tool requires golang, which can be installed in Kali with `apt update && apt install golang`.  Once golang is installed ffuf can be installed using `go get github.com/ffuf/ffuf`.  To start a simple directory brute force the following command can be used:

`ffuf -w /path/to/wordlist.txt -u https://target/FUZZ`

The `FUZZ` keyword tells the tool at which point you want to fuzz for files and directories.

### Common Wordlists Used

A number of wordlists that are commonly used for directory/file fuzzing are available in Kali Linux:

`/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt`

`/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

`/usr/share/wordlists/dirb/common.txt`


