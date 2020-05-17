# Token Impersonation Privilege Escalation

This section will assume that you already have a meterpreter session / shell on the target box. If you are looking for a safe environment to test this technique, the webite https://tryhackme.com has a great room named Alfred that goes through compromising an environment through the Jenkins CI/CD pipeline, then excalating privileges using this method. 

## Token Impersonation using Meterpreter and Incognito

To be able to impersonate the token of another and escalate privileges, user you must first check if you have the `SeImpersonatePrivilege` and `SeDebugPrivilege` privileges assigned to the compromised user accout.  To list the privileges assigned to an account the following command can be used within a shell on the target:

`whoami /priv`

Providing `SeImpersonatePrivilege` and `SeDebugPrivilege` are set to enabled, Incognito can now be used in the meterpreter session to list all available Delegation tokens available.  To load the incognito module within meterpreter:

`load incognito`

Once incognito has been loaded, the tokens can be listed:

`list_tokens -g`

When examining the list of available tokens, the idea is to select a token that will allow administrative privileges `BUILTIN\Administrators`, or access as the highest privilege `NT AUTHORITY\SYSTEM`.

To impersonate a specific token, the following command can be ran within the meterpreter session, providing the token is listed in the output from the aforementioned cmmand:

`impersonate_token "NT AUTHORITY\SYSTEM"`

This will now give the user the higest level of privileges on the target system.  After this point, it is recommended to migrate to another windows process that can not be closed by a standard user such as `services.exe`.  To migrate to a specific process, the `ps` command can be used within meterpreter to show a list of running processes. Select the process ID (PID) of the target process and run:

`migrate <PID>`

