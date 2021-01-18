# Lightweight Directory Access Protocol (LDAP)

The Lightweight Directory Access Protocol (LDAP) is used extensively in Active Directory environments and allows for the querying of data that are stored in a hierarchical format and is based upon a stripped down version of the x.500 Data Access Protocol standard.

Default Cleartext Port: **389**

Default Secure Port: **636** (SSL/TLS)

Active Directory Global Catalog Default Port: **3268**

## Enumerating LDAP

There are a number of tools that can be used for enumerating LDAP built into Kali Linux, which include Nmap, ldapdomaindump and ldapsearch.  This section will cover the most common enumeration tools and techniques.

### Nmap

The `ldap-search` Nmap script can be used to extract information from LDAP. The following command will assume LDAP is running on the default port of 389:

`nmap -vv --script=ldap-search <IP Address> -p 389 --script-args ldap.maxobjects=-1`

The command will dump all all objects held within LDAP's directory structure.

### ldapdomaindump

The ldapdomaindump tool built into Kali Linux can also be used to dump all objects held within an LDAP directory structure.  The command below can optionally take a username for an authenticated LDAP dump:

`ldapdomaindump -u '<DOMAIN>\<USERNAME>' -p <PASSWORD> <IP ADDRESS> -o /path/to/output.txt`

### ldapsearch

Ldapsearch can be used to run a number of queries both authenticated and unauthenticated.  The following command will produce an unauthenticated dump of all objects held within the LDAP directory structure:

`ldapsearch -LLL -x -H ldap://<domain fqdn> -b '' -s base '(objectclass=*)'`

#### ldapsearch Extract All User Objects

Extract all user objects using ldapsearch:

`ldapsearch -x -h <IP Address> -D '<domain>\<username>' -w '<password>' -b "CN=Users,DC=<domain>,DC=<domain>"`

Remembering the `DC=<domain>` would be replaced with the domain name - I.E if the domain was `mydomain.local`, it would show as `DC=mydomain,DC=local`.

#### ldapsearch  Extract All Computer Objects

Extract all computer objects using ldapsearch:

`ldapsearch -x -h <IP Address> -D '<domain>\<username>' -w '<password>' -b "CN=Computers,DC=<domain>,DC=<domain>"`

#### ldapsearch Extract All Domain Admins

Extract all domain admins objects using ldapsearch:

`ldapsearch -x -h <IP Address> -D '<domain>\<username>' -w '<password>' -b "CN=Domain Admins,CN=Users,DC=<domain>,DC=<domain>"`


### Python 3 ldap3 library

Python 3 can be used in conjunction with the ldap3 library to enumerate ldap.  To install the ldap3 package the pip3 package manager can be used

`pip3 install ldap3`

From here the following script can be customised in order to execute queries against a server with LDAP enabled:

#### Unauthenticated (Null Bind) Ldap Secure with SSL Enabled

```python
#import ldap3 library
import ldap3

# Specify connection settings to server specifying the IP Address, Port and whether or not SSL is required

s = ldap3.Server(192.168.x.x,get_info=ldap3.ALL, port =636, use_ssl = True)

# Create connection to port 636 using ldap secure (SSL)

con = ldap3.Connection(s)
con.bind()

# Dump ALl LDAP, replace DOMAIN, DOMAIN, with the LDAP domain details, if the domain was mydomain.local, it would be DC=mydomain,DC=local

con.search(search_base='DC=DOMAIN,DC=DOMAIN', search_filter='(&(objectClass=*))', search_scope='SUBTREE')

# Print returned query

print(con.entries)


```
