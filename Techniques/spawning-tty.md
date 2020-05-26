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

*References: https://netsec.ws/?p=337 - Spawning a TTY Shell*
