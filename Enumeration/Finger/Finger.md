# Finger

Finger is a Unix service that can be interacted with remotely to obtain information about computer users. When probed, it will generally list the login name and full name of the users that are currently using the system.

Default Port: **79**

## List Logged in Users

To get a list of users logged into a target system the following command can be issued:

`finger -l @target.domain.com`

The following perl script named "Finger-User-Enum" located on the Github page https://github.com/pentestmonkey/finger-user-enum, can be used to enumerate users:

`perl finger-user-enum.pl -U /path/to/common/names.txt -t 10.10.10.76`

Where "-U" specifies a text file with common usernames and "-t" is the target host or IP address.
