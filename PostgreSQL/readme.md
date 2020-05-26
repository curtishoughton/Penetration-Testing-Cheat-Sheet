# PostGreSQL

## Determining Version of PostGreSQL Server

Metasploit can be used to determine the version of a PostGreSQL server using the following auxiliary module:

`use auxiliary/scanner/postgres/postgres_version`

## Dump Database Schema using Metasploit

Setting the RHOST value to the target server and using the following auxiliary module:

`use auxiliary/scanner/postgres/postgres_schemadump`

## Create a database user using Metasploit

Setting the RHOST value to the target server and using the following auxiliary module:

`use auxiliary/admin/postgres/postgres_sql`
`set SQL CREATE USER <username>;`
