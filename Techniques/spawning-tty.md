# Spawning TTY Shells

There are a number of ways to spawn TTY shells once a machine has been compromised on an engagement, below is a list of methods that have been sourced from various places online and listed here for ease:

## Python

`python -c 'import pty;pty.spawn("/bin/bash")'`

## Script

`/usr/bin/script -qc /bin/bash /dev/null`

## Echo

`echo os.system('/bin/bash')`

## SH

`/bin/sh -i`

## Perl

`perl —e 'exec "/bin/sh";'`

## Lua

`os.execute('/bin/sh')`

## Ruby

exec "/bin/sh"

# Making the Shell Fully Interactive

To enable autocomplete within the shell use the following:

1) Background the shell process using `CTRL+Z` 

2) Enter `stty raw -echo`

3) Foreground the process again using `fg`

4) Then set the TERM variable to the name of your terminal using `export TERM=<terminalNameHere>`

Once this has been done, you will be able to use tab autocomplete and scroll through historic terminal commands using the arrow keys.

**References**: *https://netsec.ws/?p=337 - Spawning a TTY Shell*
